---
name: onboarding-doc
description: 为项目生成给开发者看的规范上手文档 docs/standards/README.md：先读哪些规范、每份规范管什么、本地环境怎么搭、日常开发流程、提交前要跑的质量门禁。从项目里已有的规范文件、依赖文件、脚本和 README 汇总，不编造命令。用户说"生成上手文档""写新人入门文档""开发者指南""规范说明文档""新同事怎么开始写代码"时使用。
---

# 生成开发者上手文档

1. 读本 skill 目录往上两级的 `shared/workflow.md`（即插件根目录下的 `shared/workflow.md`），按其中的步骤执行。
2. 读 `docs/standards/` 下所有已有的规范文件，每份提取：管什么、最重要的 3 到 5 条硬性规则、质量门禁命令。还没有规范文件时，先告诉用户建议先生成规范（`init-standards`），用户坚持时只写环境和流程部分。
3. 读本 skill 的 `references/template.md`，按模板输出 `docs/standards/README.md`。
4. 环境和命令只从这些地方取：项目 README、`package.json` 的 scripts、`Makefile`、`docker-compose*.yml`、`.env.example`、构建文件、CI 配置（`.github/workflows/`、`.gitlab-ci.yml`、`Jenkinsfile`）。取不到的命令写进汇报，问用户，**禁止**按框架惯例编一条。
5. 提交说明格式、分支规则、合并方式只写在开发流程规范 `workflow.md` 里，本文第 4 节引用它，不重复写；还没有 `workflow.md` 时，建议用户先生成（`workflow-standard`）。
6. 工具版本只写在技术选型规范 `tech-stack.md` 里，本文第 3 节引用它；还没有 `tech-stack.md` 时，在工具表里加"版本"一列，从依赖文件读取。
7. 第 6 节的部署方式是只能由用户提供的取值，用户没给时写占位 `<部署方式>`，并在汇报里列为需要补充的取值；环境列表只写用户给出的或项目配置里能找到的环境。
