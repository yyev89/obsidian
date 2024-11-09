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
- pros: more portable, widely supported
- cons: narrower selection of conditional

keywords are special words that parsed by shell, have no analog binary:
```bash
[[ 2 -eq 2 ]]; echo "two equals two"
```
- more modern and feature reach
- allows to use > < 
- support grouping expressions with () and pattern matching, regex
- pros: more support, wider selection of conditional evaluation
- cons: lack of backward compatibility, not POSIX compliant

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

- EOF - token, can be anything, which has 1:1 match
- EOF - End Of File
- EOL - End Of Line

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
`{}` protect as boundaries
`""` expands variable as an atomic token (not list)

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

- `##` longest prefix (to the last) pattern removal
- `%%` longest suffix (the the last) pattern removal
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

- `$$` PID of the parent shell
- `$BASHPID` PID of the current shell

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

### Special variables

`$?` stores the exit status of the command, script, or function

Reserved exit codes:
- `0` success
- `2` misuse of shell built-ins
- `126` cannot execute
- `127` command not found
- `128+n` fatal error signal "n"
- `130` script terminated by Control-C
- `255*` exit status out of range
- `128+` recommended to use in own scripts

- `set -e` flag that makes a script exit when a command produces a non-success exit status
- `set -u` exits when an unset variable is used
- `set -o pipefail` catches errors in piped commands

Example:
```bash
#!/usr/bin/env bash
set -e
readonly conf_file="./fqdn.properties"
readonly server_names="server1 server2 server3"
readonly default_user="mummshad"
readonly error_file=150

terminate() {
	local msg="${1}"
	local code="${2:-160}"
	echo "Error: ${msg}" >&2
	exit "${code}"
}

if [[ ! -s "${conf_file}" ]]; then
	terminate "FQDN file is empty" "${error_file}"
fi

fqdn=$(cat "${conf_file})

for server in ${server_names}; do
	echo "${default_user}@${server}.${fqdn}"
done

exit 1
```

`$#` stores the number of arguments passed to a script or function

If no arguments provided to script, it will use empty string instead. To avoid unexpected behavior, user guard clause:
```bash
#!/usr/bin/env bash
if [[ ! $# -eq 1 ]]; then
	echo "Error: provide one argument"
	exit 1
fi

echo ${1}
```

IFS (Internal Field Separator) - special shell variable
IFS=$' \t\n' Ansi-C quoting block with space, tab, new line

Redefining default value of IFS inside the script:
```bash
#!/usr/bin/env bash
IFS=":"

elements="element1:element2:element3"
for element in ${elements}; do
	echo "${element} is now separated from the elements list"
done
```

`$*`,`$@` package all the command-line arguments into 1 single variable, that holds all the values in one single place:
```bash
#!/usr/bin/env bash
echo "Number of arguments: $#"
echo "All arguments: $@"

for arg in "$@"; do
	echo "Argument: $arg"
done
```

BUT: `$@` keeps every single argument separated from each other, while `$*` groups them togather in one entity and cant be accessed individually

Adding these variables directly in the FOR loop without prior variable assignation is only recommended when IFS isn't being overwritten

`$!` stores information about the last executed command tha was sent to the background:
```bash
#!/usr/bin/env bash
jmeter_server_pid=""

start_jmeter() {
	echo "Starting jmeter..."
	jmeter-server &
	jmeter_server_pid=$(echo $!)
}

start_jmeter

# Simulating more commands running:
echo "more commands running..."

echo "Terminating jmeter server by using ${jmeter_server_pid} PID"
kill -SIGTERM "${jmeter_server_pid}"
```

`$0` can help get the name of the script being executed, and get the absolute path of the directory where a script is being executed

When used in interactive mode directly in the terminal, it gets the name of the parent shell for the current TTY session
```bash
#!/usr/bin/env bash
readonly work_dir=$(dirname $(readlink -f "$0"))
readonly script_name="${0##*/}"
cd "${work_dir}/.."
# More commands...
```

- `$_` represents the last (one!) argument of the previous command
- `$-` reflects the options or flags of the current shell

### Arrays

`declare` specify the type of data assigned to a variable:
```bash
#!/usr/bin/env bash
declare -i i=10
echo $(($i+1))
i="hello"
# Will output '0' (default value):
echo "${i}"
```

- `-i` type integer
- `-r` read only
- `-u` auto convert to UPPER case
- `-l` auto convert to lower case
- `-a` type array

To add value to an array [] with index number are used:
`course_section[0]="Intro"`

By default, if no index provided, array returns the first element with index 0

Get all elements at once:
```bash
#!/usr/bin/env bash
course_sections[0]="Intro"
course_sections[1]="Coding standards"

echo "${cource_sections[@]}"
```

Declare the array in one line, add elements:
```bash
#!/usr/bin/env bash
course_sections=("Intro" "Coding standards" "Refresher")
# Add element into array manually:
course_sections[3]="Syntax"
# Using construct to get the number of elements:
course_sections[${#course_sections[@]}]="Syntax"
# Will replace element at index 0:
course_sections="Something"

echo "${course_sections[@]}"
```

Insert element inside the array:
```bash
#!/usr/bin/env bash
declare -a servers=("server1" "server2" "server3")

# Insert new element at index 1, shifting the remaining elements:
servers=("${servers[@]:0:1})" "server1.5" "${servers[@]:1}"

echo "${servers[@]}"
```

- `unset servers[1]` delete element with index '1'
- `unset servers` delete all elements from the array

Append elements to the end of the array:
```bash
array+=("four" "five" "six")
```

Sort array with `sort` command:
```bash
printf "%s\n" "${chars[@]}" | sort
```

Associative array (-A):
```bash
#!/usr/bin/env bash
declare -A drawer=(["shirts"]="T-Shirts and polo" ["sports"]="All sports clothing" ["socks"]="Everyday socks")
# Add new element:
drawer["coats"]="Some winter coats"
# Replace existing element's value:
drawer["socks"]="Another socks"
# Remove element:
unset drawer["socks"]
# Remove all:
unset drawer[@]
# Show only key names:
echo "${!drawer[@]}"

echo "${drawer["socks"]}"
```

No-op command ("no output" equivavelnt to dry-run option):
```bash
#!/usr/bin/env bash
if [[ "$1" = "start" ]]; then
	:
else
	echo "Invalid command."
fi
```

### Logging
```bash
#!/usr/bin/env bash
log() {
	echo $(date -u +"%Y-%m-%dT%H:%M:%SZ") "${@}"
}

log "hello world"
```

### Tricks
rerun the command with the last arguments used:
```bash
cd test
# Will repeat:
!cd
```

use the argument from last command in current one:
```bash
ls -la doc.txt
# Refence the last (same as nvim doc.txt):
nvim !$
# Reference by position: !:0 (ls) !:1 (-la) !:2 (doc.txt)
```

replace command word from previous run (fix the typo for example):
```bash
yay -Qq | fuzzy --preview='yay -Qs {1}' | xargs -r yay -Qi
# Change fuzzy to fzf:
^fuzzy^fzf
```
