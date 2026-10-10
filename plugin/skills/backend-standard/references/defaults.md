# 后端规范默认规则

每章分两部分：

- **默认规则**：与技术无关，按原意写进规范，措辞换成目标技术的写法。
- **按技术确定**：要从代码推断或问用户的项，括号里是空项目时的推荐值。

最后一节是 Python + FastAPI 项目按这些规则写出的完整示例，用来参照写法和详细程度。

## 1. 基线

默认规则：
- 选一份语言公认的风格指南做基线，写明"本文没有写到的，一律以基线为准"，并附官方链接。
  - Python：PEP 8 + PEP 484 类型注解；Java：Google Java Style Guide；Go：Effective Go + `gofmt`；TypeScript/Node.js：TypeScript 官方手册 + 项目 ESLint 配置；C#：.NET 官方编码约定。
- 格式化、lint、类型检查、测试覆盖率各由一个工具强制执行，写出工具名和关键配置（例如行宽）。
- 写出一条质量门禁命令，合并前必须在本地跑通，任何一项失败就不允许合并。
- 依赖用固定的一种工具管理，锁文件必须提交；**禁止**用其他方式安装项目依赖。

按技术确定：语言版本、框架版本、格式化工具（Java：Spotless + google-java-format；Go：gofmt/goimports；Python：ruff format）、lint 工具（Java：Checkstyle 或 PMD；Go：golangci-lint；Python：ruff check）、类型检查（Python：mypy --strict；TS：tsc --noEmit）、行宽（100）、依赖管理工具（Java：Maven 或 Gradle，看已有文件；Python：uv；Node.js：pnpm）、门禁命令。

## 2. 命名

默认规则：
- 用表格写出：模块/包、类、函数/方法、变量、常量、布尔变量、私有成员的命名方式和示例。布尔变量以 `is`、`has`、`can` 开头；常量用 UPPER_SNAKE_CASE。
- 接口模型（DTO）按用途加后缀：请求体 `XxxCreateIn`、`XxxUpdateIn`，或按动作命名的 `XxxResolveIn`；响应 `XxxOut`；查询参数 `XxxQuery`。
- **禁止**：接口直接返回数据库实体或 ORM 对象，一律转换成响应模型。

按技术确定：语言的大小写惯例（Java/C# 类和方法、Go 的导出规则）；代码里已有统一的 DTO 后缀（如 `XxxReq`/`XxxVO`、`XxxRequest`/`XxxResponse`）时沿用已有写法。

## 3. 目录与分层

默认规则：
- 画出后端源码目录树，每个目录后面用注释写清楚放什么。按业务域分文件（例如 `order_service`、`user_service`），不按技术类型堆在一个文件里。
- 分层和调用方向：只允许 接口层 → 业务层 → 数据模型 / 外部服务封装，**禁止**反向调用。用表格写出每层"负责"和"禁止"：

  | 层 | 负责 | 禁止 |
  |---|---|---|
  | 接口层（路由/Controller/Handler） | 解析参数、认证和权限校验、调用业务层、封装响应 | 写业务逻辑、直接操作数据库 |
  | 业务层（Service） | 业务规则、状态机、事务边界、写操作日志 | 依赖 HTTP 请求和响应对象 |
  | 数据模型 | 表结构映射 | 业务逻辑 |
  | 外部服务封装 | 对象存储、第三方 API、消息队列等的调用 | 业务判断 |

- 不单独设 Repository 层，业务层直接使用数据库会话；同一个查询被 3 个以上 Service 复用时，才抽成函数放进对应的 Service 模块。
  - 框架本身要求数据访问层的（MyBatis Mapper、Spring Data Repository、Go 的 sqlc 生成代码），保留这一层，只写查询，不写业务判断。

按技术确定：真实目录结构（有代码时照实画，空项目按框架惯例给出建议并请用户确认）、命令行入口、后台任务目录、种子数据目录。

## 4. 编码规则

