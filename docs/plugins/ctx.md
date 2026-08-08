# ctx API 参考

`ctx` 是插件上下文对象，在 `apply(ctx, config)` 时注入。

## 基础

| 属性 | 说明 |
| --- | --- |
| `ctx.name` | 插件显示名 |
| `ctx.config` | 当前插件配置（后台表单保存后热重载更新） |
| `ctx.version` | 服务端版本号（如 `26.8.11`） |
| `ctx.log(msg)` | 带插件名前缀的日志输出 |

## ctx.router

Express app 代理，注册路由与中间件：

```js
ctx.router.get('/api/my-plugin', (req, res) => {
  res.json({ code: 0, data: 'hello' });
});
ctx.router.post('/api/my-plugin/hook', (req, res) => { ... });
ctx.router.use('/api/my-plugin', middleware);
```

支持 `get` / `post` / `put` / `delete` / `patch` / `use`。热重载后旧实例路由自动失效。

## ctx.store / ctx.model

- `ctx.store`：数据存储（弹幕 / 视频 / 字幕 / kv 等，与后台同源）
- `ctx.model`：**动态表**（Koishi 风格 `ctx.model.define`）：

```js
const notes = ctx.model.define('my_notes', {
  primary: 'id',
  fields: { id: { type: 'string' }, text: { type: 'string' }, createdAt: { type: 'number' } }
});
await notes.create({ text: 'hello' });       // 无主键自动生成 id
await notes.get(id);
await notes.update(id, { text: 'new' });
await notes.list({ page: 1, limit: 20, search: 'key', searchKey: 'text' });
await notes.remove(id);
await notes.count(); await notes.all(); await notes.clear();
```

表数据随主存储（JSON/SQLite/MySQL/PG/MongoDB）**切换自动迁移**。

## ctx.app（服务控制）

```js
ctx.app.version            // 服务端版本
ctx.app.pid                // 进程 PID
ctx.app.platform           // 平台（如 win32 x64）
ctx.app.uptime()           // 运行秒数
ctx.app.getConfig()        // 读取服务器配置
ctx.app.saveConfig(patch)  // 保存配置（合并 + 应用 trustProxy 等）
ctx.app.restart({ delay: 1500 })  // 优雅重启（广播 before:restart → 新进程等待端口 → 退出）
```

## ctx.logger（分级日志）

```js
ctx.logger.debug('scope', 'msg');
ctx.logger.info('scope', 'msg');
ctx.logger.warn('scope', 'msg');
ctx.logger.error('scope', 'msg');
ctx.logger.tail(200);      // 读取环形缓冲（后台 /api/admin/plugins/logs 可见）
```

日志是调试工具的数据源：后台「插件管理 → 插件日志」接口 `GET /api/admin/plugins/logs`。

## ctx.http

fetch 封装（带超时与 JSON 解析）：

```js
const res = await ctx.http.get('https://example.com/api');
const data = await ctx.http.json('https://example.com/api');
const r2 = await ctx.http.post('https://example.com/hook', { hello: 1 });
```

## ctx.on / ctx.emit（事件总线）

```js
ctx.on('danmaku:send', (danmu) => { ... });   // 内置事件
ctx.on('ready', () => { ... });
ctx.on('before:restart', () => { ... });
ctx.on('dispose', () => { clearInterval(timer); });   // 卸载清理
ctx.on('my:event', (payload) => { ... });      // 自定义事件
ctx.emit('my:event', { hello: 1 });            // 同步广播
```

卸载时自动清理该插件注册的全部事件监听。

## ctx.provide / ctx.service（服务层）

```js
// 提供服务（其他插件可在 manifest.inject 声明依赖）
ctx.provide('stats', { get: () => ({ ... }) });

// 获取服务
const stats = ctx.service('stats');
```

- 内置服务：`store` / `model` / `app` / `logger` / `http` / `router`
- manifest 中 `inject` 声明的服务会**直接挂到 ctx 上**：`ctx.app`、`ctx.logger`、`ctx.stats`...
- 依赖提供者按拓扑序自动先加载；服务缺失时加载失败并给出明确错误

## ctx.plugin

嵌套加载子插件（同步调用 `apply(ctx, cfg)`）：

```js
ctx.plugin(require('./sub-plugin'), { interval: 5 });
```
