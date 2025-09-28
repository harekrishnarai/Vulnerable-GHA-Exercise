# Advanced Attack Scenario 6: Bypassing Branch Protection Rules with Insecure Workflows

This scenario demonstrates how an attacker can bypass branch protection rules by exploiting a workflow that is triggered on `pull_request_target` and has the ability to approve pull requests.

## The Vulnerability

A workflow that is triggered on `pull_request_target` and has the ability to approve pull requests. The vulnerability lies in the fact that the workflow can be triggered by a pull request from a forked repository, and the workflow will run with the permissions of the base repository. This means that an attacker can create a pull request with malicious code and then use the workflow to approve their own pull request, bypassing branch protection rules.

**Vulnerable Code:**

```yaml
name: Vulnerable Workflow
on:
  pull_request_target:
    types: [opened]
jobs:
  vulnerable_job:
    runs-on: ubuntu-latest
    permissions:
      pull-requests: write
    steps:
      - uses: actions/github-script@v6
        with:
          script: |
            github.rest.pulls.createReview({
              owner: context.repo.owner,
              repo: context.repo.repo,
              pull_number: context.issue.number,
              event: 'APPROVE'
            });
```

## The Exploitation

An attacker can create a pull request with malicious code and then the workflow will automatically approve it.

**Malicious Payload:**

1.  Create a pull request with malicious code.

**Expected Outcome:**

The workflow will approve the pull request, and the malicious code will be merged into the `main` branch.

## The Mitigation

To mitigate this vulnerability, you should not use `pull_request_target` to approve pull requests. Instead, you should use a separate workflow that is triggered on `pull_request` and that is configured to only run for trusted users.

**Fixed Code:**

```yaml
name: Fixed Workflow
on:
  pull_request:
    types: [opened]
jobs:
  fixed_job:
    runs-on: ubuntu-latest
    if: github.actor == 'my-trusted-user'
    permissions:
      pull-requests: write
    steps:
      - uses: actions/github-script@v6
        with:
          script: |
            github.rest.pulls.createReview({
              owner: context.repo.owner,
              repo: context.repo.repo,
              pull_number: context.issue.number,
              event: 'APPROVE'
            });
```
