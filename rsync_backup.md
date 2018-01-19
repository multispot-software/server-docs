# Incremental Backup with Rsync

Notes on using `rsync` to implement incremental backups,
using hardlinks to save space.

> From: http://www.mikerubel.org/computers/rsync_snapshots/

```sh
TODAY=$(date -I)
FIRST_BACKUP="2017-12-18"

cd /mnt/archive/backup
rsync -a --modify-window=1 --link-dest=$FIRST_BACKUP /mnt/Antonio/data $TODAY --log-file=$TODAY.log
```

## Option `--modify-window`

The `--modify-window` option solves problems of timestamps inaccuracy on windows
resulting in deleted or re-backuped files.

## Files checksums

Files are considered existing in the backup (and thus hard-linked instead of
copied) if name and timestamp are equal, no checksum is
computed by default. To enable checksum use `-c`.

## Why `--delete` is not needed?

From: http://dev-notes.eu/2015/06/incremental-backup-on-server/
> I originally used the --delete flag on this script - thinking that it would
> be necessary to keep the latest incremental backup synchronised with the
> current source filesystem.
>
> However, `--delete` has no effect when syncing into an empty directory,
> which is basically what happens when using the --link-dest method of rsync.
> The directory specified by --link-dest is used as a reference point, and any
> files in source that are unchanged relative to this reference point are
> hardlinked to their exisiting inode in the reference directory.

## Other references

https://github.com/xolox/python-rsync-system-backup
https://github.com/Nextpertise/autorsyncbackup
https://github.com/seanh/snapshotter
