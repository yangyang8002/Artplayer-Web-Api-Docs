# 数据库管理

## 支持的存储

| 类型 | 说明 |
| --- | --- |
| JSON | 默认，零依赖，数据存于 `data/*.json` |
| SQLite | 本地文件（`data/app.db`），零配置 |
| MySQL / MariaDB | 远程数据库 |
| PostgreSQL | 远程数据库 |
| MongoDB | 文档数据库 |

## 切换存储与迁移

1. 选择目标存储类型，填写连接信息（主机 / 端口 / 用户 / 密码 / 数据库）
2. 点击**测试连接**验证
3. 点击**切换并迁移**：系统自动执行 `collectAll → restoreAll` 全量迁移（弹幕、视频映射、屏蔽词、账号、封禁白名单、登录记录、统计、字幕库），**无需重启**

::: tip
- 切换期间暂停数据写入（迁移锁），完成后自动恢复
- 目标存储不可用时切换失败，当前存储不受影响
:::

## 数据浏览

内置只读浏览（表名白名单）：danmu / videos / banned_words / accounts / security / login_logs / login_fails / subtitles / kv。支持分页与搜索，方便排查数据。

## 导出备份

一键导出全部数据的 JSON 备份文件（含存储类型信息）。
