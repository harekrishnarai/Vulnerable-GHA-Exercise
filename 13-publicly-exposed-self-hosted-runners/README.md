# Challenge 13: Publicly Exposed Self-Hosted Runners

This challenge demonstrates a vulnerability that can occur when you use a publicly exposed self-hosted runner.

## Vulnerability

The `vulnerable.yml` workflow uses a self-hosted runner that is publicly exposed. This is a security risk because an attacker can create a workflow that runs on the same runner and gain access to the host machine.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by using a GitHub-hosted runner instead of a self-hosted runner.

## Advanced Mitigation

If you must use a self-hosted runner, you should take steps to secure it. This includes:

*   **Isolating the runner from the host machine.** You can do this by using a container to run your workflows.
*   **Restricting the permissions of the runner.** You should only give the runner the permissions that it needs to do its job.
*   **Monitoring the runner for suspicious activity.** You can use a host-based intrusion detection system (HIDS) to monitor for suspicious activity on the runner.
