# Challenge 18: Leaking Secrets via `on: workflow_run`

This challenge demonstrates a vulnerability that can occur when you use the `workflow_run` event to trigger a workflow that has access to secrets.

## Vulnerability

The `vulnerable.yml` workflow is triggered when another workflow is completed. The `vulnerable_job` uses the `workflow_run` trigger, which has access to secrets. An attacker can create a pull request that triggers the "CI" workflow, which in turn triggers the `vulnerable_job`. The `vulnerable_job` then prints the secret to the console, which can be viewed by the attacker.

## Fix

The `fixed.yml` workflow mitigates this vulnerability by checking if the triggering workflow is from the same repository before accessing secrets.

## Advanced Mitigation

In addition to checking if the triggering workflow is from the same repository, you can also use a tool like `gato` to enumerate your workflows and identify any that are triggered by `workflow_run` and have access to secrets.
