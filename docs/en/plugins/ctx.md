# ctx API

`ctx` is the plugin context, injected into `apply(ctx, config)`.

## ctx.name

Current plugin name.

## ctx.config

Current plugin config (from the config form or `data/plugins.json`; updated on hot reload).

## ctx.router

Express app proxy. Register routes and middleware:

```js
ctx.router.get('/api/my-plugin', (req, res) => {
  res.json({ code: 0, data: 'hello' });
});
ctx.router.post('/api/my-plugin/hook', (req, res) => { ... });
ctx.router.use('/api/my-plugin', middleware);
```

Supports `get` / `post` / `put` / `delete` / `patch` / `use`. Routes registered by hot-reloaded old instances stop responding automatically.

## ctx.store / ctx.model

The data store (same source as the admin API):

```js
const danmuList = await ctx.store.danmuAll();
await ctx.store.danmuAdd({ id, vid, text, ... });
const videos = await ctx.store.videosAll();
await ctx.store.videoSet(vid, url);
const subs = await ctx.store.subtitleAll();
await ctx.store.kvGet(key) / ctx.store.kvSet(key, value);
```

## ctx.getServerConfig()

Reads the server config (`data/config.json`).

## ctx.http

fetch wrapper with timeouts and JSON parsing:

```js
const res = await ctx.http.get('https://example.com/api');
const data = await ctx.http.json('https://example.com/api');
const r2 = await ctx.http.post('https://example.com/hook', { hello: 1 });
```

## ctx.log(msg)

Logs with plugin name prefix: `[plugin] [my-plugin] ...`.

## ctx.on(event, fn)

Subscribe to events; returns the function:

```js
ctx.on('danmaku:send', (danmu) => { ... });   // built-in
ctx.on('my:event', (payload) => { ... });      // custom (via ctx.emit)
ctx.on('dispose', () => { clearInterval(timer); });
```

All listeners registered by a plugin are cleaned up on unload.

## ctx.emit(event, ...args)

Synchronously broadcasts an event to all plugins:

```js
ctx.emit('my:event', { hello: 1 });
```

## ctx.plugin(plugin, config)

Loads a nested plugin synchronously:

```js
ctx.plugin(require('./sub-plugin'), { interval: 5 });
```

## ctx.version

Server version string (e.g. `26.8.11`).
