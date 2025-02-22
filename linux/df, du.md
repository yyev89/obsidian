### df (disk free)
- high level
- file system info
- from metadata (fast)
- statfs() or statvfs() system calls

*"I wonder how much space is on the root drive?"*

free space on disk:
```bash
df /
```

- `-h` human-readable powers of 1024
- `-H` powers of 1000
- `-i` inodes information
- `-T` filesystem types
- `-t ext4` show only ext4
- `-x ext4` show others than ext4

show total amout of used space:
```bash
df -H --total
```

### du (disk usage)
- space used by individual files and dir
- estimate
- scans each file and dir (slower)
- stat() and lstat() system calls

*"I wonder what is taking up the space on the root drive?"*

disk usage for the current directory recursively:
```bash
du
```

- `-h` human-readable
- `--si` powers of 1000
- `-a` also show files
- `-s` total size of current directory

total summary for /tmp and /root directories:
```bash
du /tmp /root --si -s --total
```

sort output:
```bash
du -a --si / | sort -h
```

view size of the items in current dir and sort it from the largest to smallest:
```bash
du -sh * | sort -nr
```

