# Publishing & Marketplace

After developing in the [Dev environment](/en/guide/dev) and verifying, publish your plugin to npm and register it on the official marketplace (Koishi's plugin market).

## Publish to npm

```bash
cd plugins/openvideo-plugin-hello
npm publish
```

- Name it `openvideo-plugin-*` for discoverability
- `package.json` must include `main` and the `openvideoPlugin` manifest
- Client assets ship inside the package (`lib/client/`), no bundling needed
- Not published yet? Install via `npm install <local path>` or `npm install <github repo>`

## Marketplace

The registry is `plugin-registry.json` at the repo root (v2: versions + dependencies):

```json
{
  "updated": "2026-08-08",
  "plugins": [
    {
      "name": "openvideo-plugin-hello",
      "description": "My first plugin",
      "author": "your-name",
      "homepage": "https://github.com/you/openvideo-plugin-hello",
      "tags": ["tools"],
      "versions": ["1.0.0", "0.9.0"],
      "dependencies": []
    }
  ]
}
```

| Field | Notes |
| --- | --- |
| `name` | npm package name (installed via `npm install <name>@<version>`) |
| `versions` | available versions (**descending, first = latest**) |
| `dependencies` | dependent plugin packages (loaded in topological order) |

Submit by opening a PR to the [OpenVideoAPI repo](https://github.com/yangyang8002/OpenVideoAPI) adding an entry to `plugin-registry.json`.

## Configurable Sources

Admin "Dependencies → Update & install config":

- **npm mirror**: `plugin.npmRegistry` (or `OPENVIDEO_NPM_REGISTRY` env) — applies to market installs / plugin updates / dependency updates / app updates
- **Plugin registry URL**: `plugin.registry` (or `OPENVIDEO_PLUGIN_REGISTRY`) — `https://` or local `file://` (Dev environment)

## Koishi Mapping

| Koishi | OpenVideoAPI |
| --- | --- |
| `koishi-plugin-*` naming | `openvideo-plugin-*` naming |
| Koishi marketplace | official `plugin-registry.json` (versions + dependencies) |
| `koishi add` | admin "Plugins → Market" |
| market review | PR to registry |
