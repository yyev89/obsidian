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