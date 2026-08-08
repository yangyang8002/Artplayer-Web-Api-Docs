# 系统更新

系统内置完善的更新机制：后台「关于」页或「依赖与更新」页点击 **检查更新**，可对比 GitHub Release / npm / 远程清单三种来源的版本。

## 更新流程

后台点击「更新到最新版」后，会启动独立进程 `update.js`：

1. **锁定**：防并发更新（`data/.update.lock`）
2. **备份**：自动备份整个 `data/` 目录到 `data/backup_update_<时间戳>/`
3. **拉取代码**：git 部署执行 `git fetch + pull`；npm 部署下载并覆盖 npm 包
4. **清单校验**：按 `update.xml` 的 sha256 校验每个文件是否与发布版本一致
5. **依赖安装**：`npm install --production`
6. **校验版本**：确认更新后版本号
7. **重启服务**：自动停止旧进程并启动新版本

::: warning
- `data/` 目录**不参与更新**，更新前后数据安全
- 本地未提交的代码改动会导致 `git pull` 失败（可执行 `git checkout -- .` 后重试）
- Docker 部署请在宿主机执行 `docker pull` + `docker compose up -d`
:::

## 命令行更新

```bash
# 直接更新（git 来源）
node update.js

# 更新但不重启
node update.js --no-restart

# 强制更新（跳过清单校验失败）
node update.js --force

# 指定来源
node update.js --source=git     # git pull
node update.js --source=npm     # 下载 npm 包覆盖
```

更新日志写入 `data/update.log`。

## 依赖管理

「依赖与更新」页支持：

- **程序版本**：查看当前 / 最新版本、部署方式与更新说明
- **npm 依赖**：逐个依赖查看最新版本并单独更新，或一键更新全部（后台 `npm install`，完成后重启生效）
- **插件更新**：npm / URL 来源插件可一键按原来源重新安装（保留配置与启用状态）
