# Challenge 29: Insecure Use of `actions/github-script` with `schedule`

This challenge demonstrates a vulnerability that can occur when you use the `actions/github-script` action in a workflow that is triggered by the `schedule` event.

## Vulnerability

The `vulnerable.yml` workflow uses the `actions/github-script` action to execute a script on a schedule. This is a security risk because an attacker who gains write access to the repository can modify the script to execute arbitrary commands.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by regularly reviewing the script for security issues.

## Advanced Mitigation

In addition to regularly reviewing the script for security issues, you can also use a tool like `gato` to enumerate your workflows and identify any that use `actions/github-script` with the `schedule` trigger.
