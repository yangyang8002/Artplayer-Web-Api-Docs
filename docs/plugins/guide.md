# 插件指南

插件系统参考 **Koishi** 的设计：插件是一个函数 / 类 / 带 `apply` 的对象，加载时框架调用 `apply(ctx, config)`。`ctx` 是插件上下文，注入路由、存储、事件总线等能力。

## 最小插件

```js
// my-plugin.js
module.exports = {
  name: 'my-plugin',
  version: '1.0.0',
  description: '我的第一个插件',
  apply(ctx, config) {
    ctx.log('插件已加载');
  }
}
```

上传到后台「插件管理」并启用即可。

## 插件形式

```js
// 1. 函数插件
module.exports = function (ctx, config) { ... }

// 2. 类插件（constructor(ctx, config)）
module.exports = class MyPlugin {
  constructor(ctx, config) { ... }
}

// 3. 带 apply 的对象（推荐：可携带元数据与 schema）
module.exports = {
  name: 'demo', version: '1.0.0', description: '...',
  apply(ctx, config) { ... }
}
```

## 编写并发布

### 本地 / 上传方式

单文件 `.js` 插件，直接上传即可。

### GitHub 方式

1. 创建仓库，将插件文件发布为 raw 链接，如：
   `https://raw.githubusercontent.com/<user>/<repo>/master/<plugin>.js`
2. 后台「插件管理 → GitHub / URL 下载」粘贴该链接安装
3. 提交到官方市场（可选）：向主仓库提交 PR，将插件登记到 `plugin-registry.json`

### npm 方式

1. 以 npm 包形式发布插件（包入口导出 apply 对象/函数）
2. 后台输入包名安装（`plugins/node_modules/` 下）
3. npm 插件的版本 / 描述 / 作者信息自动从 `package.json` 读取

## 生命周期

- **加载**：启用（或服务启动）时调用 `apply(ctx, config)`；热加载时先卸载旧实例
- **运行**：注册的路由、事件、定时器正常工作
- **卸载**：触发 `dispose` 事件回调（清理定时器、连接等）
- **热重载**：修改配置 / 重新启用时自动重启插件实例；旧实例注册的路由自动失效

## 内置事件

插件可订阅内置事件：

```js
ctx.on('danmu:send', (danmu) => {
  // 弹幕发送成功
  // danmu = { vid, text, color, type, time, author }
});
```

## 安全约定

- 插件运行在服务进程内，**拥有与服务器相同的权限**，请勿安装来源不明的插件
- 插件抛出的异常不会导致服务崩溃（加载失败会标记为 error 状态并显示原因）
- 事件处理器异常会被捕获并记录，不影响其他插件

## 示例

仓库内置示例插件 `plugins/hello-world.js`，演示了元数据、schema、路由、事件订阅与定时任务。详见 [ctx API 参考](/plugins/ctx) 与 [配置 Schema](/plugins/schema)。
