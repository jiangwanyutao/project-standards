# 改动说明

## 0.4.0

### 新增 skill

- `tech-stack-standard`：生成 `docs/standards/tech-stack.md`，选型总表（层、选择、版本、选择理由、依据）、引入与升级依赖、选择依据三章。版本号只写在这份文件里，后端、数据库、前端规范的标题不再带版本号；本次不生成技术选型规范时，版本仍写在各规范标题里。
- `workflow-standard`：生成 `docs/standards/workflow.md`，分支、提交说明、合并请求、合并前检查、版本号与 CHANGELOG 五章。提交说明格式只写在这份文件里，上手文档第 4 节和 CLAUDE.md 的工作约定改为引用它。
- `init-standards` 生成整套规范时先生成技术选型规范，在 UI 规范之后生成开发流程规范；识别结果新增"开发流程"一行，交叉检查新增认证方式与 token 存放位置、接口文档地址、版本号和提交说明格式只写在一处三项。

### 现有规范补充

- 后端规范新增第 8 章"安全"：token 有效期、退出登录作废 refresh token、登录限流、密码哈希、接口层统一权限校验、按归属过滤数据、CORS、文件上传、依赖漏洞处理、机密信息不进仓库。
- 后端规范第 4 章新增配置项（命名、写进 `.env.example`、缺少必填配置时启动失败）、健康检查接口 `GET /health`、日志级别用法；监控指标和告警写占位 `<待补充>`。
- 后端规范第 5 章新增接口文档：由代码生成 OpenAPI 3 并写明地址，每个接口写用途和错误码，生产环境的调试页面关闭或要求登录。
- 后端、前端质量门禁的最后一项改为依赖漏洞扫描（`dependency-check`、`govulncheck`、`pip-audit`、`pnpm audit`）。
- 前端规范请求层新增 token 存放位置：access token 存内存，refresh token 在 `HttpOnly` Cookie 里，禁止写进 `localStorage`、`sessionStorage`。
- 上手文档新增"环境与部署"一节，工具表的版本改为引用技术选型规范。
- 代码托管平台加入第 2 类问题（`git remote -v` 能看出来时不问）；健康检查、漏洞扫描、密码哈希等安全基线直接写进规范，不提问。

### 规则来源

来源只记在这里，不写进生成的规范文档。

| 规则 | 来源 |
|---|---|
| 密码用 Argon2id 或 bcrypt | https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html |
| 登录失败锁定、改密码后作废会话 | https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html ；https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html |
| token 不放 `localStorage`，Cookie 用 `HttpOnly`、`Secure`、`SameSite` | https://cheatsheetseries.owasp.org/cheatsheets/HTML5_Security_Cheat_Sheet.html ；https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html |
| 权限默认拒绝、在服务端校验 | https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html |
| 携带凭证时不能用 `*` 作为允许的来源 | https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS |
| 上传文件的大小、扩展名白名单、内容类型、服务端生成文件名 | https://cheatsheetseries.owasp.org/cheatsheets/File_Upload_Cheat_Sheet.html |
| 漏洞扫描工具 | https://owasp.org/www-project-dependency-check/ ；https://go.dev/doc/security/vuln/ ；https://github.com/pypa/pip-audit ；https://pnpm.io/cli/audit |
| 配置从环境变量读取 | https://12factor.net/config |
| 健康检查接口 | https://learn.microsoft.com/en-us/azure/architecture/patterns/health-endpoint-monitoring |
| 日志级别、日志里不写机密信息 | https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html |
| 提交说明 `<type>: <描述>`、不兼容改动加 `!` | https://www.conventionalcommits.org/zh-hans/v1.0.0/ |
| 合并请求小而独立、评审看什么、建议类意见的标注 | https://google.github.io/eng-practices/review/ |
| 版本号规则 | https://semver.org/lang/zh-CN/ |
| CHANGELOG 按版本倒序、按类型分组、"未发布"一节 | https://keepachangelog.com/zh-CN/1.1.0/ |

未采纳：GitFlow 的 develop、release 分支（默认用主分支加短期功能分支）；按提交说明自动生成 CHANGELOG（要额外配工具，作为团队后续选择）。

## 0.3.0

