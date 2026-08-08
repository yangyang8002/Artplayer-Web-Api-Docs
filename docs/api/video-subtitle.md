# 视频 / 字幕 API

## 视频解析

```
GET /api/video/resolve?v={vid}
```

返回视频码对应的播放地址：

```json
{ "code": 0, "data": { "vid": "a1b2c3d4", "url": "https://..." } }
```

## 记录视频映射

```
POST /api/video/map
{ "vid": "8位码", "url": "https://..." }
```

- 视频码 4-32 位字母数字；URL 支持 http(s) 与相对路径
- 播放器首次播放自动调用
- 映射表容量受配置限制（写放大防护）

## 字幕检测

```
GET /api/subtitle/detect?url={视频地址}
```

自动检测视频同目录的字幕文件（.srt / .vtt / .ass / .ssa / .webvtt），返回多语言候选列表（如 `视频名.zh.vtt`、`视频名.en.vtt`）。

## 字幕内容

```
GET /api/subtitle/by-id?id={字幕ID}
```

返回纯文本字幕内容（text/vtt），供播放器加载。
