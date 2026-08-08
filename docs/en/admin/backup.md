# Backups

## Manual Backup

- **Contents**: database data / server config
- **Target**: local / cloud / both

## Scheduled Backup

- Enable auto backup, set interval (hours) and keep count
- Saving resets the timer; enabling for the first time runs one immediately

## Cloud Sync

Target types: FTP / FTPS, SFTP (SSH), WebDAV, OpenList (AList-compatible). Each config can be enabled independently, tested, edited or deleted. After a local backup, all enabled cloud configs sync automatically (failures are logged but don't affect the backup).

## Lists & Restore

- Local backups: download / restore / delete, multi-select batch restore
- Cloud list: download to local / delete
- **Batch restore** applies selected backups from oldest to newest with coordinated data/config restore

::: warning
Restoring **overwrites current data**. Current database connection and backup config are preserved when restoring configs.
:::
