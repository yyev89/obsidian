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