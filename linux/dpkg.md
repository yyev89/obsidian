list all installed packages:
```bash
dpkg -l
```

dpkg database location:
```bash
ls -l /var/lib/dpkg
```

info about packages, statuses, etc:
```bash
cat /var/lib/dpkg/status
```

show where the package is installed:
```bash
dpkg -L vim
```

status of the package:
```bash
dpkg -s vim
```

remove the package:
```bash
dpkg -r nginx
```