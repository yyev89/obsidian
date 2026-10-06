BGP fundamentals:

- forms static neighborships
- uses TCP port 179, so it is a client/server relationship
- advertises NLRI (network layer reachability information): prefix with mask, path attributes (PA)

Ignore paths that:
- have a next hop IP address that is not reachable
- have a path that contains your own local AS number

1. Prefer the path with the highest WEIGHT
2. Prefer the path with the highest LOCAL_PREF
3. Prefer the path what was locally ORIGINATED
4. Prefer the path with the shortest AS_PATH
5. Prefer the path with the lowest ORIGIN type
6. Prefer the path with the lowest Multi-Exit Discriminator (MED)
7. Prefer eBGP over iBGP paths
8. Prefer the path with the lowest IGP metric to the BGP next hop
9. Prefer the older path
10. Prefer the path with the peering router that has the lower router ID
11. Prefer the path with the peer who has the lower neighbor address

check configured possible neighbors (only with prefixed received are actually formed):
```
show ip bgp summary | ex Active|Idle
```

show which prefixes are learned from whom:
```
show ip bgp
```

show more details - all attributes for each network:
```
show ip bgp 1.0.0.0/24
```

Autonomous System (AS) - a collection of networks overseen by one organization.
Internet is made up of many AS.
Interior Gateway Protocols (IGP) operate within an AS (EIGRP, OSPF, IS-IS).
Exterior Gateway Protocols (EGP) connect different autonomous systems: MP-BGP (multi-protocol v4), Different Address-Families.

AS Numbers:
- 2 byte original range 1 to 65535
- 4 byte new range 65535 to 4294967295

Public - assigned by IANA through RIRs and ISPs:
- 1 to 64511
- 65536 to 4199999999

Private - assigned by you:
- 64512 to 65534
- 4200000000 to 4294967294

eBGP - peering between directly connected routers in different AS.
iBGP - peering between any routers in same AS.

Configuration example
our side:
```
conf t
router bgp 65012
neighbor 203.0.113.89 remote-as 65000
address-family ipv4 unicast
neighbor 203.0.113.89 activate
do sho run | s bgp
do sho ip bgp sum
```

remote side:
```
conf t
router bgp 65000
neighbor 203.0.113.91 remote-as 65012
address-family ipv4 unicast
neighbor 203.0.113.91 activate
do sho run | s bgp
do sho ip bgp sum
do sho ip bgp nei 203.0.113.89
```

auth (MD5-hash protected):
```
neighbor 203.0.113.91 password letmein
```

hard reset:
```
do clear ip bgp *
```

soft reset:
```
do clear ip bgp * in[out]
```

Phases:
- **Idle** - BGP process is waiting for next attempt to establish a peering
- **Connect** - TCP connection beeing established
- **Active** - TCP connection was not successfull, a second attempt is made, if successful Open msg is sent, if not return to Idle state
- **Open sent** - BPG Open messages are being sent
- **Open confirm** - BGP Open messages have been successfully sent and received
- **Established** - neighbor details match, peering is successful, Update messages can be exchanged

eBPG neighbors could be not directly connected, but need to change TTL setting (by default it assumes routers to be directly connected - minimum incoming 0, outgoing 1). iBGP TTL is 255 by default.
