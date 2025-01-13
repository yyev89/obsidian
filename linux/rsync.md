  transfer a file:
```bash
rsync path/to/source path/to/destination
```

  use archive mode (recursively copy directories, copy symlinks without resolving, and preserve permissions, ownership and modification times):
```bash
rsync -a[--archive] path/to/source path/to/destination
```

  compress the data as it is sent to the destination, display verbose and human-readable progress, and keep partially transferred files if interrupted:
```bash
rsync -zvhP[--compress --verbose --human-readable --partial --progress] path/to/source path/to/destination
```

  recursively copy directories:
```bash
rsync -r[--recursive] path/to/source path/to/destination
```

  use archive mode, resolve symlinks, and skip files that are newer on the destination:
```bash
rsync -auL[--archive --update --copy-links] path/to/source path/to/destination
```

transfer a directory from a remote host running `rsyncd` and delete files on the destination that do not exist on the source:
```bash
rsync -r[--recursive] --delete rsync://host:path/to/source path/to/destination
```

  transfer a file over SSH using a different port than the default (22) and show global progress:
```bash
rsync -e[--rsh] 'ssh -p port' --info=progress2 host:path/to/source path/to/destination
```