- 插件以代码规范（数据库、后端、前端、CLAUDE.md）为主，UI 规范改为只生成骨架：原则、主题切换机制、设计变量和状态色的变量名及所在文件、布局要点。颜色、字号、间距、圆角、阴影、布局尺寸一律写占位 `<待补充>`，删掉默认的靛紫色等具体取值。用户给了品牌色时只填进主色并报告对比度，不再推导整套颜色。
- 浅色/深色/跟随系统的主题切换、窄屏可用的响应式布局改为必须有，不再询问；前端规范同时写明这两条。
- UI 规范删掉 0.2.0 补充的细分规则（点击目标尺寸、焦点、状态消息、改控件值不跳转、文字容器高度、表单错误文案、按钮文案、加载时长分档、空状态组成），以及导航宽度、菜单项高度等具体尺寸，留给团队后续逐步补充。下面 0.2.0 中 ui-standard 一节的条目已不在当前版本里。
- 界面类型仍作为问题询问，推荐值是"后台管理"。

## 0.2.0

### 提问

- 只问两类问题：会明显改变规则的选择（JSON 字段大小写、暗色模式、写法不统一时以哪种为准、默认规则在目标技术里没有对应做法时的方案），以及只能由用户提供的取值（项目名、业务域、部署方式和备份位置）。合计不超过 8 个。
- 其余项直接用推荐值，汇报里新增"使用的推荐值"一节逐项列出。用户说"不用问"时不提问，缺的取值写占位。
- UI 主色、备份时间和保留份数、Flyway 回退写法、后台任务机制、端到端测试改为直接用推荐值。

### 新增规则及来源

来源只记在这里，不写进生成的规范文档。

**backend-standard**

| 规则 | 来源 |
|---|---|
| 新建返回 201 带 `Location`；异步任务 202 带查询状态的地址 | https://opensource.zalando.com/restful-api-guidelines/ （201 状态码一节）；https://github.com/microsoft/api-guidelines/blob/vNext/azure/Guidelines.md （Long-Running Operations） |
| `page_size` 超上限按 300 处理，非法值返回 10001 | https://google.aip.dev/158 |
| 列表写明默认排序 | https://github.com/microsoft/api-guidelines/blob/vNext/azure/Guidelines.md （Collections）；https://google.aip.dev/132 |
| 列表和详情默认不返回已删除记录 | https://google.aip.dev/132 |
| 空集合返回 `[]` | https://opensource.zalando.com/restful-api-guidelines/#124 |
| 布尔字段不返回 `null`；`null` 与缺省同义 | https://opensource.zalando.com/restful-api-guidelines/#122 、#123 |
| `PATCH` 按合并语义，`null` 表示清空 | https://github.com/microsoft/api-guidelines/blob/vNext/azure/Guidelines.md （PATCH / JSON Merge Patch） |
| 查询接口不返回机密字段 | https://github.com/microsoft/api-guidelines/blob/vNext/azure/Guidelines.md （"DO NOT return secret fields via GET"） |
| `Idempotency-Key`，24 小时内重复请求返回首次响应 | https://opensource.zalando.com/restful-api-guidelines/#230 ；https://google.aip.dev/155 |
| 请求 ID `X-Request-ID` 透传或生成，写入响应头和日志 | https://opensource.zalando.com/restful-api-guidelines/#233 |
| 错误响应不含堆栈和实现细节 | https://opensource.zalando.com/restful-api-guidelines/#177 ；https://www.rfc-editor.org/rfc/rfc9457.html#section-3.1.4 |
| 先校验权限再查存在 | https://google.aip.dev/193 |
| 限流返回 10006（429）带 `Retry-After` | https://opensource.zalando.com/restful-api-guidelines/#153 |
| Java：`finally` 不 return；`ThreadPoolExecutor`；线程命名；`ThreadLocal.remove()`；包装类型 `equals`；`SimpleDateFormat` 不做 static；foreach 不增删；`Arrays.asList` 不改 | https://github.com/alibaba/p3c/blob/master/p3c-pmd/README.md |
| Java：`switch` 穷举或带 `default`；`@Override`；禁止通配符 import | https://google.github.io/styleguide/javaguide.html §4.8.4、§6.1、§3.3.1 |
| Go：不用 `panic` 处理普通错误；`os.Exit` 只在 `main`；错误只处理一次并用 `%w`；comma ok；goroutine 可退出；不用 `init()`；不改包级可变变量 | https://github.com/uber-go/guide/blob/master/style.md |
| Go：响应切片初始化为空切片 | 由 Uber 指南 #nil-is-a-valid-slice 与 Zalando #124 的冲突推出，规定按 Zalando 执行 |
| Python：可变默认参数、宽泛 except、`assert` 校验、`try` 范围、logging 用占位符、`with` 管理资源 | https://google.github.io/styleguide/pyguide.html §2.12、§2.4、§3.10.1、§3.11 |
| Node.js：`return await`；`unhandledRejection` 处理后退出；只抛 `Error`；请求体 schema 校验和大小限制 | https://github.com/goldbergyoni/nodebestpractices §2.12、§2.10、§2.6、§2.2、§6.10、§6.14 |

