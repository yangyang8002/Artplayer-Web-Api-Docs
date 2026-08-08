# Plugin Guide

The plugin system is modeled after **Koishi**. A plugin is a function / class / object with `apply`; the framework calls `apply(ctx, config)` on load. `ctx` injects routing, storage, and an event bus.

## Minimal Plugin

```js
// my-plugin.js
module.exports = {
  name: 'my-plugin',
  version: '1.0.0',
  description: 'My first plugin',
  apply(ctx, config) {
    ctx.log('plugin loaded');
  }
}
```

Upload it in the admin "Plugins" tab and enable it.

## Plugin Forms

```js
// 1. function
module.exports = function (ctx, config) { ... }

// 2. class (constructor(ctx, config))
module.exports = class MyPlugin {
  constructor(ctx, config) { ... }
}

// 3. object with apply (recommended: carries meta & schema)
module.exports = {
  name: 'demo', version: '1.0.0', description: '...',
  apply(ctx, config) { ... }
}
```

## Writing & Publishing

### Upload

Single-file `.js` plugins can be uploaded directly.

### GitHub

Publish the plugin file and use its raw URL, e.g. `https://raw.githubusercontent.com/<user>/<repo>/master/<plugin>.js`, then install via "GitHub / URL download". Optionally submit a PR to add your plugin to `plugin-registry.json` (official marketplace).

### npm

Publish the plugin as an npm package (entry exports an apply object/function). Install by package name; version / description / author are read from its package.json automatically.

## Lifecycle

- **Load**: `apply(ctx, config)` runs when enabled (or at startup); hot reload unloads the old instance first
- **Run**: registered routes, events and timers work normally
- **Unload**: `dispose` callbacks run (clean up timers, connections)
- **Hot reload**: changing config / re-enabling restarts the instance; routes of old instances become inactive automatically

## Built-in Events

```js
ctx.on('danmaku:send', (danmu) => {
  // danmu = { vid, text, color, type, time, author }
});
```

## Safety

- Plugins run inside the server process with full permissions; never install untrusted plugins
- Plugin errors never crash the service (marked as load-failed with the reason)
- Event handler exceptions are caught and logged without affecting other plugins

## Example

The repo ships `plugins/hello-world.js`, demonstrating meta, schema, routing, event subscription and timers. See [ctx API](/en/plugins/ctx) and [Config Schema](/en/plugins/schema).
