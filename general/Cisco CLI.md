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

EUI-64 address:
```
int g0/0
ipv6 address 2001:db8::/64 eui-64
no shutdown
```

enable ipv6 on interface without explicitly configuring ipv6 address (link-local):
```
int g0/1
ipv6 enable
```

show ipv6 neighbor table (like ARP in ipv4):
```
show ipv6 neighbor
```

SLAAC (stateless address auto-configuration) config which uses NDP (neighbor discovery):
```
int g0/0
ipv6 address autoconfig
```

### ACL

numbered standard: 1-99, 1300-1999
numbered extended: 100-199, 2000-2699
named (both): name

configure standard numbered ACL:
```
access-list <number> deny[permit] <ip> <wildcard>
# example:
access-list 1 deny 1.1.1.1 0.0.0.0
# wildcard-mask can be omited, which means the same (/32):
access-list 1 deny 1.1.1.1
# allow other traffic (because of implicit deny default ACE):
access-list 1 permit any
# "any" means the same as 0.0.0.0 255.255.255.255
```

add description to the ACL:
```
access-list 1 remark ## BLOCK BOB FROM ACCOUNTING ##
```

display all ACL's on the router:
```
show access-lists
show ip access-lists
```

apply ACL to an interface:
```
int g0/0
ip access-group <number> {in|out}
```

configure standard named ACL:
```
ip access-list standard <acl-name>
[entry-number] {deny|permit} <ip> <wildcard>
# example:
ip access-list standard BLOCK_BOB
5 deny 1.1.1.1
10 permit any
remark ## CONFIGURED NOV 21 2020 ##
interface g0/0
ip access-group BLOCK_BOB in
```

check:
```
show running-config | section access-list
```

remove the rule from the list in the ACL config mode:
```
R1(config-std-nacl)# no 30
```

**BUT**: when configuring/editing numbered ACLs from global config mode, you can't delete individual entries, only the entire ACL:
```
no access-list 1
```

resequence ACL entries:
```
ip access-list resequence <acl-id> <starting-seq-num> <increment>
# example:
ip access-list resequence 1 10 10
```

extended numbered ACL:
```
access-list <number> {permit|deny} <protocol> <src-ip> <dest-ip>
```

extended named ACL:
```
ip access-list extended {name|number}
[seq-num] {permit|deny} <protocol> <src-ip> <dest-ip>
```

few examples:
```
# allow all traffic:
permit ip any any
# prevent 10.0.0.0/16 from sending UDP to 192.168.1.1/32:
deny udp 10.0.0.0 0.0.255.255 host 192.168.1.1
# prevent 172.16.1.1/32 from pinging hosts in 192.168.0.0/24:
deny icmp host 172.16.1.1 192.168.0.0 0.0.0.255
# allow traffic from 10.0.0.0/16 to access the server at 2.2.2.2/32 using HTTPS:
permit tcp 10.0.0.0 0.0.255.255 host 2.2.2.2 eq 443
# prevent all hosts using source UDP port numbers from 20000 to 30000 from accessing the server at 3.3.3.3/32:
deny udp any range 20000 30000 host 3.3.3.3
# allow hosts in 172.16.1.0/24 using a TCP source port greater than 9999 to access all TCP ports on server 4.4.4.4/32 except port 23:
permit tcp 172.16.1.0 0.0.0.255 gt 9999 host 4.4.4.4 neq 23
```

### CDP, LLDP

show CDP info:
```
# timers:
show cdp
# traffic:
show cdp traffic
# basic info about interfaces:
show cdp interface
# table: 
show cdp neighbors [detail]
# for specific neighbor:
show cdp entry <name>
```

CDP configuration:
```
# enable:
cdp run
# enable on the interface:
cdp enable
# timer:
cdp timer <sec>
# holdtime:
cdp holdtime <sec>
# enable v2:
cdp advertise-v2
```

LLDP configuration:
```
# enable:
lldp run
# on interface:
lldp transmit
lldp receive
# timers:
lldp timer <sec>
lldp holdtime <sec>
lldp reinit <sec>
```

show LLDP info:
```
show lldp
show lldp traffic
show lldp interface
# table:
show lldp neighbors [detail]
show lldp entry <name>
```

### NTP

check time (default zone is UTP):
```
show clock [detail]
```

manually configure time:
```
clock set 14:30:00 27 Dec 2020
```

manually configure calendar (hardware clock):
```
calendar set 14:35:00 27 Dec 2020
# check:
show calendar
# sync the calendar to the clock's time:
clock update-calendar
# sync the clock to the calendar:
clock read-calendar
```

