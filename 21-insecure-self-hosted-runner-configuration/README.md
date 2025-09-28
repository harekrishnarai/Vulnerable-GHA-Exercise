# Challenge 21: Insecure Self-Hosted Runner Configuration

This challenge demonstrates a vulnerability that can occur when you use a self-hosted runner that is not properly isolated.

## Vulnerability

The `vulnerable.yml` workflow runs on a self-hosted runner that is not properly isolated. This is a security risk because an attacker who gains access to the runner could also gain access to the host machine.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by running the workflow in a container on the self-hosted runner. This provides an extra layer of isolation between the workflow and the host machine.

## Advanced Mitigation

In addition to running your workflows in a container, you can also use a tool like `gato` to enumerate your self-hosted runners and identify any that are not properly isolated.
