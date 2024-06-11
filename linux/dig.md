Default output:
```bash
dig navek.org
```

Lookup for NS (or any other) records:
```bash
dig ns navek.org
```

Query against other server, instead of in resolv.conf:
```bash
dig @8.8.8.8 navek.org
```

Short version of output, just only server IP's:
```bash
dig navek.org +short
```

Show domain name, TTL, type of record and IP's:
```bash
dig navek.org +noall +answer
```

`+yaml` output in a yaml format
`+norecurse` not recursive search

Full recursive lookup with no-cached TTL:
```bash
dig navek.org +trace
```

Reverse DNS lookup:
```bash
dig -x 8.8.8.8
```