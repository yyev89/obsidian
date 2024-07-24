compare files:
```bash
diff old_file new_file
```

- `-w` ignore white spaces
- `-y` show the differences side by side
- `-u` show the differences in unified format
- `-d` try hard to find smaller set of changes
- `--color=always` output in color

compare directories recursively:
```bash
diff -r old_dir new_dir
```

compare directories, only showing the names of files that differ:
```bash
diff -r  -q old_dir new_dir
```
