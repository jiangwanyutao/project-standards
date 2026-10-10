# 前端规范默认规则

每章分"默认规则"和"按技术确定"两部分。默认规则以 React + TypeScript 的写法给出，目标框架不同时换成对应写法。

## 1. 工具

默认规则：
- 包管理工具固定一种，锁文件必须提交；**禁止**混用其他包管理工具安装依赖。
- 写明样式方案、主题变量所在的文件。
- **必须**：支持浅色、深色、跟随系统三种主题，写明主题切换逻辑所在的文件；组件里**禁止**写死颜色值，一律读主题变量（变量清单见 UI 设计规范）。
- **必须**：页面在窄屏（1024px 以下）下可用，导航可收起，**禁止**页面出现横向滚动。
- 写明基础组件库；图标只用一个图标库，**禁止**混用。
- 写明路由库、服务端状态库。
- 后端提供 OpenAPI 时，接口类型由命令从 OpenAPI 生成到固定文件，生成文件提交，**禁止**手改。
- TypeScript 开启 `strict: true`。
- 代码检查用 ESLint，写出必须启用的插件：TypeScript 规则、框架规则、无障碍规则。
- 格式化用 Prettier，写出关键配置：引号、分号、行宽 100。
- 写出一条质量门禁命令（lint、类型检查、测试、构建），合并前必须在本地跑通，任何一项失败就不允许合并。

按技术确定：
- 包管理（空项目推荐 pnpm）；Windows 上用 pnpm 时推荐 `.npmrc` 里 `node-linker=hoisted`，避免符号链接目录读取失败。
- 样式方案（空项目推荐 Tailwind CSS）；组件库（React 推荐 shadcn/ui，Vue 推荐 Element Plus 或 shadcn-vue，看用户偏好）；图标库（跟组件库配套，例如 Element Plus 用 `@element-plus/icons-vue`）。
- 路由和服务端状态：React 用 React Router + TanStack Query；Vue 用 Vue Router + TanStack Query（`@tanstack/vue-query`）。代码里已经用 Pinia/Redux 存服务端数据的，沿用并问用户是否以后改用 TanStack Query。
- ESLint 插件：React 用 `@typescript-eslint`、`eslint-plugin-react-hooks`、`eslint-plugin-jsx-a11y`；Vue 用 `typescript-eslint`、`eslint-plugin-vue`、`eslint-plugin-vuejs-accessibility`。
- 接口类型生成工具（`openapi-typescript`）和命令名（`gen:api`）。
- 门禁命令按 `package.json` 里真实存在的脚本写；缺的脚本列为"需要新增的配置"。

## 2. 目录

默认规则：画出 `src/` 目录树，每个目录用注释写清楚放什么。推荐结构（React）：

```
src/
  main.tsx
  App.tsx
  routes/                  # 路由表，页面组件一律懒加载
  pages/                   # 页面。一个页面一个目录，目录名用 PascalCase
    OrderList/
      index.tsx            # 页面入口
      components/          # 只在这个页面里用的组件
  components/
    ui/                    # 组件库 CLI 生成的基础组件，只允许做主题层面的调整
    app/                   # 应用外壳：导航、面包屑、路由守卫
    common/                # 跨页面复用的组件：PageHeader、Pagination、StatusBadge、ConfirmDialog、EmptyState
  api/
    client.ts              # 唯一的请求封装
    orders.ts              # 每个资源一个文件，里面是请求函数
  queries/                 # 服务端状态 hooks，每个资源一个文件，如 useOrders.ts
  hooks/                   # 与业务无关的通用 hooks
  types/                   # 接口类型：api.gen.ts 由命令生成；字段名和后端一致
  lib/                     # 工具函数、枚举和中文显示名的映射
  styles/
```

- **禁止**：在 `src/` 下另建本表之外的顶层目录；确实需要时先改这份规范。
- 导入路径统一用 `@/` 别名；**禁止**三层及以上的相对路径，例如 `../../../`。

按技术确定：Vue 项目页面目录用 `views/`、通用逻辑用 `composables/`、`queries/` 里是 `useXxx` composables；组件库是 npm 包（Element Plus、Ant Design）而不是 CLI 生成的源码时，删掉 `components/ui/`。有代码时按真实目录画，和推荐结构不一致的地方列出来问用户是否调整。

## 3. 组件复用规则

