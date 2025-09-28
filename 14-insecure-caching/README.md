# Challenge 14: Insecure Caching

This challenge demonstrates a vulnerability that can occur when you use insecure caching in your GitHub Actions workflows.

## Vulnerability

The `vulnerable.yml` workflow uses the `actions/cache` action to cache dependencies. However, the cache key is not specific enough, which can lead to a cache poisoning attack. An attacker can create a pull request that poisons the cache with malicious dependencies. When a subsequent workflow uses the same cache, it will download the malicious dependencies.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by using a more specific cache key. The cache key now includes the hash of the `package-lock.json` file, which ensures that the cache is only used for the exact same dependencies.

## Advanced Mitigation

In addition to using a more specific cache key, you can also use a tool like `npm-ci` to install your dependencies. `npm-ci` will install the exact versions of your dependencies from the `package-lock.json` file, which will help to prevent cache poisoning attacks.
