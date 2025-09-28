# GitHub Actions Vulnerabilities and Fixes

This repository contains a collection of challenges designed to help you learn about common GitHub Actions vulnerabilities and how to fix them. Each challenge is inspired by the work of security researcher Adnan E Khan.

## Challenges

1.  **[Command Injection from Issue Title](./01-command-injection-issue-title/)**
    *   **Vulnerability:** Using the issue title directly in a `run` step can lead to command injection.
    *   **Fix:** Pass the issue title as an environment variable to the script.

2.  **[Command Injection from Pull Request Title](./02-command-injection-pr-title/)**
    *   **Vulnerability:** Using the pull request title directly in a `run` step can lead to command injection.
    *   **Fix:** Pass the pull request title as an environment variable to the script.

3.  **[Command Injection from Pull Request Branch Name](./03-command-injection-pr-branch-name/)**
    *   **Vulnerability:** Using the pull request branch name directly in a `run` step can lead to command injection.
    *   **Fix:** Pass the branch name as an environment variable to the script.

4.  **[Command Injection from Issue Body](./04-command-injection-issue-body/)**
    *   **Vulnerability:** Using the issue body directly in a `run` step can lead to command injection.
    *   **Fix:** Pass the issue body as an environment variable to the script.

5.  **[Untrusted Checkout with `pull_request_target`](./05-untrusted-checkout-pull_request_target/)**
    *   **Vulnerability:** Using `pull_request_target` with an explicit checkout of the pull request's head SHA can lead to the execution of untrusted code with access to secrets.
    *   **Fix:** When using `pull_request_target`, checkout the base SHA of the pull request instead of the head SHA.

6.  **[Insecure Workflow Triggers (`pull_request_target`)](./06-insecure-workflow-triggers/)**
    *   **Vulnerability:** Using the `pull_request_target` trigger without proper precautions can expose secrets to untrusted code.
    *   **Fix:** Use the `pull_request` trigger instead, which does not have access to secrets for forked repositories.

7.  **[Unpinned Dependencies (Actions)](./07-unpinned-dependencies/)**
    *   **Vulnerability:** Using a floating version of a GitHub Action (e.g., `@main`) can lead to a supply chain attack if the action is compromised.
    *   **Fix:** Pin the action to a specific commit hash.

8.  **[Lack of Token Permissions](./08-lack-of-token-permissions/)**
    *   **Vulnerability:** The default `GITHUB_TOKEN` has broad permissions, which can be a security risk if the token is compromised.
    *   **Fix:** Restrict the token's permissions to the minimum required for the workflow to function.

9.  **[Hardcoded Credentials in Workflow](./09-hardcoded-credentials/)**
    *   **Vulnerability:** Storing secrets directly in the workflow file exposes them to anyone with read access to the repository.
    *   **Fix:** Use GitHub repository secrets to store sensitive information.

10. **[Artifact Poisoning](./10-artifact-poisoning/)**
    *   **Vulnerability:** An attacker can poison a build artifact in one job, which is then used by a subsequent job with higher privileges.
    *   **Fix:** Do not use artifacts from forked repositories in sensitive workflows.

11. **[Script Injection via `run` Step](./11-script-injection-run-step/)**
    *   **Vulnerability:** Directly embedding user-provided input into a `run` step can lead to script injection.
    *   **Fix:** Pass user input as an environment variable.

12. **[Insecure Use of `fromJSON`](./12-insecure-use-of-fromjson/)**
    *   **Vulnerability:** Using `fromJSON` with untrusted input can lead to the execution of arbitrary code.
    *   **Fix:** Avoid using `fromJSON` with untrusted input. Instead, parse the input in a script.

13. **[Publicly Exposed Self-Hosted Runners](./13-publicly-exposed-self-hosted-runners/)**
    *   **Vulnerability:** A publicly accessible self-hosted runner can be a target for attackers.
    *   **Fix:** Use GitHub-hosted runners for public repositories.

14. **[Insecure Caching](./14-insecure-caching/)**
    *   **Vulnerability:** An attacker can poison the cache with malicious dependencies.
    *   **Fix:** Use `npm ci` instead of `npm install` to ensure that the exact versions of dependencies from the `package-lock.json` file are used.

15. **[CodeQL Analysis Missing](./15-codeql-analysis-missing/)**
    *   **Vulnerability:** Not having CodeQL analysis in the CI/CD pipeline can lead to security vulnerabilities going undetected.
    *   **Fix:** Add CodeQL analysis to the workflow.
16. **[TOCTOU Race Condition with `pull_request_target`](./16-toctou-race-condition/)**
    *   **Vulnerability:** An attacker can modify a pull request after a workflow is triggered but before the code is checked out, leading to a race condition.
    *   **Fix:** Checkout the base SHA of the pull request instead of the head SHA.

17. **[Insecure Environment Variable Parsing](./17-insecure-environment-variable-parsing/)**
    *   **Vulnerability:** Using `eval` on an environment variable that contains untrusted user input can lead to command injection.
    *   **Fix:** Treat user input as a string and avoid using `eval`.

18. **[Leaking Secrets via `on: workflow_run`](./18-leaking-secrets-via-workflow-run/)**
    *   **Vulnerability:** A workflow triggered by `workflow_run` can have access to secrets, which can be leaked if the triggering workflow is from a forked repository.
    *   **Fix:** Check if the triggering workflow is from the same repository before accessing secrets.

