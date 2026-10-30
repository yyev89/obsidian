check APT statistics:
```bash
apt-cache stats
```

show all packages, not only installed ones:
```bash
apt-cache pkgnames
```

with "vim" in the name:
```bash
apt-cache pkgnames vim
```

print all available info about packages:
```bash
apt-cache dump | less
```

display all unmet dependencies available in the cache:
```bash
apt-cache unmet | less
```

get additional info about the package:
```bash
apt-cache show atop
```

show info about packages required for atop to be installed:
```bash
apt-cache depends atop
```

similar but for reverse dependencies:
```bash
apt-cache rdepends atop
```

clean APT cache:
```bash
du -h /var/cache/apt
apt-get clean
# Only not needed packages:
apt-get autoclean
```

remove the package and all its configuration files:
```bash
apt-get purge <package>
# Other variant:
apt-get remove --purge <package>
```