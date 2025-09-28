# Challenge 19: Insecure `actions/github-script` Usage

This challenge demonstrates a vulnerability that can occur when you use the `actions/github-script` action with untrusted input.

## Vulnerability

The `vulnerable.yml` workflow uses the `actions/github-script` action to execute a script that is influenced by the title of an issue. This is a security risk because an attacker can create an issue with a malicious title that includes shell commands.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by avoiding the use of `exec` with untrusted input. Instead, the workflow uses `console.log` to safely output the user-provided content.

## Advanced Mitigation

In addition to avoiding the use of `exec` with untrusted input, you can also use a tool like `gato` to enumerate your workflows and identify any that use `actions/github-script` with untrusted input.
