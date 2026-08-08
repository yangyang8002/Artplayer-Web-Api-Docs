# Marketplace

The plugin marketplace is the official plugin index (`plugin-registry.json` at the main repo root). The admin fetches and displays it under "Plugins → Plugin market".

## Usage

1. Open admin "Plugins" → "Plugin market"
2. Click refresh to pull the latest registry (cached 10 minutes)
3. Click "Install" on a plugin (installs via URL)
4. Enable it in the plugin list

## Submitting a Plugin

1. Publish your plugin to GitHub (raw link accessible)
2. Open a PR to the [Artplayer-Web-Api repo](https://github.com/yangyang8002/Artplayer-Web-Api) adding an entry to `plugin-registry.json`:

```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "Plugin description",
  "author": "You",
  "homepage": "https://github.com/you/my-plugin",
  "tags": ["tools", "demo"],
  "url": "https://raw.githubusercontent.com/you/my-plugin/main/my-plugin.js"
}
```

## How It Works

- Fetched from `raw.githubusercontent.com` via `GET /api/admin/plugins/market`
- Installed plugins are marked "Installed"
- Registry updates ship with releases

## Hosting Tips

- **Single-file plugins**: keep them in the repo, use raw links
- **npm plugins**: publish to npm; install by package name
- Any plugin following the [plugin guide](/en/plugins/guide) conventions is recognized
