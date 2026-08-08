# Danmaku API

## Fetch

```
GET /api/danmu/?id={vid}
GET /api/danmu/v3/?id={vid}
GET /api/danmu/v3/{vid}
```

v1 response: `{ code: 0, data: [{ id, vid, text, color, type, time, author, date }] }`

v3 (DPlayer-compatible): `{ code: 0, data: [[time, type, color, author, text]] }` where type `0` scroll, `1` top, `2` bottom; color is a decimal value.

## Send

```
POST /api/danmu/
POST /api/danmu/v3/
```

```json
{
  "id": "vid", "text": "hello", "color": 16777215,
  "type": 0, "time": 12.5, "author": "nick"
}
```

### Limits

- Per-IP per-minute cap (default 10)
- Text length cap (configurable)
- Banned words → 403 `{code: 2}`
- Successful sends broadcast the `danmaku:send` event to plugins
