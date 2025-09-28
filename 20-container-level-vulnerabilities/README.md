# Challenge 20: Container-Level Vulnerabilities in Actions

This challenge demonstrates a vulnerability that can occur when you use a container image with known vulnerabilities in your GitHub Actions workflows.

## Vulnerability

The `vulnerable.yml` workflow uses an old version of the `node` container image, which may have known vulnerabilities. This is a security risk because an attacker could exploit a vulnerability in the container image to gain access to the runner.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by using a more recent version of the `node` container image.

## Advanced Mitigation

In addition to using a more recent version of the container image, you can also use a container image scanning tool to scan your container images for known vulnerabilities.
