# Challenge 22: Dependency Confusion

This challenge demonstrates a vulnerability that can occur when you use a private package registry without properly scoping your package names.

## Vulnerability

The `vulnerable.yml` workflow installs a package from a private registry. However, the package name is not scoped, which means that an attacker can create a package with the same name and publish it to a public registry. When the workflow runs, it will download the malicious package from the public registry instead of the private one.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by using a scoped package name. This ensures that the workflow always downloads the package from the correct registry.

## Advanced Mitigation

In addition to using scoped package names, you can also use a tool like `gato` to enumerate your dependencies and identify any that are vulnerable to dependency confusion.
