# 发布与插件市场

开发（[Dev 环境](/guide/dev)）与验证完成后，把插件发布到 npm 并登记到官方市场（对应 插件市场）。

## 发布到 npm

```bash
cd plugins/openvideo-plugin-hello
npm publish
```

- 包名建议 `openvideo-plugin-*`，便于搜索与识别
- `package.json` 必须包含 `main`（入口）与 `openvideoPlugin`（manifest）
- 前端资源放在包内（`lib/client/`），无需打包构建
- 私有/未发布阶段：生产环境可用 `npm install <本地路径>` 或 `npm install <GitHub 仓库>` 安装

## 插件市场

市场清单为仓库根目录 `plugin-registry.json`（v2：版本 + 依赖）：

```json
{
  "updated": "2026-08-08",
  "plugins": [
    {
      "name": "openvideo-plugin-hello",
      "description": "我的第一个插件",
      "author": "your-name",
      "homepage": "https://github.com/you/openvideo-plugin-hello",
      "tags": ["工具"],
      "versions": ["1.0.0", "0.9.0"],
      "dependencies": []
    }
  ]
}
```

| 字段 | 说明 |
| --- | --- |
| `name` | npm 包名（市场安装走 `npm install <name>@<version>`） |
| `versions` | 可用版本列表（**降序，首个为最新**，后台可下拉选择） |
| `dependencies` | 依赖的其他插件包名（加载时按拓扑序） |

提交方式：向 [OpenVideoAPI 仓库](https://github.com/yangyang8002/OpenVideoAPI) 提交 PR，在 `plugin-registry.json` 添加条目。

## 市场源可配置

后台「依赖与更新 → 更新与安装配置」可修改：

- **npm 镜像源**：`plugin.npmRegistry`（或环境变量 `OPENVIDEO_NPM_REGISTRY`），作用于市场安装 / 插件更新 / 依赖更新 / 程序更新
- **插件市场源**：`plugin.registry`（或 `OPENVIDEO_PLUGIN_REGISTRY`），支持 `https://` 与本地 `file://`（Dev 环境）

## 与插件框架的对应

| 插件框架 | OpenVideoAPI |
| --- | --- |
| 插件命名约定 | `openvideo-plugin-*` 命名 |
| 插件市场（registry.npmjs.org 搜索） | 官方 `plugin-registry.json`（版本 + 依赖） |
| 市场一键安装 | 后台「插件管理 → 市场」 |
| 提交市场审核 | PR 登记 registry |
