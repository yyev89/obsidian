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