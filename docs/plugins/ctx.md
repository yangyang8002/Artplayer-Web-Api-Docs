# ctx API 参考

`ctx` 是插件上下文对象，在 `apply(ctx, config)` 时注入。

## ctx.name

当前插件名称。

## ctx.config

当前插件配置（来自后台配置表单或 `data/plugins.json`，热重载时更新）。

## ctx.router

Express app 代理。可注册路由与中间件：

```js
ctx.router.get('/api/my-plugin', (req, res) => {
  res.json({ code: 0, data: 'hello' });
});

ctx.router.post('/api/my-plugin/hook', (req, res) => { ... });
ctx.router.use('/api/my-plugin', middleware);
```

支持：`get` / `post` / `put` / `delete` / `patch` / `use`。

::: tip
路由包装了实例校验：插件热重载后，旧实例注册的路由自动失效（不再响应）。
:::

## ctx.store / ctx.model

数据存储对象，提供全部数据操作（与后台 API 同源）：

```js
const danmuList = await ctx.store.danmuAll();        // 全部弹幕
await ctx.store.danmuAdd({ id, vid, text, ... });    // 新增弹幕
const videos = await ctx.store.videosAll();          // 视频映射
await ctx.store.videoSet(vid, url);
const subs = await ctx.store.subtitleAll();
await ctx.store.kvGet(key) / ctx.store.kvSet(key, value);
await ctx.store.bannedAll(includeSubscriptions);
```

## ctx.getServerConfig()

读取服务器配置（`data/config.json`）：

```js
const cfg = ctx.getServerConfig();
if (cfg.pow.enabled) { ... }
```

## ctx.http

fetch 封装（带超时与 JSON 解析）：

```js
const res = await ctx.http.get('https://example.com/api');   // Response
const data = await ctx.http.json('https://example.com/api'); // 已解析 JSON（失败返回 null）
const r2 = await ctx.http.post('https://example.com/hook', { hello: 1 });
```

## ctx.log(msg)

带插件名前缀的日志输出：

```js
ctx.log('处理完成');  // → [插件] [my-plugin] 处理完成
```

## ctx.on(event, fn)

事件订阅，返回原函数（便于链式）：

```js
// 订阅内置事件
ctx.on('danmu:send', (danmu) => { ... });

// 订阅自定义事件（其他插件 ctx.emit 广播）
ctx.on('my:event', (payload) => { ... });

// 卸载清理回调（dispose）
ctx.on('dispose', () => {
  clearInterval(timer);
});
```

卸载插件时，该插件注册的全部事件监听自动清理。

## ctx.emit(event, ...args)

同步广播事件给所有插件：

```js
ctx.emit('my:event', { hello: 1 });
```

## ctx.plugin(plugin, config)

嵌套加载子插件（同步调用 `apply(ctx, cfg)`）：

```js
ctx.plugin(require('./sub-plugin'), { interval: 5 });
```

## ctx.version

服务端版本号字符串（如 `26.8.11`）。
