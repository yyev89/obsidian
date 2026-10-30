show all physical volumes (drives/partitions/RAID):
```bash
pvdisplay
```

show logical volumes:
```bash
lvdisplay
```

create physical volumes out of drives:
```bash
pvcreate /dev/sdb /dev/sdc /dev/sdd /dev/sde
```

create volume group named "bucket":
```bash
vgcreate bucket /dev/sdb /dev/sdc /dev/sdd /dev/sde
# Show created group:
vgdisplay
```

create logical volume named "BIG_SLICE":
```bash
lvcreate -L 32G -n BIG_SLICE bucket 
```

create a filesystem on the lv:
```bash
mkfs.ext4 /dev/bucket/BIG_SLICE
```

extend the lv with 5 gigabytes:
```bash
lvextend -L+5G /dev/bucket/BIG_SLICE
```
