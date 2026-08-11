# API Management

## Per-API Switches

Each API path can be controlled independently:

| Default rule | Description |
| --- | --- |
| `/api/config/public` | Player public config |
| `/api/danmu/` | Danmaku read/write (v1 compatible) |
| `/api/danmu/v3/` | Danmaku read/write (DPlayer compatible) |
| `/api/video/map` | Video mapping record |
| `/api/video/resolve` | Video address resolution |
| `/api/subtitle/detect` | Subtitle detection |
| `/api/pow/verify` | PoW verification |

Each rule can set:

- **Enable / disable**: disabled paths return 403
- **RPS limit**: requests per second (sliding window, in-memory)
- **Bandwidth limit**: response bytes per second

## Statistics

- Three time-buckets: 1s (kept 1 day), 60s (kept 30 days), 1 hour (kept 90 days, capped by `retentionDays`)
- Statistics are persisted to disk (60s interval + on exit), survive restarts
- **Live chart**: bucket precision chosen by span (≤1 day: second buckets, ≤30 days: minute buckets, otherwise hour buckets); stacked bars show request distribution per API
- Shows: total requests, total traffic, requests & traffic per path, uptime

## Retention

`retentionDays` (1-90 days) controls how long hour buckets are kept; takes effect immediately after saving.
