# Marketplace

The plugin marketplace is the official plugin index (`plugin-registry.json` at the main repo root). The admin fetches and displays it; **installation goes through npm** (version-selectable).

## Registry Format (v2: versions + dependencies)

```json
{
  "updated": "2026-08-08",
  "plugins": [
    {
      "name": "openvideo-plugin-demo",
      "description": "Official demo plugin",
      "author": "yangyang8002",
      "homepage": "https://github.com/yangyang8002/OpenVideoAPI",
      "tags": ["official", "demo"],
      "versions": ["1.0.0"],
      "dependencies": []
    }
  ]
}
```

| Field | Description |
| --- | --- |
| `name` | npm package name (installed via `npm install`) |
| `versions` | available versions (**descending, first = latest**), selectable in the UI |
| `dependencies` | dependent plugin packages (loaded in topological order) |

## Usage

1. Admin "Plugins" → "Plugin market" → Refresh
2. Pick a version → Install (`npm install <name>@<version>` in background)
3. Enable it in the plugin list

## Configurable Registry URL

`data/config.json`:

```json
{ "plugin": { "registry": "https://your-mirror/plugin-registry.json" } }
```

or env `OPENVIDEO_PLUGIN_REGISTRY`. Defaults to the official repo.

## Submitting a Plugin

1. Publish your plugin as an npm package (follow the [plugin guide](/en/plugins/guide) manifest conventions)
2. Open a PR to the [OpenVideoAPI repo](https://github.com/yangyang8002/OpenVideoAPI) adding an entry to `plugin-registry.json` (with versions & dependencies)

## Hosting Tips

- Plugins **must** be npm packages (local dev: `npm install <local path>` into `plugins/`)
- Name packages `openvideo-plugin-*` for discoverability
- Client assets live under `lib/client/` inside the package and are injected via `/api/plugins/client/*` — **no bundler needed**
