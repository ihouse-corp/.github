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
