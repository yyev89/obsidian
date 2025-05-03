simple start config:
```
hostname LAB-ASA
domain-name cisco.com
username bob password supersecret123
enable password supersecret123
# enable ssh and allow specific network:
ssh version 2
ssh 10.0.0.0 255.0.0.0 management
# enable http:
http server enable
http 10.0.0.0 255.0.0.0 management
```

AAA:
```
aaa authentication ssh console LOCAL
aaa authentication http console LOCAL
aaa authentication serial console LOCAL
```

clock:
```
clock timezone EST -5
clock summer-time EDT recurring
```

check interfaces:
```
show interface ip brief
```

management interface conf:
```
int m0/0
 nameif management
 management-only
 security-level 99
 no shut
sh run int m0/0
```

add default route:
```
route management 10.0.11.0 255.255.255.0 10.0.2.1
```

logging setup:
```
logging enable
logging buffered debugging
logging trap informational
logging host management 10.0.3.4
```

ntp setup:
```
ntp server 10.0.2.1 source management
```

disable snmp services (can be vulnarability):
```
no snmp-server enable
```

upgrade version:
```
dir
show version
# check md5 after download:
verify /md5 disk0:/{file}
boot system disk0:/{file}
asdm image disk0:/{file}
reload
```

ACL typically inbound and on the outside interface. Example with DMZ:
```
int g0/2
 namif ASA-DMZ
 security-level 50
 ip address 172.16.0.1 255.255.255.0
access-list outside_access_in extended permit tcp any host 172.16.0.2 eq http
access-list outside_access_in extended permit tcp any host 172.16.0.2 eq https
access-list outside_access_in extended deny ip any any
access-group outside_access_in in interface outside
```

check ACLs:
```
sh run access-list
sh access-list
```

static NAT configuration (one-to-one mapping):
```
object network web-outside
 host 56.10.22.8
!
object network web-inside
 host 172.16.0.2
!
nat (dmz,outside) static web-outside
# check:
sh run object
sh run nat
sh nat [detail]
sh xlate
```

dynamic NAT configuration (one-to-many mapping):
```
object network nat-pool
 range 56.10.22.9 56.10.22.11
!
object network inside
 subnet 10.1.10.0 255.255.255.0
!
nat (inside,outside) dynamic nat-pool
```

dynamic PAT configuration (one-to-many mapping with port address translation):
```
object network nat-pool
 range 56.10.22.9 56.10.22.11
!
object network inside
 subnet 10.1.10.0 255.255.255.0
!
nat (inside,outside) dynamic pat-pool nat-pool
```

policy NAT based on conditions:
```
object network inside
 subnet 10.1.10.0 255.255.255.0
!
object network IP-12
 host 56.10.22.12
!
object network IP-13
 host 56.10.22.13
!
object network Web-1
 host 4.2.2.2
!
object network Web-2
 host 8.8.8.8
!
nat (inside,outside) source dynamic inside IP-12 destination static Web-1 Web-1
nat (inside,outside) source dynamic inside IP-13 destination static Web-2 Web-2
```

security contexts (multiple logical FWs in a single appliance):
```
# change mode (reboot required):
mode multiple
# create contexts:
context Finance
 config-url disk0:/Finance.cfg
context Guest
 config-url disk0:/Guest.cfg
# allocate interfaces to contexts:
context Finance
 allocate-interface g1/2
 allocate-interface g1/3
context Guest
 allocate-interface g1/2
 allocate-interface g1/3
```

change context:
```
changeto context {name}
# admin context:
changeto context admin
# back to system:
changeto system
# check mode:
show mode
```

MPF (class-maps to match traffic, policy-maps to apply inspection):
```
sh run class-map
sh run policy-map
# disable some things:
policy-map global_policy
class inspection_default
 no inspect esmptp
 no inspect skinny
# see matches:
show service-policy
```

for active/standby HA there must be LAN failover link for configuration replication and sync, and Hello messages (keep-alives); standby addresses must be configured on interfaces:
```
int g0/0
 description ASA-Outside
 ip address 56.10.22.7 255.255.255.240 standby 56.10.22.8
!
int g0/1
 description ASA-Inside
 ip address 10.1.10.1 255.255.255.0 standby 10.1.10.2
```

active/standby config example:
```
failover
failover lan unit {primary|secondary}
failover lan interface LANFAILOVER g0/7
failover polltime unit msec 200 holdtime 2
failover polltime interface msec 500 holdtime 5
failover key ***
failover replication http
failover link STATEFAILOVER g0/8
failover interface ip LANFAILOVER 1.1.1.1 255.255.255.252 standby 1.1.1.2
failover interface ip STATEFAILOVER 2.2.2.1 255.255.255.252 standby 2.2.2.2
```

check status:
```
show failover
# flip standby to active (from active):
failover exec standby failover active
```

