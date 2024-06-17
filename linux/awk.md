print third column in text file:
```bash
awk '{ print $3 }' minimovies.txt
```

print third column in second line of text file:
```bash
awk 'NR == "2" { print $3 }' minimovies.txt
```

run the awk script file:
```bash
awk -f hello.awk
```

simple script example:
```bash
#!/usr/bin/awk -f
BEGIN {
	print "hello, world!"
}
```

`'{}'` tells the shell to treat the entire block as a single
`NR` number of records (number of lines contained in a text)
`NF` number of fields (number of variables in each line)
`FILENAME` variable which holds name of the file, provided to awk

print number of lines, first, and fourth columns of file:
```bash
awk '{ print NR, $1, $4 }' size.txt
```

print last column in each line:
```bash
awk '{ print $NF }' size.txt
```

