# ihouse-corp

ihouse-corp 承载 JClaw 相关的产品源码、发布仓、GitOps 仓和协作自动化。

默认文档语言是中文；必要的 GitHub、Actions、CI/CD 和平台术语保留英文。

## 仓库治理

- 默认分支改动走 pull request。
- 一个 PR 只做一件可解释、可 review、可回滚的变化。
- issue、PR、checks、runbook 和发布记录是协作事实来源。
- 仓库内更具体的 `README.md`、`CONTRIBUTING.md`、`AGENTS.md` 或项目文档优先于组织级默认规则。

## 协作边界

- 项目级运维 runbook、runner 清单、secret 状态和 backlog 留在对应项目仓库。
- `.github` 仓库只维护组织主页和默认社区文件。
- 不在仓库、issue、PR、日志或聊天中暴露明文 secret。

## 默认文档

- [组织级默认说明](https://github.com/ihouse-corp/.github/blob/main/README.md)
- [默认贡献规范](https://github.com/ihouse-corp/.github/blob/main/CONTRIBUTING.md)
