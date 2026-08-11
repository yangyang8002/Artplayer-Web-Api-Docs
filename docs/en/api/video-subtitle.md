# Video / Subtitle API

## Video Resolution

```
GET /api/video/resolve?v={vid}
```

Returns the playable URL for a video ID:

```json
{ "code": 0, "data": { "vid": "a1b2c3d4", "url": "https://..." } }
```

## Record Video Mapping

```
POST /api/video/map
{ "vid": "8-char id", "url": "https://..." }
```

- Video ID: 4-32 alphanumeric chars; URL supports http(s) and relative paths
- Called automatically on first playback
- Mapping table capacity is capped by config (write-amplification protection)

## Subtitle Detection

```
GET /api/subtitle/detect?url={video URL}
```

Auto-detects same-directory subtitle files (.srt / .vtt / .ass / .ssa / .webvtt) and returns a multi-language candidate list (e.g. `video.zh.vtt`, `video.en.vtt`).

For `/d/` links of a configured OpenList (AList-compatible) instance, it also lists same-folder subtitles on the cloud via the instance API.

## OpenList Direct Link Resolution

```
GET /api/video/resolve-link?url={OpenList link}
```

When the URL matches a configured OpenList instance, it calls the instance API (`/api/fs/get`) to return the cloud direct link (secondary address):

```json
{ "code": 0, "data": { "url": "https://cdn.../video.mp4?sign=...", "matched": true, "original": "https://instance/d/video.mp4" } }
```

- Unmatched instances return `matched: false` with the original link (played as-is)
- On failure it returns 502 and the player falls back to the original link
- The player plays the direct link, but **vid and subtitle detection stay keyed to the original link** (changing direct-link signatures never affect danmaku/subtitle associations)

## Subtitle Content

```
GET /api/subtitle/by-id?id={subtitle ID}
```

Returns the plain-text subtitle content (text/vtt) for the player to load.
