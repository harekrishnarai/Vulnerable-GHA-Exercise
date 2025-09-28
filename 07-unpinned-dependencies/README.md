# Challenge 7: Unpinned Dependencies (Actions)

This challenge demonstrates a vulnerability that can occur when using unpinned dependencies in your GitHub Actions workflows.

## Vulnerability

The `vulnerable.yml` workflow uses a floating version of a GitHub Action (e.g., `@main`). This can lead to a supply chain attack if the action is compromised. An attacker could push a malicious update to the `main` branch of the action, and any workflow that uses the `@main` tag will automatically start using the malicious version.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by pinning the action to a specific commit hash. This ensures that the workflow always uses the exact same version of the action, even if the `main` branch is updated.

## Advanced Mitigation

In addition to pinning the action to a specific commit hash, you can also use a tool like `dependabot` to automatically create pull requests to update your pinned actions to the latest version. This will help you to stay up-to-date with the latest security fixes and features, while still protecting you from supply chain attacks.
