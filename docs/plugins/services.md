# 服务层

服务是插件间协作的桥梁（对应 Koishi 的 Service）。框架内置一组服务，插件可以**提供服务给其他插件**（`provide`），也可以**声明依赖消费服务**（`inject`）。

## 内置服务

| 服务 | 说明 |
| --- | --- |
| `store` | 数据存储（弹幕 / 视频 / 字幕 / kv，与后台同源） |
| `model` | 动态表模型（`ctx.model.define`，见 [数据模型](/plugins/model)） |
| `app` | 服务控制：`version` / `pid` / `uptime()` / `getConfig()` / `saveConfig()` / `restart()` |
| `logger` | 分级日志：`debug/info/warn/error(scope, msg)` + 环形缓冲 |
| `http` | fetch 封装：`get/post/json`（带超时） |
| `router` | Express 路由（`ctx.router.get/post/use...`） |

在 `manifest.inject` 中声明的服务会**直接挂到 ctx 上**：

```json
{ "inject": ["store", "model", "app", "logger", "http"] }
```

```js
apply(ctx, config) {
  ctx.logger.info('hello', '版本 ' + ctx.app.version);
  ctx.app.uptime();
}
```

## 提供服务

插件 A 注册服务，插件 B 声明依赖即可使用（对应 Koishi 的 `ctx.provide`）：

```js
// 插件 A：openvideo-plugin-stats
module.exports = {
  name: 'stats',
  provide: ['danmuStats'],          // manifest 声明
  apply(ctx, config) {
    const state = { count: 0 };
    ctx.on('danmu:send', () => { state.count++; });
    ctx.provide('danmuStats', {
      count: () => state.count
    });
  }
};
```

```js
// 插件 B：声明依赖
// package.json: "openvideoPlugin": { "inject": ["danmuStats"] }
module.exports = {
  apply(ctx, config) {
    setInterval(() => {
      ctx.danmuStats.count();       // 直接可用
    }, 60000);
  }
};
```

## 依赖解析规则

- 启用插件 B 时，自动**按拓扑序先启用并加载**提供 `danmuStats` 的插件 A
- 循环依赖 → 明确报错
- 依赖服务不存在（提供者未安装）→ 加载失败并给出清晰原因
- 内置服务名不可被插件覆写（`store/model/app/logger/router/http/version...` 为保留名）

## 运行时获取

未在 `inject` 中声明的服务，可随时通过 `ctx.service(name)` 获取：

```js
const s = ctx.service('danmuStats');
if (s) s.count();
```

## 与 Koishi 的对应

| Koishi | OpenVideoAPI |
| --- | --- |
| `ctx.provide('name', service)` | 同 |
| `inject: ['service']` | 同（manifest.inject） |
| `ctx.service` 访问 | 同 |
| Service 生命周期（dispose 清理） | `ctx.on('dispose', ...)` 手动清理 |
