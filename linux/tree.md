print files and directories up to 'num' levels of depth (where 1 means the current directory):
 ```bash
tree -L {{num}}
```

print directories only:
```bash
tree -d
```

print hidden files too with colorization on:
```bash
tree -a -C
```

print permissions:
```bash
tree -p
```

print the tree without indentation lines, showing the full path instead (use -N to not escape non-printable characters):
```bash
tree -i -f
```

print the size of each file and the cumulative size of each directory, in human-readable format:
```bash
tree -s -h --du
```

print files within the tree hierarchy, using a wildcard (glob) pattern, and pruning out directories that don't contain matching files:
```bash
tree -P '{{*.txt}}' --prune
```

print directories within the tree hierarchy, using the wildcard (glob) pattern, and pruning out directories that aren't ancestors of the wanted one:
```bash
tree -P {{directory_name}} --matchdirs --prune
```

print the tree ignoring the given directories:
```bash
tree -I '{{directory_name1|directory_name2}}'
```