未采纳：problem+json 错误格式、游标分页（与默认的统一响应和 `page`/`page_size` 分页冲突）；枚举 UPPER_SNAKE_CASE、批量操作 207、ETag/If-Match、错误动态值进结构化字段（价值较低或与现有规则重复）；JSON 字段一律 snake_case（由用户选择，不写成默认）。

**database-standard**

| 规则 | 来源 |
|---|---|
| 其他小数用 `NUMERIC`，禁止浮点 | 阿里巴巴 Java 开发手册（黄山版）五、MySQL 数据库 建表规约第 6 条，https://github.com/alibaba/p3c |
| PostgreSQL 禁止 `char(n)`、`money`、`serial`、时间精度、`timetz`、`CURRENT_TIME` | https://wiki.postgresql.org/wiki/Don%27t_Do_This |
| 业务唯一字段建唯一索引 | 阿里巴巴 Java 开发手册 索引规约第 1 条 |
| 单表索引不超过 15 个 | https://docs.gitlab.com/development/database/adding_database_indexes/#index-limitations |
| 事务里只放数据库语句 | https://docs.gitlab.com/development/database/transaction_guidelines/ |
| 分页 `ORDER BY` 以唯一字段结尾 | https://docs.gitlab.com/development/database/pagination_guidelines/#offset-pagination |
| join 不超过三张表、禁止左模糊、`IN` 不超过 1000、`count(*)`、多表带别名、禁止存储过程、业务代码不 `TRUNCATE` | 阿里巴巴 Java 开发手册 索引规约第 2、4 条，SQL 语句第 1、7、9、11、13 条；https://docs.gitlab.com/development/sql/#like--indexes |
| `NOT EXISTS` 代替 `NOT IN` | https://wiki.postgresql.org/wiki/Don%27t_Do_This#Don.27t_use_NOT_IN ；https://docs.gitlab.com/development/sql/ |
| 时间范围不用 `BETWEEN` | https://wiki.postgresql.org/wiki/Don%27t_Do_This （Don't use BETWEEN） |
| 批量更新按主键分批，推荐每批 1000 行 | https://docs.gitlab.com/development/database/iterating_tables_in_batches/ |
| 新增字段默认值只用常量 | https://squawkhq.com/docs/adding-field-with-default |
| 禁止直接改名；删字段分两次发布 | https://docs.gitlab.com/development/database/avoiding_downtime_in_migrations/ ；https://squawkhq.com/docs/renaming-column 、/docs/ban-drop-column |
| 结构迁移不超过 3 分钟，超时改后台分批任务，任务可重复执行且不引用模型代码 | https://docs.gitlab.com/development/migration_style_guide/#how-long-a-migration-should-take ；https://docs.gitlab.com/development/database/batched_background_migrations/ |
| PostgreSQL：`CONCURRENTLY` 建删索引且不在事务里；`USING INDEX` 加唯一约束；`NOT VALID` 后 `VALIDATE`；一个迁移最多一个外键且索引先建；`NOT NULL` 用 CHECK 过渡；禁止直接改类型；`lock_timeout`/`statement_timeout`；迁移可重复执行 | https://squawkhq.com/docs/rules 下各规则页；https://docs.gitlab.com/development/database/foreign_keys/ ；https://docs.gitlab.com/development/migration_style_guide/ |

