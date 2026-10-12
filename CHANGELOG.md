# 改动说明

## 0.6.0

### 前端规范

- 新增第 9 章"安全"：链接协议白名单、`target="_blank"` 加 `noopener noreferrer`、`VITE_*` 变量视为公开、生产环境不公开 source map、外部对象不直接展开合并、新增依赖前确认漏洞和维护状态；Vue 的 `:style`、`:is` 不接受用户输入。
- 第 1 章：`react-hooks/exhaustive-deps` 设为 `error`，开启 `no-console`；Vue 项目类型检查用 `vue-tsc`。
- 第 3 章：已有公共组件不够用时先扩展；导出、复制这类全站能力统一封装。
- 第 5 章：禁止在页面里覆盖公共组件样式；禁止留 `console.log`；`Suspense` 外面包错误边界；用字符串字面量联合类型代替 `enum`；导出的函数写明参数和返回类型。
- 第 6 章：React 的清理函数、`AbortController`、函数式更新、渲染中不读写 `ref.current`、Context 只放低频数据；Vue 的 `toRefs`/`storeToRefs`、vue-query 的 key 不放 `.value`、`watch` 传 getter、`computed` 无副作用。
- 第 7 章：退出、401、切换账号共用一个会话清理函数；URL 参数、`localStorage`、`postMessage` 的数据先校验结构再用。
- 第 8 章：不做快照测试；异步内容用 `findBy*` 等待；端到端测试只清理自己创建的数据；React Testing Library 的查询优先级、`user-event`、MSW；Vue 的 `createTestingPinia`。

### 后端规范

- 第 1 章：语言有通行的源码静态安全扫描工具时加进门禁（Python `bandit`、Go `gosec`），放在依赖漏洞扫描前面。
- 第 4 章：日志和操作日志的脱敏范围；Java 构造器注入、DTO 用 `record`、集合防御性复制、`Optional` 的用法；Go 外部调用带超时的 `context`、接口定义在使用方；FastAPI 通过 `Depends` 获取会话和客户端、声明 `response_model`、校验写在 Pydantic 字段上、异步路由禁止阻塞调用。
- 第 5 章：导出复用列表的查询条件和数据权限；下拉选项走专门的 options 接口。
- 第 7 章：FastAPI 测试结束清空 `dependency_overrides`；Go 测试带 `-race`。
- 第 8 章：写操作多的接口限流；JWT 校验算法、过期时间、`iss`、`aud`；签名密钥至少 32 字节；权限码格式 `<模块>:<资源>:<动作>`；只算启用的角色，停用用户立即拒绝；`.gitignore` 加上私钥、证书和备份文件；静态安全扫描的误报处理。

### 其他规范

- 后端规范第 6 章：错误响应额外带 `request_id`，与响应头 `X-Request-ID` 相同；前端规范第 7 章：错误提示里一并显示请求 ID。
- 技术选型规范：用户指定或代码里在用的版本已经停止安全更新时，在汇报里单独提醒并建议升级。
- UI 规范 1.1 第 8 条：编号、权限码、提交号、代码路径用等宽字体，作为"全站一种字体"的例外。
- 数据库规范：MySQL 时区通过连接属性固定、不改实例全局时区；相对时间的边界用数据库时钟；删库重建类操作先核对、备份、得到同意。
- UI 规范：新增 1.1 第 13 条"文案只写业务含义"、1.7 动效、多层浮层规则、表格操作列固定在右侧、列表页默认能力、环比为 0 时的显示、4.3 新页面验收。
- 开发流程规范：合并请求写出没有验证到的部分；涉及认证、权限、支付、上传、加密的改动按安全一章逐条评审；改公共组件时写出检查过的调用方。
- CLAUDE.md 模板新增三条工作约定：改表前先读真实表结构；删库类操作先核对、备份、得到同意；只在用户明确要求时部署。
- 上手文档模板新增"常见改动要同步修改的位置"表，常见问题改为"现象 → 先看哪个文件 → 处理办法"。

### 规则来源

来源只记在这里，不写进生成的规范文档。

| 规则 | 来源 |
|---|---|
| 错误响应带请求 ID、技术标识用等宽字体、列表页默认能力、导出口径、公共组件先扩展、页面不覆盖公共样式、动效、多层浮层、文案、操作列固定、环比、新页面验收、会话清理、options 接口、权限码格式、停用角色和用户、操作日志脱敏、密钥长度、MySQL 时区属性、数据库时钟、删库确认、`.gitignore` 范围、端到端测试数据清理、改动影响表、改表前读表结构、部署前确认、验证记录写未验证部分 | https://github.com/jiangwanyutao/scaffold-master （`AGENTS.md`、`DESIGN.md`、`PROJECT_MAP.md`、`README.md`、`docs/`） |
| FastAPI、Python、TypeScript、React、Vue、Java、Go 的编码、测试、安全规则，静态安全扫描，安全专项评审 | https://github.com/affaan-m/ECC `rules/`（main 分支 `b92f181`，2.2.3）。Copyright (c) 2026 Affaan Mustafa，MIT License。规则按本插件的格式改写成中文 |

## 0.5.0

- `ui-standard` 的输出文件从 `docs/standards/ui.md` 改为 `docs/standards/design.md`；目标项目已有 `ui.md` 时沿用原名，按已有文件的流程合并。标题固定为 `# UI 设计规范`，不写版本号（共用步骤里"版本写进标题"一条只适用于后端、数据库、前端规范）。章节改为四部分：
  1. 视觉风格：原则、主题切换、颜色变量、状态色，新增字体字号间距圆角阴影表和图标规则。
  2. 控件组件规格（新增）：所有控件的状态外观（默认、悬停、键盘焦点、选中、禁用、报错、加载中），组件清单写用途、变体、尺寸和使用约束。
  3. 页面结构：整体框架，新增路由与菜单表；列表页、详情页、表单页三种页面模板。
  4. 页面逻辑（新增）：加载、空状态、错误，筛选与分页，表单提交，离开未保存的表单，不可撤销操作，权限，接口错误；各页面的数据来源、交互和权限表；业务对象的状态流转图。
- 有代码时读组件目录、路由表和页面源码，每个组件和页面写出源码路径，汇报新增"覆盖的组件和页面"一项。
- 上手文档、CLAUDE.md 模板改为引用 `design.md`；`init-standards` 交叉检查新增"组件位置和目录名与前端规范一致"。

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
