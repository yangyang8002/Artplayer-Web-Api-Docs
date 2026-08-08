# Quick Start

<span class="badge">v26.8.11</span><span class="badge">Node ≥ 18</span><span class="badge">MIT</span>

ArtPlayer Web API is a self-hosted, zero-build danmaku video player with a web admin panel: PoW firewall, multi-database, plugin system, i18n and themes.

## Option 1: npm

```bash
npm install -g artplayer-web-api
artplayer-web-api

# or from source
git clone https://github.com/yangyang8002/OpenVideoAPI.git
cd Artplayer-Web-Api
npm install
npm start
```

The service listens on `http://localhost:1919` by default.

## Option 2: Docker

```bash
docker pull yangyang8002/artplayer-web-api:latest
docker run -d -p 1919:1919 -v ./data:/app/data yangyang8002/artplayer-web-api:latest
```

See [Docker](/en/guide/docker) for details.

## First Run

1. Open `http://localhost:1919/admin/` and log in with `admin / admin123`
2. The **first-run wizard** guides you through: UI language, timezone, database type, new password and admin path
3. Re-login with the new password

::: warning
Change the default password immediately after first login.
:::

## Player URL

```
http://localhost:1919/player/?url=VIDEO_URL
```

- mp4 / m3u8 / flv direct links
- A unique 8-char video ID (vid) is generated on first play; danmaku are stored per vid
- DPlayer-compatible endpoint: `/api/danmu/v3/?id={vid}`

## Project Layout

```
server.js             main server (single file, no build)
lib/                  core modules (store / cloud / plugin)
public/               frontend (admin.html, player.html, i18n.js)
theme/                theme system
plugins/              plugin directory
data/                 data (config.json, danmu.json, videos.json ...)
update.js             standalone updater
update.xml            sha256 manifest
```

## Next Steps

- [Player](/en/guide/player)
- [Admin Overview](/en/admin/overview)
- [Plugin Development](/en/plugins/guide)
- [API Reference](/en/api/reference)
