list interfaces with detailed info:
```bash
ip address|addr|a
```

list interfaces with brief network layer info (colored):
```bash
ip -br -c a
```

display the routing table:
```bash
ip route|r
```

show neighbors (ARP table):
```bash
ip neighbour|n
```

make an interface up/down:
```bash
ip link set interface up|down
```

add/delete an IP address to and interface:
```bash
ip a add|del ip/mask dev interface
```

add a default route:
```bash
ip route add default via ip dev interface
# Exalmple:
ip route add 192.168.55.0/24 via 192.168.1.254 dev eth0
```