# Challenge 26: Insecure Use of `actions/github-script` with `pull_request_target`

This challenge demonstrates a vulnerability that can occur when you use the `actions/github-script` action in a workflow that is triggered by the `pull_request_target` event.

## Vulnerability

The `vulnerable.yml` workflow uses the `actions/github-script` action to execute a script that is influenced by the title of a pull request. This is a security risk because an attacker can create a pull request with a malicious title that includes shell commands.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by using the `pull_request` trigger instead of `pull_request_target`. This ensures that the workflow does not have access to secrets for forked repositories.

## Advanced Mitigation

In addition to using the `pull_request` trigger, you can also use a tool like `gato` to enumerate your workflows and identify any that use `actions/github-script` with the `pull_request_target` trigger.
