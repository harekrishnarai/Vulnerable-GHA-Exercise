# Advanced Attack Scenario 8: Advanced techniques for exfiltrating data from self-hosted runners

This scenario demonstrates how an attacker can use advanced techniques, such as DNS tunneling, to exfiltrate sensitive data from a self-hosted runner.

## The Vulnerability

A workflow that runs on a self-hosted runner and has access to sensitive data. The vulnerability lies in the fact that the self-hosted runner is not properly secured, which allows an attacker to exfiltrate the sensitive data.

**Vulnerable Code:**

```yaml
name: Vulnerable Workflow
on:
  push:
    branches: [ main ]
jobs:
  vulnerable_job:
    runs-on: self-hosted
    steps:
      - name: Vulnerable Step
        run: |
          # This script has access to sensitive data
          cat /etc/sensitive-data
```

## The Exploitation

An attacker can use DNS tunneling to exfiltrate the sensitive data. DNS tunneling is a technique that allows an attacker to exfiltrate data over the DNS protocol. This is often effective because many organizations do not monitor DNS traffic for malicious activity.

**Malicious Payload:**

An attacker can use a tool like `iodine` to create a DNS tunnel. Once the tunnel is established, the attacker can use it to exfiltrate the sensitive data.

**Expected Outcome:**

The attacker exfiltrates the sensitive data from the self-hosted runner.

## The Mitigation

To mitigate this vulnerability, you should properly secure your self-hosted runners. This includes:

*   **Isolating your self-hosted runners from the host machine.** You can do this by using a container to run your workflows.
*   **Restricting the permissions of your self-hosted runners.** You should only give your self-hosted runners the permissions that they need to do their job.
*   **Monitoring your self-hosted runners for suspicious activity.** You can use a host-based intrusion detection system (HIDS) to monitor for suspicious activity on your self-hosted runners.
*   **Monitoring your DNS traffic for malicious activity.** You can use a network intrusion detection system (NIDS) to monitor for suspicious DNS traffic from your self-hosted runners.