默认规则：
- **必须**：函数的参数和返回值写完整类型（静态类型语言天然满足时省略本条）。
- **必须**：框架是异步模型时，I/O 全部异步；**禁止**在异步函数里调用同步 I/O，确实需要时放到线程池执行。
- **必须**：配置从环境变量或配置中心读取，**禁止**在代码里硬编码地址、密钥、账号。
- **必须**：日志用统一的日志库，输出结构化字段；**禁止**用打印语句代替日志（命令行工具的输出除外）；**禁止**在日志里写密码、token、API Key。
- **禁止**：捕获异常后不处理就继续运行。捕获后要么转换成业务异常抛出，要么记录日志后重新抛出。
- **禁止**：在模块加载时产生副作用（例如加载时连接数据库）；语言有可变默认参数问题的（Python），禁止可变默认参数。
- **必须**：函数不超过 50 行，文件不超过 400 行，嵌套不超过 3 层，超过的要拆分。有条件的话用自动测试或 lint 规则拦截。
- **必须**：魔法数字和字符串定义成常量或枚举；枚举集中放在一个文件里，取值与数据库的 `CHECK` 约束（或字典表）保持一致。
- **禁止**：在请求处理过程中随手启动线程、协程或定时器执行后台任务，一律交给项目的任务机制（任务表 + worker、消息队列、框架调度器）。

按语言补充的编码规则（只写目标语言那一组，写进本章，和上面的规则放在一起）：

- **Java**：
  - **禁止**：在 `finally` 块里 `return`。
  - **必须**：线程池用 `ThreadPoolExecutor` 显式创建，**禁止**用 `Executors` 的工厂方法；线程和线程池都要命名；自定义的 `ThreadLocal` 用完在 `finally` 里调用 `remove()`。
  - **必须**：包装类型（`Integer`、`Long` 等）用 `equals` 比较，**禁止**用 `==`；**禁止**把 `SimpleDateFormat` 定义成 static 变量，日期格式化用 `DateTimeFormatter`。
  - **禁止**：在 foreach 循环里增删集合元素（用 `Iterator.remove` 或 `removeIf`）；**禁止**对 `Arrays.asList` 返回的列表调用 `add`、`remove`、`clear`。
  - **必须**：每个 `switch` 穷举全部情况或带 `default`；能加 `@Override` 的方法都加；**禁止**通配符 import（`import java.util.*`）。
- **Go**：
  - **禁止**：用 `panic` 处理普通错误；`os.Exit` 和 `log.Fatal*` 只允许出现在 `main()` 里。
  - **必须**：同一个错误只处理一次，记日志和向上返回二选一；向上返回时用 `fmt.Errorf("...: %w", err)` 包装。
  - **必须**：类型断言用 comma ok 写法：`v, ok := x.(T)`。
  - **必须**：每个 goroutine 都有明确的退出方式（`context` 取消或关闭 channel），并且能等待它结束；**禁止**使用 `init()`；**禁止**修改包级可变变量，依赖通过构造函数注入。
  - **必须**：接口响应里的切片在序列化前初始化为空切片（`make([]T, 0)`），nil 切片会被 `encoding/json` 输出成 `null`。
- **Python**：
  - **禁止**：可变对象作为参数默认值；裸 `except:` 和 `except Exception:`（最外层的异常处理器除外，且必须记录日志或重新抛出）；用 `assert` 校验参数。
  - **必须**：`try` 块只包住可能抛出该异常的那几行。
  - **必须**：logging 调用的第一个参数是带 `%s` 占位符的字符串字面量，**禁止**用 f-string 拼接日志消息。
  - **必须**：文件、连接等资源用 `with` 或 `async with` 管理。
- **Node.js**：
  - **必须**：async 函数里返回 Promise 时写 `return await`，保留完整的错误调用栈。
  - **必须**：进程注册 `unhandledRejection` 和 `uncaughtException` 处理，记录日志后退出进程，由进程管理器重启。
  - **必须**：只抛出 `Error` 或其子类；请求体用 schema 校验（zod、JSON Schema），并在中间件里限制请求体大小。

