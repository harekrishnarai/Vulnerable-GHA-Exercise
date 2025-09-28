# Advanced Attack Scenario 4: Advanced artifact poisoning techniques, including path traversal and symlink attacks

This scenario demonstrates how an attacker can use advanced artifact poisoning techniques, such as path traversal and symlink attacks, to overwrite sensitive files on the runner.

## The Vulnerability

A workflow that uses artifacts to pass data between jobs, but does not properly validate the artifact contents. The vulnerability lies in the fact that the `actions/download-artifact` action does not validate the artifact contents, which allows an attacker to create a malicious artifact that uses path traversal or symlinks to overwrite sensitive files on the runner.

**Vulnerable Code:**

```yaml
name: Vulnerable Workflow
on:
  pull_request_target:
    types: [opened]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          ref: ${{ github.event.pull_request.head.sha }}
      - name: Build
        run: |
          # This script creates a malicious artifact that uses a symlink to
          # overwrite the /etc/passwd file.
          ln -s /etc/passwd my-artifact
      - uses: actions/upload-artifact@v3
        with:
          name: my-artifact
          path: my-artifact
  deploy:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - uses: actions/download-artifact@v3
        with:
          name: my-artifact
      - name: Deploy
        run: |
          # This script is from the artifact, which is a symlink to /etc/passwd.
          # The following command will overwrite the /etc/passwd file.
          echo "root:x:0:0:root:/root:/bin/bash" > my-artifact
```

## The Exploitation

An attacker can create a pull request that creates a malicious artifact. When the workflow is triggered, the `deploy` job will download the malicious artifact and overwrite the `/etc/passwd` file.

**Malicious Payload:**

1.  Create a pull request that creates a malicious artifact.

**Expected Outcome:**

The `/etc/passwd` file on the runner will be overwritten.

## The Mitigation

To mitigate this vulnerability, you should not use artifacts from forked repositories in sensitive workflows. You should also validate the artifact contents before using them.

**Fixed Code:**

```yaml
name: Fixed Workflow
on:
  pull_request:
    types: [opened]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Build
        run: |
          # This script creates a safe artifact.
          echo "hello world" > my-artifact
      - uses: actions/upload-artifact@v3
        with:
          name: my-artifact
          path: my-artifact
  deploy:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - uses: actions/download-artifact@v3
        with:
          name: my-artifact
      - name: Deploy
        run: |
          # This script is now safe because the artifact is from a trusted source.
          cat my-artifact
```
