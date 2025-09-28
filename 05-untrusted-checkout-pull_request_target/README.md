# Challenge 5: Untrusted Checkout with `pull_request_target`

This challenge demonstrates a vulnerability that can occur when using `pull_request_target` with an explicit checkout of the pull request's head SHA.

## Vulnerability

The `vulnerable.yml` workflow is triggered when a new pull request is opened. The `vulnerable_job` uses the `pull_request_target` trigger, which has access to secrets. However, it also checks out the pull request's head SHA, which means that it is executing untrusted code with access to secrets.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by checking out the pull request's base SHA instead of the head SHA. This ensures that the workflow is executing trusted code.

## Advanced Mitigation

In addition to checking out the base SHA, you can also use a separate workflow that is triggered on `pull_request` to run tests on the pull request's head SHA. This workflow will not have access to secrets, so it will not be able to exfiltrate them.
