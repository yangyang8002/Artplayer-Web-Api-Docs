# 快速开始

<span class="badge">v26.8.11</span><span class="badge">Node ≥ 18</span><span class="badge">MIT</span>

ArtPlayer Web API 是一个自托管、零构建的弹幕视频播放器 + Web 管理后台。支持 PoW 防火墙、多数据库、插件系统、多语言与多主题。

## 方式一：npm 安装（推荐）

```bash
# 全局安装（可作系统服务运行）
npm install -g artplayer-web-api
artplayer-web-api

# 或克隆仓库运行
git clone https://github.com/yangyang8002/Artplayer-Web-Api.git
cd Artplayer-Web-Api
npm install
npm start
```

服务默认监听 `http://localhost:1919`。

## 方式二：Docker

```bash
docker pull yangyang8002/artplayer-web-api:latest
docker run -d -p 1919:1919 -v ./data:/app/data yangyang8002/artplayer-web-api:latest
```

或使用 docker-compose，详见 [Docker 部署](/guide/docker)。

## 首次启动

1. 打开 `http://localhost:1919/admin/`，使用默认账号 `admin / admin123` 登录
2. 系统会引导完成**初始化向导**：选择界面语言、时区、数据库类型，并设置新密码与管理入口路径
3. 初始化完成后，使用新密码重新登录

::: warning 安全提示
首次登录后**请立即修改默认密码**，并建议将管理入口路径改为自定义值。
:::

## 播放器地址

```
http://localhost:1919/player/?url=视频地址
```

- 支持 mp4 / m3u8 / flv 直链
- 视频首次播放会自动生成 8 位视频码（vid），弹幕按 vid 存储
- 支持 DPlayer 兼容接口 `/api/danmu/v3/?id={vid}`

## 目录结构

```
server.js             服务主程序（单文件，无构建）
lib/                  核心模块（store / cloud / plugin）
public/               前端页面（admin.html 后台、player.html 播放器、i18n.js）
theme/                主题系统
plugins/              插件目录
data/                 数据目录（config.json、danmu.json、videos.json ...）
update.js             独立更新进程
update.xml            sha256 版本清单
```

## 下一步

- [播放器使用](/guide/player)
- [管理后台总览](/admin/overview)
- [插件开发](/plugins/guide)
- [API 参考](/api/reference)
