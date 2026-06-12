# 贡献规范

这个文件是 ihouse-corp 的组织级默认贡献规范。仓库内如果有更具体的 `README.md`、`CONTRIBUTING.md`、`AGENTS.md`、issue 或 runbook，以更具体的规则为准。

## 写作语言

- README、CONTRIBUTING、issue、PR 和运维说明默认用中文。
- 必要的专业术语保留英文，例如 `pull request`、`workflow_dispatch`、`self-hosted runner`、`runner group`、`required checks`、`branch protection`。
- 文件路径、workflow 名称、runner label、API 字段、命令和环境名保持原文。
- 如果某个仓库服务外部英文协作者，可以在该仓库补充英文说明；组织级默认口径仍是中文优先。

## Pull Request 规则

- 不直接推送 `main`。
- 源码、workflow、基础设施和文档改动都走 pull request。
- 一个 PR 只做一件可解释、可 review、可回滚的变化。
- 非琐碎变更先有 issue 或等价任务上下文，PR body 写明关联目标、范围和验证方式。
- 默认 squash merge，合并后删除 head branch。
- 被替代或不再适合合并的 PR 应说明事实依据后关闭，避免长期噪音。

仓库需要硬门禁时，优先使用 GitHub 原生能力：

- 合并前必须有 pull request；
- 阻止直接推送 `main`；
- `required checks` 必须通过；
- 分支必须与 base branch 保持最新；
- 阻止 force push 和默认分支删除。

`required checks` 不是独立仓库开关。它通过 branch protection 或 rulesets 强制执行。当前套餐或仓库设置不支持时，不能用文档口径假装已经强制生效。

## GitHub Actions

- 默认 `GITHUB_TOKEN` 权限保持最小化。
- 需要写权限的 workflow 必须显式声明最小 `permissions:`。
- 真实发布、生产 apply、商店上传和带副作用的运维动作应使用明确的手动入口、environment 或等价审批边界。
- smoke、dry-run、plan 和只读 drift check 应与真实发布动作分开，避免把健康检查做成生产操作。
- 绿灯检查代表“当前 PR 可合并”的证据之一，不等于“已经发布”或“运行态已验证”。

## Runner 和权限边界

`self-hosted runner` 默认按最小权限放置。

- 单仓专用、带签名材料、设备、缓存或本地工具链状态的 runner，优先注册为 repository runner。
- 多仓确实共享的 CI、review 或受控部署能力，可以注册为 organization runner。
- organization runner 必须通过 runner group 限定 selected repositories。
- runner label 只描述能力和调度意图，不替代 runner group、repository scope 或 secret boundary。

## Secrets 和受控配置

- 禁止提交 secret、token、private key、证书、provisioning profile、`.env`、kubeconfig 或任何可直接访问生产资源的凭据。
- 真实签名、App Store、SSH、云平台和生产服务凭据应放在受控 secret manager、GitHub environment secret 或等价受控密钥面。
- GitHub variables 可以放非密文配置，例如 secret 名称、project ID、bundle ID、team ID、runner label。
- repository / environment secret 只开放给真正需要的仓库或环境。
- organization variables 和 secrets 默认使用 selected repository visibility，除非明确证明对所有仓库开放是安全且必要的。
- 密钥值不进入 issue、PR、commit、日志、聊天或公开文档。

## 参考

- [GitHub Docs: Managing access to self-hosted runners](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/managing-access-to-self-hosted-runners-using-groups)
- [GitHub Docs: Security hardening for GitHub Actions](https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions)
- [GitHub Docs: About protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
- [GitHub Docs: About rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets)
