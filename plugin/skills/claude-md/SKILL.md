---
name: claude-md
description: 为项目生成或补全给 AI 编程助手看的根目录 CLAUDE.md：用 @docs/standards/*.md 引用各份规范并声明"与之冲突时以规范为准"，写出目录说明、根目录规则、工作约定和常用命令。已有 CLAUDE.md 时只补引用和缺少的章节，不改已有内容。用户说"生成 CLAUDE.md""写 CLAUDE.md""让 Claude 遵守项目规范""把规范接到 CLAUDE.md""初始化 AI 编程说明"时使用。
---

# 生成 CLAUDE.md

1. 读本 skill 目录往上两级的 `shared/workflow.md`（即插件根目录下的 `shared/workflow.md`），按其中的步骤执行。
2. 列出 `docs/standards/` 下已有的规范文件；一份都没有时，告诉用户建议先生成规范（`init-standards`），用户坚持时"规范"一节先不写。
3. 读本 skill 的 `references/template.md`，按模板生成内容。
4. 目标项目已有 `CLAUDE.md` 时：
   - 不改动已有内容，只补：缺少的规范引用、模板里有而已有文件里没有的章节。
   - 已有内容和规范冲突的（例如已有文件写"提交说明用英文"，规范写中文），列出来问用户，不自行修改。
   - 写入前把要补的内容列给用户确认。
5. 项目里有 `AGENTS.md` 时，Claude Code 默认只读 `CLAUDE.md`：在 `CLAUDE.md` 里加一行 `@AGENTS.md` 引用它，并检查两者没有互相矛盾的规定，有矛盾的列给用户确认。
6. 写完后检查：每个 `@` 引用的文件都存在；"常用命令"里的每条命令都能在项目文件（scripts、Makefile、README、CI 配置）里找到出处；CLAUDE.md 本身不超过 200 行。
7. 汇报里提示一句：`@` 引用的规范每次对话都会全部加载；规范总量很大时，可以把只对某个目录有效的规范改放 `.claude/rules/`，用 `paths` 限定在对应目录下才加载。
