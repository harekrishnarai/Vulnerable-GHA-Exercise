# Challenge 11: Script Injection via `run` Step

This challenge demonstrates a vulnerability that can occur when you directly embed user-provided input into a `run` step.

## Vulnerability

The `vulnerable.yml` workflow has a `run` step that directly embeds user-provided input from an issue title into a shell command. This is a security risk because an attacker can create an issue with a malicious title that includes shell commands.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by passing the user input as an environment variable to the script. This ensures that the user input is treated as a single string and not executed as a command.

## Advanced Mitigation

In addition to using environment variables, you can also use input validation to ensure that the user input does not contain any malicious characters.
