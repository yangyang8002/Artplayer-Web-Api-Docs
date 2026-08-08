# 弹幕 API

## 获取弹幕

```
GET /api/danmu/?id={vid}
GET /api/danmu/v3/?id={vid}
GET /api/danmu/v3/{vid}
```

### v1 格式

```json
{ "code": 0, "data": [ { "id": "...", "vid": "...", "text": "...", "color": "#ffffff", "type": "right", "time": 1.5, "author": "anonymous", "date": "..." } ] }
```

### v3（DPlayer 兼容）

```json
{ "code": 0, "data": [ [time, type, color, author, text] ] }
```

其中 type：`0` 滚动、`1` 顶部、`2` 底部；color 为十进制色值。

## 发送弹幕

```
POST /api/danmu/
POST /api/danmu/v3/
```

请求体（v3）：

```json
{
  "id": "视频码",      // 或 player
  "text": "弹幕内容",
  "color": 16777215,   // 十进制颜色
  "type": 0,           // 0 滚动 / 1 顶部 / 2 底部
  "time": 12.5,        // 视频时间点
  "author": "昵称"
}
```

### 限制

- 每 IP 每分钟发送上限（配置 `danmakuLimit.maxPerMinute`，默认 10）
- 内容长度上限（配置 `danmakuLimit.maxLength`）
- 命中屏蔽词返回 403 `{code: 2}`
- 弹幕发送成功会广播 `danmaku:send` 事件给插件

## 弹幕频率限制

弹幕写接口使用每 IP 滑动窗口限速（默认 60 次 / 60 秒），叠加业务频率限制，防刷屏。
