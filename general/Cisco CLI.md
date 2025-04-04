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
show ip interface brief | exclude una
```

### VLANs

discover vlans on a switch:
```
show vlan brief
```

add interfaces to a vlan:
```
interface range g1/0 - 3
switchport mode access
switchport access vlan 10
```

rename vlan:
```
# also creates vlan if doesn't exist:
vlan 10
name ENGINEERING
```

trunk interface configuration:
```
interface g0/0
switchport trunk encapsulation dot1q
switchport mode trunk
# check:
show interfaces trunk
```

configure list of vlans allowed:
```
int g0/0
switchport trunk allowed vlan 10,30
switchport trunk allowed vlan add 20
switchport trunk allowed vlan remove 20
```

change native vlan (better to choose unused vlan for security):
```
switchport trunk native vlan 1001
```

configure router on a stick:
```
interface g0/0
no shutdown
interface g0/0.10
encapsulation dot1q 10
ip address 192.168.1.62 255.255.255.192
```

reset to default settings on the interface:
```
default interface g0/1
```

### Layer-3 switches

enable layer 3 routing on a switch:
```
ip routing
interface g0/1
no switchport
```

SVI (switch virtual interface) configuration:
```
interface vlan10
ip address <ip> <mask>
no shutdown
```

get info about the interface switch mode:
```
show interfaces g0/0 switchport
```

disable DTP (dynamic trunking protocol):
```
switchport nonegotiate
# switchport mode access also disables DTP on interface
```

### Spanning Tree

enable portfast option on the interface (only connected to end-host in access mode):
```
interface g0/0
spanning-tree portfast
# enable on trunk port:
spanning-tree portfast trunk
# enable on all access ports:
(config)# spanning-tree portfast default
```

enable BPDU guard:
```
interface g0/2
spanning-tree bpduguard enable
(config)# spanning-tree postfast bpduguard default
```

enable classic spanning tree (not rapid):
```
spanning-tree mode pvst
# rapid (default):
spanning-tree mode rapid-pvst
```

configure primary (secondary) root bridge:
```
spanning-tree vlan 1 root primary[secondary]
```

configure port settings:
```
spanning-tree vlan 1 cost 200
spanning-tree vlan 1 port-priority 32
```

show interface details:
```
show spanning-tree interface <name> detail
```

ErrDisable recovery feature:
```
# status:
show errdisable recovery
# enable for bpduguard:
errdisable recovery cause bpduguard
# change default interval:
errdisable recovery interval <seconds>
```

activate BPDU filter (on all portfast-enabled ports):
```
spanning-tree portfast bpdufilter default
```

enable Root Guard on the interface:
```
spanning-tree guard root
```

### EtherChannel

check current load-balancing method:
```
show etherchannel load-balance
```

change method:
```
port-channel load-balance src-dst-mac
```

create etherchannel:
```
interface range g0/0 - 3
channel-group 1 mode desirable[active|passive|auto|desirable|on]
```

manually change etherchannel protocol:
```
channel-protocol lacp[pagp]
```

show summary:
```
show etherchannel summary
show etherchannel port-channel
```

layer 3 etherchannel:
```
int range g0/0 - 3
no switchport
channel-group 1 mode active
int po1
ip address <ip> <mask>
```

### RIP

configuration:
```
router rip
version 2
no auto-summary
network 10.0.0.0
network 172.16.0.0
```

configure passive interface (which has no neighbors and there is no need to send anything there, including loopback interfaces):
```
passive-interface g2/0
```

send default gateway to RIP neighbors:
```
default-information originate
```

get useful info:
```
show ip protocols
```

set max paths to load-balance for ECMP:
```
maximum-paths 8
```

set default administrative distance:
```
distance 85
```

### EIGRP

configuration:
```
router eigrp <AS-number>
no auto-summary
passive-interface g2/0
network 10.0.0.0
network 172.16.1.0 0.0.0.15
```

set router id:
```
eigrp router-id 1.1.1.1
```

create loopback interface on a router:
```
interface loopback 0[int l0]
ip address 1.1.1.1 255.255.255.255
```

show eigrp neighbors:
```
show ip eigrp neighbors
```

show all network topology:
```
show ip eigrp topology
```

### OSPF

configuration:
```
router ospf <process-id>
network <ip> <wildcard-mask> area <number>
passive-interface <number>
default-information originate
# example:
router ospf 1
network 10.0.12.0 0.0.0.3 area 0
passive-interface g0/2
```

manually set router id:
```
router-id 1.1.1.1
# refresh router id from global config mode:
clear ip ospf process
```

show ospf database on a router:
```
show ip ospf database
```

check neighbors:
```
show ip ospf neighbor
```

view detailed info about interfaces:
```
show ip ospf interface
# short and more compact version:
show ip ospf interface brief
```

change default reference bandwidth:
```
auto-cost reference-bandwidth <megabits-per-second>
```

change cost manually on the interface:
```
int g0/0
ip ospf cost 10000
```

activate OSPF directly on the interface:
```
int <number>
ip ospf <process-id> area <number>
# example:
int g0/0
ip ospf 1 area 0
```

configure all interfaces as passive:
```
router ospf 1
passive-interface default
# set interface as active:
no passive-interface g0/0
```

change interface priority:
```
int g0/0
ip ospf priority <0-255>
```

configure the OSPF network type on the interface:
```
ip ospf network broadcast[non-broadcast|point-to-point|point-to-multipoint]
```

shutdown OSPF as a process (without losing configuration):
```
router ospf 1
shutdown
```

set intervals on the interface:
```
ip ospf hello-interval <seconds>
ip ospf dead-interval <seconds>
# reset to defaults:
no ip ospf hello-interval
no ip ospf dead-inerval
```

configure auth for OSPF neighborship on the interface:
```
# set pass:
ip ospf authentication-key pass123
# enable auth:
ip ospf authentication
```

point-to-point setup for serial connection:
```
show controllers <interface-id>
encapsulation ppp[hdlc]
# on DCE side:
clock rate <bits-per-second>
```

### HSRP

1st router configuration:
```
interface g0/0
# set version of protocol(1 or 2):
standby version 2
# group number and virtual ip address:
standby 1 ip 172.16.0.254
# set priority (active router election):
standby 1 priority 200
# enable preemtion only on the active router (higher priority router takes back his role):
standby 1 preempt
```

2nd:
```
standby version 2
standby 1 ip 172.16.0.254
show standby
```

### IPv6

configuration:
```
ipv6 unicast-routing
int g0/0
ipv6 adress 2001:db8:0:0::1/64
no shutdown
show ipv6 interface brief
```