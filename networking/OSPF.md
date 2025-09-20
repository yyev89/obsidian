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
### Routers
- internal routers - all interfaces in single area
- backbone routers - at least one interface in area 0
- area border routers (ABR)
	- interface(s) in area 0 _and_ another area
	- maintain an LSDB for _each_ area
	- summarizes LSAs between areas
- autonomous system border routers (ASBR) - redistributing foreign routes into OSPF