按技术确定：异步模型（Spring MVC 是同步模型，本条按"不在请求线程里做长耗时操作"改写）、配置库（Spring `@ConfigurationProperties`、pydantic-settings、viper）、日志库（SLF4J + Logback、logging、zap/slog）、后台任务机制（空项目直接用任务表 + worker）。

## 5. RESTful 约定

默认规则：
- 所有接口加版本前缀 `/api/v1`。
- 资源名用复数名词，多个单词用 kebab-case，例如 `/api/v1/orders`、`/api/v1/order-items`。路径最多嵌套两层。
- 用表格写出各操作的方法、路径和成功状态码：

  | 操作 | 方法与路径 | 成功时的 HTTP 状态码 |
  |---|---|---|
  | 列表 | `GET /<父资源>/{id}/<资源>` 或 `GET /<资源>` | 200 |
  | 详情 | `GET /<资源>/{id}` | 200 |
  | 新建 | `POST /<资源>` | 201，响应头 `Location` 给出新资源的地址 |
  | 整体修改 | `PUT /<资源>/{id}` | 200 |
  | 部分修改 | `PATCH /<资源>/{id}` | 200 |
  | 删除（软删除） | `DELETE /<资源>/{id}` | 200，`data` 为 `null` |
  | 状态动作 | `POST /<资源>/{id}/<动词>`，例如 `/approve`、`/cancel` | 200 |
  | 异步任务 | `POST /<资源>/{id}/<动词>` | 202，`data` 中返回任务 ID，响应头 `Location` 给出查询任务状态的地址 |

- **禁止**：在路径里用动词表示增删改查，例如 `/getOrder`、`/orders/delete`。只有不属于增删改查的状态流转才用 `POST /<资源>/{id}/<动词>`。
- 查询参数和 JSON 字段统一用一种大小写，前端类型定义与之相同，不做转换。
- 时间以 ISO 8601 格式返回并带时区，例如 `2026-09-23T08:00:00Z`。ID 以数字返回。
- 分页参数：`page` 从 1 开始；`page_size` 默认 20，最大 300。`page_size` 超过 300 时按 300 处理，不报错；`page` 或 `page_size` 小于 1、不是整数时返回 10001。
- 排序参数：`sort=<字段>`，`order=desc` 或 `asc`。每个列表接口在接口文档里写明默认排序。
- 列表和详情默认不返回已软删除的记录。
- **必须**：集合字段为空时返回 `[]`，**禁止**返回 `null`。
- **禁止**：布尔字段返回 `null`。字段值为 `null` 和不返回该字段按同一种含义处理。
- `PATCH` 请求体只包含要修改的字段，没出现的字段不改；字段值为 `null` 表示清空该字段，只在 `PATCH` 里有这个含义。
- **禁止**：任何查询接口返回密码、密钥、token 等机密字段，响应模型里不定义这些字段。
- 创建订单、支付等重复提交会造成损失的接口，支持 `Idempotency-Key` 请求头：同一个键 24 小时内重复请求时，直接返回第一次的响应，不再执行。
- 每个请求都有请求 ID：请求头带了 `X-Request-ID` 就沿用，没带就生成一个；写进响应头和这次请求的所有日志。

按技术确定：JSON 字段大小写（空项目推荐 snake_case，与数据库字段一致；代码里已经统一用 camelCase 的沿用 camelCase，分页参数相应写成 `pageSize`）；版本前缀（代码里已有统一前缀的沿用）。

## 6. 统一响应

