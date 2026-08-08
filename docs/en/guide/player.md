# Player

```
http://localhost:1919/player/?url=https://example.com/video.mp4
```

## URL Parameters

| Param | Description |
| --- | --- |
| `url` | Video URL (mp4 / m3u8 / flv) |
| `vid` | Fixed video ID (auto-generated from URL otherwise) |
| `theme` | Player theme override (e.g. `theme=bilibili`) |
| `lang` | Player language (zh / zhHant / wyw / en / ja / fr) |

## Danmaku

- Send scrolling / top / bottom danmaku with custom colors
- Settings: opacity, density, speed, bottom margin, stacking
- Danmaku are shared by all viewers of the same vid
- Admin maintains a banned-word list; matching danmaku are rejected
- DPlayer compatible: `/api/danmu/v3/?id={vid}`

## Subtitles

- Auto-detection of sibling subtitle files (srt/vtt/ass/ssa) with multi-language matching (e.g. `video.zh.vtt`)
- The admin subtitle library can be applied to videos; remote subtitles can be localized (downloaded) to the server

## Quick Codes

In the admin "Videos" tab, generate embed codes (HTML / Markdown / JS / direct link) for any video.

## Features

- Screenshot (PNG download; may fail on cross-origin videos)
- Picture-in-Picture, web fullscreen, native fullscreen
- Decode capability statistics (hardware/software)
