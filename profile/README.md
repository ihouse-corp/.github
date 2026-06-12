# ihouse-corp

This organization hosts the current JClaw repositories and shared automation.

## Repository Governance

- Default branch work must go through pull requests.
- Repositories use squash-only merge policy by default.
- Merge commits, rebase merges, and repository auto-merge are disabled by default.
- Head branches should be deleted after merge.
- GitHub-native branch protection, required status checks, and rulesets are the preferred hard enforcement path when the plan supports them.

## Runner Placement

Self-hosted runners are scoped by least privilege.

- Repository-specific release runners stay registered to the owning repository.
- Organization runners are reserved for shared, reusable CI or review capacity.
- Organization runners must use runner groups with selected repository access.
- Runner labels must describe capability and lane, not act as the only security boundary.

Current placement policy:

- Android mobile release runners belong to the mobile repository.
- Windows desktop release runners belong to the Windows desktop repository.
- Shared Linux CI, review, production deployment, and Apple capacity may use organization runner groups only when access is restricted to the repositories that need them.

## References

- [GitHub Docs: Managing access to self-hosted runners](https://docs.github.com/en/actions/how-tos/manage-runners/self-hosted-runners/manage-access)
- [GitHub Docs: Security hardening for GitHub Actions](https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions)
- [GitHub Docs: About protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
- [GitHub Docs: About rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets)
