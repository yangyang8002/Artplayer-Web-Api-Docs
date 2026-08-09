# 数据模型

插件可通过 `ctx.model.define(name, schema)` 定义自己的数据表（对应 Koishi 的 `ctx.model`），数据随主存储（JSON / SQLite / MySQL / PostgreSQL / MongoDB）**切换自动迁移**。

## 定义表

```js
const notes = ctx.model.define('hello_notes', {
  primary: 'id',                                   // 主键字段（默认 id）
  fields: {
    id: { type: 'string' },
    text: { type: 'string' },
    score: { type: 'number' },
    active: { type: 'boolean' },
    createdAt: { type: 'number' }
  }
});
```

字段 `type` 支持：`string` / `number` / `boolean` / `json`。写入时按类型自动规整。

## 表 API

```js
await notes.create({ text: 'hi', score: 10 });     // 无主键自动生成 id，自动补 createdAt
await notes.get('id');                             // 单条查询
await notes.update('id', { text: 'new' });         // 部分更新
await notes.remove('id');                          // 删除
await notes.count();                               // 行数
await notes.clear();                               // 清空

// 分页查询（searchKey 指定搜索字段，默认主键）
const { list, total } = await notes.list({ page: 1, limit: 20, search: 'key', searchKey: 'text' });

await notes.all();                                 // 全部行
```

## 存储与迁移

- JSON 后端：集中存于 `data/plugin_tables.json`
- SQL 后端 / MongoDB：存储于 kv 键 `plugin_tables`
- 后台「数据库管理」切换存储时，插件表数据**自动随迁移**（`collectAll/restoreAll` 已包含）
- 整表读写实现，单表建议 ≤ 1 万行

## 使用场景

- 插件的设置/统计数据持久化（无需在服务器配置里塞自定义字段）
- 弹幕机器人数据、签到记录、自定义榜单等
- 注意：核心业务数据（弹幕/视频/字幕）请通过 `ctx.store` 的专用 API 操作
