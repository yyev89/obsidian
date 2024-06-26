print first byte of the file:
```bash
cut -b 1 message.txt
# Use range:
cut -b 7-11 message.txt
```

`-c` character
`-f` field
`-b` byte

use the delimeter:
```bash
cut -d " " -f 1,2 message.txt
cut -d ":" -f 1 /etc/passwd
```
