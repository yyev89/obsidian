default output:
```bash
dig navek.org
```

lookup for NS (or any other) records:
```bash
dig ns navek.org
```

query against other server, instead of in resolv.conf:
```bash
dig @8.8.8.8 navek.org
```

short version of output, just only server IP's:
```bash
dig navek.org +short
```

show domain name, TTL, type of record and IP's:
```bash
dig navek.org +noall +answer
```

`+yaml` output in a yaml format
`+norecurse` not recursive search

full recursive lookup with no-cached TTL:
```bash
dig navek.org +trace
```

reverse DNS lookup:
```bash
dig -x 8.8.8.8
```