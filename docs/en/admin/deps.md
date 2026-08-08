# Dependencies & Updates

This tab centralizes app version, npm dependency and plugin updates.

## App Version

- **Check update** compares GitHub Releases, npm registry and the remote `update.xml`, taking the highest version
- Shows current / latest version and deployment type (git-source / npm-global / docker / source)
- "Update to latest" runs the full pipeline: backup → update → verify → restart

## npm Dependencies

- List with current / latest version and status
- **Per-package update**: `npm install <pkg>@latest` in background
- **Update all**: one click for everything

::: tip
Dependency updates run in the background; **restart the service** to apply. Check results are cached for 30 minutes.
:::

## Plugin Updates

Lists installed plugins (version / source / status). npm and URL plugins can be updated from their original source (config preserved); file-based plugins need manual re-upload.

See [Plugins](/en/admin/plugins).
