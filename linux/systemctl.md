show all running services:
```bash
systemctl status
```

list failed units:
```bash
systemctl --failed
```

start, stop, reload, restart, show the status a service:
```bash
systemctl start|stop|restart|reload|status unit
```

enable or disable unit to start on bootup:
```bash
systemctl enable|disable unit
```

reload systemd, scan for new or chaged units:
```bash
systemctl daemon-reload
```

check if unit is active, enabled, failed:
```bash
systemctl is-active|is-enabled|is-failed unit
```

list all service, socket, automount units filtering by running or failed status:
```bash
systemctl list-units --type=service|socket|automount --state=failed|running
```

other types of units:
- device
- mount
- swap
- target
- path
- timer
- snapshot
- slice
- scope

show the contents and absolute path of a unit file:
```bash
systemctl cat unit
```

show low-level properties of a unit:
```bash
systemctl show unit
```

show unit's dependency tree:
```bash
systemctl list-dependencies unit
```

mark (unmark) a unit as completely unstartable, manual or auto:
```bash
systemctl mask|unmask unit
```

show unit status:
```bash
systemctl list-unit-files
```