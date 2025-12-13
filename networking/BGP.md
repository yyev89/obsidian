Ground rules:
* an AS must look like a single "thing" from the outside
	- internal routes should not be exposed
	- policy shoud apper the same from all edges
- whoever has the packet determines how it will exit their AS
- it should be possible to ask to enter an AS at a particular point
- it should be possible to ask for other special packet handling
- it should be possible to hide information when needed without impacting overall routing operation
- it should be possible to scale to hundreds of milliones of routes, paths etc.

**NLRI** - network layer reachability information - reachable destination of any kind (v4, v6 etc)

**Attribute** - anything describing info about NLRI (policy markers, routing info etc)

**Route** - NLRI + attributes

**Speaker** - a device that runs BGP, the BGP process running on a device

**Peer** - a device running BGP that has a relationship with the local device

#### Path attributes:

- Weight - influences a best route for the router (outbound)
- Local Preference - influences the best route for all routers in an AS (outbound)
- AS_Path - lists the number of ASN's in the path (in/outbound)
- Origin - valued implying if route is IGP or EGP (outbound)
- MED - influences best route for routers in another AS (inbound)

#### Internal and External BGP

iBGP:
- BGP connectivity within the same AS
- routers do not update AS path
- should always be meshed

eBGP:
- external connectivity to other AS's
- routers update AS path

Public ASNs: 64,496 through 64,511 (assigned by IANA)
Private ASNs: 64,512 through 65,534

#### Updates:
- default route only
- full updates
- partial updates 

#### Advertising routes:
- network command
- residstribution
- propagation 
- aggregate-address command

#### Synchronization:
- if an AS provides transit service to another AS, then BGP should not advertise a route until all of the routers within the AS have learned about the route via an IGP

### Configuration

#### BGP configuration requirements:
- the router's own ASN
```
router bgp {asn}
```
- the IP address of each neighbor and that neighbor's ASN
```
neighbor {ip-address} remote-as {remote-asn}
```

#### BGP neighbor requirements:
- a local router's ASN must match the neighboring router's reference to that ASN
- the BGP router IDs of the two routers must not be the same
- if configured, auth must pass
- each router must establish a TCP connection with its neighbors
