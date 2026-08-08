# ctx API

`ctx` is the plugin context injected into `apply(ctx, config)`.

## Basics

| Property | Description |
| --- | --- |
| `ctx.name` | plugin display name |
| `ctx.config` | current plugin config (updated on hot reload) |
| `ctx.version` | server version (e.g. `26.8.11`) |
| `ctx.log(msg)` | log with plugin name prefix |

## ctx.router

Express app proxy:

```js
ctx.router.get('/api/my-plugin', (req, res) => res.json({ code: 0, data: 'hello' }));
ctx.router.post('/api/my-plugin/hook', (req, res) => { ... });
ctx.router.use('/api/my-plugin', middleware);
```

Supports `get` / `post` / `put` / `delete` / `patch` / `use`. Routes of hot-reloaded old instances stop responding.

## ctx.store / ctx.model

- `ctx.store`: data store (danmaku / videos / subtitles / kv ...)
- `ctx.model`: **dynamic tables** (Koishi-style `ctx.model.define`):

```js
const notes = ctx.model.define('my_notes', {
  primary: 'id',
  fields: { id: { type: 'string' }, text: { type: 'string' }, createdAt: { type: 'number' } }
});
await notes.create({ text: 'hello' });
await notes.get(id);
await notes.update(id, { text: 'new' });
await notes.list({ page: 1, limit: 20, search: 'key', searchKey: 'text' });
await notes.remove(id);
await notes.count(); await notes.all(); await notes.clear();
```

Table data migrates automatically with the main storage (JSON/SQLite/MySQL/PG/MongoDB).

## ctx.app (service control)

```js
ctx.app.version            // server version
ctx.app.pid                // process PID
ctx.app.platform           // platform
ctx.app.uptime()           // uptime in seconds
ctx.app.getConfig()        // read server config
ctx.app.saveConfig(patch)  // save config (merged)
ctx.app.restart({ delay: 1500 })  // graceful restart
```

## ctx.logger (leveled logs)

```js
ctx.logger.debug('scope', 'msg');
ctx.logger.info('scope', 'msg');
ctx.logger.warn('scope', 'msg');
ctx.logger.error('scope', 'msg');
ctx.logger.tail(200);      // ring buffer (visible via GET /api/admin/plugins/logs)
```

## ctx.http

fetch wrapper with timeouts:

```js
const res = await ctx.http.get('https://example.com/api');
const data = await ctx.http.json('https://example.com/api');
const r2 = await ctx.http.post('https://example.com/hook', { hello: 1 });
```

## ctx.on / ctx.emit (events)

```js
ctx.on('danmaku:send', (danmu) => { ... });
ctx.on('ready', () => { ... });
ctx.on('before:restart', () => { ... });
ctx.on('dispose', () => { clearInterval(timer); });
ctx.on('my:event', (payload) => { ... });
ctx.emit('my:event', { hello: 1 });
```

All listeners registered by a plugin are cleaned up on unload.

## ctx.provide / ctx.service (service layer)

```js
ctx.provide('stats', { get: () => ({ ... }) });   // provide a service
const stats = ctx.service('stats');                // consume a service
```

- Built-in services: `store` / `model` / `app` / `logger` / `http` / `router`
- Services declared in `inject` are attached to ctx directly (`ctx.app`, `ctx.logger`, `ctx.stats`...)
- Providers load in topological order; missing services fail with a clear error

## ctx.plugin

Loads a nested plugin synchronously:

```js
ctx.plugin(require('./sub-plugin'), { interval: 5 });
```
