# 插件指南

插件系统参考 **Koishi** 设计：插件是一个 **npm 包**，包内 `main` 导出 `apply(ctx, config)`（函数 / 类 / 带 `apply` 的对象），加载时框架调用 `apply(ctx, config)`。`ctx` 注入服务、路由、数据模型与事件总线。

## 最小插件（npm 包）

```
my-plugin/
├── package.json
└── lib/
    └── index.js
```

```json
{
  "name": "openvideo-plugin-my-plugin",
  "version": "1.0.0",
  "main": "lib/index.js",
  "openvideoPlugin": {
    "name": "my-plugin",
    "description": "我的第一个插件",
    "inject": ["store", "app", "logger"]
  }
}
```

```js
module.exports = {
  apply(ctx, config) {
    ctx.logger.info('my-plugin', '插件已加载，服务端 ' + ctx.version);
  }
}
```

后台「插件管理」输入 npm 包名安装（可指定版本），启用后生效。

## 插件形式

```js
// 1. 函数
module.exports = function (ctx, config) { ... }

// 2. 类（constructor(ctx, config)）
module.exports = class MyPlugin {
  constructor(ctx, config) { ... }
}

// 3. 带 apply 的对象（推荐：配合 manifest 声明能力）
module.exports = {
  name: 'demo', version: '1.0.0', description: '...',
  apply(ctx, config) { ... }
}
```

## 包 manifest（package.json 的 openvideoPlugin 字段）

```json
{
  "openvideoPlugin": {
    "name": "demo",
    "description": "展示用描述",
    "inject": ["store", "model", "app", "logger", "http"],
    "provide": ["stats"],
    "schema": [ ... ],
    "client": {
      "admin": { "styles": [], "scripts": ["lib/client/admin/debug.js"], "tabs": [{ "id": "debug", "title": "调试" }] },
      "player": { "styles": [], "scripts": ["lib/client/player/overlay.js"], "replaces": false }
    }
  }
}
```

| 字段 | 说明 |
| --- | --- |
| `name` / `description` | 展示用元数据（缺省取包名/描述） |
| `inject` | 依赖的服务名数组，加载时自动按拓扑序先加载提供者 |
| `provide` | 本插件提供的服务名（配合 `ctx.provide(name, svc)` 注册） |
| `schema` | 配置表单定义（见 [配置 Schema](/plugins/schema)） |
| `client.admin` | 后台前端扩展：样式 / 脚本 / tab 列表 |
| `client.player` | 播放器前端扩展：样式 / 脚本 / 是否替换播放器 |

## 编写并发布

1. 按上述结构创建 npm 包，本地 `npm pack` 验证
2. 发布到 npm registry（或先 `npm install <本地路径>` 测试）
3. 后台安装：`npm 包名` 或 `npm 包名@版本`
4. 提交到官方市场（可选）：向主仓库提交 PR，在 `plugin-registry.json` 登记版本与依赖

## 生命周期

- **加载**：启用（或服务启动）时按依赖拓扑序调用 `apply(ctx, config)`；热重载先卸载旧实例
- **运行**：注册的路由、事件、定时器正常工作；`ctx.provide` 注册的服务对其他插件可用
- **卸载**：触发 `dispose` 回调（清理定时器、连接），事件监听自动清理，旧路由自动失效
- **重启**：`ctx.app.restart()` 优雅重启（先广播 `before:restart`，新进程等待端口释放）

## 内置事件

```js
ctx.on('danmaku:send', (danmu) => { /* { vid, text, color, type, time, author } */ });
ctx.on('ready', () => { /* 全部启用插件加载完成 */ });
ctx.on('before:restart', () => { /* 重启前清理 */ });
```

## 前端扩展

- **后台 tab**：`OpenVideoAdmin.registerTab({ id, title, mount(el) })`，自动出现在侧边栏（`OpenVideoAdmin.api(url)` 自动携带管理员鉴权）
- **播放器替换**：`OpenVideoPlayer.replace({ name, init(ctx) })` 完全接管播放区渲染
- **播放器钩子**：`OpenVideoPlayer.onReady(fn)`、`ctx.on('video:load', fn)`

详见 [ctx API](/plugins/ctx) 与 [配置 Schema](/plugins/schema)。

## 安全约定

- 插件运行在服务进程内，**拥有与服务器相同的权限**，请勿安装来源不明的插件
- 插件异常不会导致服务崩溃（加载失败标记为 error 并显示原因）
- 事件处理器异常被捕获并记录，不影响其他插件

## 示例

仓库内置 `plugins/openvideo-plugin-demo`：演示服务提供、动态表、事件总线、日志、后台调试 tab 与播放器浮层。
