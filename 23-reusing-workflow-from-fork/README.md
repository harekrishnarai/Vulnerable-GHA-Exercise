# Challenge 23: Reusing Workflow from a Forked Repository

This challenge demonstrates a vulnerability that can occur when you reuse a workflow from a forked repository.

## Vulnerability

The `vulnerable.yml` workflow reuses a workflow from a forked repository. This is a security risk because the forked repository could have been modified to include malicious code.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by pinning the reusable workflow to a specific commit hash. This ensures that the workflow always uses the exact same version of the reusable workflow, even if the forked repository is updated.

## Advanced Mitigation

In addition to pinning the reusable workflow to a specific commit hash, you can also use a tool like `gato` to enumerate your workflows and identify any that reuse workflows from forked repositories.
