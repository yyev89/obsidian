list all vms on host:
```bash
vboxmanage list vms
```

list all running vms:
```bash
vboxmanage list runningvms
```

start vm:
```bash
vboxmanage startvm <name>
# In headless mode:
vboxmanage startvm <name> --type headless
```

show properties of vm:
```bash
vboxmanage showinfo <name>
```

shutdown vm:
```bash
vboxmanage controlvm <name> acpipowerbutton
# Force shutdown:
vboxmanage controlvm <name> poweroff
```

restart vm:
```bash
vboxmanage controlvm <name> reset
```

remove vm from virtualbox:
```bash
vboxmanage unregistervm <name>
# Delete vm completely:
vboxmanage unregistervm <name> --delete
```