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

- `'{}'` tells the shell to treat the entire block as a single
- `NR` number of records (number of lines contained in a text)
- `NF` number of fields (number of variables in each line)
- `FILENAME` variable which holds name of the file, provided to awk

print number of lines, first, and fourth columns of file:
```bash
awk '{ print NR, $1, $4 }' size.txt
```

print last column in each line:
```bash
awk '{ print $NF }' size.txt
```

`-F` specify the field separator:
```bash
awk -F ":" '{ print $1 }' sizeV1.txt
```

`BEGIN` token with meaning not to expect any input\

`-v` declare a variable before executing the action block or program:
```bash
awk -v var="hello, world" 'BEGIN { print var }'
awk -F "|" -v var="Employee's first name: " '{ print var, $2 }' employees.txt
```

using conditions:
```bash
awk -F "|" -v high_salary="9000" '$7 >= high_salary { print $2 }' employees.txt
awk -F "|" -v high_salary="9000" -v low_salary="6500" '$7 >= high_salary || $7 <= low_salary { print $2 }' employees.txt
```

combine with shell:
```bash
#!/usr/bin/env bash
awk -v hello="hello world" 'BEGIN {
	print hello
}'
```


