# Plugins (Admin)

The plugin system is designed in the **插件框架 style**. The admin "Plugins" tab provides full management.

## Install

| Method | Description |
| --- | --- |
| Upload .js file | Direct upload (≤ 1MB) |
| GitHub / URL | Install from a raw URL |
| npm package | `npm install` into `plugins/node_modules/` |

Plugins are **disabled** after install; enable them manually. State persists in `data/plugins.json`.

::: danger Security
Plugins run inside the server process with full permissions. Only install from trusted sources. Marketplace entries are hosted in the main repo's `plugin-registry.json`.
:::

## Plugin Cards

Each plugin shows: name, version, author, description, homepage (from module meta or npm package.json), status (running / stopped / load-failed with error), and actions: enable/disable (hot reload), config (auto-generated form), update, uninstall.

## Config Forms

Plugins export a `schema` array; the admin renders a form automatically. Supported types: `string` / `number` / `boolean` / `select` / `textarea`. Saving hot-reloads the plugin.

## Marketplace

The "Plugin market" panel fetches the official registry (`plugin-registry.json`) and offers one-click install. Installed plugins are marked.

## Updates

npm / URL plugins can be updated from their original source (config & enabled state preserved). File-based plugins require manual re-upload.

See [Plugin Guide](/en/plugins/guide) to develop plugins.
