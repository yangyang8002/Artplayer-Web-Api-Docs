# Update

The update system compares GitHub Releases, npm registry and the remote `update.xml` manifest.

## In the Admin Panel

Click "Update to latest" in the About / Dependencies tab. The standalone `update.js` process:

1. Locks against concurrent updates
2. Backs up `data/` to `data/backup_update_<timestamp>/`
3. Pulls code (git pull for git deploys; npm package overlay otherwise)
4. Verifies every file against the sha256 manifest (`update.xml`)
5. Installs dependencies
6. Verifies the new version
7. Restarts the service

::: warning
- `data/` is never touched by updates
- Uncommitted local changes break `git pull` (recover with `git checkout -- .`)
- Docker deployments must update on the host: `docker pull` + `docker compose up -d`
:::

## CLI

```bash
node update.js                     # update (git source)
node update.js --no-restart        # update without restart
node update.js --force             # skip manifest verification failures
node update.js --source=npm        # update from npm package
```

Logs go to `data/update.log`.

## Dependencies

The "Dependencies & Updates" tab supports:

- **App version**: current / latest version, deployment type, release notes
- **npm dependencies**: per-package latest version check and individual updates, or update all (background `npm install`, restart to apply)
- **Plugin updates**: npm / URL plugins can be re-installed from their original source, keeping config & enabled state
