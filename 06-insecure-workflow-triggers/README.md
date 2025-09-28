# Challenge 6: Insecure Workflow Triggers (`pull_request_target`)

This challenge demonstrates a vulnerability that can occur when using the `pull_request_target` trigger without proper precautions.

## Vulnerability

The `vulnerable.yml` workflow is triggered when a new pull request is opened. The `vulnerable_job` uses the `pull_request_target` trigger, which has access to secrets. However, it also checks out the pull request's head SHA, which means that it is executing untrusted code with access to secrets.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by using the `pull_request` trigger instead of `pull_request_target`. This ensures that the workflow does not have access to secrets for forked repositories.

## Advanced Mitigation

In addition to using the `pull_request` trigger, you can also use a separate workflow that is triggered on `pull_request_target` to perform checks that do not require access to secrets. This can be useful for things like linting and running tests.
