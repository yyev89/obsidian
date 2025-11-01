search for pattern in all files in current directory:
```bash
grep div *.html
```

- `-c` counts matches in all files in current directory
- `-i` case insensitive
- `-v` every line that doesnt match (inverted)
- `-x` search for line
- `-w word` match the whole word (not part)
- `-n` line number
- `-r` recursive

use regexp for matches:
```bash
ls --help | grep -e '-U'
```

show extra 3 lines before the match:
```bash
grep -w -B 3 iv *.html
```

- `-A` 3 lines after
- `-C` 3 lines before and 3 lines after

show exact pattern 404:
```bash
grep '\b404\b' fake_webserver_logs.txt
```

search for 404 or 500:
```bash
grep -E '404|500' fake_webserver_logs.txt
```

show all ip addresses in log:
```bash
grep -E '([0-9]{3}\.){3}[0-9]{1,3}' fake_webserver.logs.txt
```