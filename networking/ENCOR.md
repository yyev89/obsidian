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

### ARP

check the table (all entries, only entries using IP):
```
show arp
show ip arp
```

Proxy ARP allowes a device (usually a router) to respond to ARP requests for IP addresses that are not its own
```
# enable/disable globally:
[no] ip arp proxy disable
# on interface:
[no] ip proxy-arp
```

check if enabled:
```
show ip interface g0/0
...
Proxy ARP is enabled
...
```

Gratuitous ARP is an ARP reply message sent without receiving an ARP request and serves the purpose of udate switches and hosts MAC address tables
- announcing when an interface is enabled
- announcing a change in MAC address
- failover between redundant devices (FHRP)

manually configure ARP entry:
```
arp 192.168.1.2 5254.001e.b897 arpa
```

check detailed info:
```
show arp 192.168.1.2 detail
```

clear arp table:
```
clear arp
```

NOTE: the device will send a unicast ARP request to try to refresh each entry before removing it from the table (if there is no reply after 3 attempts, the entry is cleared)

set the ARP age in seconds:
```
interface g0/0
 arp timeout 180
```