未采纳：禁用外键或外键一律 `CASCADE`、时间用 `datetime`、软删除字段叫 `is_deleted`、表名用复数、`char` 定长字符串、字符串一律 `text` 加 CHECK（与现有默认规则冲突）；keyset 分页（与 `page`/`page_size` 分页冲突）。

**frontend-standard**

| 规则 | 来源 |
|---|---|
| React：派生值渲染时计算、开销大用 `useMemo`、用 `key` 重置状态、用户操作逻辑放事件处理函数、禁止链式 effect、禁止 effect 回传父组件 | https://react.dev/learn/you-might-not-need-an-effect |
| Query key 包含查询函数用到的全部变量；减少重复请求用 `staleTime` | https://tanstack.com/query/latest/docs/framework/react/guides/query-keys ；/guides/important-defaults |
| Vue：组件名多个单词；`scoped` 样式；模板只写简单表达式 | https://vuejs.org/style-guide/rules-essential.html ；/rules-strongly-recommended.html |
| TS：禁止 `any`、`@ts-ignore`、`@ts-nocheck`；断言要注释理由；禁止 `const enum`、`namespace`；非组件模块具名导出、禁止 `export let` | https://google.github.io/styleguide/tsguide.html |
| 不修改函数参数、不嵌套三元、布尔 prop 省略 `true` | https://github.com/airbnb/javascript （7.12、7.13、15.6）；https://github.com/airbnb/javascript/tree/master/react |

取舍：默认导出与具名导出两个来源矛盾，规定非组件模块只用具名导出；React 组件具名导出，懒加载的页面入口用默认导出（`React.lazy` 的要求）；Vue 单文件组件不涉及。

**ui-standard**

| 规则 | 来源 |
|---|---|
| 点击目标不小于 24×24 CSS 像素 | https://www.w3.org/TR/WCAG22/#target-size-minimum |
| 焦点可见且不被遮挡 | https://www.w3.org/TR/WCAG22/#focus-visible 、#focus-not-obscured-minimum |
| 状态消息用 `role="status"` / `aria-live` | https://www.w3.org/TR/WCAG22/#status-messages |
| 改变控件值不自动提交或跳转 | https://www.w3.org/TR/WCAG22/#on-input |
| 文字容器不写死高度 | https://www.w3.org/TR/WCAG22/#text-spacing |
| 不用 placeholder 代替标签；输入框宽度匹配内容；报错不清空输入；长表单顶部汇总错误；错误文案写清原因和改法 | https://design-system.service.gov.uk/components/text-input/ 、/components/error-message/ |
| 按钮文案动词开头、不用"确定"、不截断；危险按钮只用于不可撤销操作；少用禁用按钮；图标按钮带 tooltip | https://design-system.service.gov.uk/components/button/ ；https://atlassian.design/foundations/content/designing-messages/error-messages ；https://carbondesignsystem.com/components/button/usage/ |
| 加载时间分档（<1 秒不显示等）、不引起布局跳动 | https://primer.style/product/ui-patterns/loading/ |
| 空状态标题加说明加最多一个主操作；错误写具体原因 | https://primer.style/product/ui-patterns/empty-states/ |

未采纳：必填不加星号、选填标"（选填）"（与现有"必填加 *"冲突）。

**claude-md**

| 规则 | 来源 |
|---|---|
| CLAUDE.md 不超过 200 行；`@` 引用同样在启动时加载 | https://code.claude.com/docs/en/memory#write-effective-instructions |
| 约定写成能检查的形式；逐行删减测试；不写逐个文件的说明；强调词只加在个别条目 | https://code.claude.com/docs/en/best-practices#write-an-effective-claude-md |
| 每次必做的动作建议改成 hook | https://code.claude.com/docs/en/best-practices#set-up-hooks |
| 多个说明文件不互相矛盾；HTML 注释不进上下文；`CLAUDE.local.md` 加入 `.gitignore` | https://code.claude.com/docs/en/memory |
| 有 `AGENTS.md` 时在 CLAUDE.md 里 `@AGENTS.md` | https://code.claude.com/docs/en/memory ；https://agents.md/ |

取舍：规范仍用 `@docs/standards/*.md` 引用，不改放 `.claude/rules/`。`.claude/rules/` 按路径加载能节省上下文，但规范文件同时要给人看、要在 `docs/` 下统一维护；这一点作为可选做法在汇报里提示用户。
