# 前端扩展

插件可以扩展后台与播放器前端（对应 Koishi 的控制台插件）。机制：**manifest 声明 → 服务端聚合 → 运行时注入**，插件脚本里调用全局 API 注册能力。

## 声明

`package.json` 的 `openvideoPlugin.client`：

```json
{
  "openvideoPlugin": {
    "client": {
      "admin": {
        "styles": ["lib/client/admin/panel.css"],
        "scripts": ["lib/client/admin/panel.js"],
        "tabs": [{ "id": "hello", "title": "hello" }]
      },
      "player": {
        "styles": ["lib/client/player/hook.css"],
        "scripts": ["lib/client/player/hook.js"],
        "replaces": false
      }
    }
  }
}
```

资源路径相对插件包目录；仅允许 `.js` / `.css`。服务端通过 `/api/plugins/manifest` + `/api/plugins/client/*` 注入（后台资源需管理员鉴权，播放器资源公开）。

## 后台 tab（OpenVideoAdmin）

```js
// lib/client/admin/panel.js
(function () {
    OpenVideoAdmin.registerTab({
        id: 'hello',                          // 唯一 id
        title: 'hello',                       // 侧边栏显示名
        mount(el) {                           // tab 首次激活时调用，el 为面板容器
            el.innerHTML = '<div class="card"><h3>Hello 插件</h3><div id="helloOut"></div></div>';
            OpenVideoAdmin.api('/api/plugin/hello').then(d => {
                document.getElementById('helloOut').textContent = JSON.stringify(d);
            });
        }
    });
})();
```

`OpenVideoAdmin.api(url, opts)` 自动携带管理员鉴权头；tab 会自动出现在侧边栏「关于」上方。

## 播放器扩展（OpenVideoPlayer）

### 钩子（推荐，不改动默认播放器）

```js
// lib/client/player/hook.js
(function () {
    OpenVideoPlayer.onReady(function (ctx) {
        // ctx: { container, url, vid, config, danmaku, resolve, on, emit, setUrl }
        var el = document.createElement('div');
        el.textContent = 'hello plugin';
        ctx.container.appendChild(el);
    });
})();
```

### 事件订阅

```js
ctx.on('video:load', function (ev) { /* { url, vid } */ });
ctx.on('config:ready', function (cfg) { /* /api/config/public 数据 */ });
```

### 播放器替换（完全接管）

```js
OpenVideoPlayer.replace({
    name: 'my-player',
    init(ctx) {
        // 自己渲染播放器到 ctx.container，使用 ctx.danmaku.get/send、ctx.resolve 等 API
    },
    load(ctx, url) { /* 切换视频时调用 */ }
});
```

`client.player.replaces: true` 时后台插件卡片会标记「替换播放器」。

## 资源注入流程

1. 播放器/后台页面加载 → `GET /api/plugins/manifest?scope=player|admin`
2. 按顺序注入每个插件的 styles → scripts
3. 脚本执行时调用 `OpenVideoPlayer` / `OpenVideoAdmin` 全局 API 注册
4. 播放器 boot 前检查 `replacement`：有则交给插件渲染，否则默认启动

## 与 Koishi 的对应

| Koishi | OpenVideoAPI |
| --- | --- |
| Console 插件（vue 页面/组件） | 后台 tab（原生 JS/CSS，零构建） |
| `ctx.console` 服务 | `OpenVideoAdmin` / `OpenVideoPlayer` 全局 API |
| 插件前端资源打包 | 包内 `lib/client/` 原生资源，服务端注入 |
