### Patching

update all installed packages using only security-related updates:
```bash
dnf update --security
```

update the installed kernel package to the latest available version:
```bash
dnf update kernel
```

list the installed kernel module directories:
```bash
ls /lib/modules
```

list the files and directories in /boot, including installed kernels and related files:
```bash
ls /boot
```

display available and installed kernel package versions:
```bash
dnf list kernel
```

remove the specified older kernel package:
```bash
dnf remove kernel-5.14.0-687.42.1.el9_8
```

set the maximum number of install-only packages, such as kernels, that DNF keeps installed:
```
/etc/dnf/dnf.conf:
installonly_limit=3
```

install the kpatch live-kernel-patching utility:
```bash
dnf install kpatch
```

list the live kernel patches currently installed or available to kpatch:
```bash
kpatch list
```

install the kpatch patch matching the currently running kernel version:
```bash
dnf install "kpatch-patch = $(uname -r)"
```

display the changelog for the specified kpatch package, which can be used to review CVEs addressed by the patch:
```bash
rpm -q --changelog kpatch-patch-4_18_0-348-1-3.el8 | less
```

download available package updates without installing them, allowing packages to be pre-cached before a maintenance window:
```bash
dnf update --downloadonly
```

install DNF Automatic so package updates can be downloaded or applied automatically according to its configuration:
```bash
dnf install dnf-automatic
```

### SELinux

list all SELinux port definitions and show which SELinux types are assigned to each port:
```bash
semanage port -l
```

add TCP port **2022** to the SELinux `ssh_port_t` type, allowing SSH to use this non-default port:
```bash
semanage port -a -t ssh_port_t -p tcp 2022
```

list all SELinux boolean settings and show whether each one is currently enabled (on) or disabled (off):
```bash
getsebool -a
```

enable the SELinux boolean `httpd_enable_homedirs`, allowing the Apache HTTP server to access users home directories. `-P` makes the change persistent across reboots:
```bash
setsebool [-P] httpd_enable_homedirs=1
```