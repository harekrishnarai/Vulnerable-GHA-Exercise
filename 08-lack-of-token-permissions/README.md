# Challenge 8: Lack of Token Permissions

This challenge demonstrates a vulnerability that can occur when a GitHub Actions workflow has overly permissive token permissions.

## Vulnerability

The `vulnerable.yml` workflow has the default `GITHUB_TOKEN` permissions, which are broad. This can be a security risk if the token is compromised. An attacker who gains access to the token could potentially use it to perform a wide range of malicious activities, such as creating new branches, pushing code, and deleting repositories.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by restricting the token's permissions to the minimum required for the workflow to function. In this case, the workflow only needs to read the contents of the repository, so the `contents` permission is set to `read`.

## Advanced Mitigation

In addition to restricting the token's permissions, you can also use a tool like `gato` to enumerate the permissions of your workflows and identify any that are overly permissive.