默认规则：
1. **优先用组件库**：需要新的界面元素时先找组件库里的现成组件，找不到才自己写。
2. 组件库生成的基础组件目录（如 `components/ui/`）**禁止**写业务逻辑或调用接口，也不为业务需求修改它的行为，需要定制时在外面包一层。
   - **例外**：可以新增可选、且默认值保持原有行为的 props，并在 prop 上写注释说明用途；用 CLI 重新生成组件时要把这些 prop 补回来。
3. 同一个界面模式在 2 个及以上页面出现时，**必须**提取到 `components/common/`；**禁止**复制粘贴后再小改。
4. `components/common/` 里的组件需要数据时只能通过 `queries/` 的 hooks 获取，**禁止**直接调用 `api/` 的请求函数。
5. 只在一个页面里用的组件放在这个页面的 `components/` 下，第二个页面也要用时再提升到 `common/`。
6. 状态、优先级、类型等枚举值的中文名和颜色，统一在 `lib/enums.ts` 定义一份，由 `StatusBadge` 等组件读取；**禁止**在页面里硬编码枚举值的中文名。

按技术确定：组件库是 npm 包时第 2 条改为"**禁止**修改 `node_modules` 或覆盖组件库内部样式类，定制通过主题变量或外包一层组件完成"。

## 4. 命名

默认规则：用表格写"对象 | 规则 | 示例"：

| 对象 | 规则 | 示例 |
|---|---|---|
| 组件文件、组件名 | PascalCase | `OrderStatusBadge.tsx` |
| 以目录形式组织的组件 | 目录名就是组件名，入口文件是 `index.tsx` | `pages/OrderList/index.tsx` |
| Hook | `use` 开头的 camelCase | `useOrders.ts` |
| 其他 TS 文件 | camelCase | `client.ts`、`enums.ts` |
| 变量、函数 | camelCase | `orderList` |
| 常量 | UPPER_SNAKE_CASE | `DEFAULT_PAGE_SIZE` |
| 类型、接口 | PascalCase，不加 `I` 前缀 | `Order`、`OrderCancelIn` |
| 事件相关 | props 以 `on` 开头，处理函数以 `handle` 开头 | `onCancel` 传入 `handleCancel` |
| 布尔 props 或变量 | 以 `is`、`has`、`can` 开头 | `isLoading` |

- **禁止**：props 名称挪用 DOM 属性名表达别的意思，例如用 `style` 表示"风格类型"。

按技术确定：Vue 单文件组件用 `.vue`、PascalCase 文件名；事件用 `emit('cancel')`，模板里写 `@cancel`；props 在模板中用 kebab-case。

## 5. 组件与模板

默认规则：
- **必须**：只写函数组件；一个文件只放一个组件，纯展示的无状态小组件除外。
- **必须**：用 TypeScript 类型声明 props，可选 props 给出默认值。
- **必须**：没有子元素的标签自闭合；多个属性时每行一个。
- **必须**：列表的 `key` 用稳定 ID，**禁止**用数组下标。**例外**：没有 ID、顺序固定且不会增删重排的静态序列可以用位置，并加注释说明。
- **必须**：`<img>` 写有意义的 `alt`；可交互元素要能用键盘操作。
- **必须**：组件不超过 200 行，超过就拆分成子组件或 hook。
- **禁止**：直接插入 HTML 字符串（`dangerouslySetInnerHTML`），确实需要时先用 DOMPurify 处理。
- 样式只用项目的样式方案（例如 Tailwind 类名，合并用 `cn()`）；**禁止**写行内 `style`，运行时动态计算的值（坐标、拖拽尺寸）除外。
- TypeScript：
  - **禁止**：`any`，类型未知时用 `unknown` 再收窄；**禁止** `@ts-ignore`、`@ts-nocheck`。`@ts-expect-error` 只允许在测试文件里用，并在同一行写明原因。
  - 类型断言 `as` 和非空断言 `!` 只在加了注释说明理由时使用，其余情况改成运行时检查。
  - **禁止**：`const enum`、`namespace`。
  - 非组件模块（`api/`、`lib/`、`hooks/`、`queries/`）只用具名导出，**禁止** `export default`；**禁止**导出可变绑定（`export let`）。
- **禁止**：修改或重新赋值函数参数；嵌套三元表达式。

