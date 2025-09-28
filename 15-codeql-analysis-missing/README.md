# Challenge 15: CodeQL Analysis Missing

This challenge demonstrates a vulnerability that can occur when you do not have CodeQL analysis in your CI/CD pipeline.

## Vulnerability

The `vulnerable.yml` workflow does not have CodeQL analysis. This is a security risk because it can lead to security vulnerabilities going undetected.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by adding CodeQL analysis to the workflow.

## Advanced Mitigation

In addition to adding CodeQL analysis to your workflow, you can also use a tool like `gato` to enumerate your workflows and identify any that do not have CodeQL analysis.
