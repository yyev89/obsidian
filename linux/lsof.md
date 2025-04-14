show for concrete file:
```bash
lsof /var/log/syslog
```

open files in current directory:
```bash
lsof +d .
```

for user:
```bash
lsof -u yyev89
```

for command:
```bash
lsof -c vim
```

for PID:
```bash
lsof -p 1234
```

combine (with "add" flag):
```bash
lsof +d . -a -c vim
```

open network connections(ipv4 and port):
```bash
sudo lsof -i 4 :8080
```

check for node running on localhost:
```bash
sudo lsof -c node -a -i
```

same without domain name:
```bash
sudo lsof -c node -a -ni
```

for specific ip address:
```bash
sudo lsof -i @127.0.0.1
```

full syntax:
```bash
sudo lsof -i<protocol>@<host><port>
# example:
sudo lsof -iTCP:443
```