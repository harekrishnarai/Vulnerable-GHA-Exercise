# Challenge 9: Hardcoded Credentials in Workflow

This challenge demonstrates a vulnerability that can occur when you hardcode credentials in your GitHub Actions workflows.

## Vulnerability

The `vulnerable.yml` workflow has a hardcoded API key in the `env` block. This is a security risk because the API key is stored in plain text in the workflow file. Anyone with read access to the repository can view the API key.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by using a GitHub secret to store the API key. This ensures that the API key is not stored in plain text in the workflow file.

## Advanced Mitigation

In addition to using GitHub secrets, you can also use a secrets management solution like HashiCorp Vault or AWS Secrets Manager to store your secrets. These solutions provide additional security features, such as auditing and rotation.
