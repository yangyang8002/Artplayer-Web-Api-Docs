# 插件管理

插件系统采用 **Koishi 风格**设计。后台「插件管理」提供安装、启停、配置、更新与前端扩展管理。

## 安装插件

- 输入 **npm 包名**（可加 `@版本`）安装，如 `openvideo-plugin-demo@1.0.0`
- 或从「插件市场」选择版本一键安装（走 npm）

::: danger 安全警告
插件运行在服务进程内，可执行任意代码。**仅安装可信来源**的 npm 包。市场条目由官方 `plugin-registry.json` 维护。
:::

## 插件列表

每个插件卡片显示：

- **包信息**：显示名、版本、作者、描述、主页（来自 package.json + openvideoPlugin manifest）
- **能力徽章**：后台 tab 数量、播放器替换 / 扩展、提供服务（provide）、依赖服务（inject）
- **状态**：运行中 / 已停止 / 加载失败（含错误原因）
- **操作**：启用 / 停用（依赖提供者自动按拓扑序先加载）、配置（schema 自动生成表单）、更新（`@latest`，保留配置）、卸载

## 前端扩展

- **后台 tab**：插件 manifest 声明 `client.admin.tabs`，脚本内 `OpenVideoAdmin.registerTab({ id, title, mount(el) })` 注册，自动出现在侧边栏
- **播放器扩展**：`client.player.scripts` 注入播放器页，`OpenVideoPlayer.onReady(fn)` / `ctx.on('video:load')` 钩子
- **播放器替换**：`client.player.replaces: true`，脚本内 `OpenVideoPlayer.replace({ name, init(ctx) })` 完全接管播放区
- 资源由 `/api/plugins/manifest` + `/api/plugins/client/*` 注入（后台资源需管理员鉴权）

## 插件日志

插件日志（`ctx.logger`）写入环形缓冲，`GET /api/admin/plugins/logs` 可查——调试工具的基础设施。

## 依赖与更新页

「依赖与更新」页列出全部插件的版本 / 来源 / 状态，npm 插件可一键更新。

## 开发插件

请阅读 [插件指南](/plugins/guide) 与 [ctx API 参考](/plugins/ctx)。
