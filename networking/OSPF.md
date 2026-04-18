### Tables
#### Neighbor table
- directly connected OSPF routers
- state of adjacency
```
show ip ospf neighbor
```
#### Topology table
- everything OSPF knows about
- link state database (LSDB)
- each enty in LSDB is know as link state advertisement (LSA)
```
show ip ospf database
```
#### Routing table
- router's routing table (not solely a function of OSPF)
- OSPF will contribute its best routes to routing table
### Packets
#### Hello
- priodically sent to 224.0.0.5 (multicast address for all OSPF routers)
- discover other OSPF routers
- includes info about sending router
- determines whether adjacency will form
- some networks don't support multicast, hello packets sent unicast (typically every 30 seconds)
1. **Router ID** - 32 bit identity of each router
2. **Hello interval** - frequency of periodic Hello's
3. **Dead interval** - duration to remember neighbor
4. **Neighbors** - neighbor router ID(s) seen on link, validates two-way reachability
5. **Area ID** - OSPF area interface belongs to
6. **Authentication data** - password restricted peering
7. **Network mask** - subnet mask for link
8. **Area type** - normal, stub, NSSA
9. **DR** - designated router on multi-access link (any link with potential for multi-access), central point for all updates on link, reduces redundant updates
10. **BDR** - backup designated router
11. **Priority** - 0-255 (default: 1)
 
2, 3, 5, 6, 7, 8 must match for adjacency
#### Database descriptor (DBD)
- summary of LSAs in each router's LSDB
- avoids sending full LSDB for each neighbor
#### Link state request (LSR)
- sent to request full LSA(s)
#### Link state update (LSU)
- includes requested LSA(s)
#### Link state acknowledgment (LSAck)
- sent to confirm reception of LSU
### Areas
- OSPF routers maintain identical LSDBs
- changes _anywhere_ propagated _everywhere_
- network can be segregated using **areas** (limits propagation to confined sections)
- area design create a 2-tier hierarchy:
	- area 0 - top of hierarchy - backbone area
	- area # - all other areas: 1 - 4294967295
- traffic between areas must traverse area 0
- assures loop free area topologies
- hub and spoke design
1. **Normal** - default area type
2. **Stub** - no redistributed routes, replaced with default route
3. **NSSA** - not so stubby area, no redistribution except from local area, optionally replaced with default route
### Routers
- internal routers - all interfaces in single area
- backbone routers - at least one interface in area 0
- area border routers (ABR)
	- interface(s) in area 0 _and_ another area
	- maintain an LSDB for _each_ area
	- summarizes LSAs between areas
- autonomous system border routers (ASBR) - redistributing foreign routes into OSPF

### OSPF Adjacency
- state: **DOWN** - initial state when OSPF first configured, sending periodic Hellos to 224.0.0.5
- state: **INIT** - received a Hello packet, outbound Hellos now include Peer router ID
- state: **2-WAY** - router sees itself in neighbor's Hello (reachability), routers decide if adjacency will proceed
- state: **EXSTART** - master / slave election, governs reliable DBD exchange, higher router ID becomes master
- state: **EXCHANGE** - master /slave election is complete, slave sends confirming DBD, peers exchange LSDB summaries
- state: **LOADING** - peers know LSAs in neighbor's LSDB, peers begin requesting full LSAs (LSR, LSU, LSAck)
- state: **FULL** - LSDB's are synchronized, adjacency complete

### Configuring OSPF commands

check if any dynamic protocols are enabled:
```
sh ip protocols
# show LSDB for the router:
sh ip ospf database [router router-id]
```

enable OSPF process number 110:
```
conf t
router ospf 110
# set router-id:
router-id 1.1.1.1
```

check interfaces in OSPF:
```
sh ip ospf interface [brief]
```

add interface to OSPF:
```
network 10.0.12.1 0.0.0.0 area 0
```

check if any neighbors in OSPF exist:
```
sh ip ospf neighbor
```

show routing information base (routing table) for OSPF:
```
sh ip ospf rib
```

add couple of interfaces (loopbacks starting with 10.2.) to OSPF via network with /16 mask:
```
network 10.2.0.0 0.0.255.255 area 0
```

add interface to OSPF from interface configuration:
```
conf t
interface Loopback27
ip ospf 110 area 0
```

### Cost

OSPF uses delay - additive metric based on link speed, less delay - better
- formula = reference bandwidth / link bandwidth
- default reference bandwidth = 100 mbps

check reference bandwidth/cost:
```
show ip ospf
! "Reference bandwidth unit is 100 mbps"
show ip ospf interface brief
! Cost of each interface in the "Cost" column
```

change reference bandwidth:
```
conf t
router ospf 110
auto-cost reference-bandwidth 1000
```

Note: cost field is 16 bits in size, max value is 65535

### LSA types
1. **Router LSA** - router identifies itself and it's links (within local area)
	- IP networks, subnet masks, consts for each router link
	- used to build topology map of local area
2. **Network LSA** - sent by designated router (DR) (within local area)
	- when multiple routers connected to the same multi-access link
	- DR interface IP
	- DR router ID
	- network mask
	- router ID of all attached routers
3. **Summary LSA** - contain IP networks from foreign areas (outside local area)
	- sent by Area Border Routers (ABRs), in both directions
	- summarizes type 1, type 2 from foreign areas
	- includes network ID and mask, and cost for ABR to reach target network
	- ABRs generate a type 3 LSA for _each_ IP network in a foreign area
	- ABRs generate type 3 LSAs in each direction, for each area they border
	- ABRs generate type 3 LSAs from other type 3 LSAs from foreign areas
	- each type 3 LSA includes one IP subnet
	- type 3 LSAs can create IP summarization boundaries
4. Routes redistributed into OSPF domain learned via type 4 and type 5 (outside OSPF domain)
	- **ASBR-Summary LSA** - instructions to reach ASBRs
	- sent by ABR, when ASBR is in a foreign area
5. **External LSA** - contain an IP subnet redistributed into OSPF
	- sent by Autonomous System Border Routers (ASBRs)
	- forwarded unchanged throughout OSPF domain
