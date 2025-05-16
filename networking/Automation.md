The various functions of network devices can be logically divided up into *planes*:

- Data plane: tasks involved in forwarding user data/traffic from one interface to another (forwarding plane)
- Control plane: controls what the data plane does, ex. by building router's routing table and other tables (overhead work)
- Management plane: consists of protocols that are used to manage devices and does'n directly affect the forwarding of data (SSH, telnet, SNMP, syslog)

__SDN__ (Software-Defined Networking) is an approach that centralizes the control plane into an application called a *controller*, which can interact with devices via APIs and control plane functions.

**SBI** (Southbound Interface) is used for communications between the controller and the network devices - communication protocol + API:
- OpenFlow
- Cisco OpFlex
- Cisco onePK
- NETCONF

**NBI** (Northbound Interface) allows us to interact with the controller, access the data it gathers about the network, program it and make changes via the SBI.

**Underlay** is the underlying physical network of devices and connections, which provide IP connectivity (ie. using IS-IS).

**Overlay** is the virtual network built on top of the physical underlay network.

**Fabric** is the combination of the overlay and underlay: the physical and virutual network as a whole.

