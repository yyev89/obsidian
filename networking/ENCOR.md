### Layer 2 forwarding

useful commands:
```
show mac address-table
show mac address-table aging-time
show mac address-table learning
show mac address-table dynamic
show mac address-table count
(config) mac address-table aging time <time>
(config) [no] mac address-table learning vlan <vlans>
(config) mac address-table static <mac-address> vlan <vlan> {interface <interface> | drop}
clear mac address-table dynamic
```
