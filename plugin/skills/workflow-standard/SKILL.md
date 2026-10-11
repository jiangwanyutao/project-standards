---
name: workflow-standard
description: 为项目生成开发流程规范 docs/standards/workflow.md，包含分支、提交说明、合并请求（描述模板、评审人数、评审时看什么）、合并前检查（质量门禁和 CI）、版本号与 CHANGELOG 五章。有仓库时从 git log、分支、CI 配置推断现有做法，空项目用推荐值。提交说明格式只写在这份文件里，上手文档和 CLAUDE.md 引用它。用户说"生成开发流程规范""定分支规则""提交说明怎么写""代码评审规范""合并请求模板""版本号和 CHANGELOG 怎么管"时使用。
---

# 生成开发流程规范

1. 读本 skill 目录往上两级的 `shared/workflow.md`（即插件根目录下的 `shared/workflow.md`），按其中的步骤执行。
2. 读本 skill 的 `references/defaults.md`。
3. 读仓库的现有做法，每条结论记下依据：
   - `git log --oneline -30`：提交说明格式。历史提交已有统一格式的照写；没有统一格式时用默认格式，并在汇报里说明。
   - `git branch -a`：主分支名和功能分支的命名方式。
   - `git remote -v` 和 CI 配置（`.github/workflows/`、`.gitlab-ci.yml`、`Jenkinsfile`、`azure-pipelines.yml`）：代码托管平台、CI 里跑了哪些命令。
   - 根目录的 `CHANGELOG*`、`VERSION`，以及构建文件里的版本字段：版本号写在哪里、按什么格式。
4. 输出 `docs/standards/workflow.md`：
   - 标题写成 `# 开发流程规范`。
   - 章节固定为：1. 分支　2. 提交说明　3. 合并请求　4. 合并前检查　5. 版本号与 CHANGELOG。
5. 质量门禁命令不在本文重复写，写"后端规范第 1 章、前端规范第 1 章的质量门禁"，并附各自执行的目录；对应规范还没生成时，先生成规范或在汇报里列为需要补充。
6. 本文生成后，`docs/standards/README.md` 和 `CLAUDE.md` 里关于分支、提交说明、合并方式的内容改为引用本文；两份文件已存在时按 `shared/workflow.md` 第 4 步列出改动，用户确认后再改。
