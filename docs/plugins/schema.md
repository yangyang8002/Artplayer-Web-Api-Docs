# 配置 Schema

插件导出 `schema` 数组后，后台「插件管理」自动生成**配置表单**，保存后插件热重载。

## 字段定义

```js
module.exports = {
  schema: [
    { key: 'interval',  label: '间隔（秒）', type: 'number', default: 60, hint: '定时任务执行间隔' },
    { key: 'verbose',   label: '详细日志',   type: 'boolean', default: true },
    { key: 'level',     label: '日志级别',   type: 'select', default: 'info',
      options: [ { value: 'debug', label: '调试' }, { value: 'info', label: '普通' } ] },
    { key: 'message',   label: '自定义消息', type: 'textarea', default: 'Hello' }
  ],
  apply(ctx, config) { ... }
}
```

## 字段属性

| 属性 | 类型 | 说明 |
| --- | --- | --- |
| `key` | string | 配置键名（必填） |
| `label` | string | 表单显示名称 |
| `type` | string | `string` / `number` / `boolean` / `select` / `textarea`（默认 string） |
| `default` | any | 默认值（用户未设置时使用） |
| `hint` | string | 字段下方提示文字 |
| `options` | array | select 类型的选项：`[{ value, label }]` |

## 读取配置

`apply(ctx, config)` 的 `config` 即合并后的配置对象；运行期也可随时通过 `ctx.config` 读取。

```js
apply(ctx, config) {
  const interval = config.interval ?? 60;
  setInterval(() => { ... }, interval * 1000);
}
```

## 说明

- 配置持久化于 `data/plugins.json`（每个插件独立）
- 保存配置后插件自动**热重载**（卸载 → 重新加载，配置生效）
- 若插件未导出 schema，后台不显示「配置」按钮，仍可通过 API 直接修改配置
