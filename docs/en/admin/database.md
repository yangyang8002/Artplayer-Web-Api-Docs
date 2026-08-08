# Database

## Supported Storages

| Type | Notes |
| --- | --- |
| JSON | default, zero-dependency, files under `data/` |
| SQLite | local file (`data/app.db`) |
| MySQL / MariaDB | remote database |
| PostgreSQL | remote database |
| MongoDB | document database |

## Switch & Migrate

1. Pick the target type and fill connection info
2. Click **Test connection**
3. Click **Switch & migrate**: all data (danmaku, videos, banned words, accounts, bans, login records, stats, subtitles) is migrated automatically, **no restart needed**

::: tip
- Writes are paused (migration lock) during the switch
- If the target is unreachable, the switch fails and the current storage is untouched
:::

## Data Browser

Read-only browsing of whitelisted tables (danmu / videos / banned_words / accounts / security / login_logs / login_fails / subtitles / kv) with pagination and search.

## Export

One-click JSON export of all data.
