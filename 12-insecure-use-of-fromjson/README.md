# Challenge 12: Insecure Use of `fromJSON`

This challenge demonstrates a vulnerability that can occur when you use the `fromJSON` function with untrusted input.

## Vulnerability

The `vulnerable.yml` workflow has a `run` step that uses the `fromJSON` function to parse a JSON string from an issue title. This is a security risk because an attacker can create an issue with a malicious title that includes a malicious JSON payload.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by avoiding the use of `fromJSON` with untrusted input. Instead, the workflow passes the user input as an environment variable to a script, which can then safely parse the JSON.

## Advanced Mitigation

In addition to avoiding the use of `fromJSON` with untrusted input, you can also use a JSON schema to validate the JSON before parsing it.
