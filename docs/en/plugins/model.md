# Data Model

Plugins can define their own tables via `ctx.model.define(name, schema)` (the framework's `ctx.model`). Data migrates automatically when the main storage switches (JSON / SQLite / MySQL / PostgreSQL / MongoDB).

## Define

```js
const notes = ctx.model.define('hello_notes', {
  primary: 'id',
  fields: {
    id: { type: 'string' },
    text: { type: 'string' },
    score: { type: 'number' },
    active: { type: 'boolean' },
    createdAt: { type: 'number' }
  }
});
```

Field `type` supports: `string` / `number` / `boolean` / `json`. Records are coerced on write.

## Table API

```js
await notes.create({ text: 'hi', score: 10 });     // auto id + createdAt
await notes.get('id');
await notes.update('id', { text: 'new' });
await notes.remove('id');
await notes.count();
await notes.clear();
const { list, total } = await notes.list({ page: 1, limit: 20, search: 'key', searchKey: 'text' });
await notes.all();
```

## Storage & Migration

- JSON backend: `data/plugin_tables.json`
- SQL / MongoDB: kv key `plugin_tables`
- Storage switches (admin "Database" tab) **migrate plugin tables automatically**
- Whole-table read/write; keep tables ≤ ~10k rows

## Use Cases

- Plugin settings/stats persistence
- Bot data, leaderboards, etc.
- For core data (danmaku/videos/subtitles) use `ctx.store` dedicated APIs
