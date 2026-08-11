# Server Configuration

The config panel is split into several sections; some settings require a restart to take effect.

## PoW Proof-of-Work

- **Enable**: clients must solve a proof-of-work challenge before write requests such as sending danmaku
- **Difficulty**: 1-6, higher means slower client computation and stronger anti-spam

## Rate Limit

- **Window (sec)** and **Limit**: global per-IP request frequency limit (e.g. 60 requests / 60 sec)

## Danmaku Rate Limit

- Max danmaku an IP can send per minute (default 10)

## Danmaku Rendering

- **Per-second limit**: max danmaku rendered per second by the player (default 250)
- **Speed jitter %**: random ±% speed per scrolling danmaku (default 5)

## CDN

- When enabled, player page assets (frontend scripts etc.) load from the CDN domain

## Themes

- **Player theme**: default player theme (10 built-in)
- **Admin theme**: admin panel colors, applies instantly (saved locally)

## Account

- Change password (current + new password)
- Change username (requires current password)

## Admin Entry

- `security.adminPath`: custom admin entry path (e.g. `secret` → `/secret/`)
- `trustProxy`: trust X-Forwarded-For behind a reverse proxy (to get the real client IP)

## Security & Sessions

- Session validity (minutes): login token expiry
