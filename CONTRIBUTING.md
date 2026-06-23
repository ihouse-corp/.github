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

## 快速开发前最小治理基线

- 当前先采用 GitHub 官方能力：`CODEOWNERS`、Dependabot for GitHub Actions、
  reusable workflows，以及已有 OIDC/WIF。
- `CODEOWNERS` 只做 owner review 路由，不是 Free plan 硬门禁替代物。
- Dependabot 本轮只维护 GitHub Actions，不默认扩张到 npm、Go、Gradle、
  Bundler 等依赖生态。
- 后续一边用一边优化；发现 CI、review、runner、发布或凭证问题时，先开 issue，
  写清现象、影响和建议，再按 `issue -> PR -> 验证 -> closeout` 闭环。
- 当前不额外加 repo-local PR guard、merge controller 或自研审批胶水。

## AI 协作默认口径

- 组织级默认只吸收可复用的协作纪律，不默认要求团队安装第三方 AI skill pack、CLI helper 或本机浏览器/账号型工具。
- `Ponytail` 一类材料适合作为代码改动纪律参考：少抽象、少依赖、少扩散、最小 diff。
- `karpathy-skills` 一类材料适合作为协作方法参考：少假设、明确 tradeoff、只改必要范围、先定义验证标准。
- `Agent-Reach` 不属于 `ihouse-corp` 默认工具链。它如果被个别仓库采用，只能作为可选外部调研 helper，不能替代官方文档、issue/PR、checks、日志和 runbook 这些正式事实面。
- 任何会触碰本机 cookie、token、浏览器登录态、第三方账号或额外平台权限的工具，都不能只靠组织默认文档直接推广为“大家默认可用”。
- 某个仓库如果要正式引入新的 AI helper、skill bundle 或 operator wrapper，必须在该仓库自己的 issue / PR 中说明用途、边界、验证方式、失败退路，以及为什么 GitHub 官方能力或现有组织默认能力不足。

## GitHub Actions

- 默认 `GITHUB_TOKEN` 权限保持最小化。
- 需要写权限的 workflow 必须显式声明最小 `permissions:`。
- 真实发布、生产 apply、商店上传和带副作用的运维动作应使用明确的手动入口、environment 或等价审批边界。
- smoke、dry-run、plan 和只读 drift check 应与真实发布动作分开，避免把健康检查做成生产操作。
- 绿灯检查代表“当前 PR 可合并”的证据之一，不等于“已经发布”或“运行态已验证”。
- 对发布型任务，默认完成标准不是“代码已修”或“PR 已合并”，而是面向真实交付面的闭环：新包 / 新镜像 / 新构建已发布，目标环境已更新，并且相关公网或运行态验收已经通过。
- 如果任务要求的是上线结果、商店包、桌面安装包、生产部署或公网可用性，就不要把源码仓 CI、局部 smoke 或 staging 观察误报成最终完成；这些都只是走向完成的中间证据。

## Review Automation

JClaw 生产/开发仓默认接入 `ihouse-corp/github-automation` 的
`Review Automation`，并通过 `ihouse-review-bot[bot]` 发布 advisory review。

- Review 默认在 PR `opened`、`reopened`、`synchronize`、`ready_for_review` 时触发。
- Review 是当前 HEAD 的 advisory signal，不是 branch protection required check，也不替代 build/test/release gate。
- push 新 commit 后，以最新 HEAD 的 review 结果为准；旧 HEAD 的 clean 或 finding 只能作为历史参考。
- clean 结果必须来自有效 structured review artifact，并带有当前 HEAD 标记；只看到一句 `Didn't find any major issues.` 但没有当前 HEAD 证据时，不应当作完整验收。
- P0/P1 finding 应以 inline PR review comment 出现；provider、runtime、schema 或 workflow failure 只能当 failure 处理，不能当 clean。
- `.github` 和 `github-automation` 是组织默认社区文件仓与 helper 仓，不是默认 Review Automation consumer wrapper 覆盖对象。
- `demo-repository` 是 demo/optional 仓，可以保持 workflow disabled，避免无效噪音。

## Runner 和权限边界

`self-hosted runner` 默认按最小权限放置。

- 单仓专用、带签名材料、设备、缓存或本地工具链状态的 runner，优先注册为 repository runner。
- 多仓确实共享的 CI、review 或受控部署能力，可以注册为 organization runner。
- organization runner 必须通过 runner group 限定 selected repositories。
- runner label 只描述能力和调度意图，不替代 runner group、repository scope 或 secret boundary。
- ARC 整体是 JClaw org 的基础设施能力；review 只是其中一条 runner lane。

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
