# Client Extensions

Plugins can extend the admin and player frontends (the framework's console plugins). Mechanism: **manifest declaration → server aggregation → runtime injection**; plugin scripts register capabilities via global APIs.

## Declaration

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

Paths are relative to the package dir; only `.js` / `.css` allowed. Assets are injected via `/api/plugins/manifest` + `/api/plugins/client/*` (admin assets require auth; player assets are public).

## Admin Tabs (OpenVideoAdmin)

```js
(function () {
    OpenVideoAdmin.registerTab({
        id: 'hello',
        title: 'hello',
        mount(el) {   // called on first activation; el is the panel container
            el.innerHTML = '<div class="card"><h3>Hello</h3><div id="helloOut"></div></div>';
            OpenVideoAdmin.api('/api/plugin/hello').then(d => {
                document.getElementById('helloOut').textContent = JSON.stringify(d);
            });
        }
    });
})();
```

`OpenVideoAdmin.api(url, opts)` adds the admin auth header automatically; tabs appear in the sidebar above "About".

## Player Extensions (OpenVideoPlayer)

### Hooks (recommended)

```js
(function () {
    OpenVideoPlayer.onReady(function (ctx) {
        // ctx: { container, url, vid, config, danmaku, resolve, on, emit, setUrl }
        var el = document.createElement('div');
        el.textContent = 'hello plugin';
        ctx.container.appendChild(el);
    });
})();
```

### Events

```js
ctx.on('video:load', function (ev) { /* { url, vid } */ });
ctx.on('config:ready', function (cfg) { /* /api/config/public data */ });
```

### Player Replacement (full takeover)

```js
OpenVideoPlayer.replace({
    name: 'my-player',
    init(ctx) { /* render your own player in ctx.container; use ctx.danmaku.get/send, ctx.resolve */ },
    load(ctx, url) { /* called when switching videos */ }
});
```

## Injection Flow

1. Admin/player page loads → `GET /api/plugins/manifest?scope=player|admin`
2. Inject each plugin's styles → scripts in order
3. Scripts register via the `OpenVideoPlayer` / `OpenVideoAdmin` globals
4. Before player boot, `replacement` is checked: plugin takes over, otherwise the default player starts

## Framework Mapping

| 插件框架 | OpenVideoAPI |
| --- | --- |
| Console plugins (vue pages/components) | admin tabs (plain JS/CSS, zero build) |
| `ctx.console` service | `OpenVideoAdmin` / `OpenVideoPlayer` globals |
| Bundled frontend assets | `lib/client/` raw assets injected by the server |
