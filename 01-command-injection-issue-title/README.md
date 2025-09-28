# Challenge 1: Command Injection

This challenge demonstrates a command injection vulnerability that can occur when using the issue title directly in a `run` step.

## Vulnerability

The `vulnerable.yml` workflow is triggered when a new issue is opened. The `run` step in the `vulnerable_job` directly uses the issue title in a shell command. An attacker can create an issue with a malicious title that includes shell commands.

In this challenge, the `flag.txt` file is created on the runner during the workflow execution. This means that you cannot simply clone the repository to read the flag. You will need to exploit the command injection vulnerability to read the flag.

**Vulnerable Code:**
```yaml
name: Challenge 1
on:
  issues:
    types: [opened]
jobs:
  challenge1:
    runs-on: ubuntu-latest
    steps:
      - name: Challenge 1
        run: |
          echo "The flag is in a file called flag.txt"
          echo "flag{this_is_the_flag}" > flag.txt
          echo "Your input: ${{ github.event.issue.title }}"
```

## Hint

Try to use a shell command to read the contents of the `flag.txt` file.

## Solution

Create an issue with the title: `'; cat flag.txt'`