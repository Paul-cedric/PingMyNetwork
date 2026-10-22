[PingMyNetwork Sandbox Challenge](https://pingmynetwork.com/sandbox)

# MST, Named EIGRP and DHCP 



<img width="3189" height="1230" alt="image" src="https://github.com/user-attachments/assets/22687ec9-d803-48f6-bb99-fb95492ed05d" />



## Overview

This lab demonstrates MST configuration across four Layer 3 switches (DW1–DW4), Named EIGRP for dynamic routing, and DHCP services provided by DW1 to three PCs.
MST is configured on all switches with two instances to manage Layer 2 loop prevention and optimize traffic paths across VLANs.
Named EIGRP enables dynamic route exchange between Layer 3 switches, ensuring network reachability, while DHCP automates IP address assignment to end hosts.



## IP Addressing and VLANs

There are 4 vlans 10,20,30,40
First addresses are used as gateway. Addresses 1 to 10 are excluded from the dhcp pools.

Vlan 10 --> 192.168.10.0/24
Vlan 20 --> 192.168.20.0/24
Vlan 40 --> 192.168.40.0/24



## Devices configs

### DW1 
### DW2 
### DW3
### DW4

