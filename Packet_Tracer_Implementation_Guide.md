# Cisco Packet Tracer Implementation Guide
## Future Vision Smart Campus Network Configuration

---

## Table of Contents

1. [Pre-Implementation Checklist](#pre-implementation-checklist)
2. [Configuration Overview](#configuration-overview)
3. [Core Router Configuration](#core-router-configuration)
4. [Building Router Configuration](#building-router-configuration)
5. [Switch Configuration](#switch-configuration)
6. [Firewall Configuration](#firewall-configuration)
7. [OSPF Routing Configuration](#ospf-routing-configuration)
8. [ACL Security Configuration](#acl-security-configuration)
9. [DHCP Configuration](#dhcp-configuration)
10. [End Device Configuration](#end-device-configuration)
11. [Testing & Verification](#testing--verification)
12. [Troubleshooting Guide](#troubleshooting-guide)

---

## Pre-Implementation Checklist

Before starting configuration, ensure:

- [ ] Topology is complete (see [Network Diagram Instructions](./NETWORK_DIAGRAM_INSTRUCTIONS.md))
- [ ] All devices are powered on
- [ ] All cables are properly connected (no red dots)
- [ ] You have the [Smart Campus Network Design Report](./Smart_Campus_Network_Design_Report.md) open for reference
- [ ] IP addressing plan is understood
- [ ] VLAN plan is documented
- [ ] Configuration backup plan is in place

---

## Configuration Overview

### Configuration Order

Follow this sequence for optimal implementation:

1. **Core Infrastructure First**: ISP Edge Router, Firewall
2. **Building Routers**: Router-A, Router-B, Router-C, Router-D
3. **Switches**: Configure VLANs and trunking
4. **Routing Protocol**: Enable OSPF on all routers
5. **Security**: Apply ACLs and firewall rules
6. **Services**: Configure DHCP, DNS
7. **End Devices**: Configure IP addressing
8. **Testing**: Verify connectivity and security

### Access Methods

**For Routers and Switches:**
- Click on device → **CLI** tab
- Or use **Config** tab for GUI configuration (not recommended for production)

**For End Devices (PCs, Servers):**
- Click on device → **Desktop** tab → **IP Configuration**

---

## Core Router Configuration

### Router-CORE (ISP Edge Router) - Cisco ISR 4331

This is the central hub router connecting all building routers.

#### Step 1: Basic Configuration

```cisco
enable
configure terminal

! Set hostname
hostname Router-CORE

! Configure console password
line console 0
 password cisco
 login
 logging synchronous
 exec-timeout 5 0
 exit

! Configure enable password
enable secret class

! Configure VTY (Telnet/SSH) access
line vty 0 4
 password cisco
 login
 logging synchronous
 transport input ssh
 exec-timeout 5 0
 exit

! Banner
banner motd #
****************************************************
*  Future Vision Smart Campus - Core Router       *
*  Unauthorized Access is Prohibited              *
****************************************************
#

! Service configuration
service password-encryption
no ip domain-lookup

end
write memory
```

#### Step 2: Interface Configuration

```cisco
configure terminal

! WAN Interface to Firewall
interface GigabitEthernet 0/0/0
 description WAN Link to ASA Firewall
 ip address 192.168.100.2 255.255.255.252
 no shutdown
 exit

! Link to Building A - Router-A
interface GigabitEthernet 0/0/1
 description Fiber Link to Router-A (Administration)
 ip address 192.168.1.1 255.255.255.252
 no shutdown
 exit

! Link to Building B - Router-B
interface GigabitEthernet 0/1/0
 description Fiber Link to Router-B (Academic)
 ip address 192.168.2.1 255.255.255.252
 no shutdown
 exit

! Link to Building C - Router-C
interface GigabitEthernet 0/1/1
 description Fiber Link to Router-C (Services)
 ip address 192.168.3.1 255.255.255.252
 no shutdown
 exit

! Link to Building D - Router-D
interface Serial 0/2/0
 description Link to Router-D (Sports)
 ip address 192.168.4.1 255.255.255.252
 clock rate 64000
 no shutdown
 exit

end
write memory
```

#### Step 3: Verify Configuration

```cisco
! Verify interfaces
show ip interface brief

! Expected output shows all interfaces "up/up"
! Gi0/0/0: 192.168.100.2
! Gi0/0/1: 192.168.1.1
! Gi0/1/0: 192.168.2.1
! Gi0/1/1: 192.168.3.1
! Se0/2/0: 192.168.4.1

! Test connectivity (after configuring firewall)
ping 192.168.100.1
```

---

## Building Router Configuration

### Router-A (Administration Building) - Cisco ISR 4321

#### Step 1: Basic Configuration

```cisco
enable
configure terminal

hostname Router-A

! Security configuration
enable secret class
service password-encryption
no ip domain-lookup

! Console and VTY
line console 0
 password cisco
 login
 logging synchronous
 exit

line vty 0 4
 password cisco
 login
 transport input ssh
 exit

banner motd #
****************************************************
*  Building A - Administration Router             *
*  Authorized Personnel Only                      *
****************************************************
#

end
write memory
```

#### Step 2: Interface Configuration (WAN)

```cisco
configure terminal

! WAN Interface to Core Router
interface GigabitEthernet 0/0/0
 description WAN Link to Core Router
 ip address 192.168.1.2 255.255.255.252
 no shutdown
 exit

end
write memory
```

#### Step 3: Configure Subinterfaces (VLANs)

**⚠️ CRITICAL: Router-on-a-Stick Configuration**

When using subinterfaces for VLAN routing (router-on-a-stick):
- ✅ **Physical interface**: NO IP address, only `no shutdown`
- ✅ **Subinterfaces**: Each has its own IP address with VLAN encapsulation

**Common Error**: If you assign an IP to the physical interface, you'll get:
```
% 192.168.10.0 overlaps with GigabitEthernet0/0/1
```

**Correct Configuration**:

```cisco
configure terminal

! LAN Interface Configuration (Trunk Port)
! IMPORTANT: Physical interface has NO IP address
interface GigabitEthernet 0/0/1
 description LAN Trunk to Switch-A
 no ip address           ← CRITICAL: No IP on physical interface!
 no shutdown
 exit

! VLAN 10 - Admin Staff
interface GigabitEthernet 0/0/1.10
 description Admin Staff VLAN
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.128
 no shutdown
 exit

! VLAN 11 - Admin Wi-Fi
interface GigabitEthernet 0/0/1.11
 description Admin Wi-Fi VLAN
 encapsulation dot1Q 11
 ip address 192.168.10.129 255.255.255.192
 no shutdown
 exit

! VLAN 12 - Admin Cameras
interface GigabitEthernet 0/0/1.12
 description Admin Security Cameras
 encapsulation dot1Q 12
 ip address 192.168.10.193 255.255.255.224
 no shutdown
 exit

end
write memory
```

#### Step 4: Verify Configuration

```cisco
show ip interface brief
show vlans

! Test WAN connectivity
ping 192.168.1.1
```

---

### Router-B (Academic Building) - Cisco ISR 4331

Follow similar pattern as Router-A, but with Building B VLANs:

```cisco
enable
configure terminal

hostname Router-B

! Basic security (same as Router-A)
enable secret class
service password-encryption
no ip domain-lookup

! WAN Interface
interface GigabitEthernet 0/0/0
 description WAN Link to Core Router
 ip address 192.168.2.2 255.255.255.252
 no shutdown
 exit

! LAN Trunk Interface
! IMPORTANT: No IP address on physical interface for router-on-a-stick
interface GigabitEthernet 0/0/1
 description LAN Trunk to Switch-B
 no ip address           ← No IP on physical interface!
 no shutdown
 exit

! VLAN 20 - Student Labs
interface GigabitEthernet 0/0/1.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.128
 no shutdown
 exit

! VLAN 21 - Teachers
interface GigabitEthernet 0/0/1.21
 encapsulation dot1Q 21
 ip address 192.168.20.129 255.255.255.192
 no shutdown
 exit

! VLAN 22 - Smart Boards
interface GigabitEthernet 0/0/1.22
 encapsulation dot1Q 22
 ip address 192.168.20.193 255.255.255.224
 no shutdown
 exit

! VLAN 23 - Academic Cameras
interface GigabitEthernet 0/0/1.23
 encapsulation dot1Q 23
 ip address 192.168.20.225 255.255.255.224
 no shutdown
 exit

end
write memory
```

---

### Router-C (Services & Library) - Cisco ISR 4321

```cisco
enable
configure terminal

hostname Router-C

enable secret class
service password-encryption
no ip domain-lookup

! WAN Interface
interface GigabitEthernet 0/0/0
 description WAN Link to Core Router
 ip address 192.168.3.2 255.255.255.252
 no shutdown
 exit

! LAN Trunk Interface
! IMPORTANT: No IP address on physical interface for router-on-a-stick
interface GigabitEthernet 0/0/1
 description LAN Trunk to Switch-C
 no ip address           ← No IP on physical interface!
 no shutdown
 exit

! VLAN 30 - Library Users
interface GigabitEthernet 0/0/1.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.192
 no shutdown
 exit

! VLAN 31 - Servers
interface GigabitEthernet 0/0/1.31
 encapsulation dot1Q 31
 ip address 192.168.30.65 255.255.255.224
 no shutdown
 exit

! VLAN 32 - Security Monitoring
interface GigabitEthernet 0/0/1.32
 encapsulation dot1Q 32
 ip address 192.168.30.97 255.255.255.224
 no shutdown
 exit

end
write memory
```

---

### Router-D (Sports & Events) - Cisco ISR 4221

```cisco
enable
configure terminal

hostname Router-D

enable secret class
service password-encryption
no ip domain-lookup

! WAN Interface
interface GigabitEthernet 0/0/0
 description WAN Link to Core Router
 ip address 192.168.4.2 255.255.255.252
 no shutdown
 exit

! LAN Trunk Interface
! IMPORTANT: No IP address on physical interface for router-on-a-stick
interface GigabitEthernet 0/0/1
 description LAN Trunk to Switch-D
 no ip address           ← No IP on physical interface!
 no shutdown
 exit

! VLAN 40 - Sports Staff
interface GigabitEthernet 0/0/1.40
 encapsulation dot1Q 40
 ip address 192.168.40.1 255.255.255.224
 no shutdown
 exit

! VLAN 41 - Scoreboards
interface GigabitEthernet 0/0/1.41
 encapsulation dot1Q 41
 ip address 192.168.40.33 255.255.255.224
 no shutdown
 exit

! VLAN 99 - Guest Wi-Fi
interface GigabitEthernet 0/0/1.99
 encapsulation dot1Q 99
 ip address 192.168.99.1 255.255.255.0
 no shutdown
 exit

end
write memory
```

---

## Switch Configuration

### Switch-A (Administration Building) - Cisco Catalyst 2960-24TT

#### Step 1: Basic Configuration

```cisco
enable
configure terminal

hostname Switch-A

enable secret class
service password-encryption
no ip domain-lookup

line console 0
 password cisco
 login
 exit

line vty 0 15
 password cisco
 login
 exit

banner motd #
****************************************************
*  Building A - Administration Switch              *
****************************************************
#

end
write memory
```

#### Step 2: Create VLANs

```cisco
configure terminal

! Create VLANs
vlan 10
 name Admin-Staff
 exit

vlan 11
 name Admin-WiFi
 exit

vlan 12
 name Admin-Cameras
 exit

end
write memory
```

#### Step 3: Configure Trunk Port (to Router)

```cisco
configure terminal

! Trunk port to Router-A
interface GigabitEthernet 0/1
 description Trunk to Router-A
 switchport mode trunk
 switchport trunk allowed vlan 10,11,12
 no shutdown
 exit

end
write memory
```

#### Step 4: Assign Access Ports to VLANs

```cisco
configure terminal

! VLAN 10 - Admin Staff PCs (Ports 2-21)
interface range FastEthernet 0/2-21
 description Admin Staff PCs
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
 no shutdown
 exit

! VLAN 10 - Printers (Ports 22-23)
interface range FastEthernet 0/22-23
 description Network Printers
 switchport mode access
 switchport access vlan 10
 no shutdown
 exit

! VLAN 11 - Admin Wi-Fi AP (Port 24)
interface FastEthernet 0/24
 description Admin WiFi Access Point
 switchport mode access
 switchport access vlan 11
 no shutdown
 exit

! VLAN 12 - IP Cameras (Ports 1-2) - Using first ports
interface range FastEthernet 0/1,FastEthernet 0/2
 description IP Security Cameras
 switchport mode access
 switchport access vlan 12
 no shutdown
 exit

end
write memory
```

---

### Switch-B (Academic Building) - Hierarchical Architecture

**Important**: Building B uses a **hierarchical three-tier switch architecture** to accommodate 75 student PCs + 20 teacher devices + 10 smart boards + cameras. This design provides better scalability and VLAN segregation.

**Architecture**:
```
Router-B (Gi0/0/1)
       |
       | [Trunk: VLANs 20,21,22,23]
       |
  Switch-B (Core/Distribution) - Cisco Catalyst 3650-24PS
       |
   ____|____________________
  |         |              |
VLAN 20   VLAN 21        VLAN 22
Switch-B1 Switch-B2      Switch-B3
(Students)(Teachers)  (Smart Boards)
```

#### Step 1: Configure Switch-B (Core/Distribution Switch)

```cisco
enable
configure terminal

hostname Switch-B

! Basic security
enable secret class
service password-encryption

! Create VLANs
vlan 20
 name Student-Labs
 exit
vlan 21
 name Teachers
 exit
vlan 22
 name Smart-Boards
 exit
vlan 23
 name Academic-Cameras
 exit

! Trunk to Router-B
interface GigabitEthernet 1/0/1
 description Trunk to Router-B
 switchport mode trunk
 switchport trunk allowed vlan 20,21,22,23
 no shutdown
 exit

! Trunk to Switch-B1 (Student Labs)
interface GigabitEthernet 1/0/2
 description Trunk to Switch-B1
 switchport mode trunk
 switchport trunk allowed vlan 20
 no shutdown
 exit

! Trunk to Switch-B2 (Teachers)
interface GigabitEthernet 1/0/3
 description Trunk to Switch-B2
 switchport mode trunk
 switchport trunk allowed vlan 21
 no shutdown
 exit

! Trunk to Switch-B3 (Smart Boards)
interface GigabitEthernet 1/0/4
 description Trunk to Switch-B3
 switchport mode trunk
 switchport trunk allowed vlan 22
 no shutdown
 exit

! VLAN 23 - Academic Cameras (directly connected)
interface range GigabitEthernet 1/0/22-24
 description Academic Cameras
 switchport mode access
 switchport access vlan 23
 no shutdown
 exit

end
write memory
```

#### Step 2: Configure Switch-B1 (Student Labs - VLAN 20)

```cisco
enable
configure terminal

hostname Switch-B1

! Basic security
enable secret class
service password-encryption

! Create VLAN
vlan 20
 name Student-Labs
 exit

! Trunk to Switch-B
interface GigabitEthernet 0/1
 description Trunk to Switch-B Core
 switchport mode trunk
 switchport trunk allowed vlan 20
 no shutdown
 exit

! Student Lab PCs (24 ports for students)
interface range FastEthernet 0/2-24
 description Student Lab PCs
 switchport mode access
 switchport access vlan 20
 spanning-tree portfast
 no shutdown
 exit

end
write memory
```

#### Step 3: Configure Switch-B2 (Teachers - VLAN 21)

```cisco
enable
configure terminal

hostname Switch-B2

! Basic security
enable secret class
service password-encryption

! Create VLAN
vlan 21
 name Teachers
 exit

! Trunk to Switch-B
interface GigabitEthernet 0/1
 description Trunk to Switch-B Core
 switchport mode trunk
 switchport trunk allowed vlan 21
 no shutdown
 exit

! Teacher ports (20 ports for teacher laptops/devices)
interface range FastEthernet 0/2-21
 description Teacher Devices
 switchport mode access
 switchport access vlan 21
 spanning-tree portfast
 no shutdown
 exit

! Academic WiFi AP (VLAN 21)
interface FastEthernet 0/24
 description Academic WiFi AP
 switchport mode access
 switchport access vlan 21
 no shutdown
 exit

end
write memory
```

#### Step 4: Configure Switch-B3 (Smart Boards - VLAN 22)

```cisco
enable
configure terminal

hostname Switch-B3

! Basic security
enable secret class
service password-encryption

! Create VLAN
vlan 22
 name Smart-Boards
 exit

! Trunk to Switch-B
interface GigabitEthernet 0/1
 description Trunk to Switch-B Core
 switchport mode trunk
 switchport trunk allowed vlan 22
 no shutdown
 exit

! Smart Boards (10 IoT smart boards)
interface range FastEthernet 0/2-11
 description Interactive Smart Boards
 switchport mode access
 switchport access vlan 22
 no shutdown
 exit

end
write memory
```

**Benefits of Hierarchical Design**:
- ✅ Scalability: Each access switch handles specific user groups
- ✅ Better performance: Traffic isolated at access layer
- ✅ Easier management: VLAN segregation per switch
- ✅ Port capacity: 72+ access ports total (24 per switch × 3)

---

### Switch-C (Services & Library) - Cisco Catalyst 2960-24TT

```cisco
enable
configure terminal

hostname Switch-C

enable secret class
service password-encryption

! Create VLANs
vlan 30
 name Library-Users
 exit
vlan 31
 name Servers
 exit
vlan 32
 name Security-Monitoring
 exit

! Trunk to Router-C
interface GigabitEthernet 0/1
 description Trunk to Router-C
 switchport mode trunk
 switchport trunk allowed vlan 30,31,32
 exit

! Library PCs (VLAN 30)
interface range FastEthernet 0/2-18
 switchport mode access
 switchport access vlan 30
 spanning-tree portfast
 exit

! Servers (VLAN 31)
interface range FastEthernet 0/22-24
 description Servers (NVR, IoT Management, Security Monitor)
 switchport mode access
 switchport access vlan 31
 exit

end
write memory
```

---

### Switch-D (Sports & Events) - Cisco Catalyst 2960-24TT

```cisco
enable
configure terminal

hostname Switch-D

enable secret class
service password-encryption

! Create VLANs
vlan 40
 name Sports-Staff
 exit
vlan 41
 name Scoreboards
 exit
vlan 99
 name Guest-WiFi
 exit

! Trunk to Router-D
interface GigabitEthernet 0/1
 description Trunk to Router-D
 switchport mode trunk
 switchport trunk allowed vlan 40,41,99
 exit

! Sports Staff PCs (VLAN 40)
interface range FastEthernet 0/2-4
 switchport mode access
 switchport access vlan 40
 exit

! Scoreboards (VLAN 41)
interface range FastEthernet 0/5-7
 switchport mode access
 switchport access vlan 41
 exit

! Guest WiFi APs (VLAN 99)
interface range FastEthernet 0/23-24
 switchport mode access
 switchport access vlan 99
 exit

end
write memory
```

---

## Firewall Configuration

### Cisco ASA 5506-X Configuration

**Note**: In Packet Tracer, ASA configuration may be simplified. Use available ASA device (5505 or 5506-X).

```cisco
enable
configure terminal

hostname ASA-Campus-FW

! Configure interfaces
interface GigabitEthernet 1/1
 nameif outside
 security-level 0
 ip address dhcp
 no shutdown
 exit

interface GigabitEthernet 1/2
 nameif inside
 security-level 100
 ip address 192.168.100.1 255.255.255.252
 no shutdown
 exit

! Default route to Internet
route outside 0.0.0.0 0.0.0.0 dhcp 1

! Static route to campus networks
route inside 192.168.0.0 255.255.0.0 192.168.100.2 1

! NAT Configuration (PAT)
object network CAMPUS-NET
 subnet 192.168.0.0 255.255.0.0
 nat (inside,outside) dynamic interface
 exit

! Access Control Lists
access-list OUTSIDE-IN extended deny ip any 192.168.0.0 255.255.0.0
access-list OUTSIDE-IN extended permit tcp any any established
access-list OUTSIDE-IN extended permit icmp any any echo-reply
access-list OUTSIDE-IN extended deny ip any any

access-list INSIDE-OUT extended permit tcp any any eq 80
access-list INSIDE-OUT extended permit tcp any any eq 443
access-list INSIDE-OUT extended permit tcp any any eq 53
access-list INSIDE-OUT extended permit udp any any eq 53
access-list INSIDE-OUT extended permit icmp any any
access-list INSIDE-OUT extended deny ip any any log

! Apply ACLs
access-group OUTSIDE-IN in interface outside
access-group INSIDE-OUT in interface inside

end
write memory
```

---

## OSPF Routing Configuration

### Configure OSPF on Router-CORE

```cisco
configure terminal

router ospf 1
 router-id 1.1.1.1
 log-adjacency-changes
 passive-interface GigabitEthernet 0/0/0
 network 192.168.1.0 0.0.0.3 area 0
 network 192.168.2.0 0.0.0.3 area 0
 network 192.168.3.0 0.0.0.3 area 0
 network 192.168.4.0 0.0.0.3 area 0
 default-information originate
 exit

end
write memory
```

### Configure OSPF on Router-A

```cisco
configure terminal

router ospf 1
 router-id 10.10.10.10
 log-adjacency-changes
 passive-interface GigabitEthernet 0/0/1
 network 192.168.1.0 0.0.0.3 area 0
 network 192.168.10.0 0.0.0.127 area 0
 network 192.168.10.128 0.0.0.63 area 0
 network 192.168.10.192 0.0.0.31 area 0
 exit

end
write memory
```

### Configure OSPF on Router-B

```cisco
configure terminal

router ospf 1
 router-id 20.20.20.20
 log-adjacency-changes
 passive-interface GigabitEthernet 0/0/1
 network 192.168.2.0 0.0.0.3 area 0
 network 192.168.20.0 0.0.0.127 area 0
 network 192.168.20.128 0.0.0.63 area 0
 network 192.168.20.192 0.0.0.31 area 0
 network 192.168.20.224 0.0.0.31 area 0
 exit

end
write memory
```

### Configure OSPF on Router-C

```cisco
configure terminal

router ospf 1
 router-id 30.30.30.30
 log-adjacency-changes
 passive-interface GigabitEthernet 0/0/1
 network 192.168.3.0 0.0.0.3 area 0
 network 192.168.30.0 0.0.0.63 area 0
 network 192.168.30.64 0.0.0.31 area 0
 network 192.168.30.96 0.0.0.31 area 0
 exit

end
write memory
```

### Configure OSPF on Router-D

```cisco
configure terminal

router ospf 1
 router-id 40.40.40.40
 log-adjacency-changes
 passive-interface GigabitEthernet 0/0/1
 network 192.168.4.0 0.0.0.3 area 0
 network 192.168.40.0 0.0.0.31 area 0
 network 192.168.40.32 0.0.0.31 area 0
 network 192.168.99.0 0.0.0.255 area 0
 exit

end
write memory
```

### Verify OSPF

On each router, verify OSPF is working:

```cisco
! Check OSPF neighbors
show ip ospf neighbor

! Expected: See neighbor relationships with adjacent routers
! Status should be "FULL"

! Check OSPF routing table
show ip route ospf

! Expected: See routes learned via OSPF (marked with 'O')

! Check OSPF database
show ip ospf database
```

---

## ACL Security Configuration

**⚠️ Important Note - Packet Tracer Compatibility**:
Cisco Packet Tracer does NOT support the `log` keyword in ACL statements. If you see configurations with `log` in other guides, **remove it for Packet Tracer**. All ACL configurations below are Packet Tracer compatible.

**Production IOS vs Packet Tracer**:
- ❌ Production: `deny ip 192.168.99.0 0.0.0.255 192.168.10.0 0.0.0.127 log` ← Has logging
- ✅ Packet Tracer: `deny ip 192.168.99.0 0.0.0.255 192.168.10.0 0.0.0.127` ← No log keyword

---

### Router-D: Guest Wi-Fi Isolation ACL

**Purpose**: Complete isolation of guest network from ALL internal networks (CRITICAL security requirement).

```cisco
configure terminal

! Extended ACL to restrict Guest Wi-Fi
ip access-list extended GUEST-RESTRICTION
 ! Block access to Admin VLAN
 deny ip 192.168.99.0 0.0.0.255 192.168.10.0 0.0.0.127
 ! Block access to Server VLAN
 deny ip 192.168.99.0 0.0.0.255 192.168.30.64 0.0.0.31
 ! Block access to Security Monitoring
 deny ip 192.168.99.0 0.0.0.255 192.168.30.96 0.0.0.31
 ! Allow HTTP/HTTPS
 permit tcp 192.168.99.0 0.0.0.255 any eq 80
 permit tcp 192.168.99.0 0.0.0.255 any eq 443
 ! Allow DNS
 permit tcp 192.168.99.0 0.0.0.255 any eq 53
 permit udp 192.168.99.0 0.0.0.255 any eq 53
 ! Deny everything else
 deny ip 192.168.99.0 0.0.0.255 any
 exit

! Apply ACL to Guest VLAN interface
interface GigabitEthernet 0/0/1.99
 ip access-group GUEST-RESTRICTION in
 exit

end
write memory
```

### Router-B: Student Lab Restrictions ACL

```cisco
configure terminal

! Extended ACL to restrict student access
ip access-list extended STUDENT-RESTRICTION
 ! Block access to Admin VLANs
 deny ip 192.168.20.0 0.0.0.127 192.168.10.0 0.0.0.127
 deny ip 192.168.20.0 0.0.0.127 192.168.10.128 0.0.0.63
 deny ip 192.168.20.0 0.0.0.127 192.168.10.192 0.0.0.31
 ! Allow access to library
 permit ip 192.168.20.0 0.0.0.127 192.168.30.0 0.0.0.63
 ! Allow access to servers (for educational content)
 permit ip 192.168.20.0 0.0.0.127 192.168.30.64 0.0.0.31
 ! Allow internet access
 permit ip 192.168.20.0 0.0.0.127 any
 exit

! Apply ACL to Student VLAN interface
interface GigabitEthernet 0/0/1.20
 ip access-group STUDENT-RESTRICTION in
 exit

end
write memory
```

### All Routers: SSH Access Control ACL

```cisco
configure terminal

! Standard ACL for management access (Packet Tracer compatible - no log keyword)
access-list 1 remark MANAGEMENT ACCESS
access-list 1 permit 192.168.10.0 0.0.0.127
access-list 1 permit 192.168.30.96 0.0.0.31
access-list 1 deny any

! Apply to VTY lines
line vty 0 4
 access-class 1 in
 transport input ssh
 exit

end
write memory
```

---

## DHCP Configuration

### Router-A: DHCP Pools

```cisco
configure terminal

! Exclude gateway and static IPs
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp excluded-address 192.168.10.129 192.168.10.135
ip dhcp excluded-address 192.168.10.193 192.168.10.195

! VLAN 10 - Admin Staff
ip dhcp pool ADMIN-STAFF
 network 192.168.10.0 255.255.255.128
 default-router 192.168.10.1
 dns-server 8.8.8.8 8.8.4.4
 domain-name futurevision.edu
 exit

! VLAN 11 - Admin Wi-Fi
ip dhcp pool ADMIN-WIFI
 network 192.168.10.128 255.255.255.192
 default-router 192.168.10.129
 dns-server 8.8.8.8 8.8.4.4
 domain-name futurevision.edu
 exit

! VLAN 12 - Admin Cameras
ip dhcp pool ADMIN-CAMERAS
 network 192.168.10.192 255.255.255.224
 default-router 192.168.10.193
 dns-server 8.8.8.8
 exit

end
write memory
```

### Router-B: DHCP Pools (Student Labs, Teachers, etc.)

```cisco
configure terminal

! Exclude ranges
ip dhcp excluded-address 192.168.20.1 192.168.20.10
ip dhcp excluded-address 192.168.20.129 192.168.20.135
ip dhcp excluded-address 192.168.20.193 192.168.20.195
ip dhcp excluded-address 192.168.20.225 192.168.20.230

! VLAN 20 - Student Labs
ip dhcp pool STUDENT-LABS
 network 192.168.20.0 255.255.255.128
 default-router 192.168.20.1
 dns-server 8.8.8.8 8.8.4.4
 domain-name futurevision.edu
 exit

! VLAN 21 - Teachers
ip dhcp pool TEACHERS
 network 192.168.20.128 255.255.255.192
 default-router 192.168.20.129
 dns-server 8.8.8.8 8.8.4.4
 domain-name futurevision.edu
 exit

! VLAN 22 - Smart Boards
ip dhcp pool SMART-BOARDS
 network 192.168.20.192 255.255.255.224
 default-router 192.168.20.193
 dns-server 8.8.8.8
 exit

! VLAN 23 - Academic Cameras
ip dhcp pool ACADEMIC-CAMERAS
 network 192.168.20.224 255.255.255.224
 default-router 192.168.20.225
 dns-server 8.8.8.8
 exit

end
write memory
```

### Router-C: DHCP Pools (Library, Servers, Monitoring)

```cisco
configure terminal

ip dhcp excluded-address 192.168.30.1 192.168.30.10
ip dhcp excluded-address 192.168.30.65 192.168.30.75
ip dhcp excluded-address 192.168.30.97 192.168.30.100

! VLAN 30 - Library Users
ip dhcp pool LIBRARY-USERS
 network 192.168.30.0 255.255.255.192
 default-router 192.168.30.1
 dns-server 8.8.8.8 8.8.4.4
 domain-name futurevision.edu
 exit

! VLAN 31 - Servers (typically static, but pool for dynamic)
ip dhcp pool SERVERS
 network 192.168.30.64 255.255.255.224
 default-router 192.168.30.65
 dns-server 8.8.8.8
 exit

! VLAN 32 - Security Monitoring
ip dhcp pool SECURITY-MONITOR
 network 192.168.30.96 255.255.255.224
 default-router 192.168.30.97
 dns-server 8.8.8.8
 exit

end
write memory
```

### Router-D: DHCP Pools (Sports Staff, Guest WiFi)

```cisco
configure terminal

ip dhcp excluded-address 192.168.40.1 192.168.40.5
ip dhcp excluded-address 192.168.40.33 192.168.40.35
ip dhcp excluded-address 192.168.99.1 192.168.99.10

! VLAN 40 - Sports Staff
ip dhcp pool SPORTS-STAFF
 network 192.168.40.0 255.255.255.224
 default-router 192.168.40.1
 dns-server 8.8.8.8 8.8.4.4
 domain-name futurevision.edu
 exit

! VLAN 41 - Scoreboards
ip dhcp pool SCOREBOARDS
 network 192.168.40.32 255.255.255.224
 default-router 192.168.40.33
 dns-server 8.8.8.8
 exit

! VLAN 99 - Guest Wi-Fi (Large pool)
ip dhcp pool GUEST-WIFI
 network 192.168.99.0 255.255.255.0
 default-router 192.168.99.1
 dns-server 8.8.8.8 8.8.4.4
 domain-name guest.futurevision.edu
 lease 0 2 0
 exit

end
write memory
```

---

## End Device Configuration

### Configure PCs for DHCP

For each end-user PC/Laptop:

1. Click on the device
2. Go to **Desktop** tab
3. Click **IP Configuration**
4. Select **DHCP**
5. Verify IP address is received from correct subnet

**Example for Admin-PC-1 (Building A, VLAN 10):**
- Expected IP: 192.168.10.11 to 192.168.10.126
- Gateway: 192.168.10.1
- DNS: 8.8.8.8

### Configure Servers with Static IPs

**For NVR Server (Building C, VLAN 31):**

1. Click on NVR-Server
2. Desktop → IP Configuration → Static
3. Configure:
   - IP Address: 192.168.30.70
   - Subnet Mask: 255.255.255.224
   - Default Gateway: 192.168.30.65
   - DNS Server: 8.8.8.8

**For IoT Management Server:**
- IP Address: 192.168.30.71
- Subnet Mask: 255.255.255.224
- Default Gateway: 192.168.30.65

**For Security Monitor Station:**
- IP Address: 192.168.30.98
- Subnet Mask: 255.255.255.224
- Default Gateway: 192.168.30.97

---

## Testing & Verification

### Connectivity Tests

#### Test 1: Verify OSPF Neighbor Relationships

On each router:
```cisco
show ip ospf neighbor

! Expected: See FULL state for all neighbors
! Router-CORE should have 4 neighbors
! Building routers should have 1 neighbor (Router-CORE)
```

#### Test 2: Verify Routing Tables

```cisco
show ip route

! Expected: See routes to all subnets
! Routes should be marked with 'O' (OSPF), 'C' (Connected), or 'L' (Local)
```

#### Test 3: End-to-End Connectivity

**From Admin-PC-1 (192.168.10.x):**
```
ping 192.168.1.1       (Router-CORE)
ping 192.168.20.1      (Router-B Gateway)
ping 192.168.30.70     (NVR Server)
ping 8.8.8.8           (Internet - through firewall)
```

#### Test 4: Verify ACL Restrictions

**From Guest-WiFi device (192.168.99.x):**
```
ping 192.168.10.10     (Should FAIL - Admin blocked)
ping 192.168.30.70     (Should FAIL - Server blocked)
ping 8.8.8.8           (Should SUCCEED - Internet allowed)
```

**From Student-Lab PC (192.168.20.x):**
```
ping 192.168.10.10     (Should FAIL - Admin blocked)
ping 192.168.30.5      (Should SUCCEED - Library allowed)
ping 8.8.8.8           (Should SUCCEED - Internet allowed)
```

#### Test 5: DHCP Verification

On a PC:
```
ipconfig /all         (Shows DHCP-assigned IP)
ipconfig /renew       (Renews DHCP lease)
```

Expected output should show:
- IP in correct subnet
- Correct gateway
- DNS servers configured

#### Test 6: DNS Resolution

```
nslookup www.google.com

! Should resolve to an IP address if internet connectivity exists
```

### Security Tests

#### Test 7: Firewall NAT

From any internal PC:
```
ping 8.8.8.8

! If successful, NAT is working
! Traffic is being translated by ASA firewall
```

#### Test 8: SSH Access Control

Try to SSH to routers from:
1. Admin PC (Should SUCCEED)
2. Student PC (Should FAIL)
3. Guest WiFi (Should FAIL)

```
ssh -l cisco 192.168.1.1
```

### Performance Tests

#### Test 9: Traceroute

```
tracert 192.168.30.70

! Shows the path packets take
! Should show: Local gateway → Router-CORE → Destination Router → Destination
```

#### Test 10: VLAN Isolation

From VLAN 10 device, try to reach VLAN 11 device on same switch:
```
ping 192.168.10.130

! Should work (both Building A networks)
! Demonstrates VLAN routing through router
```

---

## Troubleshooting Guide

### Common Issues and Solutions

#### Issue 1: Interfaces show "administratively down"

**Symptoms:** Interface status shows "admin down"

**Solution:**
```cisco
configure terminal
interface GigabitEthernet 0/0/0
 no shutdown
 exit
end
```

#### Issue 2: OSPF neighbors not forming

**Symptoms:** `show ip ospf neighbor` shows no neighbors

**Checks:**
1. Verify interfaces are up: `show ip interface brief`
2. Check OSPF is enabled: `show ip ospf`
3. Verify network statements: `show run | section ospf`
4. Check OSPF area matches (must be same area)

**Solution:**
```cisco
! On both routers, verify:
show ip ospf interface

! Ensure network statement includes interface IP
router ospf 1
 network 192.168.1.0 0.0.0.3 area 0
 exit
```

#### Issue 3: DHCP not assigning addresses

**Symptoms:** PCs show 169.254.x.x (APIPA)

**Checks:**
1. Verify DHCP pool is configured: `show ip dhcp pool`
2. Check DHCP bindings: `show ip dhcp binding`
3. Verify excluded addresses: `show ip dhcp excluded-address`

**Solution:**
```cisco
! Clear DHCP bindings and retry
clear ip dhcp binding *

! Verify pool configuration
show run | section dhcp
```

#### Issue 4: Cannot ping across VLANs

**Symptoms:** Can ping within VLAN but not across

**Checks:**
1. Verify trunk port: `show interfaces trunk`
2. Check VLAN configuration: `show vlan brief`
3. Verify router subinterfaces: `show ip interface brief`

**Solution:**
```cisco
! On switch
interface GigabitEthernet 0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,11,12
 exit
```

#### Issue 5: ACL blocking legitimate traffic

**Symptoms:** Traffic that should work is blocked

**Checks:**
1. View ACL: `show access-lists`
2. Check ACL matches: `show access-lists GUEST-RESTRICTION`
3. Verify application: `show ip interface GigabitEthernet 0/0/1.99`

**Solution:**
```cisco
! Remove ACL temporarily to test
interface GigabitEthernet 0/0/1.99
 no ip access-group GUEST-RESTRICTION in
 exit

! Modify ACL rules as needed
! Re-apply ACL
interface GigabitEthernet 0/0/1.99
 ip access-group GUEST-RESTRICTION in
 exit
```

#### Issue 6: No internet connectivity

**Symptoms:** Cannot reach external IPs (8.8.8.8)

**Checks:**
1. Verify firewall NAT: `show nat` (on ASA)
2. Check default route: `show ip route`
3. Verify firewall interfaces: `show interface`

**Solution:**
```cisco
! On Router-CORE
show ip route

! Should see default route pointing to firewall

! On ASA
show route

! Verify routes to campus networks
```

#### Issue 7: SSH Configuration and RSA Key Generation

**Symptoms:** Cannot SSH to routers, or error "No RSA keys found"

**Root Cause:** RSA keys not properly generated for SSH

**⚠️ Important - Packet Tracer Syntax**:
The common production IOS syntax does NOT work in Packet Tracer:
```cisco
crypto key generate rsa modulus 2048  ← WRONG for Packet Tracer
```

**Correct Solutions for Packet Tracer**:

**Method 1: Interactive Mode (Recommended for Packet Tracer)**
```cisco
! Step 1: Configure domain name (REQUIRED)
configure terminal
ip domain-name futurevision.edu
exit

! Step 2: Generate RSA keys interactively
crypto key generate rsa
! When prompted "How many bits in the modulus [512]:", enter: 2048

! Step 3: Enable SSH version 2
configure terminal
ip ssh version 2
ip ssh time-out 60
ip ssh authentication-retries 3
end
write memory
```

**Method 2: Modern IOS Syntax (May work in newer Packet Tracer versions)**
```cisco
configure terminal
ip domain-name futurevision.edu
crypto key generate rsa general-keys modulus 2048
ip ssh version 2
end
write memory
```

**Verification Commands**:
```cisco
! Check if RSA keys exist
show crypto key mypubkey rsa

! Expected output: Should show public key with 2048-bit modulus

! Verify SSH is enabled
show ip ssh

! Expected: SSH Enabled - version 2.0
```

**Common SSH Errors**:
1. **"% Please define a domain-name first"**
   - Solution: Configure `ip domain-name futurevision.edu` first

2. **"% Key name not specified"**
   - Solution: Use interactive mode instead

3. **Connection timeout when trying to SSH**
   - Check VTY ACL allows your source network
   - Verify `transport input ssh` on VTY lines

#### Issue 8: Router Physical Interface IP Overlap Error

**Symptoms:** When configuring subinterfaces, error appears:
```
% 192.168.30.0 overlaps with GigabitEthernet0/0/1
```

**Root Cause:** Physical interface has an IP address configured, conflicting with router-on-a-stick (subinterface) configuration.

**Solution:**
```cisco
! Remove IP address from physical interface
configure terminal
interface GigabitEthernet 0/0/1
 no ip address           ← CRITICAL FIX
 description LAN Trunk to Switch
 no shutdown
 exit
end
write memory
```

**Key Learning:**
- ✅ Physical interface: NO IP address, only `no shutdown`
- ✅ Subinterfaces: Each has its own IP address with VLAN encapsulation

---

## Section 13: User Authentication & SSH Access

### Overview

Enhanced security using local username/password authentication instead of simple shared passwords. This provides:
- Individual user accountability
- Privilege level control (read-only vs full access)
- Better audit trail
- SSH-only management access

### Configuration Steps

#### Step 1: Create Local User Database

Configure on **all routers** (Router-CORE, Router-A, Router-B, Router-C, Router-D):

```cisco
configure terminal

! Admin users (privilege 15 - full access)
username admin privilege 15 secret Admin@123
username netadmin privilege 15 secret NetAdmin@456

! Building-specific users (privilege 7 - limited access)
! Choose based on router:
! For Router-A:
username adminstaff privilege 7 secret Staff@2024

! For Router-B:
username teacher privilege 7 secret Teacher@2024

! For Router-C:
username librarian privilege 7 secret Library@2024

! For Router-D:
username sportsstaff privilege 7 secret Sports@2024

! For Router-CORE:
username monitor privilege 1 secret Monitor@789

end
write memory
```

#### Step 2: Configure Console and VTY Lines

```cisco
configure terminal

! Console access
line console 0
 login local        ← Uses username/password instead of simple password
 exec-timeout 5 0
 logging synchronous
 exit

! VTY (SSH) access
line vty 0 4
 login local        ← Username/password required
 transport input ssh ← SSH only (no Telnet)
 exec-timeout 10 0
 logging synchronous
 exit

end
write memory
```

#### Step 3: Enable Additional Security Features

```cisco
configure terminal

! Enable password encryption
service password-encryption

! Set enable secret (for privileged EXEC mode)
enable secret Cisco@Class123

! Login banner (legal notice)
banner login ^
========================================
AUTHORIZED ACCESS ONLY
Unauthorized access is prohibited
All activities are monitored and logged
========================================
^

end
write memory
```

### Privilege Levels Explained

| Privilege Level | Access | Typical Users |
|----------------|--------|---------------|
| 15 | Full access (configure, show, debug) | Network administrators |
| 7 | Limited config, most show commands | Department managers |
| 1 | User EXEC only (basic show commands) | Monitoring tools |

### Testing Authentication

**Test from Admin PC**:
```cisco
! SSH to router
ssh admin@192.168.1.1
! Password: Admin@123

! Once logged in:
Router> enable
! Password: Cisco@Class123

! Now in privileged EXEC mode
Router#
```

**Test Privilege Levels**:
```cisco
! As privilege 15 user (admin):
Router> enable
Router# configure terminal  ← Should work

! As privilege 1 user (monitor):
Router> enable
Router# configure terminal  ← Should be DENIED
% Access denied
```

### Security Benefits

- ✅ Individual accountability (each user has unique credentials)
- ✅ No shared passwords
- ✅ Privilege separation (not everyone needs full access)
- ✅ SSH encryption (no plaintext credentials)
- ✅ Telnet disabled (transport input ssh)
- ✅ Automatic session timeout

---

## Configuration Backup

### Save Configuration to TFTP (if available)

```cisco
! On each device
copy running-config tftp:
! Enter TFTP server IP when prompted
! Enter filename: Router-A-config-backup.txt
```

### Save in Packet Tracer

1. File → Save As
2. Save as: `Future_Vision_Smart_Campus_Configured.pkt`
3. Keep backups at different stages:
   - `_topology_only.pkt`
   - `_basic_config.pkt`
   - `_ospf_configured.pkt`
   - `_acl_applied.pkt`
   - `_fully_tested.pkt`

---

## Configuration Checklist

Use this checklist to track implementation progress:

### Core Infrastructure
- [ ] Router-CORE basic configuration
- [ ] Router-CORE interface configuration
- [ ] Firewall basic configuration
- [ ] Firewall NAT configuration
- [ ] Internet connectivity verified

### Building Routers
- [ ] Router-A configured (Admin)
- [ ] Router-B configured (Academic)
- [ ] Router-C configured (Services)
- [ ] Router-D configured (Sports)
- [ ] All WAN links operational
- [ ] All subinterfaces created

### Switches
- [ ] Switch-A VLANs and trunking
- [ ] Switch-B VLANs and trunking
- [ ] Switch-C VLANs and trunking
- [ ] Switch-D VLANs and trunking
- [ ] All access ports assigned

### Routing
- [ ] OSPF configured on Router-CORE
- [ ] OSPF configured on Router-A
- [ ] OSPF configured on Router-B
- [ ] OSPF configured on Router-C
- [ ] OSPF configured on Router-D
- [ ] All OSPF neighbors established
- [ ] All routes learned and verified

### Security
- [ ] Guest Wi-Fi isolation ACL (Router-D)
- [ ] Student restriction ACL (Router-B)
- [ ] SSH access control ACLs (All routers)
- [ ] Firewall ACLs configured
- [ ] Security tested and verified

### Services
- [ ] DHCP pools configured (Router-A)
- [ ] DHCP pools configured (Router-B)
- [ ] DHCP pools configured (Router-C)
- [ ] DHCP pools configured (Router-D)
- [ ] DHCP tested on end devices

### End Devices
- [ ] Admin PCs configured
- [ ] Student Lab PCs configured
- [ ] Teacher laptops configured
- [ ] Library PCs configured
- [ ] Servers configured (static IPs)
- [ ] All devices receiving correct IPs

### Testing
- [ ] Ping tests successful
- [ ] OSPF verification complete
- [ ] ACL restrictions verified
- [ ] DHCP working correctly
- [ ] Internet access confirmed
- [ ] Cross-VLAN routing working
- [ ] Security policies enforced

---

## Conclusion

Congratulations! You have successfully implemented the Future Vision Smart Campus network in Cisco Packet Tracer. This configuration provides:

- **14 VLANs** across 4 buildings
- **OSPF routing** for dynamic path selection
- **Comprehensive security** with ACLs and firewall
- **DHCP services** for automatic IP assignment
- **Scalable design** for future expansion

### Next Steps

1. **Documentation**: Document any custom changes made
2. **Testing**: Perform comprehensive testing using the checklist
3. **Optimization**: Fine-tune OSPF timers and ACLs as needed
4. **Presentation**: Prepare demonstration scenarios
5. **Maintenance**: Plan regular configuration backups

### Learning Outcomes Achieved

✅ Enterprise network design implementation  
✅ VLSM and IP address planning  
✅ Router-on-a-stick configuration  
✅ OSPF dynamic routing protocol  
✅ Access Control Lists (ACLs)  
✅ Firewall configuration and NAT  
✅ Network security best practices  
✅ Troubleshooting and verification

---

**Implementation Guide Version**: 1.0  
**Last Updated**: December 2025  
**Course**: Computer Networks - Smart Campus Project  
**Total Configuration Time**: 4-6 hours for complete implementation

---

*For additional support, refer to the [Smart Campus Network Design Report](./Smart_Campus_Network_Design_Report.md) and [Network Diagram Instructions](./NETWORK_DIAGRAM_INSTRUCTIONS.md).*
