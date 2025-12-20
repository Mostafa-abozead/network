# Future Vision Smart Campus Network
## Professional Network Design & Implementation Presentation

---

## Table of Contents

1. [Project Overview](#slide-1-project-overview)
2. [Network Architecture](#slide-2-network-architecture)
3. [Building Distribution](#slide-3-building-distribution)
4. [IP Addressing & VLANs](#slide-4-ip-addressing--vlans)
5. [Hierarchical Switch Design](#slide-5-hierarchical-switch-design)
6. [Security Architecture](#slide-6-security-architecture)
7. [Routing Protocol - OSPF](#slide-7-routing-protocol---ospf)
8. [Access Control Lists](#slide-8-access-control-lists)
9. [Enhanced Authentication](#slide-9-enhanced-authentication)
10. [DHCP Configuration](#slide-10-dhcp-configuration)
11. [Testing & Validation](#slide-11-testing--validation)
12. [Implementation Highlights](#slide-12-implementation-highlights)
13. [Technical Specifications](#slide-13-technical-specifications)
14. [Budget Overview](#slide-14-budget-overview)
15. [Demo: Network Functionality](#slide-15-demo-network-functionality)
16. [Q&A](#slide-16-qa)

---

## Slide 1: Project Overview

### Future Vision Smart Campus Network

**Project Goal**: Design and implement a comprehensive, secure, and scalable network infrastructure for a K-12 educational institution with external query facility.

**Key Objectives:**
- ✅ Connect 4 buildings with high-speed fiber optic backbone
- ✅ Support 141+ end-user devices (campus + query station)
- ✅ Implement multi-layered security architecture with ACL-based controls
- ✅ Provide guest WiFi with complete isolation
- ✅ Enable centralized camera monitoring and IoT management
- ✅ Ensure 99.9% uptime with multi-protocol routing (OSPF + BGP + EIGRP)
- ✅ Library Query Station (AS 500) with controlled access to library resources only

**Implementation Platform:** Cisco Packet Tracer

**Total Budget:** $300,150 (updated with Query Station equipment)

> 📸 *[IMAGE PLACEHOLDER: Campus overview map showing 4 buildings + Library Query Station]*

---

## Slide 2: Network Architecture

### Hub-and-Spoke Distributed Routing Architecture with Library Query Station

```
                   [Router-CORE]
              ________|________
             |     |     |     |
         Router-A B    C      D
             |     |     |     |
         Building 1-4 (with switches)
                       |
                  (BGP - 10.0.0.0)
                       |
        Library Building Router (AS 600)
                       |
                  (BGP Connection)
                       |
        Library Query Station (AS 500)
           Central Router (10.0.0.0)
                  |
         (EIGRP AS 1 - Internal)
            ______|______
           |             |
   Router-Q1         Router-Q2
   (11.0.0.0)        (12.0.0.0)
      |                 |
   Switch-Q1         Switch-Q2
   (13.0.0.0)        (14.0.0.0)
      |                 |
   AP,Laptop,PC      AP,Laptop,PC
```

**Core Components:**
- **8 Routers**: 1 Central Hub + 4 Building Routers + 3 Query Station Routers (AS 500)
- **9 Switches**: 4 buildings (hierarchical design in Building B) + 2 Query Station switches
- **14 VLANs**: Segmented for different user groups
- **Library Query Station (AS 500)**: Connected to Library Building Router (AS 600) via BGP

**Library Query Station Components:**
- **1 Central Router**: BGP connection (10.0.0.0) to Library Building Router AS 600
- **2 Internal Routers**: EIGRP AS 1 routing (11.0.0.0, 12.0.0.0)
- **2 Switches**: Access layer (13.0.0.0, 14.0.0.0)
- **6 End Devices**: 2 Access Points, 2 Laptops, 2 PCs

**Connection Type:**
- Inter-building: Fiber optic (1-10 Gbps capable)
- Intra-building: Cat6a UTP (10 Gbps capable)
- Library Query Station: BGP peering over dedicated link

> 📸 *[IMAGE PLACEHOLDER: Full network topology diagram showing all routers, switches, and Library Query Station connections]*

---

## Slide 3: Building Distribution

### Building A - Administration
**Purpose:** Central administrative operations

**Network Devices:**
- 20 Staff Desktop PCs
- 2 Network Printers
- 2 IP Security Cameras
- 1 Staff WiFi Access Point

**VLANs:**
- VLAN 10: Admin Staff (192.168.10.0/25)
- VLAN 11: Admin WiFi (192.168.10.128/26)
- VLAN 12: Admin Cameras (192.168.10.192/27)

> 📸 *[IMAGE PLACEHOLDER: Building A floor plan with network devices]*

---

### Building B - Academic Block
**Purpose:** Primary teaching and learning facilities

**Network Devices:**
- 75 Student Lab PCs (3 labs)
- 20 Teacher Laptops
- 10 Smart Interactive Boards
- 4 IP Security Cameras

**VLANs:**
- VLAN 20: Student Labs (192.168.20.0/25)
- VLAN 21: Teachers (192.168.20.128/26)
- VLAN 22: Smart Boards (192.168.20.192/27)
- VLAN 23: Academic Cameras (192.168.20.224/27)

**Special Feature:** Hierarchical 3-tier switch architecture

> 📸 *[IMAGE PLACEHOLDER: Building B floor plan with 3 labs and hierarchical switches]*

---

### Building C - Services & Library
**Purpose:** Information resources and technical infrastructure

**Network Devices:**
- 15 Library PCs
- 2 Library Staff PCs
- 1 Main NVR Server
- 1 IoT Management Server
- 1 Security Monitor Station

**VLANs:**
- VLAN 30: Library Users (192.168.30.0/26)
- VLAN 31: Servers (192.168.30.64/27)
- VLAN 32: Security Monitoring (192.168.30.96/27)

**Critical Services:** 24/7 NVR recording, centralized IoT management

**External Connectivity:** Library Building Router (AS 600) provides BGP connection to Library Query Station (AS 500) via 10.0.0.0 network

> 📸 *[IMAGE PLACEHOLDER: Building C server room with NVR and security monitoring setup]*

---

### Library Query Station (AS 500)
**Purpose:** Remote query and research facility with controlled access

**Network Architecture:**
- **Central Router**: BGP connection to Library Building Router AS 600 (10.0.0.0)
- **Internal Routing**: EIGRP AS 1 between two internal routers
  - Router-Q1: 11.0.0.0 network, connects to Switch-Q1 (13.0.0.0)
  - Router-Q2: 12.0.0.0 network, connects to Switch-Q2 (14.0.0.0)

**Network Devices:**
- 2 Query Station Switches (Switch-Q1, Switch-Q2)
- 2 Access Points (one per switch)
- 2 Laptops (one per switch)
- 2 PCs (one per switch)

**Security Controls:**
- ✅ **Permitted Access**: VLAN30 (192.168.30.0) - Library PCs only
- ❌ **Denied Access**: All other campus networks
- ❌ **Denied Access**: Guest WiFi VLAN99 (192.168.99.0/24)

**Key Features:**
- Isolated query environment
- Controlled access to library resources only
- Complete segregation from administrative and student networks

> 📸 *[IMAGE PLACEHOLDER: Library Query Station topology showing 3 routers, 2 switches, and end devices]*

---

### Building D - Sports & Events Hall
**Purpose:** Athletic facilities and event hosting

**Network Devices:**
- 3 Staff PCs (Coaches + Event Management)
- 3 IoT Digital Scoreboards
- 2 High-Density Guest WiFi Access Points

**VLANs:**
- VLAN 40: Sports Staff (192.168.40.0/27)
- VLAN 41: Scoreboards (192.168.40.32/27)
- VLAN 99: Guest WiFi (192.168.99.0/24)

**Security Focus:** Complete guest WiFi isolation from internal networks

> 📸 *[IMAGE PLACEHOLDER: Building D sports hall with scoreboards and guest WiFi coverage]*

---

## Slide 4: IP Addressing & VLANs

### VLSM Strategy: Efficient IP Address Allocation

**Private IP Space:** 192.168.0.0/16 (Campus) + 10.0.0.0/8 + 11-14.0.0.0/8 (Query Station)

**VLAN Distribution Across 4 Buildings:**

| Building | VLANs | IP Range | Total IPs | Usage |
|----------|-------|----------|-----------|-------|
| **A (Admin)** | 3 | 192.168.10.0/24 | 256 | 22 devices |
| **B (Academic)** | 4 | 192.168.20.0/24 | 256 | 109 devices |
| **C (Services)** | 3 | 192.168.30.0/24 | 256 | 20 devices |
| **D (Sports)** | 3 | 192.168.40.0/27 + 99.0/24 | 280+ | 8 devices + guests |

**Library Query Station Networks:**

| Network | Purpose | IP Range | Connected Devices |
|---------|---------|----------|-------------------|
| **10.0.0.0** | BGP Link to AS 600 | 10.0.0.0/8 | Router connection |
| **11.0.0.0** | EIGRP Link - Router Q1 | 11.0.0.0/8 | Internal routing |
| **12.0.0.0** | EIGRP Link - Router Q2 | 12.0.0.0/8 | Internal routing |
| **13.0.0.0** | Switch-Q1 LAN | 13.0.0.0/8 | AP, Laptop, PC |
| **14.0.0.0** | Switch-Q2 LAN | 14.0.0.0/8 | AP, Laptop, PC |

**WAN Links:** Point-to-point /30 subnets (2 usable IPs each)

**Design Benefits:**
- ✅ Minimal IP waste with VLSM
- ✅ 100% growth capacity in each subnet
- ✅ Clear hierarchical structure
- ✅ Easy troubleshooting
- ✅ Query Station isolated addressing scheme

> 📸 *[IMAGE PLACEHOLDER: Visual VLAN diagram showing color-coded network segments including Query Station]*

---

## Slide 5: Hierarchical Switch Design

### Building B - Scalability Challenge Solved

**Problem:** 75 students + 20 teachers + 10 smart boards + cameras = 109+ devices

**Solution:** Hierarchical 3-Tier Switch Architecture

```
            Router-B
                |
         Switch-B (Core)
      ______|_______
     |      |       |
  Switch-B1 B2     B3
  (VLAN 20) (21)  (22)
  24 ports  24    12
```

**Access Layer Switches:**
- **Switch-B1:** Student Labs (VLAN 20) - 24 ports
- **Switch-B2:** Teachers (VLAN 21) - 24 ports
- **Switch-B3:** Smart Boards (VLAN 22) - 12 ports

**Benefits:**
- ✅ 72+ access ports total
- ✅ VLAN segregation at access layer
- ✅ Better traffic management
- ✅ Easier troubleshooting
- ✅ Scalable for future expansion

> 📸 *[IMAGE PLACEHOLDER: Building B hierarchical switch diagram with 4 switches]*

---

## Slide 6: Security Architecture

### Defense in Depth Strategy

**Multi-Layered Security Approach:**

```
Layer 1: Network   → Access Control Lists (ACLs)
Layer 2: Access    → VLAN Segmentation (14 VLANs)
Layer 3: Device    → SSH-only Management + Authentication
Layer 4: Routing   → Protocol-based isolation (OSPF/BGP/EIGRP)
```

**Key Security Features:**

1. **Access Control Lists (ACLs)**
   - Library Query Station isolation (access only to VLAN30)
   - **Library Server Protection (VLAN 31) - Only Admin & Library PCs allowed**
   - Guest WiFi complete isolation (including Query Station)
   - Student restrictions from admin networks
   - Management access control

2. **Enhanced Authentication**
   - 35 user accounts with privilege levels
   - SSH v2 with 2048-bit RSA encryption
   - Telnet disabled network-wide
   - Individual accountability

3. **Network Segmentation**
   - 14 VLANs for different user groups
   - Library Query Station segregation (AS 500)
   - IoT device isolation
   - Guest network complete isolation

4. **Query Station Security Controls**
   - ✅ **Permitted:** VLAN30 (Library PCs) only
   - ❌ **Denied:** All admin networks (VLAN10)
   - ❌ **Denied:** All academic networks (VLAN20)
   - ❌ **Denied:** Sports networks (VLAN40)
   - ❌ **Denied:** Guest WiFi (VLAN99)
   - ❌ **Denied:** Library Servers (VLAN31)

5. **Library Server Protection (VLAN 31)**
   - ✅ **Permitted:** Admin Building (full access)
   - ✅ **Permitted:** Library PCs (VLAN30)
   - ❌ **Denied:** All other networks (Academic, Sports, Query Station, Guest WiFi)

> 📸 *[IMAGE PLACEHOLDER: Security layers diagram showing defense in depth with Query Station controls]*

---

## Slide 7: Routing Protocol - Multi-Protocol Architecture

### OSPF + BGP + EIGRP Configuration

The network implements a **multi-protocol routing architecture** combining three protocols for optimal routing:

**1. OSPF (Internal Campus Routing)**
- **Protocol:** OSPFv2, Process ID 1, Area 0
- **Scope:** Campus buildings (Router-CORE, Router-A, Router-B, Router-C, Router-D)
- **Router IDs:** 1.1.1.1 (CORE), 10.10.10.10 (A), 20.20.20.20 (B), etc.

| Criteria | OSPF Advantage |
|----------|----------------|
| **Scalability** | Handles hundreds of routers efficiently |
| **Convergence** | Sub-second (1-5 seconds) vs RIP's 30+ seconds |
| **Bandwidth** | No hop count limit, bandwidth-based metrics |
| **Standards** | Open standard (RFC 2328), vendor-neutral |

**2. BGP (External Library Connection) - NEW**
- **Purpose:** Connect Library Query Station (AS 500) to Library Building Router (AS 600)
- **Connection:** 10.0.0.0 network
- **Protocol:** BGP (Border Gateway Protocol)
- **AS Numbers:**
  - Library Query Station: AS 500
  - Library Building Router: AS 600

**BGP Configuration:**
```cisco
! On Library Building Router (AS 600)
router bgp 600
 neighbor 10.0.0.2 remote-as 500
 network 192.168.30.0 mask 255.255.255.0

! On Library Query Station Central Router (AS 500)
router bgp 500
 neighbor 10.0.0.1 remote-as 600
 redistribute eigrp 1
```

**3. EIGRP (Query Station Internal Routing) - NEW**
- **Purpose:** Internal routing within Library Query Station
- **AS Number:** AS 1 (EIGRP)
- **Scope:** Router-Q1 and Router-Q2 within Query Station
- **Networks:** 11.0.0.0/8, 12.0.0.0/8, 13.0.0.0/8, 14.0.0.0/8

**EIGRP Configuration:**
```cisco
! On Query Station Routers
router eigrp 1
 network 11.0.0.0
 network 12.0.0.0
 network 13.0.0.0
 network 14.0.0.0
 no auto-summary
```

**Network Design Benefits:**
- ✅ OSPF for fast campus internal routing
- ✅ BGP for policy-based external connectivity
- ✅ EIGRP for efficient Query Station internal routing
- ✅ Protocol independence and optimal path selection

> 📸 *[IMAGE PLACEHOLDER: Multi-protocol topology showing OSPF Area 0, BGP peering, and EIGRP AS 1]*

---

## Slide 8: Access Control Lists

### Security Policy Enforcement

**ACLs Deployed (All Packet Tracer Compatible)**

**Critical Security Rules:**

### 1. Library Query Station Access Control (NEW - CRITICAL)
**Purpose:** Restrict Query Station to access only library PCs (VLAN30)

```cisco
! On Library Building Router (AS 600) or Query Station Central Router
! Permit access to VLAN30 (Library PCs) only
access-list 100 permit ip 13.0.0.0 0.255.255.255 192.168.30.0 0.0.0.255
access-list 100 permit ip 14.0.0.0 0.255.255.255 192.168.30.0 0.0.0.255

! Deny access to all other campus networks
access-list 100 deny ip 13.0.0.0 0.255.255.255 192.168.10.0 0.0.0.255
access-list 100 deny ip 14.0.0.0 0.255.255.255 192.168.10.0 0.0.0.255
access-list 100 deny ip 13.0.0.0 0.255.255.255 192.168.20.0 0.0.0.255
access-list 100 deny ip 14.0.0.0 0.255.255.255 192.168.20.0 0.0.0.255
access-list 100 deny ip 13.0.0.0 0.255.255.255 192.168.40.0 0.0.0.255
access-list 100 deny ip 14.0.0.0 0.255.255.255 192.168.40.0 0.0.0.255

! Deny access to Guest WiFi VLAN99
access-list 100 deny ip 13.0.0.0 0.255.255.255 192.168.99.0 0.0.0.255
access-list 100 deny ip 14.0.0.0 0.255.255.255 192.168.99.0 0.0.0.255

! Apply to outbound interface
interface GigabitEthernet 0/0/0
 ip access-group 100 out
```
**Result:** 
- ✅ Query Station CAN access VLAN30 (192.168.30.0) - Library PCs
- ❌ Query Station BLOCKED from Admin (VLAN10)
- ❌ Query Station BLOCKED from Academic (VLAN20)
- ❌ Query Station BLOCKED from Sports (VLAN40)
- ❌ Query Station BLOCKED from Guest WiFi (VLAN99)

### 2. Guest WiFi Isolation (CRITICAL)
```cisco
! Block ALL internal networks including Query Station
deny ip 192.168.99.0 0.0.0.255 192.168.10.0 0.0.0.127
deny ip 192.168.99.0 0.0.0.255 192.168.30.64 0.0.0.31
deny ip 192.168.99.0 0.0.0.255 13.0.0.0 0.255.255.255
deny ip 192.168.99.0 0.0.0.255 14.0.0.0 0.255.255.255
! Allow only HTTP/HTTPS/DNS
permit tcp any any eq 80
permit tcp any any eq 443
```
**Result:** ❌ Guests BLOCKED from internal networks and Query Station, ✅ Internet allowed

### 3. Library Server Protection (VLAN 31) - NEW
```cisco
! Protect Library Servers - Only Admin and Library PCs allowed
ip access-list extended PROTECT-LIBRARY-SERVERS
 ! Permit Admin Building
 permit ip 192.168.10.0 0.0.0.255 192.168.30.64 0.0.0.31
 ! Permit Library PCs (VLAN 30)
 permit ip 192.168.30.0 0.0.0.63 192.168.30.64 0.0.0.31
 ! Deny all others
 deny ip any 192.168.30.64 0.0.0.31
!
! Applied to VLAN 31 interface
interface GigabitEthernet 0/0/1.31
 ip access-group PROTECT-LIBRARY-SERVERS in
```
**Result:** 
- ✅ Admin Building CAN access servers
- ✅ Library PCs CAN access servers
- ❌ Students BLOCKED from servers (VLAN 31)
- ❌ Sports/Events BLOCKED from servers
- ❌ Query Station BLOCKED from servers
- ❌ Guest WiFi BLOCKED from servers

### 4. Student Lab Restrictions
```cisco
! Block admin networks
deny ip 192.168.20.0 0.0.0.127 192.168.10.0 0.0.0.127
! Block Query Station
deny ip 192.168.20.0 0.0.0.127 13.0.0.0 0.255.255.255
deny ip 192.168.20.0 0.0.0.127 14.0.0.0 0.255.255.255
! Block Library Servers (VLAN 31)
deny ip 192.168.20.0 0.0.0.127 192.168.30.64 0.0.0.31
! Allow library PCs
permit ip 192.168.20.0 0.0.0.127 192.168.30.0 0.0.0.63
```
**Result:** ❌ Students BLOCKED from admin, Query Station, and servers, ✅ Library PCs allowed

### 5. Management Access Control
```cisco
access-list 1 permit 192.168.10.0 0.0.0.127
access-list 1 deny any
! Applied to VTY lines
```
**Result:** ✅ Only admin staff can SSH to routers

> 📸 *[IMAGE PLACEHOLDER: ACL security matrix showing allowed/blocked traffic including Query Station restrictions]*

---

## Slide 9: Enhanced Authentication

### Individual Accountability & Privilege Separation

**Traditional Approach:** Simple shared passwords
**Our Approach:** Username/password with privilege levels

**User Database (35 Total Accounts):**

| Privilege Level | Access Rights | User Examples | Count |
|----------------|---------------|---------------|-------|
| **15** | Full access (config, debug) | admin, netadmin | 10 (2 per router) |
| **7** | Limited config, show commands | teacher, librarian | 20 (4 building users) |
| **1** | Read-only (user EXEC) | monitor | 5 (1 per router) |

**Security Configuration:**
```cisco
! Create users with privilege levels
username admin privilege 15 secret Admin@123
username teacher privilege 7 secret Teacher@2024
username monitor privilege 1 secret Monitor@789

! Console & VTY require username/password
line console 0
 login local
line vty 0 4
 login local
 transport input ssh  ← SSH only, no Telnet
```

**Benefits:**
- ✅ Individual accountability
- ✅ Role-based access control
- ✅ No shared passwords
- ✅ Encrypted management access

> 📸 *[IMAGE PLACEHOLDER: Authentication hierarchy diagram with 3 privilege levels]*

---

## Slide 10: DHCP Configuration

### Automatic IP Assignment Across All VLANs

**14 DHCP Pools Configured on Building Routers**

**Example Configuration:**
```cisco
! Exclude infrastructure IPs
ip dhcp excluded-address 192.168.10.1 192.168.10.10

! Admin Staff Pool
ip dhcp pool ADMIN-STAFF
 network 192.168.10.0 255.255.255.128
 default-router 192.168.10.1
 dns-server 8.8.8.8 8.8.4.4
 domain-name futurevision.edu
 lease 1 0 0  ← 1 day lease
```

**Lease Time Strategy:**
- **Staff/Teachers:** 1 day (stable, long-term)
- **Students:** 8 hours (shorter, rotating)
- **Guests:** 2 hours (temporary access)

**Pool Sizing:**
- 100% growth capacity in each subnet
- First 10 IPs reserved for infrastructure
- Sufficient IPs for current + future devices

**DHCP Features:**
- ✅ Automatic configuration (IP, gateway, DNS)
- ✅ Reduced administrative overhead
- ✅ Domain name assignment
- ✅ Appropriate lease times per user type

> 📸 *[IMAGE PLACEHOLDER: DHCP process flow diagram from request to acknowledgment]*

---

## Slide 11: Testing & Validation

### Comprehensive 7-Phase Testing Strategy

**Phase 1: DHCP & Basic Connectivity**
- ✅ All campus devices receive correct IP addresses
- ✅ Query Station devices receive IPs from their respective subnets
- ✅ Gateway reachability verified
- ✅ DNS configuration confirmed

**Phase 2: Multi-Protocol Routing**
- ✅ OSPF neighbors in FULL state (Campus buildings)
- ✅ BGP peering established (AS 500 ↔ AS 600)
- ✅ EIGRP neighbors UP (Query Station internal routers)
- ✅ Route redistribution working correctly

**Phase 3: Query Station Access Control (NEW)**
- ✅ Query Station CAN access VLAN30 (Library PCs 192.168.30.0)
- ❌ Query Station BLOCKED from Admin networks (VLAN10)
- ❌ Query Station BLOCKED from Student networks (VLAN20)
- ❌ Query Station BLOCKED from Sports networks (VLAN40)
- ❌ Query Station BLOCKED from Guest WiFi (VLAN99)

**Phase 4: Routing & Inter-VLAN**
- ✅ Admin PC: Broad access verified
- ✅ Student PC: Restrictions enforced
- ✅ Teacher PC: Moderate access confirmed
- ✅ Guest WiFi: Complete isolation verified (including Query Station)

**Phase 5: Management Access (SSH)**
- ✅ SSH from admin networks: SUCCESS
- ❌ SSH from student/guest networks: BLOCKED
- ❌ SSH from Query Station: BLOCKED
- ✅ Username/password authentication working

**Phase 6: Service Access**
- ✅ DNS resolution functional
- ✅ HTTP/HTTPS traffic flows correctly
- ✅ Traceroute shows proper routing paths
- ✅ BGP route advertisement verified

**Phase 7: Cross-Building & Query Station Routing**
- ✅ OSPF neighbors in FULL state
- ✅ All routes learned and propagated
- ✅ BGP session established between AS 500 and AS 600
- ✅ EIGRP convergence within Query Station
- ✅ Traceroute from campus to Query Station shows multi-hop routing

**Test Results:** 25+ scenarios executed, 100% pass rate

> 📸 *[IMAGE PLACEHOLDER: Testing dashboard showing green checkmarks for all phases including Query Station tests]*

---

## Slide 12: Implementation Highlights

### Critical Fixes & Enhancements

**1. Router-on-a-Stick Configuration Fix**
```cisco
! WRONG (causes IP overlap error)
interface GigabitEthernet 0/0/1
 ip address 192.168.10.1 255.255.255.128

! CORRECT
interface GigabitEthernet 0/0/1
 no ip address  ← Physical interface stays layer 2
 no shutdown
```

**2. SSH Configuration for Packet Tracer**
```cisco
! Does NOT work in Packet Tracer
crypto key generate rsa modulus 2048

! Works in Packet Tracer
crypto key generate rsa
! Prompt: 2048
```

**3. ACL Packet Tracer Compatibility**
- **Issue:** `log` keyword not supported in Packet Tracer
- **Solution:** Removed from all 17 ACLs
- **Impact:** Security policies unchanged, PT compatible

**4. Hierarchical Switch Architecture**
- **Before:** Single 24-port switch for 109+ devices
- **After:** 1 core + 3 access switches = 72+ ports
- **Benefit:** Scalability and better management

**All configurations tested and verified in Cisco Packet Tracer!**

> 📸 *[IMAGE PLACEHOLDER: Before/after comparison of switch architecture]*

---

## Slide 13: Technical Specifications

### Network Infrastructure Summary

**Core Network Equipment:**
- **Routers:** 8 (5 Campus + 3 Query Station: Cisco ISR 4331, 4321, 4221)
- **Switches:** 9 (7 Campus + 2 Query Station: Cisco Catalyst 3650-24PS, 2960-24TT)
- **Wireless:** 5 APs (3 Campus + 2 Query Station) + 1 Controller

**End-User Devices:**
- **Campus Devices:** 135
  - PCs/Laptops: 135
  - Printers: 2
  - IP Cameras: 12
  - Smart Boards: 10
  - IoT Scoreboards: 3
  - Servers: 3
- **Query Station Devices:** 6
  - PCs: 2
  - Laptops: 2
  - Access Points: 2

**Total End-User Devices:** 141+

**Network Configuration:**
- **VLANs:** 14 (Campus)
- **ACLs:** Enhanced with Query Station restrictions
- **DHCP Pools:** 14 (Campus) + 2 (Query Station)
- **User Accounts:** 35
- **Routing Protocols:**
  - OSPF Area 0 (Campus)
  - BGP (AS 500 ↔ AS 600)
  - EIGRP AS 1 (Query Station internal)

**Performance Metrics:**
- **Fiber Backbone:** 1-10 Gbps capable
- **LAN Speed:** 10 Gbps (Cat6a)
- **OSPF Convergence:** 1-5 seconds
- **BGP Convergence:** 30-60 seconds
- **EIGRP Convergence:** Sub-second
- **Network Uptime Target:** 99.9%

**Library Query Station Specifications:**
- **BGP AS Number:** AS 500
- **Connection:** 10.0.0.0 to Library Building Router (AS 600)
- **Internal Networks:** 11.0.0.0, 12.0.0.0, 13.0.0.0, 14.0.0.0
- **Routing:** EIGRP AS 1 for internal routing

> 📸 *[IMAGE PLACEHOLDER: Technical specifications infographic with icons including Query Station]*

---

## Slide 14: Budget Overview

### Total Project Investment: $300,150

**Budget Breakdown:**

| Category | Cost (USD) | % of Total |
|----------|-----------|------------|
| **End-User Devices** | $103,850 | 34.6% |
| **IoT & Smart Devices** | $63,700 | 21.2% |
| **Contingency & Maintenance** | $38,933 | 13.0% |
| **Professional Services** | $21,400 | 7.1% |
| **Routers & Network Equipment** | $19,050 | 6.3% |
| **Cabling & Infrastructure** | $16,350 | 5.4% |
| **Security & Surveillance** | $11,500 | 3.8% |
| **Switches** | $10,200 | 3.4% |
| **Wireless Infrastructure** | $9,850 | 3.3% |
| **Software & Licensing** | $6,550 | 2.2% |

**Query Station Investment:** +$15,467
- 3 Routers (Query Station): $5,600
- 2 Switches: $2,400
- 6 End Devices: $5,000
- Cabling & Setup: $2,467

**Implementation Phases:**
1. **Phase 1:** Core Infrastructure ($65,800) - Months 1-2
2. **Phase 2:** Security & Connectivity ($27,200) - Months 2-3
3. **Phase 3:** End-User Deployment ($166,350) - Months 3-4
4. **Phase 4:** Query Station Setup ($15,467) - Month 4
5. **Phase 5:** Optimization & Training ($38,933) - Month 4-5

> 📸 *[IMAGE PLACEHOLDER: Pie chart showing budget distribution with Query Station]*

---

## Slide 15: Demo - Network Functionality

### Live Demonstration Scenarios

**Demo 1: DHCP in Action**
1. Power on a student PC in Building B
2. Select DHCP on IP Configuration
3. **Result:** Receives IP 192.168.20.15, Gateway 192.168.20.1, DNS 8.8.8.8

**Demo 2: Guest WiFi Isolation**
1. Connect guest device to VLAN 99
2. Attempt to ping admin server: `ping 192.168.10.10`
3. **Result:** ❌ Request timeout (ACL blocks internal networks)
4. Attempt to ping Query Station: `ping 13.0.0.10`
5. **Result:** ❌ Request timeout (ACL blocks Query Station)
6. Ping internet: `ping 8.8.8.8`
7. **Result:** ✅ Reply received (Internet access allowed)

**Demo 3: Query Station Access Control (NEW)**
1. From Query Station PC (13.0.0.10), ping Library VLAN: `ping 192.168.30.10`
2. **Result:** ✅ Reply received (VLAN30 access permitted)
3. From Query Station PC, ping Admin gateway: `ping 192.168.10.1`
4. **Result:** ❌ Request timeout (ACL blocks admin access)
5. From Query Station PC, ping Student network: `ping 192.168.20.10`
6. **Result:** ❌ Request timeout (ACL blocks student network)
7. From Query Station PC, ping Guest WiFi: `ping 192.168.99.10`
8. **Result:** ❌ Request timeout (ACL blocks Guest WiFi VLAN99)

**Demo 4: BGP Routing (NEW)**
1. On Library Building Router (AS 600), run: `show ip bgp summary`
2. **Result:** BGP neighbor 10.0.0.2 (AS 500) in Established state
3. On Query Station Router, run: `show ip bgp`
4. **Result:** Routes learned from AS 600 (192.168.30.0/24)

**Demo 5: EIGRP Internal Routing (NEW)**
1. On Query Station Central Router, run: `show ip eigrp neighbors`
2. **Result:** 2 neighbors (Router-Q1 and Router-Q2) in UP state
3. Traceroute from Switch-Q1 device to Switch-Q2 device
4. **Result:** Path through central router showing EIGRP routing

**Demo 6: SSH Access Control**
1. From Admin PC, SSH to Router-CORE: `ssh admin@192.168.1.1`
2. **Result:** ✅ Login successful (Admin network allowed)
3. From Student PC, SSH to Router-CORE: `ssh admin@192.168.1.1`
4. **Result:** ❌ Connection timeout (VTY ACL blocks student network)

**Demo 7: OSPF Campus Routing**
1. On Router-CORE, run: `show ip ospf neighbor`
2. **Result:** 4 neighbors in FULL state (A, B, C, D)
3. Traceroute from Building A to Building C: `tracert 192.168.30.1`
4. **Result:** Path shown through Router-CORE (multi-hop routing)

> 📸 *[IMAGE PLACEHOLDER: Screenshot of Packet Tracer showing Query Station ACL tests and routing protocols]*

---

## Slide 16: Q&A

### Common Questions & Expert Answers

**Q1: Why use multi-protocol routing (OSPF + BGP + EIGRP)?**
**A:** Each protocol serves a specific purpose: OSPF provides fast convergence for internal campus routing (1-5 seconds), BGP enables policy-based external connectivity between autonomous systems (AS 500 and AS 600), and EIGRP offers efficient internal routing within the Query Station with minimal overhead. This multi-protocol approach optimizes routing for different network segments.

**Q2: Why hierarchical switches only in Building B?**
**A:** Building B has 109+ devices (75 students + 20 teachers + 10 smart boards + cameras), exceeding a single 24-port switch capacity. Other buildings have fewer devices that fit within single switch port counts. This demonstrates scalable design when needed.

**Q3: Why is the Query Station only allowed to access VLAN30 (Library PCs)?**
**A:** The Library Query Station is a remote facility designed specifically for research and query purposes. Access is restricted to only library resources (VLAN30) to maintain security and prevent unauthorized access to administrative, academic, or guest networks. This implements the principle of least privilege access control.

**Q4: How do you handle password management with 35 accounts?**
**A:** We implement strong password policies (complexity, expiration), use privilege levels for role-based access, and employ SSH encryption. In production, integrate with RADIUS/TACACS+ for centralized authentication and audit trails.

**Q5: What happens if the Query Station central router fails?**
**A:** Query Station devices lose connectivity to campus resources. The two internal routers (Router-Q1 and Router-Q2) continue to communicate via EIGRP, maintaining local network functionality. For high availability, implement redundant BGP connections with dual routers.

**Q6: Can we add more buildings or query stations in the future?**
**A:** Yes! The hub-and-spoke architecture with Router-CORE as the central hub makes expansion straightforward. For new buildings, connect via fiber to Router-CORE with OSPF. For new query stations, establish BGP peering with appropriate AS numbers and implement similar ACL restrictions.

**Q7: Why BGP instead of OSPF for Query Station connection?**
**A:** BGP provides policy-based routing control and autonomous system separation. This allows independent administration of the Query Station (AS 500) and Library (AS 600), enables granular access control, and prepares the network for future external connections or partner organizations.

**Q8: What's the most critical security feature?**
**A:** The Query Station ACL that restricts access to only VLAN30 (Library PCs) while blocking all other campus networks and guest WiFi (VLAN99). This ensures remote facilities cannot access sensitive administrative, academic, or guest networks, maintaining network segmentation and data security.

**Q9: Can this design scale to a university campus?**
**A:** Yes, with modifications. Add multiple Area 0 routers for redundancy, implement multi-area OSPF for larger scale, add distribution layer switches, enhance with QoS for voice/video, and deploy additional query stations with unique AS numbers. Core principles remain the same.

**Q10: How is Query Station traffic monitored?**
**A:** ACL hit counters track permitted and denied traffic from Query Station networks (13.0.0.0, 14.0.0.0), BGP logs show peering status and route advertisements, EIGRP metrics reveal internal routing health, and DHCP bindings track device connections within the Query Station.

**Q11: What routing protocol should be removed if not needed?**
**A:** None should be removed as each serves a distinct purpose. However, if Query Station is decommissioned, remove BGP peering between AS 500 and AS 600, and remove EIGRP AS 1 configuration from Query Station routers. OSPF remains essential for campus routing.

---

## Thank You!

### Future Vision Smart Campus Network

**Project Highlights:**
- ✅ 4 Buildings Connected
- ✅ 9 Switches (7 Campus + 2 Query Station)
- ✅ Enhanced Security ACLs
- ✅ 35 User Accounts
- ✅ 100% Test Pass Rate
- ✅ $300,150 Budget
- ✅ Library Query Station (AS 500)

**Key Achievements:**
- Multi-layered security architecture with ACL-based controls
- Scalable hierarchical design
- Multi-protocol routing (OSPF + BGP + EIGRP)
- Query Station isolation (VLAN30 access only)
- Complete guest isolation (VLAN99)
- Individual user accountability
- Packet Tracer verified

**Contact Information:**
- Repository: Mostafa-abozead/network
- Documentation: 8 comprehensive guides
- Testing: 20+ validated scenarios

**Ready for Questions!**

> 📸 *[IMAGE PLACEHOLDER: Campus network success celebration image or final topology diagram]*

---

## Appendix: Additional Resources

### Documentation Available

1. **Smart_Campus_Network_Design_Report.md** - Complete technical specifications
2. **Packet_Tracer_Implementation_Guide.md** - Step-by-step configuration (42KB)
3. **TESTING_GUIDE.md** - 20+ test scenarios (22KB)
4. **NETWORK_DIAGRAM_INSTRUCTIONS.md** - Topology creation guide
5. **DOCUMENTATION_UPDATE_SUMMARY.md** - All implementation changes (20KB)
6. **README.md** - Project overview and quick start
7. **QUICK_START_GUIDE.md** - Getting started guide
8. **PROJECT_REQUIREMENTS_VERIFICATION.md** - Requirements checklist

### Quick Reference

**Default Credentials:**
- Username: `admin` / Password: `Admin@123` (Privilege 15)
- Enable Secret: `Cisco@Class123`

**Key IP Addresses:**
- Router-CORE: 192.168.1.1 (to Building A)
- Firewall Inside: 192.168.100.1
- Building A Gateway: 192.168.10.1
- Building B Gateway: 192.168.20.1
- Building C Gateway: 192.168.30.1
- Building D Gateway: 192.168.40.1

**Critical Commands:**
```cisco
show ip ospf neighbor
show ip route ospf
show access-lists
show ip dhcp binding
show crypto key mypubkey rsa
```

---

*Presentation Version: 1.0*  
*Date: December 2025*  
*Project: Future Vision Smart Campus Network*  
*Institution: Educational Network Design*

---

## Presentation Notes for Presenter

### Introduction Tips
- Start with the campus overview and emphasize the real-world applicability
- Highlight that this is production-ready, not just academic
- Mention all configurations are tested and verified

### Technical Depth
- Adjust detail level based on audience (IT professionals vs management)
- For technical audience: Deep dive into OSPF and ACLs
- For management: Focus on security, budget, and ROI

### Demo Preparation
- Have Packet Tracer file open and ready
- Pre-identify PCs for each demo scenario
- Prepare backup screenshots if live demo fails
- Practice demo timing (5-7 minutes total)

### Handling Questions
- If unsure, refer to documentation files
- Technical questions: Show configurations in Packet Tracer
- Budget questions: Reference detailed Bill of Materials
- Security questions: Demonstrate ACL testing

### Timing Guide
- Full presentation: 45-60 minutes
- Short version: 20-25 minutes (Slides 1, 2, 5, 6, 11, 15)
- Demo only: 10-15 minutes (Slide 15 + live interaction)

### Visual Aids
- Use image placeholders for actual campus/building photos
- Add Packet Tracer screenshots for topology
- Include graphs for budget and performance metrics
- Show real DHCP lease table and routing table outputs

---

**End of Presentation**
