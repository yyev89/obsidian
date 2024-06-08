to check if command is built-in or not:
```bash
type echo
```

list of system keywords:
```bash
compgen -k
```

list of system built-in's:
```bash
compgen -b
```

search for PID (oldest):
```bash
pgrep -o bash
```

### built-ins and keywords:
built-ins commands are programs
built-in [ ] have analog binary "test":
```bash
test 2 -eq 2; echo "two equals two"
[ 2 -eq 2 ]; echo "two equals two"
man test
man [
```
pros: more portable, widely supported
cons: narrower selection of conditional

keywords are special words that parsed by shell, have no analog binary:
```bash
[[ 2 -eq 2 ]]; echo "two equals two"
```
more modern and feature reach
allows to use > < 
support grouping expressions with () and pattern matching, regex
pros: more support, wider selection of conditional evaluation
cons: lack of backward compatibility, not POSIX compliant

### guard clause:
```bash
[[ -f "file.txt" ]] || echo "file does not exist"
```
```bash
[[ -z ${1} ]] || echo "argument is empty"
```
```bash
[[ -f "file.txt" ]] || { echo "file does not exist"; exit 1; }
```

### heredocs:
output to stdout:
```bash
cat <<EOF
line1
line2
lineN
EOF
```

output to file:
```bash
cat > file.txt<<EOF
line1
line2
lineN
EOF
```
EOF - token, can be anything, which has 1:1 match
EOF - End Of File
EOL - End Of Line

```bash
ssh ubuntu@192.168.1.1 <<EOF
mkdir heredocs
echo "Here Docs sample contenct for a textfile" > ~/heredocs/heredocsfile.txt
EOF
```
```bash
sudo docker exec my_postgres_container bash -c "psql -U postgres -d employees << EOF
select * from employee;
EOF"
```

1. Assigne file descriptor `8` to `abc.txt`
2. Read the third character (starting from index `0`) from file descriptor `8`
3. Append the `d` character on the `3rd` position of file descriptor `8`
4. Close file descriptor `8`:
```bash
exec 8<> abc.txt
read -n 3 <& 8
echo "d" >& 8
exec 8>&-
```

Herestrings:
```bash
cat <<< "Hello world"

PRINT_THIS_MESSAGE="This is message"
cat <<< $PRINT_THIS_MESSAGE
```

### Expansions
Variable expansion:
```bash
#!/usr/bin/env bash
height=180
# Wrong:
echo "Your height is: $heightcm"
# Correct:
echo "Your height is: ${height}cm"
# Default value 'Unknown' for undefined varible 'name':
echo "Hello, ${name:-Unknown}"
```
`{}` - protect as boundaries
`""` - expands variable as an atomic token (not list)

String manipulation:
```bash
name="John Doe"
# Prints 'Hello, John':
echo "Hello, ${name:0:4}"
```

String replacement:
```bash
path="/home/user/file.txt"
# Change 'file' to 'data':
echo "{path/file/data}"
```

String length:
```bash
name="John Doe"
# Outputs '8':
echo "${#name}"
```

Using prefix and suffix (case sensetive):
```bash
greetings="Hello world"
# Remove 'H' or 'Hello' from string, must match the beginning:
echo "${greetings#H}"
echo "${greetings#Hello}"
# Remove 'd':
echo "${greetings%d}"
# Remove everything before space to 'DevOps Engineer':
export position="Junion DevOps Engineer"
echo "${position#* }"
# Remove last word after space to 'Junior DevOps':
echo "${position% *}"
```

`##` - longest prefix (to the last) pattern removal
`%%` - longest suffix (the the last) pattern removal
```bash
my_text_file="/home/my_username/text_file.txt"
# Output 'text_file.txt':
echo "${my_text_file##*/}"
# Output '/home/my_username/text_file':
echo "${my_text_file%.*}"
echo "${my_text_file%%.*}"
```

Brace expansion:
```bash
echo file{1,2,3}
# Using range:
echo file{1..5}
# Nested:
echo {a,b}{1,2,3}
# Step-based range with step '2' (bash 4.0+):
echo file{1..10..2}
# Prefix and postfix:
echo pre-{A..C}-post
# Using in loops:
for i in {1..3}; do touch "file${i}.txt"; done
```

Command substitution expansion:
```bash
file_count=$(find . -type f | wc -l)
# Deprecated, not a best practice:
file_count=`find . -type f | wc -l`
```

subshell - child process by parent shell, that inherits env variables, but don't propagade variables back to parent shell, and generates an additional PID (can be performance impact)

Stores only stdout, NOT stderr, for this use:
```bash
files=$(ls -j 2>&1)
```

Example - we are reassigning var to 'b' inside a subshell, but back on the parent shell, it's value is still 'a':
```bash
#!/usr/bin/env bash
var="a"
subshell=$(var="b")
# Output 'a':
echo "${var}"
# Output 'b':
echo "${subshell}"
```

`$$` - PID of the parent shell
`$BASHPID` - PID of the current shell

Propagate values to parent shell example:
```bash
#!/usr/bin/env bash
# Creates unique number:
tmpfile="/tmp/$$.tmp"
counter=1

echo "${counter}" > ${tmpfile}

(
counter="$(($(cat ${tmpfile}) + 1))"
echo "${counter}" > ${tmpfile}
)

counter=$(cat ${tmpfile})
echo "${counter}"
# Removes a single temp file:
unlink "${tmpfile}"
```