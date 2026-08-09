# Services

Services let plugins cooperate (the framework's Service model). The framework provides built-in services; plugins can **provide** services to others (`provide`) and **consume** them via `inject`.

## Built-in Services

| Service | Description |
| --- | --- |
| `store` | data store (danmaku / videos / subtitles / kv) |
| `model` | dynamic tables (`ctx.model.define`, see [Data Model](/en/plugins/model)) |
| `app` | `version` / `pid` / `uptime()` / `getConfig()` / `saveConfig()` / `restart()` |
| `logger` | leveled logs: `debug/info/warn/error(scope, msg)` + ring buffer |
| `http` | fetch wrapper: `get/post/json` (with timeout) |
| `router` | Express routing (`ctx.router.get/post/use...`) |

Services declared in `manifest.inject` are attached to ctx directly:

```js
apply(ctx, config) {
  ctx.logger.info('hello', 'version ' + ctx.app.version);
}
```

## Providing Services

Plugin A provides; plugin B declares the dependency (插件框架 `ctx.provide`):

```js
// Plugin A: openvideo-plugin-stats
module.exports = {
  name: 'stats',
  provide: ['danmuStats'],
  apply(ctx, config) {
    const state = { count: 0 };
    ctx.on('danmu:send', () => { state.count++; });
    ctx.provide('danmuStats', { count: () => state.count });
  }
};
```

```js
// Plugin B: "openvideoPlugin": { "inject": ["danmuStats"] }
module.exports = {
  apply(ctx, config) {
    setInterval(() => { ctx.danmuStats.count(); }, 60000);
  }
};
```

## Resolution Rules

- Enabling plugin B auto-enables and loads its provider A first (topological order)
- Circular dependencies fail with a clear error
- Missing providers fail the load with a clear reason
- Built-in service names are reserved (`store/model/app/logger/router/http/version...`)

## Runtime Access

```js
const s = ctx.service('danmuStats');
if (s) s.count();
```

## Framework Mapping

| 插件框架 | OpenVideoAPI |
| --- | --- |
| `ctx.provide('name', service)` | same |
| `inject: ['service']` | same (manifest.inject) |
| `ctx.service` access | same |
