
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