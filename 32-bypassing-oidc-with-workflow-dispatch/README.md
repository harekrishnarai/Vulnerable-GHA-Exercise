# Advanced Attack Scenario 2: Bypassing OIDC with `workflow_dispatch` and User-Controlled Inputs

This scenario demonstrates how an attacker can bypass OIDC authentication in a GitHub Actions workflow by manipulating user-controlled inputs in a `workflow_dispatch` trigger.

## The Vulnerability

A workflow that uses OIDC to authenticate to a cloud provider, but also uses a `workflow_dispatch` trigger with user-controlled inputs. The vulnerability lies in the fact that the user-controlled inputs are not properly validated before being used to construct the OIDC token.

**Vulnerable Code:**

```yaml
name: Vulnerable Workflow
on:
  workflow_dispatch:
    inputs:
      role_to_assume:
        description: 'The ARN of the role to assume'
        required: true
jobs:
  vulnerable_job:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          role-to-assume: ${{ github.event.inputs.role_to_assume }}
          aws-region: us-east-1
      - name: Vulnerable Step
        run: |
          # This script is now running with the assumed role
          aws s3 ls
```

## The Exploitation

An attacker can use the user-controlled `role_to_assume` input to specify a role that they control. This will cause the workflow to assume the attacker's role, giving them access to the cloud provider.

**Malicious Payload:**

An attacker can trigger the workflow with the following input:

```json
{
  "role_to_assume": "arn:aws:iam::123456789012:role/attacker-role"
}
```

**Expected Outcome:**

The workflow will assume the attacker's role, giving them access to the cloud provider. The attacker can then use this access to exfiltrate data, launch new instances, or perform other malicious activities.

## The Mitigation

To mitigate this vulnerability, you should properly validate the user-controlled inputs before using them to construct the OIDC token. For example, you can use a regular expression to ensure that the `role_to_assume` input only contains a valid ARN.

**Fixed Code:**

```yaml
name: Fixed Workflow
on:
  workflow_dispatch:
    inputs:
      role_to_assume:
        description: 'The ARN of the role to assume'
        required: true
jobs:
  fixed_job:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
      - name: Validate input
        run: |
          if ! [[ "${{ github.event.inputs.role_to_assume }}" =~ ^arn:aws:iam::[0-9]{12}:role/[a-zA-Z0-9_-]+$ ]]; then
            echo "Invalid role ARN"
            exit 1
          fi
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          role-to-assume: ${{ github.event.inputs.role_to_assume }}
          aws-region: us-east-1
      - name: Fixed Step
        run: |
          # This script is now running with the assumed role
          aws s3 ls
```
