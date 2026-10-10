---
name: backend-standard
description: 为项目生成后端编码规范 docs/standards/backend.md，包含基线与工具、命名、目录与分层、编码规则、RESTful 约定、统一响应与错误码、测试七章。有代码时读代码推断语言和框架（Java/Spring Boot、Go、Python、Node.js、.NET 等）和现有写法，空项目先问技术选型。用户说"生成后端规范""写后端编码规范""给后端定规矩""后端代码规范怎么写"时使用，即使没有明确说"规范文件"也应使用。
---

# 生成后端规范

1. 读本 skill 目录往上两级的 `shared/workflow.md`（即插件根目录下的 `shared/workflow.md`），按其中的步骤执行。
2. 读本 skill 的 `references/defaults.md`：每一章的默认规则，以及每章需要按目标技术确定的项。
3. 输出 `docs/standards/backend.md`：
   - 标题写成 `# 后端编码规范（<语言 版本> + <主框架>）`，例如 `# 后端编码规范（Java 17 + Spring Boot 3）`。
   - 章节固定为：1. 基线　2. 命名　3. 目录与分层　4. 编码规则　5. RESTful 约定　6. 统一响应　7. 测试。
   - 项目不对外提供 HTTP 接口（例如纯消息消费服务）时，第 5、6 章改写成它实际的对外接口约定，并在汇报里说明。
4. 同一项目有多个后端服务且语言不同时，每种语言一份文件（`backend-java.md`、`backend-go.md`），先问用户是否这样拆分。
5. 已经生成或准备生成数据库规范时，本文涉及表结构、迁移、种子数据的地方只写"见数据库规范第 N 节"，不重复。
