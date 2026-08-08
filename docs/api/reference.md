# 接口总览

所有接口返回 JSON，格式统一为 `{ code, msg, data }`。`code === 0` 表示成功。

## 公共接口（无需认证）

| 方法 | 路径 | 说明 |
| --- | --- | --- |
| GET | `/api/config/public` | 播放器公共配置（主题、弹幕限制等） |
| GET | `/api/danmu/?id={vid}` | 获取弹幕（v1 兼容格式） |
| POST | `/api/danmu/` | 发送弹幕（v1） |
| GET | `/api/danmu/v3/?id={vid}` | 获取弹幕（DPlayer 兼容数组格式） |
| GET | `/api/danmu/v3/{vid}` | 同上（路径参数） |
| POST | `/api/danmu/v3/` | 发送弹幕（v3） |
| GET | `/api/video/resolve?v={vid}` | 通过视频码解析视频地址 |
| POST | `/api/video/map` | 记录视频映射（`{vid, url}`） |
| GET | `/api/subtitle/detect?url=` | 检测视频同目录字幕 |
| GET | `/api/subtitle/by-id?id=` | 按 ID 加载字幕内容 |
| POST | `/api/pow/verify` | PoW 工作量证明校验 |

## 管理接口（需要 Bearer Token）

`/api/admin/*` 全部需要请求头 `Authorization: Bearer <token>`。登录接口：

```
POST /api/admin/login  { username, password } → { data: { token, firstRun } }
```

完整的管理接口列表见 [管理 API](/api/admin)。

## 通用约定

- 所有写接口有每 IP 限速（防刷盘）
- 数据迁移期间写接口返回 503
- 弹幕内容长度、作者长度受配置限制
- PoW 启用时，写请求需先通过 `/api/pow/verify` 获取凭证
