# 插件管理

插件系统采用 **Koishi 风格**设计。后台「插件管理」提供完整的安装、启停、配置与更新能力。

## 安装插件

三种安装方式：

| 方式 | 说明 |
| --- | --- |
| 上传 .js 文件 | 直接上传插件脚本（≤ 1MB） |
| GitHub / URL 下载 | 填写插件直链（raw 链接等）下载安装 |
| npm 包 | 输入 npm 包名，自动 `npm install` 到 `plugins/node_modules/` |

安装后插件默认**停用**，需手动启用。插件状态持久化于 `data/plugins.json`。

::: danger 安全警告
插件运行在服务进程内，可执行任意代码（注册路由、访问数据、执行命令）。**仅安装可信来源**的插件。官方插件市场的条目均托管于主仓库 `plugin-registry.json`。
:::

## 插件列表

每个插件卡片显示：

- 元数据：名称、版本、作者、描述、主页（来自插件导出或 npm package.json）
- 状态：运行中 / 已停止 / 加载失败（含错误信息）
- 操作：**启用 / 停用**（热加载 / 卸载）、**配置**（自动生成表单）、**更新**、**卸载**

## 配置表单

插件可导出 `schema` 数组，后台自动生成配置表单：

```js
module.exports = {
  schema: [
    { key: 'interval', label: '间隔（秒）', type: 'number', default: 60, hint: '定时任务间隔' },
    { key: 'enabled', label: '启用', type: 'boolean', default: true },
    { key: 'level', label: '级别', type: 'select', default: 'info', options: [{ value: 'info', label: '普通' }] }
  ],
  apply(ctx, config) { /* ... */ }
}
```

支持类型：`string` / `number` / `boolean` / `select` / `textarea`。保存配置后插件**热重载**。

## 插件市场

「插件市场」面板从官方 registry（`plugin-registry.json`）拉取可安装插件列表，一键安装。已有插件会标记「已安装」。

## 插件更新

npm / URL 来源插件可在「插件管理」或「依赖与更新」页一键更新（按原来源重新安装，**保留配置与启用状态**）；文件型插件需手动上传新版。

## 开发插件

请阅读 [插件指南](/plugins/guide) 与 [ctx API 参考](/plugins/ctx)。
