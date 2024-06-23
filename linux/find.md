show only files:
```bash
find Videos/ -type f
```

show only directories:
```bash
find Videos/ -type d
```

find exact name:
```bash
find Downloads/ -name receipt.pdf
```

find case-insensetive name:
```bash
find Downloads/ -iname receipt.pdf
```

find all pdf files:
```bash
find Downloads/ -name '*.pdf'
```

depth of search:
```bash
find Downloads/ -mindepth 2
find Downloads/ -maxdepth 1
```

combine different options:
```bash
find Downloads/ -name '*.pdf' -name '*.txt' # AND
find Downloads/ -name '*.pdf' -o -name '*.txt' # OR
```

exclude from search results:
```bash
find Downloads/ -type f -not -name '*.pdf'
```

show files bigger/smaller than exact size:
```bash
find Downloads/ -size +9M
find Downloads/ -size -2M
```

search and delete(!):
```bash
find Downloads/ -size +20M -delete
```

perform action for every file in search:
```bash
find Downloads/ -type f -exec sha1sum {} \;
```

find with permissions:
```bash
find Downloads/ -perm 644
# At least: -644
# Any of these: /644
```

find with modified minute:
```bash
# 5 mins from current time:
find -mmin 5
# 5 mins and less:
find -mmin -5
# more than 5 mins ago:
find -mmin +5
```

find with modification content time (24 hour period):
```bash
find -mtime <number>
```

find with change metadata time:
```bash
find -cmin -5
```