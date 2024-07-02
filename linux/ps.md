list all running processes:
```bash
ps aux
```

list all running processes including full command string:
```bash
ps auxww
```

search for a process that matches a string (no matching itself):
```bash
ps aux | grep [s]tring
```

list all processes of the current user in extra full format:
```bash
ps --user $(id -u) -F
```

list all processes of the current user as a tree:
```bash
ps --user $(id -u) f
```

get the parent PID of a process:
```bash
ps -o ppid= -p pid
```

sort processes by memory consumption:
```bash
ps --sort size
```

