# 配置 Schema

在 `package.json` 的 `openvideoPlugin.schema` 数组声明配置项，后台「插件管理」自动生成配置表单，保存后插件热重载。

```json
{
  "openvideoPlugin": {
    "schema": [
      { "key": "interval",  "label": "间隔（秒）", "type": "number", "default": 60, "hint": "定时任务执行间隔" },
      { "key": "verbose",   "label": "详细日志",   "type": "boolean", "default": true },
      { "key": "level",     "label": "日志级别",   "type": "select", "default": "info",
        "options": [ { "value": "debug", "label": "调试" }, { "value": "info", "label": "普通" } ] },
      { "key": "message",   "label": "自定义消息", "type": "textarea", "default": "Hello" }
    ]
  }
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

`apply(ctx, config)` 的 `config` 即合并后的配置对象；运行期通过 `ctx.config` 读取。

```js
apply(ctx, config) {
  const interval = config.interval ?? 60;
  setInterval(() => { ... }, interval * 1000);
}
```

## 说明

- 配置持久化于 `data/plugins.json`（每个插件独立）
- 保存配置后插件自动**热重载**（卸载 → 重新加载）
- 未声明 schema 的插件后台不显示「配置」按钮（仍可通过 API 修改）
