# project-standards

Claude Code 插件：为项目生成开发规范文件。有代码时读代码识别使用的技术和现有写法，空项目先问技术选型。

## 安装

```
/plugin marketplace add jiangwanyutao/project-standards
/plugin install project-standards@project-standards
```

更新：`/plugin marketplace update project-standards` 后执行 `/plugin update project-standards`。改了 `plugin/` 下的内容要同时升 `plugin/.claude-plugin/plugin.json` 的 `version`，否则更新时会提示已是最新。

## skills

| skill | 生成的文件 | 触发说法示例 |
|---|---|---|
| `init-standards` | 下面全部文件 | 生成项目规范、初始化规范、给项目定规矩 |
| `backend-standard` | `docs/standards/backend.md` | 生成后端规范 |
| `database-standard` | `docs/standards/database.md` | 生成数据库规范 |
| `frontend-standard` | `docs/standards/frontend.md` | 生成前端规范 |
| `ui-standard` | `docs/standards/ui.md`（骨架：主题切换、变量名和占位，取值由团队后续补充） | 生成 UI 规范、设计规范 |
| `onboarding-doc` | `docs/standards/README.md`（给开发者看） | 生成上手文档 |
| `claude-md` | 根目录 `CLAUDE.md`（给 AI 看，`@` 引用各规范） | 生成 CLAUDE.md |

## 规则怎么定

每条规则按下面的优先级取值：

1. 用户在对话里明确说的；
2. 目标项目代码里已经统一使用的写法；
3. 各 skill `references/defaults.md` 里的默认规则，按目标项目的技术改写。

代码里写法不统一、默认规则在目标技术里没有对应做法、需要具体取值（项目名、主色、业务域）时会停下来问。目标文件已存在时，先列出新增和冲突的条目，确认后再合并写入。

## 目录

| 路径 | 内容 |
|---|---|
| `.claude-plugin/marketplace.json` | 插件市场清单 |
| `plugin/.claude-plugin/plugin.json` | 插件清单 |
| `plugin/shared/workflow.md` | 所有 skill 共用的步骤：识别技术、提问、取值优先级、已有文件处理、写作要求、写完后的检查 |
| `plugin/skills/<skill>/SKILL.md` | 各 skill 的步骤 |
| `plugin/skills/<skill>/references/` | 默认规则或文档模板 |
