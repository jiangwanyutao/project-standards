---
name: frontend-standard
description: 为项目生成前端编码规范 docs/standards/frontend.md，包含工具、目录、组件复用规则、命名、组件与模板、状态管理、请求层、测试八章。有代码时读 package.json 和源码推断框架（React、Vue、Angular、Svelte 等）、组件库、样式方案和现有写法，空项目先问选型。用户说"生成前端规范""写前端编码规范""给前端定规矩""前端代码怎么组织"时使用。
---

# 生成前端规范

1. 读本 skill 目录往上两级的 `shared/workflow.md`（即插件根目录下的 `shared/workflow.md`），按其中的步骤执行。
2. 读本 skill 的 `references/defaults.md`。
3. 输出 `docs/standards/frontend.md`：
   - 标题写成 `# 前端编码规范（<框架 版本> + <语言> + <构建工具> + <样式方案> + <组件库>）`，例如 `# 前端编码规范（Vue 3 + TypeScript + Vite + Element Plus）`。
   - 章节固定为：1. 工具　2. 目录　3. 组件复用规则　4. 命名　5. 组件与模板　6. 状态管理　7. 请求层　8. 测试。React 项目第 5、6 章可以写成"组件与 JSX""Hooks 与状态"。
4. 请求层拆响应的写法必须和后端规范的统一响应结构一致；后端规范已存在时先读它，JSON 字段的大小写也按它写。
5. 颜色、字号、间距等视觉取值不写在本文，只写"见 UI 设计规范"。
