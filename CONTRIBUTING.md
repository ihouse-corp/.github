# Contributing

## Pull Request Policy

- Do not push directly to `main`.
- Create a pull request for every source, workflow, infrastructure, or documentation change.
- Keep pull requests scoped to one explainable, reviewable change.
- Prefer squash merge after review and validation.
- Delete the head branch after merge.

When GitHub-native branch protection or rulesets are available, repositories should enforce:

- pull request required before merging;
- direct pushes to `main` blocked;
- required status checks passing before merge;
- branch must be up to date before merge;
- force pushes and branch deletion blocked.

Required checks are not a standalone repository setting. They are enforced through branch protection or rulesets. If a private repository cannot enable those GitHub features on the current plan, maintainers must still follow the PR-only process and keep the missing hard gate tracked as an upgrade blocker.

## Self-Hosted Runner Policy

Use the narrowest runner scope that matches the job.

Repository-level runners are preferred when all of these are true:

- the runner exists for one repository's release or platform workflow;
- the runner has platform-specific SDKs, signing material, caches, or device/toolchain state;
- other repositories should not be able to schedule jobs on it;
- the runner is scarce or operationally stateful.

Organization-level runners are appropriate when all of these are true:

- more than one repository legitimately needs the same capability;
- the runner is safe for shared CI or review workloads;
- access is controlled through an organization runner group;
- the runner group is restricted to selected repositories;
- workflow permissions and secrets stay least-privilege.

Labels describe runner capability and scheduling intent. They do not replace runner groups, repository scope, or secret boundaries.

## Current JClaw Runner Placement

- Android mobile release capacity stays repository-scoped to the mobile repository.
- Windows desktop release capacity stays repository-scoped to the Windows desktop repository.
- Shared Linux CI, review, production deployment, and Apple capacity may use organization runner groups when the group is restricted to the repositories that need it.

This keeps release machines close to the repository that owns the artifact and prevents unrelated repositories from accidentally consuming specialized runners.

## Secrets And Variables

- Do not commit secrets, tokens, private keys, certificates, provisioning profiles, `.env` files, or kubeconfigs.
- Keep real signing and App Store secrets in a managed secret store when possible.
- GitHub variables may hold non-secret secret names, project IDs, bundle IDs, team IDs, and workload identity references.
- Repository or environment secrets should be used only when the value is needed by that repository or environment.
- Organization variables and secrets must use selected repository visibility unless they are intentionally safe for every repository.

## GitHub Actions Permissions

- Default `GITHUB_TOKEN` permissions should be read-only unless a workflow explicitly needs write access.
- Workflows that need write access must declare the minimum `permissions:` block.
- Pull request review automation must not be treated as a hard merge gate unless it is visible to GitHub as a required check.

## References

- [GitHub Docs: Managing access to self-hosted runners](https://docs.github.com/en/actions/how-tos/manage-runners/self-hosted-runners/manage-access)
- [GitHub Docs: Security hardening for GitHub Actions](https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions)
- [GitHub Docs: About protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
- [GitHub Docs: About rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets)