site-to-site VPN between ASA and router example configuration, ASA side:
```
object network local-lan1
 subnet 10.1.10.0 255.255.255.0
 description Local VPN Subnet
!
object network remote-lan1
 subnet 10.2.10.0 255.255.255.0
 description Remote VPN Subnet
!
access-list outside_cryptomap_1 extended permit ip object local-lan1 object remote-lan1
!
nat (inside,outside) source static local-lan1 local-lan1 destination static remote-lan1 remote-lan1 no-proxy-arp route-lookup
!
crypto ikev1 policy 1
 authentication pre-share
 encryption aes-256
 hash sha
 group 5
!
tunnel-group 12.88.43.99 type ipsec-l2l
tunnel-group 12.88.43.99 general-attributes
 default-group-policy GroupPolicy_12.88.43.99
tunnel-group 12.88.43.99 ipsec-attributes
 ikev1 pre-shared-key 53&63!422#dHs
!
group-policy GroupPolicy_12.88.43.99 internal
group-policy GroupPolicy_12.88.43.99 attributes
 vpn-tunnel-protocol ikev1
!
crypto ipsec ikev1 transform-set ESP-AES-256-SHA esp-aes-256 esp-sha-hmac
!
crypto map outside_map 1 match address outside_cryptomap_1
crypto map outside_map 1 set peer 12.88.43.99
crypto map outside_map 1 set ikev1 transform-set ESP-AES-256-SHA
crypto map outside_map interface outside
!
crypto ikev1 enable outside
!
route outside 10.2.10.0 255.255.255.0 56.10.22.7
```

site-to-site VPN between ASA and router example configuration, ASA side:
```
ip access-list extended vpn
 permit ip 10.2.10.0 0.0.0.255 10.1.10.0 0.0.0.255
!
ip access-list extended nat
 deny ip 10.2.10.0 0.0.0.255 10.1.10.0 0.0.0.255
!
crypto isakmp policy 1
 authentication pre-share
 encr aes 256
 hash sha
 group 5
!
crypto isakmp key 53&63!422#dHs address 56.10.22.7
!
crypto ipsec transform-set ESP-AES-256-SHA esp-aes 256 esp-sha-hmac
 mode tunnel
!
crypto map LAB-VPN 1 ipsec-isakmp
 description Tunnel to 56.10.22.7
 set peer 56.10.22.7
 set transform-set ESP-AES-256-SHA
 match address vpn
!
interface g1
 crypto map LAB-VPN
!
ip route 10.1.10.0 255.255.255.0 12.88.43.1
```

### ACL

create ACL for allowing ping from anywhere to anything:
```
access-list global_access extended permit icmp any any
show access-list [acl_name]
```

apply it to work on every interface (can be applied only 1 at a time, next will override this):
```
access-group global_access global
show run access-group
```

traffic between two interfaces with the same security level is denied by default

create ACL to allow traceroute from linux machines via UDP and apply it to inside interface:
```
access-list Traceroute permit udp any any range 33434 33464
access-group Traceroute in interface inside
```

rename ACL:
```
access-list Traceroute rename inside_in
```

create object-group for http/https traffic:
```
object-group service web tcp
description Web Browsing
port-object eq http
port-object eq https
```

create ACL with the OG:
```
access-list inside_in remark Allow web browsing from the inside
access-list inside_in permit tcp any any object-group web
```

create object for one server-host:
```
object network Server-Intranet
host 172.16.0.1
description Intranet Server
```

create ACL with the object:
```
access-list outside_in remark Allow HTTPS
access-list outside_in permit tcp any object Server-Intranet eq https
access-group outside_in in interface outside
```

packet-tracer utility:
```
packet-tracer input inside tcp 192.168.0.1 80 123.1.7.10 80
```

remove ACL from the firewall (BE CAREFULL):
```
clear configure access-list [acl_name]
```

### NAT

static NAT conf example:
```
object network Server-Intranet-Public
host 200.1.1.1
desc Intranet server mapped ip
exit
object network Server-Intranet-Outside
host 172.16.0.1
desc Intranet server real ip
nat (dmz,outside) static Server-Intranet-Public
```

dynamic NAT conf example:
```
object network Inside-Subnet
subnet 192.168.0.0 255.255.255.0
desc Workstation subnet
exit
object network Internet-PAT
host 200.1.1.254
desc Mapped general internet ip
exit
nat (inside,outside) source dynamic Inside-Subnet Internet-PAT desc General internet access
```

dynamic NAT using pool:
```
object network Internet-PAT-Pool
range 200.1.1.253 200.1.1.254
exit
nat (inside,outside) source dynamic Inside-Subnet pat-pool Internet-PAT-Pool desc General internet access
```

identity NAT conf example:
```
# access router from the PC via ssh:
object network WS-1
host 192.168.0.1
desc Workstation 1
nat (inside,outside) static 192.168.0.1
exit
access-list inside_in permit tcp 192.168.0.1 255.255.255.255 20.0.0.1 255.255.255.255 eq 22
```