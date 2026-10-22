[PingMyNetwork Sandbox Challenge](https://pingmynetwork.com/sandbox)

# MST, Named EIGRP and DHCP 

```

```

<img width="3189" height="1230" alt="image" src="https://github.com/user-attachments/assets/22687ec9-d803-48f6-bb99-fb95492ed05d" />

```

```

## Overview

This lab demonstrates MST configuration across four Layer 3 switches (DW1–DW4), Named EIGRP for dynamic routing, and DHCP services provided by DW1 to three PCs.<br>
MST is configured on all switches with two instances to manage Layer 2 loop prevention and optimize traffic paths across VLANs.<br>
Named EIGRP enables dynamic route exchange between Layer 3 switches, ensuring network reachability, while DHCP automates IP address assignment to end hosts.<br>
<br>

## IP Addressing and VLANs

There are 4 vlans 10,20,30,40<br>
First addresses are used as gateway. Addresses 1 to 10 are excluded from the dhcp pools<br>.
<br>
Vlan 10 --> 192.168.10.0/24<br>
Vlan 20 --> 192.168.20.0/24<br>
Vlan 40 --> 192.168.40.0/24<br>
<br>

## Configurations

### DW1 

```
Current configuration : 2531 bytes
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname DSW1
!
boot-start-marker
boot-end-marker
!
!
!
ip dhcp excluded-address 192.168.10.1 192.168.10.9
ip dhcp excluded-address 192.168.20.1 192.168.20.9
ip dhcp excluded-address 192.168.40.1 192.168.40.9
!
ip dhcp pool Clients10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1 
 dns-server 8.8.8.8 
ip dhcp pool Clients20
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1 
 dns-server 8.8.8.8 
ip dhcp pool Clients40
 network 192.168.40.0 255.255.255.0
 default-router 192.168.40.1 
 dns-server 8.8.8.8 
!
no aaa new-model
!
ip routing
!
ip cef
no ipv6 cef
!
spanning-tree mode mst
spanning-tree portfast edge default
spanning-tree extend system-id
!
spanning-tree mst configuration
 name CAMPUS
 revision 1
 instance 1 vlan 10, 20
 instance 2 vlan 40
!
spanning-tree mst 1 priority 8192
!
interface FastEthernet0/1
 switchport access vlan 10
!
interface GigabitEthernet0/1
 switchport trunk allowed vlan 10,20,40
 negotiation auto
!
interface GigabitEthernet0/2
 switchport trunk allowed vlan 10,20,40
 negotiation auto
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan10
 ip address 192.168.10.1 255.255.255.0
!
interface Vlan20
 ip address 192.168.20.2 255.255.255.0
!
router eigrp INE
 !
 address-family ipv4 unicast autonomous-system 100
  !
  topology base
  exit-af-topology
  network 192.168.0.0
  network 192.168.0.0 0.0.255.255
 exit-address-family
!
ip forward-protocol nd
!
ip http server
ip http secure-server
!
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
!
control-plane
!
line con 0
line aux 0
line vty 0 4
!
!
end
```
### DW2 
```
Current configuration : 2140 bytes
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname DSW2
!
boot-start-marker
boot-end-marker
!
no aaa new-model
!
ip routing
!
ip cef
no ipv6 cef
!
spanning-tree mode mst
spanning-tree portfast edge default
spanning-tree extend system-id
!
spanning-tree mst configuration
 name CAMPUS
 revision 1
 instance 1 vlan 10, 20
 instance 2 vlan 40
!
spanning-tree mst 1 priority 4096
!
interface FastEthernet0/1
 switchport access vlan 20
!
interface GigabitEthernet0/1
 switchport trunk allowed vlan 10,20,40
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
interface GigabitEthernet0/2
 switchport trunk allowed vlan 10,20,40
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan20
 ip address 192.168.20.1 255.255.255.0
 ip helper-address 192.168.10.1
!
router eigrp INE
 !
 address-family ipv4 unicast autonomous-system 100
  !
  topology base
  exit-af-topology
  network 192.168.0.0
  network 192.168.0.0 0.0.255.255
 exit-address-family
!
ip forward-protocol nd
!
ip http server
ip http secure-server
!
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
!
control-plane
!
line con 0
line aux 0
line vty 0 4
!
!
end
```
### DW3
```
Current configuration : 2105 bytes
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname DSW3
!
boot-start-marker
boot-end-marker
!
no aaa new-model
!
ip routing
!
ip cef
no ipv6 cef
!
spanning-tree mode mst
spanning-tree portfast edge default
spanning-tree extend system-id
!
spanning-tree mst configuration
 name CAMPUS
 revision 1
 instance 1 vlan 10, 20
 instance 2 vlan 40
!
interface GigabitEthernet0/1
 switchport trunk allowed vlan 10,20,40
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
interface GigabitEthernet0/2
 switchport trunk allowed vlan 10,20,40
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan30
 ip address 192.160.30.1 255.255.255.0
!
interface Vlan20
 ip address 192.168.20.3 255.255.255.0
!
router eigrp INE
 !
 address-family ipv4 unicast autonomous-system 100
  !
  topology base
  exit-af-topology
  network 192.168.0.0
  network 192.168.0.0 0.0.255.255
 exit-address-family
!
ip forward-protocol nd
!
ip http server
ip http secure-server
!
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
!
control-plane
!
line con 0
line aux 0
line vty 0 4
!
!
end
```
### DW4
```
Current configuration : 2164 bytes
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname DSW4
!
boot-start-marker
boot-end-marker
!
no aaa new-model
!
ip routing
!
ip cef
no ipv6 cef
!
spanning-tree mode mst
spanning-tree portfast edge default
spanning-tree extend system-id
!
spanning-tree mst configuration
 name CAMPUS
 revision 1
 instance 1 vlan 10, 20
 instance 2 vlan 40
!
interface FastEthernet0/1
 switchport access vlan 40
!
interface GigabitEthernet0/1
 switchport trunk allowed vlan 10,20,40
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
interface GigabitEthernet0/2
 switchport trunk allowed vlan 10,20,40
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan40
 ip address 192.168.40.1 255.255.255.0
 ip helper-address 192.168.10.1
!
interface Vlan20
 ip address 192.168.20.4 255.255.255.0
!
router eigrp INE
 !
 address-family ipv4 unicast autonomous-system 100
  !
  topology base
  exit-af-topology
  network 192.168.0.0
  network 192.168.0.0 0.0.255.255
 exit-address-family
!!
ip forward-protocol nd
!
ip http server
ip http secure-server
!
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
!
control-plane
!
line con 0
line aux 0
line vty 0 4
!
end
```

