perform action for every item from other cmd:
```bash
find . -type f | xargs sha1sum
```

use "null bite" instead of new line character in find, and as separator in xargs (example: filenames with spaces like 'example 3.ogg'):
```bash
find . -type f -print0 | xargs -0 sha1sum
```

process one line at a time(not multiple at once):
```bash
cat hostnames | xargs -n1 host -t A # print IP address
```

process one line at a time(not multiple at once):
```bash
cat hostnames | xargs -n1 host -t A # print IP address
cat hostnames | xargs -I{} host -t A {} 8.8.8.8 # put placeholders for arguments, use google's dns server
```

run in parallel 4 arguments from pipe:
```bash
cat hostnames | xargs -P4 host -t A
```

pass to shell and read from command_string instead of stdin:
```bash
cat hostnames | xargs -I{} -P4 sh -c "host -t A {} 8.8.8.8 | tail -n 1"
```