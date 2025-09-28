# Challenge 16: TOCTOU Race Condition with `pull_request_target`

This challenge demonstrates a time-of-check to time-of-use (TOCTOU) race condition vulnerability that can occur when using the `pull_request_target` trigger.

## Vulnerability

The `vulnerable.yml` workflow is triggered when a new pull request is opened. The `vulnerable_job` uses the `pull_request_target` trigger, which has access to secrets. The workflow checks out the base SHA of the pull request, but then it checks out the head SHA of the pull request in a separate step. This creates a race condition where an attacker can modify the pull request after the workflow is triggered but before the head SHA is checked out.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by checking out the base SHA of the pull request and not the head SHA. This ensures that the workflow is executing trusted code.

## Advanced Mitigation

In addition to checking out the base SHA, you can also use a separate workflow that is triggered on `pull_request` to run tests on the pull request's head SHA. This workflow will not have access to secrets, so it will not be able to exfiltrate them.