按技术确定：
- React：
  - 布尔 props 值为 `true` 时省略值，写成 `<Button disabled />`。
  - 组件用具名导出；只有被路由懒加载的页面入口（`pages/Xxx/index.tsx`）用默认导出，因为 `React.lazy` 要求默认导出。
- Vue：
  - 用 `<script setup lang="ts">` + `defineProps<T>()`、`withDefaults`；`v-for` 必须带稳定 `:key`；**禁止** `v-html`，确实需要时先用 DOMPurify；**禁止** `v-if` 和 `v-for` 写在同一元素上。
  - 组件名由多个单词组成（根组件 `App` 除外），例如 `OrderList.vue`，**禁止** `List.vue`。
  - 除 `App` 和布局组件外，组件样式都加 `scoped`。
  - 模板里只写简单表达式，复杂表达式移到 `computed` 或方法里；复杂的 `computed` 拆成多个简单的 `computed`。

## 6. 状态管理

默认规则：
- React 遵守 Hooks 规则：只在组件或 hook 的最外层调用，不放在条件语句或循环里。
- **服务端数据只放在服务端状态库（TanStack Query）里**，**禁止**把查询结果再复制一份到组件状态里。
- Query key 按 `[资源, 维度, 参数]` 组织，例如 `['orders', 'list', params]`、`['orders', 'detail', id]`；查询函数里用到的会变化的变量（分页、筛选条件、id）都必须放进 key。
- 要减少重复请求时设置 `staleTime`，**禁止**在各个页面逐个关掉 `refetchOnWindowFocus` 等默认开关。
- 修改数据用 mutation，成功后让相关查询失效；**禁止**手动改缓存来假装更新成功。
- 纯界面状态（弹窗开关、当前页签）放组件状态；需要跨页面保留的筛选条件放进 URL 查询参数。
- 不引入全局状态库存放服务端数据。

按技术确定：
- React 的 Effect 规则（写进本章）：
  - **禁止**：把能从 props 或 state 算出来的值另存为 state，或用 `useEffect` 同步它，直接在渲染时计算；计算开销大时用 `useMemo`。
  - prop（例如 `id`）变化需要清空组件全部状态时，给组件传 `key={id}`，**禁止**在 `useEffect` 里逐个重置 state。
  - 由用户操作引起的逻辑（提交表单、发请求、通知父组件）写在事件处理函数里；只有"组件显示出来就要执行"的代码才放 `useEffect`。
  - **禁止**：多个 `useEffect` 链式触发 `setState`（A 变了改 B，B 变了改 C）。
  - **禁止**：子组件在 `useEffect` 里把数据回传给父组件，数据由父组件获取后往下传。
- Vue 用 `ref`/`computed`，只在 `setup` 顶层调用 composables；需要全局客户端状态（登录用户、界面偏好）时用 Pinia，Pinia 里**禁止**存放接口列表数据。

## 7. 请求层

默认规则：
- 所有请求都经过 `api/client.ts` 里的唯一封装函数，它负责：
  - 附加认证信息（token）；
  - 拆开后端统一响应：成功码时返回 `data`，否则抛出带错误码、消息、`detail` 的 `ApiError`；
  - 遇到 401 跳转登录页。
- 页面和组件**禁止**直接调用 `fetch` 或 axios，只能通过 `queries/` 里的 hooks 获取数据。
- 错误提示统一交给服务端状态库的全局错误回调，用 toast 显示 `message`；页面只处理需要特殊交互的错误码（例如状态冲突时刷新详情）。

按技术确定：底层用 `fetch` 还是 axios（看依赖）；认证方式（Bearer token、Cookie）；统一响应结构和成功码按后端规范；toast 组件（组件库自带的 `ElMessage`、shadcn 的 sonner）。

## 8. 测试

默认规则：
- 写出测试框架、测试文件位置和命名：测试文件和组件放在一起，命名为 `Xxx.test.tsx`。
- 重点测这几类：`components/common/` 里的组件、按状态启用或禁用的操作按钮、表单校验、`lib/` 下的工具函数。
- 写明是否有端到端测试；有的话写出框架、目录和命令，没有的话写"暂无端到端测试"。

按技术确定：React 用 Vitest + Testing Library；Vue 用 Vitest + `@vue/test-utils` 或 `@testing-library/vue`；端到端测试用 Playwright，覆盖登录和核心业务流程，不放进合并门禁（空项目直接用这个推荐值）。