默认规则：
- 所有接口的响应体都是 `{code, message, data}`，出错时额外带 `detail`。`code` 为 0 表示成功。
- HTTP 状态码按语义返回，**禁止**所有情况都返回 200。
- 成功响应统一由两个辅助函数封装（普通结果、分页结果），写出它们所在的文件。
- 错误由全局异常处理器统一输出，接口层**禁止**自己拼装错误响应。业务代码抛出业务异常（携带错误码和 `detail`），由异常处理器映射成 HTTP 状态码。
- 写出四个示例：成功、分页（`items`、`total`、`page`、`page_size`）、业务错误、参数校验失败（`detail` 为 `[{field, error}]` 列表）。示例数据用符合目标项目业务的内容。
- 错误码为 5 位数字，前两位表示业务域。通用错误码固定为：

  | 码 | HTTP | 含义 |
  |---|---|---|
  | 10001 | 400 | 参数校验失败（框架默认的 422 等统一改为 400） |
  | 10002 | 401 | 未登录，或 token 无效 |
  | 10003 | 403 | 没有权限 |
  | 10004 | 404 | 资源不存在，或已被删除 |
  | 10005 | 409 | 数据冲突，例如违反唯一约束 |
  | 10006 | 429 | 请求过于频繁（项目有限流时才登记） |
  | 50000 | 500 | 服务器内部错误。`message` 固定为"服务器内部错误"，`detail` 为空，详细信息只写日志 |

- 业务错误码从 20001 起按业务域分段，在表里登记。新增错误码时，同时登记到代码里的错误码枚举和本表。
- **禁止**：错误响应的任何字段（包括 `detail`）包含异常堆栈、SQL、类名等实现细节。
- **必须**：先校验权限，再查资源是否存在。没有权限时不论资源是否存在都返回 10003，有权限而资源不存在时返回 10004，避免泄露资源是否存在。
- 有限流的接口，被限流时返回 10006（HTTP 429），带 `Retry-After` 响应头。

按技术确定：辅助函数和异常处理器的实现位置（Spring：`@RestControllerAdvice`；FastAPI：`exception_handler`；Gin：中间件）；业务域分段（空项目问用户有哪些业务域）；已有统一响应结构（例如 `{code, msg, data}`、`{success, data, error}`）时沿用已有结构。

## 7. 测试

默认规则：
- 写出测试框架、测试文件命名、测试函数命名方式。函数名表达"行为 + 条件"，例如 `test_cancel_order_rejects_paid_order`。
- 每条用例按"准备 → 执行 → 断言"三段组织，只验证一个行为。
- 单元测试不连数据库；集成测试连独立的测试库，通过 HTTP 客户端调用接口。写出两类测试的目录。
- 覆盖率不低于 80%。状态机、防重复、权限校验这类逻辑必须有测试。

按技术确定：测试框架（Java：JUnit 5 + Mockito + Testcontainers；Go：testing + testify；Python：pytest + httpx；Node.js：Vitest 或 Jest）、覆盖率工具（JaCoCo、`go test -cover`、pytest-cov）、测试库名（见数据库规范的库名规则）、Java 方法名风格（`cancelOrder_rejectsPaidOrder` 或 `@DisplayName`，代码里已有的沿用）。

## 示例：Python 3.12 + FastAPI 项目的完整写法

下面的内容只用来参照详细程度和措辞。目标项目是 Python + FastAPI 时，可以直接按它组织，但目录、命令、错误码必须换成目标项目真实的内容。

````markdown
# 后端编码规范（Python 3.12 + FastAPI）

## 1. 基线

- **基线**：PEP 8（https://peps.python.org/pep-0008/）加 PEP 484 类型注解（https://peps.python.org/pep-0484/）。本文没有写到的，一律以 PEP 8 为准。
- **工具强制**：
  - `ruff check` 和 `ruff format` 负责风格与 lint，行宽 100。
  - `mypy --strict` 负责类型检查。
  - `pytest --cov` 负责测试和覆盖率。
  - 合并前必须在 `backend/` 下本地跑通质量门禁：`uv run ruff format --check . && uv run ruff check . && uv run mypy app tests && uv run pytest --cov`，任何一项失败就不允许合并。
- 依赖用 uv 管理：`uv add` / `uv sync`，锁文件 `uv.lock` 必须提交。**禁止**使用 `pip install` 装项目依赖。

## 2. 命名

| 对象 | 规则 | 示例 |
|---|---|---|
| 模块、包、函数、变量 | snake_case | `order_service.py`、`cancel_order()` |
| 类、Pydantic 模型 | PascalCase | `OrderOut`、`OrderCancelIn` |
| 常量 | UPPER_SNAKE_CASE | `MAX_PAGE_SIZE = 300` |
| 布尔变量 | 以 `is_`、`has_`、`can_` 开头 | `is_paid` |
| 私有成员 | 以单下划线开头 | `_build_items()` |

