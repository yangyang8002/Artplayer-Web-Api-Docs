# Plugin Guide

The plugin system is modeled after **Koishi**. A plugin is an **npm package** whose `main` exports `apply(ctx, config)` (function / class / object with apply); the framework calls it on load. `ctx` injects services, routing, data models and an event bus.

## Minimal Plugin (npm package)

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
    "description": "My first plugin",
    "inject": ["store", "app", "logger"]
  }
}
```

```js
module.exports = {
  apply(ctx, config) {
    ctx.logger.info('my-plugin', 'loaded on ' + ctx.version);
  }
}
```

Install by npm package name in the admin "Plugins" tab (optional `@version`), then enable.

## Plugin Forms

```js
// 1. function
module.exports = function (ctx, config) { ... }

// 2. class (constructor(ctx, config))
module.exports = class MyPlugin {
  constructor(ctx, config) { ... }
}

// 3. object with apply (recommended with manifest)
module.exports = {
  name: 'demo', version: '1.0.0', description: '...',
  apply(ctx, config) { ... }
}
```

## Manifest (openvideoPlugin field)

```json
{
  "openvideoPlugin": {
    "name": "demo",
    "description": "...",
    "inject": ["store", "model", "app", "logger", "http"],
    "provide": ["stats"],
    "schema": [ ... ],
    "client": {
      "admin": { "styles": [], "scripts": ["lib/client/admin/debug.js"], "tabs": [{ "id": "debug", "title": "Debug" }] },
      "player": { "styles": [], "scripts": ["lib/client/player/overlay.js"], "replaces": false }
    }
  }
}
```

| Field | Description |
| --- | --- |
| `name` / `description` | display metadata |
| `inject` | service dependencies; providers load first (topological order) |
| `provide` | services this plugin provides (via `ctx.provide(name, svc)`) |
| `schema` | config form definition (see [Config Schema](/en/plugins/schema)) |
| `client.admin` | admin client extensions: styles / scripts / tabs |
| `client.player` | player client extensions: styles / scripts / replaces |

## Writing & Publishing

1. Create the npm package, verify with `npm pack`
2. Publish to npm (or test with `npm install <local path>`)
3. Install in the admin panel: `name` or `name@version`
4. Optionally submit a PR to add it to `plugin-registry.json` (marketplace with versions & dependencies)

## Lifecycle

- **Load**: `apply(ctx, config)` runs in dependency order when enabled (or at startup); hot reload unloads the old instance first
- **Run**: routes, events and timers work; `ctx.provide` makes services available to other plugins
- **Unload**: `dispose` callbacks run; listeners and routes of old instances are cleaned up automatically
- **Restart**: `ctx.app.restart()` gracefully restarts (broadcasts `before:restart`, the new process waits for the port)

## Built-in Events

```js
ctx.on('danmaku:send', (danmu) => { /* { vid, text, color, type, time, author } */ });
ctx.on('ready', () => { /* all enabled plugins loaded */ });
ctx.on('before:restart', () => { /* cleanup before restart */ });
```

## Client Extensions

- **Admin tabs**: `OpenVideoAdmin.registerTab({ id, title, mount(el) })`; `OpenVideoAdmin.api(url)` carries admin auth automatically
- **Player replacement**: `OpenVideoPlayer.replace({ name, init(ctx) })` takes over the player area entirely
- **Player hooks**: `OpenVideoPlayer.onReady(fn)`, `ctx.on('video:load', fn)`

See [ctx API](/en/plugins/ctx) and [Config Schema](/en/plugins/schema).

## Safety

- Plugins run inside the server process with full permissions; never install untrusted packages
- Plugin errors never crash the service (marked as load-failed with the reason)
- Event handler exceptions are caught and logged

## Example

The repo ships `plugins/openvideo-plugin-demo`: services, dynamic tables, events, leveled logging, an admin debug tab and a player overlay.
