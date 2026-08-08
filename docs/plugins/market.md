# 插件市场

插件市场是官方维护的插件索引（`plugin-registry.json`，位于主仓库根目录）。后台「插件管理 → 插件市场」拉取清单并展示，**安装走 npm**（支持指定版本）。

## Registry 格式（v2：版本 + 依赖）

```json
{
  "updated": "2026-08-08",
  "plugins": [
    {
      "name": "openvideo-plugin-demo",
      "description": "官方示例插件",
      "author": "yangyang8002",
      "homepage": "https://github.com/yangyang8002/OpenVideoAPI",
      "tags": ["官方", "示例"],
      "versions": ["1.0.0"],
      "dependencies": []
    }
  ]
}
```

| 字段 | 说明 |
| --- | --- |
| `name` | npm 包名（后台按该名称 `npm install`） |
| `versions` | 可用版本列表（**降序，首个为最新**），后台可下拉选择 |
| `dependencies` | 依赖的其他插件包名（安装时提示，加载时按拓扑序） |

## 使用方式

1. 后台「插件管理 → 插件市场」→「刷新」
2. 选择目标插件版本 →「安装」（后台执行 `npm install <name>@<version>`）
3. 安装后在「插件列表」中启用

## Registry 地址可配置

`data/config.json`：

```json
{ "plugin": { "registry": "https://你的镜像或自建/plugin-registry.json" } }
```

或环境变量 `OPENVIDEO_PLUGIN_REGISTRY`。默认指向官方仓库。

## 提交插件到市场

1. 将插件发布为 npm 包（遵循 [插件指南](/plugins/guide) 的 manifest 约定）
2. 向 [OpenVideoAPI 仓库](https://github.com/yangyang8002/OpenVideoAPI) 提交 PR，在 `plugin-registry.json` 中添加条目（含版本与依赖）

## 插件托管建议

- 插件**必须**为 npm 包（本地开发可先 `npm install <本地路径>` 放入 `plugins/`）
- 包名建议以 `openvideo-plugin-` 开头，便于识别与搜索
- 前端资源放在包内 `lib/client/` 下，由 `/api/plugins/client/*` 注入，**无需打包构建**
