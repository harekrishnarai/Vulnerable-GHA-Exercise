# Advanced Attack Scenario 3: Exploiting insecure `actions/github-script` with `pull_request_target` and `issue_comment` to exfiltrate secrets

This scenario demonstrates how an attacker can exploit a workflow that uses `actions/github-script` to comment on a pull request, but is triggered on `pull_request_target`. The attacker can use the `issue_comment` event to trigger the workflow and exfiltrate secrets.

## The Vulnerability

A workflow that uses `actions/github-script` to comment on a pull request, but is triggered on `pull_request_target`. The vulnerability lies in the fact that the `github-script` action is executed in the context of the `pull_request_target` event, which has access to secrets. An attacker can create a pull request and then use the `issue_comment` event to trigger the workflow and exfiltrate secrets.

**Vulnerable Code:**

```yaml
name: Vulnerable Workflow
on:
  pull_request_target:
    types: [opened]
  issue_comment:
    types: [created]
jobs:
  vulnerable_job:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/github-script@v6
        with:
          script: |
            const pr = context.payload.pull_request;
            if (pr) {
              github.rest.issues.createComment({
                owner: context.repo.owner,
                repo: context.repo.repo,
                issue_number: pr.number,
                body: `Thank you for your contribution! The secret is: ${{ secrets.MY_SECRET }}`
              });
            }
```

## The Exploitation

An attacker can create a pull request and then create a comment on the pull request. This will trigger the workflow, which will then post a comment on the pull request with the secret.

**Malicious Payload:**

1.  Create a pull request.
2.  Create a comment on the pull request.

**Expected Outcome:**

The workflow will post a comment on the pull request with the secret.

## The Mitigation

To mitigate this vulnerability, you should not use `actions/github-script` to comment on a pull request in a workflow that is triggered on `pull_request_target`. Instead, you should use a separate workflow that is triggered on `pull_request` to comment on the pull request.

**Fixed Code:**

```yaml
name: Fixed Workflow
on:
  pull_request:
    types: [opened]
jobs:
  fixed_job:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/github-script@v6
        with:
          script: |
            const pr = context.payload.pull_request;
            if (pr) {
              github.rest.issues.createComment({
                owner: context.repo.owner,
                repo: context.repo.repo,
                issue_number: pr.number,
                body: `Thank you for your contribution!`
              });
            }
```
