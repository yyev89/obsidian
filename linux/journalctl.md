show all messages with priority level 3 (errors) from this boot:
```bash
journalctl -b --priority=3
```

delete journal logs which are older than 2 days:
```bash
journalctl --vacuum-time=2d
```

show only last N lines and follow new messages:
```bash
journalctl --lines N --follow
```

show all messages by a specific unit:
```bash
journalctl -u unit
```

show logs for a given unit sisnce the last time it started:
```bash
journalctl _SYSTEMD_INVOCATION_ID=$(systemctl show --value --property=InvocationID unit)
```

filter messages within a time range:
```bash
journalctl --since now|today|yesterday|tomorrow --until YYYY-MM-DD HH:MM:SS
```

show all messages by a specific process:
```bash
journalctl _PID=pid
```

show all messages by a specific executable:
```bash
journalctl path/to/exec
```

show only entries logged at the error level or above:
```bash
journalctl -p err -b -x
```
- `-p` means priority or log level
- `-x` means to add explanatory help texts to log messages in the output where this is available

show only entries for specified syslog identifier:
```bash
journalctl -t dockerd
```
