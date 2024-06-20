observe usb ports on system in tree-view:
```bash
lsusb -t
```

observe PCI devices on machine:
```bash
lspci
```

observe almost every hardware on system in html format:
```bash
sudo lshw -html > hwinfo.html
# Short version of output:
sudo lshw -short
```

info about CPU:
```bash
lscpu
```

list block-storage devices (disks):
```bash
lsblk
```