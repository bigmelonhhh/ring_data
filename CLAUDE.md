# CLAUDE.md

此文件为 Claude Code (claude.ai/code) 在此仓库中工作时提供指导。

## 工作目录

所有前端开发均在 `public/` 中进行。所有命令（install、dev、lint、build）皆从 `public/` 执行，而非仓库根目录。仓库根目录仅用于仓库级文档和配置。

## 命令

| 命令 | 说明 |
|---------|-------------|
| `npm run dev` | 启动 Vite 开发服务器（端口取自 `VITE_PORT`，默认 5173） |
| `npm run lint` | 对 `.ts` 和 `.vue` 文件执行 ESLint，零警告策略 |
| `npm run lint:fix` | ESLint 自动修复 |
| `npm run typecheck` | 基于 tsconfig.app.json 执行 `vue-tsc --noEmit`（严格模式） |
| `npm run build` | 类型检查后执行生产构建 |
| `npm run preview` | 本地预览生产构建结果 |

## 交付门禁

- 修改页面、组件、Store、API 或工具函数后：必须运行 `npm run lint` 和 `npm run typecheck`
- 修改路由、Vite 配置、样式、环境变量或自动导入配置后：还必须运行 `npm run build`

## 技术栈

Vue 3.5（组合式 API，`<script setup lang="ts">`）、TypeScript（strict）、Vite 8、Vant 4（按需自动引入）、Pinia、Vue Router 4、Axios、Tailwind CSS、UnoCSS。

## 架构

### 入口与路由

- `main.ts` → `App.vue` → `<router-view />`（位于 `AppLayout.vue` 内）
- `router/index.ts` 定义所有路由，页面组件均为懒加载。`router/guard.ts` 设置文档标题并通过 `meta.requiresAuth` 强制执行鉴权。
- `AppLayout.vue` 以页面滑动过渡动画包裹整个应用外壳。除此外无其他嵌套布局。

### 目录职责

```
public/src/
├── api/modules/       # 按业务域拆分的接口函数，统一使用 utils/request.ts 中的 http
├── assets/            # 图片、图标等静态资源 —— 优先使用 webp/svg
├── components/        # 全局公共组件（Loading、Empty、NavBar、TabBar、Toast 封装、Dialog）
├── composables/       # 可复用组合式逻辑（如 useAppReady）
├── config/            # 常量：TOKEN_KEY、ROUTE_WHITE_LIST、TAB_BAR_LIST、CONTENT_LIST
├── layouts/           # AppLayout.vue —— 唯一的应用外壳布局
├── pages/             # 路由页面，入口为 **/Index.vue；页面私有子组件放在 **/components/
├── router/            # index.ts（路由定义）+ guard.ts（beforeEach 守卫）
├── stores/            # Pinia Store：app、chat、global、home、user
├── styles/            # 全局 CSS、主题变量、重置样式
├── types/             # 公共 TypeScript 类型：common、chat、home、user
└── utils/             # request.ts、storage.ts、performance.ts、format.ts、validate.ts
```

### 请求层

`utils/request.ts` 导出一个单例 `http`（`HttpClient` 实例）。所有 API 调用均须经过它 —— 禁止在页面或组件中直接裸用 `axios`。

- 拦截器处理：鉴权 token 注入（从 `TOKEN_KEY` 存储读取）、全局 loading toast（通过 `showLoading` 配置带并发计数）、统一错误码检查（`code !== 0`），以及错误 toast 提示。
- `api/modules/` 中的接口模块定义带类型的请求/响应签名，调用 `http.get<T>()` / `http.post<T>()`。

### 状态管理（Pinia）

| Store | 用途 | 持久化 |
|-------|---------|-------------|
| `app` | 全局应用标题、全局 loading 遮罩 | 仅内存 |
| `user` | 用户信息、token、`isLogin` 计算属性 | localStorage（token + user） |
| `chat` | 多会话聊天与 AI 模拟回复、置顶/删除 | 通过 `hydrateFromStorage()` 持久化到 localStorage |
| `home` | 健康数据及缓存（3 分钟 TTL）、防重复请求 | localStorage |
| `global` | 横幅轮播、内容列表 | 仅内存（从 config 初始化） |

所有 Store 均使用组合式 API（`defineStore` 配合 setup 函数）。需要持久化的 Store 暴露 `hydrateFromStorage()` 方法，并在需要保存的变更后调用 `persistState()`。需要 HMR 支持的 Store 使用 `acceptHMRUpdate`。

关键模式：
- 解构响应式字段必须使用 `storeToRefs()`，不可直接解构。
- 异步流程必须处理 loading、失败、空态和竞态条件。
- 定时器、事件监听、订阅必须在组件卸载时清理。

### H5/移动端适配

- `postcss-px-to-viewport-8-plugin` 将所有 px 转换为 vw（基准：375px 设计稿宽度）。注意：排除 `node_modules/`。
- 通过 UnoCSS 快捷方式 `safe-bottom` 及各组件自行处理 `safe-area-inset-*`。
- `index.html` 中设置 `viewport-fit=cover`、`user-scalable=no`。
- 防抖/节流通过 `utils/performance.ts`（基于 lodash-es 封装）。

### 样式方案

- Tailwind CSS + UnoCSS（presetUno + presetAttributify）。组件样式默认使用 `scoped`；全局样式仅放在 `src/styles/`。
- `uno.config.ts` 中定义 UnoCSS 快捷方式：`app-card`、`app-title`、`app-subtitle`、`safe-bottom`。
- UnoCSS 配置中通过 CSS 变量主题支持暗色模式。

### 命名规范

- 页面入口：`src/pages/**/Index.vue`
- 页面私有子组件：`src/pages/**/components/*.vue`
- 公共组件：`src/components/**/*.vue`
- 接口模块：`src/api/modules/*.ts`
- Store：`useXxxStore`，位于 `src/stores/*.ts`
- 组合式函数：`useXxx`，位于 `src/composables/*.ts`
- Interface 类型：`I` 前缀（如 `IApiResponse`）
- 联合类型/字面量类型：`T` 前缀（如 `TChatRole`）
- 跨目录导入：使用 `@/` 别名

### 构建与分包

Vite `manualChunks` 分包策略：`vue`（vue + pinia + vue-router）、`vant`、`vendor`（axios + dayjs + lodash-es）。不要新增会将这些基础依赖打回单包的分包配置。

## 详细规则

完整项目规则位于 [AGENTS.md](AGENTS.md)（仓库根目录）和 [public/.trae/rules/projectrule.md](public/.trae/rules/projectrule.md)。规则冲突时，以 projectrule.md 为准。
