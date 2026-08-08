# Config Schema

Declare config fields in the `openvideoPlugin.schema` array; the admin renders a form automatically and saving hot-reloads the plugin.

```json
{
  "openvideoPlugin": {
    "schema": [
      { "key": "interval",  "label": "Interval (sec)", "type": "number", "default": 60, "hint": "Timer interval" },
      { "key": "verbose",   "label": "Verbose", "type": "boolean", "default": true },
      { "key": "level",     "label": "Level", "type": "select", "default": "info",
        "options": [ { "value": "debug", "label": "Debug" }, { "value": "info", "label": "Info" } ] },
      { "key": "message",   "label": "Message", "type": "textarea", "default": "Hello" }
    ]
  }
}
```

## Field Attributes

| Attribute | Type | Description |
| --- | --- | --- |
| `key` | string | config key (required) |
| `label` | string | form label |
| `type` | string | `string` / `number` / `boolean` / `select` / `textarea` (default string) |
| `default` | any | default value |
| `hint` | string | hint text below the field |
| `options` | array | select options: `[{ value, label }]` |

## Reading Config

`config` in `apply(ctx, config)` is the merged config; `ctx.config` gives runtime access.

## Notes

- Config persists in `data/plugins.json` per plugin
- Saving hot-reloads the plugin
- Without a schema, no "Config" button is shown
