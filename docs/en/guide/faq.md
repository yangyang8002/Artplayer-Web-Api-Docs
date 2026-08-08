# FAQ

## How do I change the default password?

Log in, go to "Server Config" → account section. The first-run wizard also guides you.

## I forgot my custom admin path

The admin path is `security.adminPath` in `data/config.json` (default `/admin/`).

## What is a vid?

The video ID that groups danmaku. A random 8-char ID is assigned on first play of a URL and reused afterwards. Manage mappings in the Videos tab.

## Danmaku being rejected?

- Contains banned word → check Banned Words tab
- Danmaku frequency limit hit (per-minute cap) → Server Config
- Global rate limit hit → Server Config

## Can I switch databases?

Yes. "Database" tab supports JSON / SQLite / MySQL / MariaDB / PostgreSQL / MongoDB. Switching migrates all data automatically without restart.

## How to backup?

"Backups" tab: manual / scheduled backups (data + config) to local or cloud (FTP/FTPS/SFTP/WebDAV/OpenList). Batch restore is supported.

## Update broke the service?

- Check `data/update.log` and `out.log`
- `data/` is backed up before updates; revert code with `git checkout -- .` (data is unaffected)

## Are plugins safe?

Plugins run inside the server process and can execute arbitrary code. Only install from trusted sources. Official marketplace entries come from `plugin-registry.json` in the main repo.

## Video playback cross-origin issues?

The player loads videos directly from the browser. Resources without CORS headers may limit features like screenshots. Use the provided nginx config for same-origin proxying.

## Supported formats?

mp4 (native), m3u8 (hls.js), flv (flv.js).

## Issues / Contribution

- Issues: https://github.com/yangyang8002/OpenVideoAPI/issues
- Docs repo: https://github.com/yangyang8002/OpenVideoAPI-Docs
