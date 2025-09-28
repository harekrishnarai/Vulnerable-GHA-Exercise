# Challenge 17: Insecure Environment Variable Parsing

This challenge demonstrates a vulnerability that can occur when you use `eval` on an environment variable that contains untrusted user input.

## Vulnerability

The `vulnerable.yml` workflow has a `run` step that uses `eval` on an environment variable that contains untrusted user input from an issue title. This is a security risk because an attacker can create an issue with a malicious title that includes shell commands.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by treating the user input as a string and not executing it.

## Advanced Mitigation

In addition to treating the user input as a string, you can also use input validation to ensure that the user input does not contain any malicious characters.
