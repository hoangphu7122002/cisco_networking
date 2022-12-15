### CONFIGURATION

#### IP DESCRIPTION
```shell
- config: 192.168.0.0/24 
  + camera-server: 192.168.0.2 255.255.255.0 
  + sensor-server: 192.168.0.3 255.255.255.0
  + dns-server: 8.8.8.8
- vlan1 - default: 192.168.1.0/24
- vlan2 - admin: 192.168.2.0/24
- vlan3 - sensor: 192.168.3.0/24
- vlan4 - computer: 192.168.4.0/24
- routing:
  + router0: 199.99.9.1 255.255.255.252
  + router1: 199.99.9.2 255.255.255.252
```
#### 1. VLAN ROUTER
```shell
#router
interface f0/0
no shutdown

- interface f0/0.1
- encapsulation dot1Q 1
- ip address 192.168.1.1 255.255.255.0

- interface f0/0.1
- encapsulation dot1Q 1
- ip address 192.168.1.1 255.255.255.0

- interface f0/0.2
- encapsulation dot1Q 2
- ip address 192.168.2.1 255.255.255.0

- interface f0/0.3
- encapsulation dot1Q 3
- ip address 192.168.3.1 255.255.255.0

- interface f0/0.4
- encapsulation dot1Q 1
- ip address 192.168.4.1 255.255.255.0
```
#### 2. VLAN DHCP
```shell
ip dhcp pool vlan1
- network 192.168.1.0 255.255.255.0
- default-router 192.168.1.1
- dns-server 8.8.8.8
- exit

ip dhcp pool vlan2
- network 192.168.1.0 255.255.255.0
- default-router 192.168.1.1
- dns-server 8.8.8.8
- exit

ip dhcp pool vlan3
- network 192.168.1.0 255.255.255.0
- default-router 192.168.1.1
- dns-server 8.8.8.8
- exit

ip dhcp pool vlan4
- network 192.168.1.0 255.255.255.0
- default-router 192.168.1.1
- dns-server 8.8.8.8
- exit
```

#### 3. ROUTING-RIP
```shell
#router 0
router rip
- version 2
- network access 199.99.9.0
- network access 192.168.0.0
- network access 192.168.1.0
- network access 192.168.2.0
- network access 192.168.3.0
- network access 192.168.4.0
- no auto-summary

#router 1
router rip
- version 2
- network access 199.99.9.0
```

#### 4. ACCESS CONTROL LIST
```shel
#to create
access-list 1 permit 192.168.1.0 0.0.0.255
access-list 1 permit 192.168.4.0 0.0.0.255
access-list 1 deny
int f0/0.1
- ip access-group 1 out
- to delete
no access-list 1
```

#### 5. SHOW HELPER
```shel
show vlan brief
Show ip interface brief
Show interface trunk
Show running-config
Show access-lists
```