configure timezone:
```
clock timezone JST 9
```

summer-time:
```
clock summer-time EDT recurring 2 Sunday March 02:00 1 Sunday November 02:00
```

configure NTP:
```
ntp server <ip> [prefer]
```

show servers information:
```
show ntp associations
```

show overall info:
```
show ntp status
```

update hardware time:
```
ntp update-calendar
```

configure device as a reference:
```
# server mode:
ntp master
# symmetric active mode:
ntp peer <ip>
```

NTP auth:
```
ntp authenticate
ntp authentication key <key-number> md5 <key>
ntp trusted-key <key-number>
# for clients only:
ntp server <ip> key <key-number>
```

### DNS

configure router as a DNS server:
```
ip dns server
ip host R1 192.168.0.1
ip host PC1 192.168.0.101
...
ip name-server 8.8.8.8
ip domain lookup
```

as a client:
```
ip name-server <ip>
ip domain lookup
```

view configured and learned hosts:
```
show hosts
```

set the default domain name:
```
ip domain name jeremysitlab.com
```

### DHCP

configure router as a DNS server:
```
service dhcp
ip dhcp excluded-address 192.168.1.1 192.168.1.10
ip dhcp pool LAB_POOL
network 192.168.1.0 /24
dns-server 8.8.8.8
domain-name jeremysitlab.com
default-router 192.168.1.1
# lease time in <days> <hours> <minutes> or <infinite>:
lease 0 5 30
```

check ip-addresses bindings:
```
show ip dhcp binding
show ip dhcp pool
show ip dhcp server statistics
```

configure router as a relay agent:
```
int g0/1
ip helper-address 192.168.10.10
# check:
do show ip interface g0/1
```

(rare) configure router as a DHCP client:
```
int g0/1
ip address dhcp
```

### SNMP

SNMPv2c configuration:
```
snmp-server contact jeremy#jeremysitlab.com
snmp-server location Jeremy's House
snmp-server community Jeremy1 ro
snmp-server community Jeremy2 rw
# specify the NMS, version, community:
snmp-server host 192.168.1.1 version 2c Jeremy1
# configure trap types to send to NMS:
snmp-server enable traps snmp linkdown linkup
snmp-server enable traps config
```

### Syslog

message format:
```
seq:time stamp: %facility-severity-MNEMONIC:description
```

view buffer content:
```
show logging
```

configuration:
```
# configure logging to the console line (level number or keyword):
logging console 6
# to the vty lines (resets on every session):
terminal monitor
logging monitor informational
# to the buffer ([size], level):
logging buffered 8192 6
# to an external server:
logging 192.168.1.100
logging host 192.168.1.100
logging trap debugging
```

to show logs not interrupting  of typing the command:
```
line console 0
logging synchronous
```

enable timestamps:
```
service timestamps log datetime
```

enable sequense numbers:
```
service sequence-numbers
```

### telnet, SSH

configure a password on the console line:
```
line console 0
password ccna
login
end
```

require a username:
```
username jeremy secret ccnp
line console 0
login local
end
```

set timeout in minutes and seconds:
```
exec-timeout 3 30
```

assign IP address to an SVI on a switch:
```
interface vlan1
ip address 192.168.1.253 255.255.255.0
no shutdown
exit
ip default-gateway 192.168.1.254
```

telnet configuration:
```
enable secret ccna
username jeremy secret ccna
access-list 1 permit host 192.168.2.1
line vty 0 15
login local
exec-timeout 5 0
transport input telnet [ssh|all|none]
access-class 1 in
```

SSH configuration:
```
# check status:
show ip ssh
# check connections:
show ssh
# hostname and FQDN are used to name RSA keys:
hostname R1
ip domain name jeremysitlab.com
crypto key generate rsa
enable secret ccna
username jeremy secret ccna
access-list 1 permit host 192.168.2.1
ip ssh version 2
line vty 0 15
login local
exec-timeout 5 0
transport input ssh
access-class 1 in
```

### FTP, TFTP

view file systems:
```
show file systems
```

copy and set a new ios version by TFTP:
```
copy tftp: flash:
<address>
<filename>
show flash
conf t
boot system flash:<filename>
exit
write memory
reload
```

delete old file:
```
delete flash:<filename>
```

same with FTP:
```
ip ftp username cisco
ip ftp password cisco
exit
copy ftp: flash:
<address>
<filename>
```