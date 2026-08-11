# Danmaku / Videos / Subtitles

## Danmaku List

- Filter by **video ID** or **danmaku content**
- Paginated (50 per page); shows time / video ID / content / color / type (scroll / top / bottom)
- The "video ID" panel on the right is sorted by danmaku count; click to filter quickly

## Video Management

- **Add / update mapping**: enter the video ID and URL, save; optionally link subtitles (Ctrl+click multi-select)
- **Batch actions**: select all → delete selected
- **Quick codes**: generate player embed codes in HTML / Markdown / JS / direct-link formats in one click
- Mappings for the same video URL are recorded automatically (when played)

## Subtitle Management

### Adding Subtitles

- **URL**: enter a subtitle URL (language auto-detected), optionally "download now" (localize)
- **Text content**: paste WEBVTT / SRT content
- **Upload files**: .srt / .vtt / .ass / .ssa / .webvtt, multiple upload supported

### Subtitle Actions

- **Apply / unapply**: apply a subtitle to all video mappings (unapply keeps the library record)
- **Localize**: download a remote subtitle to the server's local storage (5MB limit, private addresses rejected)
- **Search**: filter by name / ID / language

### Player Side

- The player auto-detects same-directory subtitle files (multi-language matching, e.g. `video.zh.vtt`)
- Subtitles in the admin library load directly from the player's subtitle selector
