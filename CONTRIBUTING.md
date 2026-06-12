# 贡献规范

## 写作语言

- README、CONTRIBUTING、issue、PR 和运维说明默认用中文。
- 必要的专业术语保留英文，例如 `pull request`、`workflow_dispatch`、`self-hosted runner`、`runner group`、`required checks`、`branch protection`。
- 文件路径、workflow 名称、runner label、API 字段、命令和环境名保持原文。
- 如果某个仓库服务外部英文协作者，可以在该仓库补充英文说明；组织级默认口径仍是中文优先。

## Pull Request 规则

- 不直接推送 `main`。
- 源码、workflow、基础设施和文档改动都走 pull request。
- 一个 PR 只做一件可解释、可 review、可回滚的变化。
- 默认 squash merge。
- 合并后删除 head branch。

GitHub 原生硬门禁可用时，应启用：

- 合并前必须有 pull request；
- 阻止直接推送 `main`；
- `required checks` 必须通过；
- 分支必须与 base branch 保持最新；
- 阻止 force push 和默认分支删除。

`required checks` 不是独立仓库开关。它通过 branch protection 或 rulesets 强制执行。当前套餐对私有仓不可用时，必须继续执行 PR-only 流程，并把缺失硬门禁标为升级卡点。

## Runner 放置规则

优先使用最窄的 runner scope。

满足以下条件时，优先注册为 repository runner：

- runner 只服务一个仓库的发布或平台 workflow；
- runner 带有平台 SDK、签名材料、缓存、设备或本地工具链状态；
- 其他仓库不应该调度它；
- runner 稀缺、状态化，或绑定具体机器。

满足以下条件时，可以注册为 organization runner：

- 多个仓库确实需要同一种共享能力；
- 该能力适合共享 CI、review 或受控部署；
- 通过 organization `runner group` 控制访问；
- `runner group` 只开放 selected repositories；
- workflow permissions、variables 和 secrets 仍按最小权限配置。

runner label 只描述能力和调度意图，不替代 runner group、repository scope 或 secret boundary。

## 当前 JClaw runner 基线

- `jclaw-android-01`：repository runner，注册在 `jclaw-mobile-rn`。
- `win136-jclaw-01`：repository runner，注册在 `jclaw-work-windows`。
- `iHouse-AIdeMac-mini`：organization runner，属于 `jclaw-apple-release`，只给移动和 macOS 桌面仓使用。
- `gx10-jclaw-org-ci-arm64`：organization Linux CI runner group，给当前 JClaw 私有工作仓使用。
- `gx10-jclaw-org-docker-amd64`：organization heavy container runner group，按需跑容器构建。
- `gx10-jclaw-org-review-arm64`：organization review runner group，跑 PR review automation。

## Secrets 和 variables

- 禁止提交 secret、token、private key、证书、provisioning profile、`.env`、kubeconfig。
- 真实签名、App Store、SSH、云平台凭据优先放在受控 secret manager 或 GitHub environment secret。
- GitHub variables 可以放非密文配置，例如 secret 名称、project ID、bundle ID、team ID、runner label。
- repository / environment secret 只给真正需要的仓库或环境。
- organization variables 和 secrets 默认使用 selected repository visibility，除非明确证明对所有仓库开放是安全且必要的。

当前已收敛：

- JClaw runner / review 相关 organization variables 已限制到 7 个私有工作仓。
- 公开 `.github` profile 仓不接收这些 runner / review 变量。

当前剩余卡点：

- `CODEX_BOT_APP_ID`、`CODEX_BOT_PRIVATE_KEY`、`REVIEW_BOT_PROVIDER_API_KEY` 仍是 org-level secrets。GitHub 的常规更新路径需要重新提交加密后的 secret value；在不知道原值时，不直接重写。后续应通过 secret 管理面把它们改成 selected repositories。
- runner group 的 selected workflows 还没有启用。这个优化需要按组验证 PR、`workflow_dispatch` 和 reusable workflow 调度后再收窄，避免误伤 CI。
- Web / Backend 的 container workflow 在 `workflow_dispatch` 下有发布语义，不适合作为随手 runner 健康检查。后续应补明确的 dry-run / smoke 入口，或把手动容器验证与发布动作拆开。
- iOS / Android 发布 workflow 的 dry-run 不应占用 production environment。后续应把 production environment 限定到真实 TestFlight upload、Android support download publish 或生产 apply 这类有副作用动作。
- `jclaw-prod-deploy` runner group 当前作为生产部署能力边界保留；没有真实生产 apply 需求时，不用它做日常健康检查。

## GitHub Actions 权限

- 默认 `GITHUB_TOKEN` 权限保持 read-only。
- 需要写权限的 workflow 必须显式声明最小 `permissions:`。
- PR review automation 只有在 GitHub 可见 required check 中强制时，才能算硬合并门禁。
- 真实发布、TestFlight upload、Android support download publish、生产 apply 不作为日常 runner 健康检查入口；健康检查应使用 smoke、dry-run 或 plan。

## 验证入口

- Windows：`JClaw Work Windows Smoke`、`JClaw Work Windows Release` with `dry_run=true`。
- macOS：`JClaw Work Mac Apple Runner Smoke`、`JClaw Work Desktop macOS Smoke`。
- iOS：`JClaw Work iOS TestFlight` with `upload_to_testflight=false`。
- Android：`JClaw Work Android Release` with `dry_run=true` and `publish_support_download=false`。
- Web：`dashboard-ci`。
- Backend：`Storage Smoke`，常规 `Validate` / `Container` 以 PR 或 push run 为准。
- Ops：`compose-release` with `mode=plan`，`compose-drift-check` 只做 read-only drift check。

## 参考

- [GitHub Docs: Managing access to self-hosted runners](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/managing-access-to-self-hosted-runners-using-groups)
- [GitHub Docs: Security hardening for GitHub Actions](https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions)
- [GitHub Docs: About protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
- [GitHub Docs: About rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets)
