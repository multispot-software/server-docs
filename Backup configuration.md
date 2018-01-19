# Backup configuration

## Users and permissions

There is a user `backup` which performs the backup to `/mnt/backup`
(ext4 partition on 4TB Western Digital harddrive). This is the only
user with write-access to `/mnt/backup`.

There is a group `backup` that can read `/mnt/backup`.
The user `anto` is part of the backup group.

### User and Group Commands

The following commands are here as a memo for when the server is
reinstalled.

Add user `anto` to group `backup`:

`# adduser anto backup`

Add group:

`# addgroup backup`
