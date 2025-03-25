user EXEC mode:
```
Router>
```

privileged EXEC mode:
```
Router> enable[en]
Router#
```

view available commads:
```
?
# view completion of the last word:
enable pass?
# view next options:
enable password ?
```

enter global configuration mode:
```
configure terminal[conf t]
# prompt changes to:
Router(config)#
```

enable password:
```
enable password TEST
```

**Running-config** = the current, active conf file on the device, as you enter commands in the CLI - you edit the active configuration.
**Startup-config** = the conf file that will be loaded upon restart of the device.

view config file:
```
show running-config[sh run]
show startup-config
```

save the configuration:
```
write[write memory|copy running-config startup-config]
```

enable password encryption (only for enable password command):
```
service password-encryption
```

set password with better encryption than default:
```
enable secret TEST
```

cancel next commands behaviour (previous will stay the same):
```
no service password-encryption
```

execute a privileged-exec level command from global configuration mode:
```
do command
```

show arp table:
```
show arp
```

show mac-address table:
```
show mac address-table
```

remove all dynamically learned mac-addresses or just one:
```
clear mac address-table dynamic
clear mac address-table dynamic address aaaa.bbbb.cccc.dddd
# from specific interface:
clear mac address-table dynamic interface Gi0/0
```

discover interfaces, addresses and statuses (*Status* column means layer 1 status, *Protocol* - layer 2 status):
```
show ip interface brief[sh ip int br]
```

configure interface:
```
interface gigabitethernet0/0[int g0/0]
```

set up ip address with subnet mask and enable it:
```
ip address 10.255.255.254 255.0.0.0[ip add]
no shutdown[no shut]
# check the result:
do show ip interface brief[do sh ip int br]
```

get detailed info about interface:
```
show interfaces g0/0
```

get interface description:
```
show interfaces description[sh int desc]
# set description:
int g0/0
description ## to SW1 ##[desc]
```

get status of switch interfaces:
```
show interfaces status
```

configure switch interface:
```
int f0/1
speed 100
duplex full
description ## to R1 ##
```

shutdown unused switch interfaces (config-if-range):
```
interface range f0/5 - 12
# possible range coma separated:
interface range f0/5 - 6, f0/9 - 12
description ## not in use ##
shutdown
```

display router's routing table:
```
show ip route
```

add static route:
```
(config)#ip route <ip-address> <netmask> <next-hop>
(config)#ip route <ip-address> <netmask> <exit-interface>
(config)#ip route <ip-address> <netmask> <exit-interface> <next-hop>
# example:
ip route 192.168.4.0 255.255.255.0 192.168.13.3
ip route 192.168.1.0 255.255.255.0 g0/0
# default route:
ip route 0.0.0.0 0.0.0.0 203.0.113.2
```

filter output:
```
show running-config | include <string>
# example:
show running-config | include ip route
```