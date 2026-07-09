# ihouse-corp .github

这个仓库是 ihouse-corp 的 GitHub 组织级特殊仓库，只放组织主页和默认社区文件。

- 组织主页：[`profile/README.md`](profile/README.md)
- 默认贡献规范：[`CONTRIBUTING.md`](CONTRIBUTING.md)

## 组织默认协作能力

JClaw 生产/开发仓默认使用 org ARC runner lanes 承载 CI、review、Docker
和发布能力；PR advisory review 默认由 `ihouse-review-bot[bot]` 通过
`ihouse-corp/github-automation` 执行。

`.github` 仓自身只维护组织主页和默认社区文件，不作为 Review Automation
consumer wrapper 覆盖对象。

快速开发前的默认治理口径是 GitHub 官方能力优先：`CODEOWNERS` 做最小 owner
路由，Dependabot 只维护 GitHub Actions，跨仓复用走 reusable workflows，已经
存在的 OIDC/WIF 继续保留。当前先这样，不额外加 repo-local 守门胶水；后续一边
用一边优化，有问题先开 issue，再按 `issue -> PR -> 验证 -> closeout` 闭环；
closeout ownership 细节见 [`CONTRIBUTING.md`](CONTRIBUTING.md)。

## Dependabot 默认口径

组织级默认按风险分级处理 Dependabot PR：

- 低风险 patch / minor 更新：required checks 全绿且人工确认 diff 后，可以不等
  clean review 直接合并；
- major bump，或涉及 GitHub Actions、runner、permissions、release、
  security surface 的更新：仍然需要人工审查，不能按低风险 PR 直接放行；
- Review Automation 继续只作为 advisory signal，不作为这个 org 默认规则的硬门禁。

## 仓库层治理边界

仓库自己的 README、CONTRIBUTING 和 AGENTS 只保留 repo-specific delta：

- 仓库专属边界、例外、验证和回滚说明；
- 明确高于 org default 的特殊门禁；
- 该仓库独有的 workflow、runner、发布、环境或密钥约束。

不建议在仓库层重复这些组织默认段落：

- Review Automation 的基本触发 / 信号定义；
- Dependabot 的风险分级口径；
- AI 协作默认口径；
- CODEOWNERS / reusable workflows / org runner lanes 这类通用治理基线。

如果仓库需要覆盖 org default，必须写清楚覆盖原因、影响范围和验证方式。

## AI 协作边界

组织默认协作文件只落“协作纪律”，不默认把第三方 AI helper 当成组织工具链。

- `Ponytail`：作为代码改动纪律参考吸收，强调少抽象、少依赖、少扩散、最小 diff。
- `karpathy-skills`：作为协作增强价值吸收，强调少假设、明确 tradeoff、只改必要范围、先定义验证标准。
- `Agent-Reach`：不进入 `ihouse-corp` 默认工具链。它可以作为个别仓库或个别操作者的外部调研 helper，但不是组织默认依赖，也不能替代 issue、PR、checks、runbook 和官方文档这些事实面。

如果某个仓库确实需要引入额外 AI helper，应在该仓库自己的 issue / PR 中明确：

- 为什么默认 GitHub / CI / review / 文档能力不够；
- 新工具是原则参考、可选 helper，还是正式默认依赖；
- 它会不会触碰本机账号、cookie、token、浏览器态或外部平台；
- 验证和回退边界是什么。

## 使用边界

适合放在这里：

- 组织级主页说明；
- 没有自带贡献规范的仓库可继承的默认规则；
- 跨仓通用的 GitHub 协作、安全和 PR 习惯。

不放在这里：

- 单个项目的运维 runbook、发布记录、事故记录或 backlog；
- runner 实例清单、secret 当前状态、环境排障细节；
- 任何明文 secret、token、private key、证书、`.env` 或 kubeconfig。

如果某个仓库已有更具体的 `README.md`、`CONTRIBUTING.md`、`AGENTS.md` 或项目文档，以仓库内规则为准。

## 参考

- [GitHub Docs: Customizing your organization's profile](https://docs.github.com/en/organizations/collaborating-with-groups-in-organizations/customizing-your-organizations-profile)
- [GitHub Docs: Creating a default community health file](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file)
