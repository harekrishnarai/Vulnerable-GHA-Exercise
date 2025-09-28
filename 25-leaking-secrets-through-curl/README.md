# Challenge 25: Leaking Secrets through `curl`

This challenge demonstrates a vulnerability that can occur when you leak secrets to a third-party service via `curl` or other means.

## Vulnerability

The `vulnerable.yml` workflow has a `run` step that leaks a secret to a third-party service via `curl`. This is a security risk because the secret is now exposed to the third-party service.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by not leaking the secret to the third-party service.

## Advanced Mitigation

In addition to not leaking secrets to third-party services, you can also use a tool like `gato` to enumerate your workflows and identify any that leak secrets.
