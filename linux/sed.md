replace all occurances of 'sample' to 'are' from file in stdout:
```bash
sed 's/sample/are/g' poem.txt
```

`'p'` print script
`'d'` delete script
`'i'` insert data
`-n` print just the modified output
`-i` edit in place
`-e` indicates that the following block should be a sed script (used only if more than 1 script is needed)

print second line twice(unprocessed and processed line):
```bash
sed '2p' file_sample.txt
```

delete range of lines 3-5:
```bash
sed '3,5d' employees.txt
```

search and print the pattern in the file:
```bash
sed -n '/Manager/p' employees.txt
```

search for 'Manager' or 'IT':
```bash
sed -n -e '/\bManager\b/p' -e '/\bIT\b/p' employees.txt
```

change 'IT' to 'Informational Technology' on 7 line only, 1st occurance in the line:
```bash
sed -e '7 s/\bIT\b/Informational Technology/1' employees.txt
```

insert header as 1st line of the document:
```bash
sed '1iID|Name|Job|Email|Salary' employees.txt
```