Pydantic 模型的后缀约定：
- 请求体用 `XxxCreateIn`、`XxxUpdateIn`，或按动作命名的 `XxxCancelIn` 等。
- 响应用 `XxxOut`。
- 查询参数用 `XxxQuery`。
- **禁止**：直接返回 ORM 对象。

## 3. 目录与分层

```
backend/
  app/
    main.py              # 创建应用、注册路由、中间件和异常处理
    cli.py               # 命令行入口：python -m app.cli seed|backup ...
    core/                # 基础设施：config、db、security、response、errors、deps、enums
    models/              # SQLAlchemy 模型，按业务域分文件；公共基类在 base.py
    schemas/             # Pydantic 模型，按业务域分文件
    api/v1/              # 路由，一种资源一个文件
    services/            # 业务逻辑，按业务域分文件
    workers/             # 后台任务 worker
    integrations/        # 外部依赖：对象存储、第三方 API
    seeds/               # 预置数据，见数据库规范
  migrations/            # Alembic
  tests/
    unit/
    integration/
  pyproject.toml
```

各层的职责和调用方向：只允许 `api → services → models / integrations`，不能反向调用。

| 层 | 负责 | 禁止 |
|---|---|---|
| `api` | 解析参数、通过依赖注入完成认证和权限校验、调用 service、封装响应 | 写业务逻辑，直接操作数据库会话 |
| `services` | 业务规则、状态机、事务边界、写操作日志 | 依赖 FastAPI 的 `Request` / `Response`，返回 HTTP 响应 |
| `models` | 表结构 | 业务逻辑 |
| `integrations` | 封装外部服务 | 业务判断 |

不单独设 repository 层，service 直接使用会话。只有当同一个查询被 3 个以上 service 复用时，才把它抽成函数放在对应的 service 模块里。

## 4. 编码规则

- **必须**：所有函数都写完整的参数和返回值类型注解。
- **必须**：I/O 全部异步：数据库用 `AsyncSession`，HTTP 用 `httpx.AsyncClient`。**禁止**在 `async def` 里调用同步 I/O；确实需要时用 `asyncio.to_thread` 包一层。
- **必须**：配置通过 `pydantic-settings` 从环境变量读取。**禁止**在代码里硬编码地址、密钥、账号。
- **必须**：日志用标准库 `logging`，输出结构化字段。**禁止**使用 `print`（例外：`app/cli.py` 的命令行输出）；**禁止**在日志中写入密码、token、API Key。
- **禁止**：裸写 `except:` 或 `except Exception: pass`。捕获异常后要么转换成业务异常抛出，要么记录日志后再抛出。
- **禁止**：可变默认参数；在模块导入时产生副作用，例如导入时连数据库。
- **必须**：函数不超过 50 行，文件不超过 400 行，嵌套不超过 3 层，超过的要拆分。
- **必须**：魔法数字和字符串一律定义成常量或 `StrEnum`。枚举值集中放在 `app/core/enums.py`，与数据库的 `CHECK` 约束保持一致。
- **禁止**：后台任务直接使用 `asyncio.create_task`，一律写入任务表，由 worker 执行。

## 5. RESTful 约定
（按上文第 5 节的默认规则展开，路径示例用本项目的资源）

## 6. 统一响应
（按上文第 6 节的默认规则展开，示例数据和业务错误码用本项目的业务）

## 7. 测试

- 用 pytest。文件命名为 `test_<被测模块>.py`，函数命名为 `test_<行为>_<条件>`，例如 `test_cancel_order_rejects_paid_order`。
- 用例按"准备 → 执行 → 断言"三段组织，每条用例只验证一个行为。
- 单元测试放在 `tests/unit/`，不连数据库。集成测试放在 `tests/integration/`，连 `<项目>_test` 库，接口用 `httpx.AsyncClient` 调用。
- 覆盖率不低于 80%。状态机、防重复规则、权限校验这类逻辑必须有测试。
````
