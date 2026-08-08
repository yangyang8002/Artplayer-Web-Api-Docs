# API Reference

All endpoints return JSON `{ code, msg, data }`; `code === 0` means success.

## Public Endpoints

| Method | Path | Description |
| --- | --- | --- |
| GET | `/api/config/public` | Player public config |
| GET | `/api/danmu/?id={vid}` | Fetch danmaku (v1 format) |
| POST | `/api/danmu/` | Send danmaku (v1) |
| GET | `/api/danmu/v3/?id={vid}` | Fetch danmaku (DPlayer array format) |
| GET | `/api/danmu/v3/{vid}` | Same (path param) |
| POST | `/api/danmu/v3/` | Send danmaku (v3) |
| GET | `/api/video/resolve?v={vid}` | Resolve video URL by vid |
| POST | `/api/video/map` | Record mapping `{vid, url}` |
| GET | `/api/subtitle/detect?url=` | Detect sibling subtitles |
| GET | `/api/subtitle/by-id?id=` | Load subtitle content |
| POST | `/api/pow/verify` | PoW verification |

## Admin Endpoints

All `/api/admin/*` need `Authorization: Bearer <token>`:

```
POST /api/admin/login  { username, password } → { data: { token, firstRun } }
```

See [Admin API](/en/api/admin) for the full list.

## Conventions

- Write endpoints have per-IP rate limits
- During data migration, write endpoints return 503
- Danmaku length limits apply per config
- When PoW is enabled, write requests need a PoW credential first