19. **[Insecure `actions/github-script` Usage](./19-insecure-github-script-usage/)**
    *   **Vulnerability:** Using `exec` within `actions/github-script` with untrusted input can lead to command injection.
    *   **Fix:** Avoid using `exec` with untrusted input. Use `console.log` to safely output user-provided content.

20. **[Container-Level Vulnerabilities in Actions](./20-container-level-vulnerabilities/)**
    *   **Vulnerability:** Using an old container image with known vulnerabilities can expose the workflow to attacks.
    *   **Fix:** Use a recent, secure container image.

21. **[Insecure Self-Hosted Runner Configuration](./21-insecure-self-hosted-runner-configuration/)**
    *   **Vulnerability:** A self-hosted runner that is not properly isolated can be a target for attackers, who could potentially gain access to the host machine.
    *   **Fix:** Run workflows in a container on the self-hosted runner to provide an extra layer of isolation.

22. **[Dependency Confusion](./22-dependency-confusion/)**
    *   **Vulnerability:** If a package with the same name as an internal package is published to a public registry, the public package may be downloaded instead of the internal one.
    *   **Fix:** Use scoped package names to prevent dependency confusion.

23. **[Reusing Workflow from a Forked Repository](./23-reusing-workflow-from-fork/)**
    *   **Vulnerability:** Reusing a workflow from a forked repository can lead to the execution of untrusted code.
    *   **Fix:** Pin the reusable workflow to a specific commit hash.

24. **[Insecure `if` Condition with `github.event`](./24-insecure-if-condition/)**
    *   **Vulnerability:** Using an `if` condition that relies on user-controllable data (e.g., the pull request title) can be bypassed by an attacker.
    *   **Fix:** Use a more reliable condition, such as the actor's username.

25. **[Leaking Secrets through `curl`](./25-leaking-secrets-through-curl/)**
    *   **Vulnerability:** Leaking secrets to a third-party service via `curl` or other means.
    *   **Fix:** Do not send secrets to third-party services.

26. **[Insecure Use of `actions/github-script` with `pull_request_target`](./26-insecure-github-script-pull-request-target/)**
    *   **Vulnerability:** Using `actions/github-script` in a workflow triggered by `pull_request_target` can be influenced by the pull request's title.
    *   **Fix:** Use the `pull_request` trigger instead, which does not have access to secrets for forked repositories.

27. **[Insecure Use of `actions/github-script` with `issue_comment`](./27-insecure-github-script-issue-comment/)**
    *   **Vulnerability:** Using `actions/github-script` in a workflow triggered by `issue_comment` can be influenced by the comment body.
    *   **Fix:** Check the actor's permissions before performing any privileged operations.

28. **[Insecure Use of `actions/github-script` with `workflow_dispatch`](./28-insecure-github-script-workflow-dispatch/)**
    *   **Vulnerability:** Using `actions/github-script` in a workflow triggered by `workflow_dispatch` can be influenced by user input.
    *   **Fix:** Check the actor's permissions before performing any privileged operations.

29. **[Insecure Use of `actions/github-script` with `schedule`](./29-insecure-github-script-schedule/)**
    *   **Vulnerability:** A scheduled workflow with `actions/github-script` could be compromised if the repository is compromised.
    *   **Fix:** Regularly review the script for security issues.

30. **[Insecure Use of `actions/github-script` with `release`](./30-insecure-github-script-release/)**
    *   **Vulnerability:** Using `actions/github-script` in a workflow triggered by a release can be influenced by the release name.
    *   **Fix:** Check the actor's permissions before performing any privileged operations.

## Advanced Scenarios

31. **[Advanced Attack Scenario 1: Chaining Vulnerabilities for a Full Repository Takeover](./31-advanced-attack-scenario-1/)**
32. **[Advanced Attack Scenario 2: Bypassing OIDC with `workflow_dispatch` and User-Controlled Inputs](./32-bypassing-oidc-with-workflow-dispatch/)**
33. **[Advanced Attack Scenario 3: Exploiting insecure `actions/github-script` with `pull_request_target` and `issue_comment` to exfiltrate secrets](./33-insecure-github-script-exfiltration/)**
34. **[Advanced Attack Scenario 4: Advanced artifact poisoning techniques, including path traversal and symlink attacks](./34-advanced-artifact-poisoning/)**
35. **[Advanced Attack Scenario 5: Exploiting vulnerabilities in custom GitHub Actions written in JavaScript or Docker](./35-exploiting-custom-actions/)**
36. **[Advanced Attack Scenario 6: Bypassing Branch Protection Rules with Insecure Workflows](./36-bypassing-branch-protection/)**
37. **[Advanced Attack Scenario 7: Exploiting insecure webhooks that trigger workflows](./37-exploiting-insecure-webhooks/)**
38. **[Advanced Attack Scenario 8: Advanced techniques for exfiltrating data from self-hosted runners](./38-advanced-data-exfiltration/)**
39. **[Advanced Attack Scenario 9: Exploiting misconfigured runner groups and labels](./39-exploiting-misconfigured-runner-groups/)**
40. **[Advanced Attack Scenario 10: GitHub Actions CTF-style challenge](./40-github-actions-ctf/)**