translate or transform
make output to uppercase:
```bash
echo "hello, world" | tr [a-z] [A-Z]
# Alternate:
echo "hello, world" | tr [:lower:] [:upper:]
```

delete  all lowercase characters:
```bash
echo "hello, world" | tr -d [a-z]
```

delete all alphabetical characters:
```bash
cat some.txt | tr -d [:alpha:]
```

squeeze duplicate characters ('tt'):
```bash
cat message.txt | tr -s "t"
```

replace text:
```bash
cat message.txt | tr "!" "."
```
