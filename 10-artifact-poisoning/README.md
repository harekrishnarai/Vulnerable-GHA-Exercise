# Challenge 10: Artifact Poisoning

This challenge demonstrates a vulnerability that can occur when a GitHub Actions workflow uses artifacts to pass data between jobs, but does not properly validate the artifact contents.

## Vulnerability

The `vulnerable.yml` workflow has two jobs: `build` and `deploy`. The `build` job creates an artifact, and the `deploy` job downloads the artifact and uses it. The vulnerability lies in the fact that the `deploy` job does not validate the artifact contents. An attacker can create a malicious artifact that contains a malicious script. When the `deploy` job downloads the artifact, it will execute the malicious script.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by not using artifacts from forked repositories in sensitive workflows.

## Advanced Mitigation

In addition to not using artifacts from forked repositories in sensitive workflows, you can also validate the artifact contents before using them. For example, you can use a checksum to verify the integrity of the artifact.
