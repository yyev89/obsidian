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

### Layer 3 forwarding

configure multiple IP addresses on the interface:
```
int g0/0
 ip address 10.0.0.1 255.255.255.0
 ip address 10.0.5.1 255.255.255.0 secondary
 ip address 192.168.1.1 255.255.255.0 secondary
```

detailed information including secondary IPs:
```
show ip interface g0/0
```

enable directed broadcast (a message sent to the broadcast address of another subnet which the router is connected to):
```
int g0/1
 ip directed-broadcast
```

filter command to get cleaner output:
```
show ip route | exclude directly | variably
```