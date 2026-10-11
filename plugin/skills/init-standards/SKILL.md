---
name: init-standards
description: 为项目一次生成整套开发规范：技术选型、开发流程、后端、数据库、前端、UI 设计规范（docs/standards/*.md），给开发者看的上手文档（docs/standards/README.md），以及引用这些规范的 CLAUDE.md。先读代码识别项目使用的技术和现有写法，空项目先问技术选型，再依次调用各个规范 skill，最后检查各份规范之间是否一致。用户说"生成项目规范""初始化规范""给项目定规矩""建一套开发规范""新项目先把规范定下来""让团队和 Claude 按统一规范写代码"时使用，即使用户只说了其中几类规范也可以用它来统一提问和检查。
---

# 生成整套项目规范

## 1. 识别技术并一次问完

1. 读本 skill 目录往上两级的 `shared/workflow.md`（即插件根目录下的 `shared/workflow.md`），完成其中第 1、2 步，覆盖后端、数据库、前端、UI、开发流程五个方面。
2. 读下面各 skill 的 `references/defaults.md` 里"按技术确定"的项，按 `shared/workflow.md` 第 2 步的提问规则挑出要问的问题（全部 skill 合计不超过 8 个），合并成一份清单一次发给用户，同一个问题只问一次（例如 JSON 字段大小写同时影响后端和前端规范）。清单开头列出识别结果，每项附依据文件，请用户一并确认：

   | 方面 | 识别结果 | 依据 |
   |---|---|---|
   | 后端 | <语言 版本 + 框架> | <文件> |
   | 数据库 | <数据库 + ORM + 迁移工具> | <文件> |
   | 前端 | <框架 + 语言 + 构建工具 + 组件库 + 样式方案> | <文件> |
   | UI | <主题文件；已有的主题色，没有就写"待补充"> | <文件> |
   | 开发流程 | <主分支名、提交说明格式、代码托管平台和 CI> | <git log、git remote、CI 配置> |

3. 确定要生成哪些文件：项目没有前端就跳过前端和 UI 规范，没有数据库就跳过数据库规范；用户只要其中几类时按用户说的。用户要整套规范时，技术选型和开发流程规范一起生成。
4. 用户确认后再进入第 2 步。

## 2. 依次生成

按下面的顺序调用 skill（插件名 `project-standards`），每个 skill 使用第 1 步确认过的结果，不重复识别和提问；某个 skill 里遇到第 1 步没覆盖到的问题，再单独问：

1. `project-standards:tech-stack-standard`
2. `project-standards:database-standard`
3. `project-standards:backend-standard`
4. `project-standards:frontend-standard`
5. `project-standards:ui-standard`
6. `project-standards:workflow-standard`
7. `project-standards:onboarding-doc`
8. `project-standards:claude-md`

顺序的原因：技术选型是其他规范引用版本号的出处；后端规范要引用数据库规范的章节，前端请求层要和后端统一响应一致；开发流程规范引用后端、前端的质量门禁；上手文档和 CLAUDE.md 要汇总前面生成的所有规范。

## 3. 交叉检查

全部生成后逐项核对，发现不一致就改成一致，并在汇报里写出改了什么：

- 接口 JSON 字段大小写、分页参数名、统一响应结构和成功码：`backend.md` 和 `frontend.md` 第 7 章一致。
- 认证方式和 token 存放位置：`backend.md` 安全一章和 `frontend.md` 请求层一致。
- 接口文档地址：`backend.md` 第 5 章写的地址就是 `frontend.md` 生成接口类型的来源。
- 枚举存储方式：`database.md` 的状态字段写法和 `backend.md` 的枚举规则一致。
- 通用字段名、软删除字段：`database.md` 和 `backend.md` 中出现的地方一致。
- 质量门禁命令（包括依赖漏洞扫描）：各规范、`README.md`、`CLAUDE.md` 中完全相同。
- 版本号只出现在 `tech-stack.md`；提交说明格式只出现在 `workflow.md`。
- `CLAUDE.md` 的 `@` 引用覆盖了全部生成的规范文件。

## 4. 汇报

按 `shared/workflow.md` 第 6 步的要求汇报，另外列出：建议用户接下来补的配置（例如还没有的 lint 配置、迁移工具、漏洞扫描插件、CI 配置），按子项目分组。
