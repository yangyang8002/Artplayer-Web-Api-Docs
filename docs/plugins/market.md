# 插件市场

插件市场是官方维护的插件索引（`plugin-registry.json`，位于主仓库根目录）。后台「插件管理 → 插件市场」拉取该清单并展示可安装插件。

## 使用方式

1. 后台打开「插件管理」→「插件市场」
2. 点击「刷新」从 GitHub 拉取最新清单（缓存 10 分钟）
3. 点击目标插件「安装」按钮（本质是 URL 安装）
4. 安装后在「插件列表」中启用

## 提交插件到市场

如果你想把自己的插件加入官方市场：

1. 将插件代码发布到 GitHub（raw 链接可访问）
2. 向 [Artplayer-Web-Api 仓库](https://github.com/yangyang8002/Artplayer-Web-Api) 提交 PR，在 `plugin-registry.json` 中添加条目：

```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "插件描述",
  "author": "你的名字",
  "homepage": "https://github.com/you/my-plugin",
  "tags": ["工具", "示例"],
  "url": "https://raw.githubusercontent.com/you/my-plugin/main/my-plugin.js"
}
```

## 市场机制

- 市场清单通过 `GET /api/admin/plugins/market` 从 `raw.githubusercontent.com` 拉取
- 已安装的插件会在列表中标记「已安装」
- 市场更新由仓库维护者发布，与版本发布同步

## 插件托管建议

- **单文件插件**：直接放仓库根目录或 `plugins/` 下，使用 raw 链接
- **npm 插件**：发布到 npm registry，市场条目填写包名（后台安装时选择 npm 方式）
- 插件遵循 [插件指南](/plugins/guide) 的导出约定即可被系统识别
