# Challenge 28: Insecure Use of `actions/github-script` with `workflow_dispatch`

This challenge demonstrates a vulnerability that can occur when you use the `actions/github-script` action in a workflow that is triggered by the `workflow_dispatch` event.

## Vulnerability

The `vulnerable.yml` workflow uses the `actions/github-script` action to execute a script that is influenced by user input from the `workflow_dispatch` event. This is a security risk because an attacker can provide malicious input that includes shell commands.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by checking the actor's permissions before performing any privileged operations.

## Advanced Mitigation

In addition to checking the actor's permissions, you can also use a tool like `gato` to enumerate your workflows and identify any that use `actions/github-script` with the `workflow_dispatch` trigger.
