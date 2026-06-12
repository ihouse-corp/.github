# ihouse-corp

这个组织承载当前 JClaw 工作仓、发布仓、GitOps 仓和共享自动化。

默认文档语言是中文；必要的专业术语保留英文，例如 `self-hosted runner`、`runner group`、`workflow_dispatch`、`required checks`。

## 仓库治理

- 默认分支改动走 pull request。
- 仓库默认只允许 squash merge。
- 默认关闭 merge commit、rebase merge 和 repository auto-merge。
- PR 合并后删除 head branch。
- `required checks` 不是独立开关；它依赖 branch protection 或 rulesets。当前 GitHub 套餐不支持的私有仓硬门禁，先作为升级卡点记录，不用文档口径假装已经强制生效。

## Runner 放置

`self-hosted runner` 按最小权限放置。

- 单仓专用发布机器注册到对应仓库。
- 多仓共享能力放到 organization runner，并通过 `runner group` 限定 selected repositories。
- runner label 只做能力和调度描述，不替代 runner group、仓库范围或 secret 边界。
- 不把公开 profile 仓纳入 JClaw 私有 runner / review 变量可见范围。

当前放置口径：

- Android 发布 runner：注册在 `jclaw-mobile-rn`。
- Windows 发布 runner：注册在 `jclaw-work-windows`。
- Apple runner：组织级 `jclaw-apple-release`，仅给移动和 macOS 桌面仓使用。
- Linux CI / heavy container / review runner：组织级 runner group，给需要的私有工作仓使用。

## 当前卡点

- org-level review bot secrets 仍需后续在 secret 管理面重写为 selected repositories；在不知道原 secret value 的情况下，不直接改写密文。
- runner group 的 selected workflows 可以进一步收窄，但需要逐组验证不会破坏 PR / reusable workflow 调度后再启用。
- 生产 apply、真实 TestFlight upload、真实 Android support download publish 不作为日常 runner 健康检查入口。

## 参考

- [GitHub Docs: Managing access to self-hosted runners](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/managing-access-to-self-hosted-runners-using-groups)
- [GitHub Docs: Security hardening for GitHub Actions](https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions)
- [GitHub Docs: About protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
- [GitHub Docs: About rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets)
