# FUTURE VISION SMART CAMPUS
## Computer Networks Course Project Report
### Network Design and Implementation Plan

---

## TABLE OF CONTENTS

1. Case Study Overview & Requirements
2. Buildings & Users Breakdown
3. Packet Tracer Visual Diagram Description
4. Logical IP Addressing Plan (VLSM)
5. Dynamic Routing Protocol
6. Access Control List (ACL) Plan
7. Bill of Materials (Budget)

---

## 1. CASE STUDY OVERVIEW & REQUIREMENTS

### 1.1 Project Name
**Future Vision Smart Campus**

### 1.2 Project Description
Future Vision Smart Campus is a comprehensive network infrastructure solution designed for a large K-12 educational institution. The campus encompasses four dedicated buildings interconnected through high-speed Fiber Optic links, ensuring reliable and fast communication across all facilities. The network architecture prioritizes security, scalability, and performance to support modern educational technology requirements.

### 1.3 Key Requirements

#### Network Architecture
- **Topology**: Distributed Routing Architecture (Hub-and-Spoke Model)
- **Core Components**: 
  - 1 Central ISP Edge Router (Hub)
  - 4 Building Routers (Spokes)
  - 1 Dedicated Cisco ASA Firewall for perimeter security
- **Interconnection**: Fiber Optic backbone for inter-building connectivity
- **Internet Connectivity**: Secured through dedicated firewall appliance

#### Security Requirements
- **Perimeter Protection**: Cisco ASA Firewall positioned between ISP Router and Internet Cloud
- **Network Segmentation**: VLAN-based isolation for different user groups
- **Access Control**: Granular ACL policies to restrict unauthorized access
- **Guest Network Isolation**: Complete separation of guest Wi-Fi from internal resources

#### Performance Requirements
- High-speed fiber optic connections for campus backbone
- Redundant routing protocols for network resilience
- Quality of Service (QoS) support for voice and video traffic
- Scalable infrastructure to accommodate future growth

#### Compliance Requirements
- Educational institution data protection standards
- Physical security integration (IP Cameras, NVR systems)
- IoT device management and security

---

## 2. BUILDINGS & USERS BREAKDOWN

### 2.1 Building A - Administration Block

**Purpose**: Central administrative operations and staff management

**Network Devices**:
- 20 Staff Desktop PCs (Administration, HR, Finance)
- 2 Network Printers (High-capacity laser printers)
- 2 IP Security Cameras (Main entrance and reception monitoring)
- 1 Staff Wi-Fi Access Point (Secured with WPA2-Enterprise)

**VLAN Assignment**: VLAN 10 - Admin VLAN
**Security Level**: High (Restricted access, sensitive data handling)
**Typical Applications**: Student Information Systems, Financial Software, Email, Document Management

---

### 2.2 Building B - Academic Block

**Purpose**: Primary teaching and learning facilities

**Network Devices**:
- **Computer Labs**:
  - Lab 1: 25 Student PCs
  - Lab 2: 25 Student PCs
  - Lab 3: 25 Student PCs
  - Total: 75 Student PCs
- 20 Teacher Laptops (Wireless connectivity)
- 10 Smart Interactive Boards (IoT devices)
- 4 IP Security Cameras (Hallway and lab monitoring)

**VLAN Assignments**:
- VLAN 20 - Student Lab VLAN
- VLAN 21 - Teacher VLAN
- VLAN 22 - Smart Board/IoT VLAN

**Security Level**: Medium (Content filtering, internet access controls)
**Typical Applications**: Learning Management Systems, Educational Software, Research Databases, Multimedia Content

---

### 2.3 Building C - Services & Library

**Purpose**: Information resources and centralized technical infrastructure

**Network Devices**:
- 15 Library PCs (Public access workstations)
- 1 Main NVR (Network Video Recorder) Server - Centralized camera recording
- 1 IoT Management Server - Smart device coordination
- 1 Security Monitor Station (Live camera feeds)
- 2 Library Staff PCs

**VLAN Assignments**:
- VLAN 30 - Library VLAN
- VLAN 31 - Server/NVR VLAN
- VLAN 32 - Security Monitoring VLAN

**Security Level**: Medium-High (Public access with monitoring, critical server infrastructure)
**Special Features**: 
- 24/7 NVR recording from all campus cameras
- Centralized IoT device management
- Security operations center (SOC) monitoring station

---

### 2.4 Building D - Sports & Events Hall

**Purpose**: Athletic facilities and event hosting with guest services

**Network Devices**:
- 2 Coach Desktop PCs (Performance tracking, scheduling)
- 3 IoT Digital Scoreboards (Real-time score display)
- 2 High-Density Guest Wi-Fi Access Points (Event visitors and parents)
- 1 Event Management PC

**VLAN Assignments**:
- VLAN 40 - Sports Staff VLAN
- VLAN 41 - IoT Scoreboard VLAN
- VLAN 99 - Guest Wi-Fi VLAN (Completely Isolated)

**Security Level**: Low for guests, Medium for staff
**Special Features**:
- Captive portal for guest Wi-Fi authentication
- Bandwidth limitation for guest access
- Complete isolation from internal campus networks

---

## 3. PACKET TRACER VISUAL DIAGRAM DESCRIPTION

### 3.1 Network Topology Overview

The Future Vision Smart Campus network implements a **Hub-and-Spoke Distributed Routing Architecture** with multiple routing protocols. This design provides centralized control while allowing distributed intelligence at each building location, with external connectivity through ISP_2 router.

### 3.2 Core Infrastructure Components

#### Internet and External Connectivity Layer
```
[ISP_2 Router]
       |
       | BGP - 10.0.0.0/30
       |
[Router-C (Services & Library)]
       |
       |
[Central ISP Edge Router (Router-CORE)]
```

**ISP_2 Router**: External router providing connectivity using BGP protocol:
- BGP connection to Router-C (Services and Library) using network 10.0.0.0/30
- EIGRP connections to two additional edge routers using networks 11.0.0.0/30 and 12.0.0.0/30
- Provides routing between campus network and external networks

**EIGRP Segment**: Two additional routers connected to ISP_2 for edge connectivity:
- Router-E1: Connected to ISP_2 via 11.0.0.0/30, LAN segment 13.0.0.0/24
- Router-E2: Connected to ISP_2 via 12.0.0.0/30, LAN segment 14.0.0.0/24
- Each router connects to a switch with PC, Laptop, and Access Point

#### Central Hub Router
**ISP Edge Router (Router-CORE)**
- Model: Cisco ISR 4331
- Role: Central aggregation point for all building routers
- Interfaces:
  - GigabitEthernet 0/0/1: Fiber to Router-A (192.168.1.1/30)
  - GigabitEthernet 0/1/0: Fiber to Router-B (192.168.2.1/30)
  - GigabitEthernet 0/1/1: Fiber to Router-C (192.168.3.1/30)
  - Serial 0/2/0: Fiber to Router-D (192.168.4.1/30)

### 3.3 Building Distribution Layer

#### Building A - Administration
```
[Router-A (Cisco ISR 4321)]
       | GigabitEthernet 0/0
       |
[Switch-A (Cisco Catalyst 2960-24TT)]
       |
    [VLANs]
       ├── VLAN 10: Admin Staff (192.168.10.0/25 - 126 hosts)
       │   ├── 20 Staff PCs
       │   └── 2 Printers
       ├── VLAN 11: Admin Wi-Fi (192.168.10.128/26 - 62 hosts)
       │   └── 1 Staff AP
       └── VLAN 12: Admin Cameras (192.168.10.192/27 - 30 hosts)
           └── 2 Security Cameras
```

#### Building B - Academic Block (Hierarchical Architecture)

**Note**: Building B implements a **hierarchical three-tier switch architecture** for scalability and better port capacity management.

```
[Router-B (Cisco ISR 4331)]
       | GigabitEthernet 0/0/1
       |
       | [Trunk: VLANs 20,21,22,23]
       |
[Switch-B (Core/Distribution) - Cisco Catalyst 3650-24PS]
       |
   ____|____________________
  |         |              |
  |         |              |
Switch-B1  Switch-B2    Switch-B3
(VLAN 20)  (VLAN 21)    (VLAN 22)
24 ports   24 ports     12 ports

    [VLAN Distribution]
       ├── VLAN 20: Student Labs (192.168.20.0/25 - 126 hosts)
       │   └── 75 Student PCs (across Switch-B1 and additional ports)
       ├── VLAN 21: Teachers (192.168.20.128/26 - 62 hosts)
       │   └── 20 Teacher Laptops (Switch-B2)
       ├── VLAN 22: Smart Boards (192.168.20.192/27 - 30 hosts)
       │   └── 10 IoT Smart Boards (Switch-B3)
       └── VLAN 23: Academic Cameras (192.168.20.224/27 - 30 hosts)
           └── 4 Security Cameras (Switch-B directly)
```

**Hierarchical Design Benefits**:
- **Scalability**: Total 72+ access ports (24 per access switch × 3)
- **VLAN Segregation**: Each access switch handles specific user groups
- **Better Performance**: Traffic isolated at access layer
- **Easier Management**: Clear separation of student, teacher, and IoT devices
- **Port Capacity**: Sufficient for 75 student PCs + 20 teachers + 10 smart boards

#### Building C - Services & Library
```
[Router-C (Cisco ISR 4321)]
       | GigabitEthernet 0/0
       |
[Switch-C (Cisco Catalyst 2960-24TT)]
       |
    [VLANs]
       ├── VLAN 30: Library Users (192.168.30.0/26 - 62 hosts)
       │   └── 15 Library PCs + 2 Staff PCs
       ├── VLAN 31: Servers (192.168.30.64/27 - 30 hosts)
       │   ├── NVR Server
       │   └── IoT Management Server
       └── VLAN 32: Security Monitoring (192.168.30.96/27 - 30 hosts)
           └── 1 Security Monitor Station
```

#### Building D - Sports & Events
```
[Router-D (Cisco ISR 4221)]
       | GigabitEthernet 0/0
       |
[Switch-D (Cisco Catalyst 2960-24TT)]
       |
    [VLANs]
       ├── VLAN 40: Sports Staff (192.168.40.0/27 - 30 hosts)
       │   └── 2 Coach PCs + 1 Event PC
       ├── VLAN 41: Scoreboards (192.168.40.32/27 - 30 hosts)
       │   └── 3 IoT Scoreboards
       └── VLAN 99: Guest Wi-Fi (192.168.99.0/24 - 254 hosts)
           └── 2 High-Density APs
```

### 3.4 Cabling Infrastructure

**Fiber Optic Backbone**:
- Type: Single-Mode Fiber (OS2)
- Connectors: LC/UPC
- Distance: Up to 10km capability
- Bandwidth: 1 Gbps minimum, 10 Gbps capable

**Building Internal Cabling**:
- Type: Cat6a UTP
- Maximum Distance: 100 meters per segment
- Bandwidth: 10 Gbps capable

### 3.5 Cisco Packet Tracer Implementation Notes

**Router Placement**:
- Position the ISP Edge Router centrally on the canvas
- Arrange the four building routers (A, B, C, D) around the central router in a star topology
- Place the Cisco ASA Firewall above the ISP Edge Router
- Add the Internet Cloud icon above the Firewall

**Visual Enhancements**:
- Use different background colors/containers for each building section
- Add labels for building names and VLAN purposes
- Use thick fiber optic lines (select orange/yellow color) for WAN links
- Use standard ethernet lines for LAN connections
- Add text annotations showing IP addresses on major links

---

## 4. LOGICAL IP ADDRESSING PLAN (VLSM)

### 4.1 IP Addressing Strategy

The network utilizes the private IP address space **192.168.0.0/16** with Variable Length Subnet Masking (VLSM) to efficiently allocate addresses based on actual requirements. This approach minimizes IP address waste while providing room for future expansion.

### 4.2 Complete Subnet Allocation Table

| Network Segment | Network Address | Subnet Mask | CIDR | Usable IPs | First Host | Last Host | Broadcast | Purpose |
|-----------------|-----------------|-------------|------|------------|------------|-----------|-----------|---------|
| **WAN Links** |
| ISP_2-RouterC Link | 10.0.0.0 | 255.255.255.252 | /30 | 2 | 10.0.0.1 | 10.0.0.2 | 10.0.0.3 | ISP_2 to Router-C (BGP) |
| ISP_2-RouterE1 Link | 11.0.0.0 | 255.255.255.252 | /30 | 2 | 11.0.0.1 | 11.0.0.2 | 11.0.0.3 | ISP_2 to Router-E1 (EIGRP) |
| ISP_2-RouterE2 Link | 12.0.0.0 | 255.255.255.252 | /30 | 2 | 12.0.0.1 | 12.0.0.2 | 12.0.0.3 | ISP_2 to Router-E2 (EIGRP) |
| ISP-RouterA Link | 192.168.1.0 | 255.255.255.252 | /30 | 2 | 192.168.1.1 | 192.168.1.2 | 192.168.1.3 | Core to Building A |
| ISP-RouterB Link | 192.168.2.0 | 255.255.255.252 | /30 | 2 | 192.168.2.1 | 192.168.2.2 | 192.168.2.3 | Core to Building B |
| ISP-RouterC Link | 192.168.3.0 | 255.255.255.252 | /30 | 2 | 192.168.3.1 | 192.168.3.2 | 192.168.3.3 | Core to Building C |
| ISP-RouterD Link | 192.168.4.0 | 255.255.255.252 | /30 | 2 | 192.168.4.1 | 192.168.4.2 | 192.168.4.3 | Core to Building D |
| **Edge Router LAN Segments** |
| Router-E1 LAN | 13.0.0.0 | 255.255.255.0 | /24 | 254 | 13.0.0.1 | 13.0.0.254 | 13.0.0.255 | Edge Router 1 LAN |
| Router-E2 LAN | 14.0.0.0 | 255.255.255.0 | /24 | 254 | 14.0.0.1 | 14.0.0.254 | 14.0.0.255 | Edge Router 2 LAN |
| **Building A VLANs** |
| VLAN 10 - Admin Staff | 192.168.10.0 | 255.255.255.128 | /25 | 126 | 192.168.10.1 | 192.168.10.126 | 192.168.10.127 | 20 PCs + 2 Printers |
| VLAN 11 - Admin Wi-Fi | 192.168.10.128 | 255.255.255.192 | /26 | 62 | 192.168.10.129 | 192.168.10.190 | 192.168.10.191 | Staff Wireless |
| VLAN 12 - Admin Cameras | 192.168.10.192 | 255.255.255.224 | /27 | 30 | 192.168.10.193 | 192.168.10.222 | 192.168.10.223 | Security Cameras |
| **Building B VLANs** |
| VLAN 20 - Student Labs | 192.168.20.0 | 255.255.255.128 | /25 | 126 | 192.168.20.1 | 192.168.20.126 | 192.168.20.127 | 75 Student PCs |
| VLAN 21 - Teachers | 192.168.20.128 | 255.255.255.192 | /26 | 62 | 192.168.20.129 | 192.168.20.190 | 192.168.20.191 | 20 Teacher Laptops |
| VLAN 22 - Smart Boards | 192.168.20.192 | 255.255.255.224 | /27 | 30 | 192.168.20.193 | 192.168.20.222 | 192.168.20.223 | 10 IoT Smart Boards |
| VLAN 23 - Academic Cameras | 192.168.20.224 | 255.255.255.224 | /27 | 30 | 192.168.20.225 | 192.168.20.254 | 192.168.20.255 | 4 Security Cameras |
| **Building C VLANs** |
| VLAN 30 - Library Users | 192.168.30.0 | 255.255.255.192 | /26 | 62 | 192.168.30.1 | 192.168.30.62 | 192.168.30.63 | 15 Library + 2 Staff PCs |
| VLAN 31 - Servers | 192.168.30.64 | 255.255.255.224 | /27 | 30 | 192.168.30.65 | 192.168.30.94 | 192.168.30.95 | NVR + IoT Server |
| VLAN 32 - Security Monitor | 192.168.30.96 | 255.255.255.224 | /27 | 30 | 192.168.30.97 | 192.168.30.126 | 192.168.30.127 | Monitor Station |
| **Building D VLANs** |
| VLAN 40 - Sports Staff | 192.168.40.0 | 255.255.255.224 | /27 | 30 | 192.168.40.1 | 192.168.40.30 | 192.168.40.31 | 3 Staff PCs |
| VLAN 41 - Scoreboards | 192.168.40.32 | 255.255.255.224 | /27 | 30 | 192.168.40.33 | 192.168.40.62 | 192.168.40.63 | 3 IoT Scoreboards |
| VLAN 99 - Guest Wi-Fi | 192.168.99.0 | 255.255.255.0 | /24 | 254 | 192.168.99.1 | 192.168.99.254 | 192.168.99.255 | High-Density Guest |

### 4.3 Gateway Assignments

For each VLAN, the router interface will be configured with the first usable IP address as the default gateway:

- **Building A Router-A**: 
  - Gi0/0.10: 192.168.10.1 (VLAN 10)
  - Gi0/0.11: 192.168.10.129 (VLAN 11)
  - Gi0/0.12: 192.168.10.193 (VLAN 12)

- **Building B Router-B**: 
  - Gi0/0.20: 192.168.20.1 (VLAN 20)
  - Gi0/0.21: 192.168.20.129 (VLAN 21)
  - Gi0/0.22: 192.168.20.193 (VLAN 22)
  - Gi0/0.23: 192.168.20.225 (VLAN 23)

- **Building C Router-C**: 
  - Gi0/0.30: 192.168.30.1 (VLAN 30)
  - Gi0/0.31: 192.168.30.65 (VLAN 31)
  - Gi0/0.32: 192.168.30.97 (VLAN 32)

- **Building D Router-D**: 
  - Gi0/0.40: 192.168.40.1 (VLAN 40)
  - Gi0/0.41: 192.168.40.33 (VLAN 41)
  - Gi0/0.99: 192.168.99.1 (VLAN 99)

### 4.4 DHCP Scope Configuration

Each building router will run DHCP server services for its local VLANs:

**Example DHCP Configuration (Router-A, VLAN 10)**:
```
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp pool ADMIN-STAFF
 network 192.168.10.0 255.255.255.128
 default-router 192.168.10.1
 dns-server 8.8.8.8 8.8.4.4
 domain-name futurevision.edu
```

---

## 5. DYNAMIC ROUTING PROTOCOLS

### 5.1 Multi-Protocol Routing Architecture

The network now implements a **multi-protocol routing architecture** combining three protocols:
- **OSPF**: Internal campus routing (Area 0)
- **BGP**: External routing between ISP_2 and Router-C
- **EIGRP**: Edge routing segment connected to ISP_2

### 5.2 OSPF (Open Shortest Path First) - Internal Campus Routing

**Selected Protocol**: OSPF Version 2 (OSPFv2)
**Routing Process ID**: 1
**Area Design**: Single Area 0 (Backbone Area)
**Scope**: All building routers (Router-CORE, Router-A, Router-B, Router-C, Router-D)

### 5.3 BGP (Border Gateway Protocol) - External Connectivity

**Protocol Version**: BGP-4
**AS Numbers**: 
- ISP_2: AS 65000
- Campus (Router-C): AS 65001
**Connection**: ISP_2 (10.0.0.1) to Router-C (10.0.0.2) via network 10.0.0.0/30

**BGP Configuration - ISP_2**:
```cisco
router bgp 65000
 bgp router-id 10.0.0.1
 neighbor 10.0.0.2 remote-as 65001
 network 11.0.0.0 mask 255.255.255.252
 network 12.0.0.0 mask 255.255.255.252
 network 13.0.0.0 mask 255.255.255.0
 network 14.0.0.0 mask 255.255.255.0
```

**BGP Configuration - Router-C**:
```cisco
router bgp 65001
 bgp router-id 10.0.0.2
 neighbor 10.0.0.1 remote-as 65000
 network 192.168.0.0 mask 255.255.0.0
 redistribute ospf 1
```

### 5.4 EIGRP (Enhanced Interior Gateway Routing Protocol) - Edge Segment

**Routing Process AS**: 100
**Scope**: ISP_2, Router-E1, Router-E2
**Networks**: 11.0.0.0/30, 12.0.0.0/30, 13.0.0.0/24, 14.0.0.0/24

**EIGRP Configuration - ISP_2**:
```cisco
router eigrp 100
 network 11.0.0.0 0.0.0.3
 network 12.0.0.0 0.0.0.3
 no auto-summary
```

**EIGRP Configuration - Router-E1**:
```cisco
router eigrp 100
 network 11.0.0.0 0.0.0.3
 network 13.0.0.0 0.0.0.255
 no auto-summary
```

**EIGRP Configuration - Router-E2**:
```cisco
router eigrp 100
 network 12.0.0.0 0.0.0.3
 network 14.0.0.0 0.0.0.255
 no auto-summary
```

### 5.5 Justification for Multi-Protocol Architecture

#### OSPF for Internal Campus
- Fast convergence (1-5 seconds) for internal networks
- Hierarchical design with areas for scalability
- Open standard suitable for educational environment
- Well-suited for hub-and-spoke topology

#### BGP for External Routing
- Industry standard for inter-AS routing
- Policy-based routing control
- Scalability for external connections
- Path vector protocol prevents routing loops between autonomous systems

#### EIGRP for Edge Segment
- Cisco-optimized protocol for specific segment
- Fast convergence with DUAL algorithm
- Automatic summarization capabilities
- Low bandwidth utilization compared to other protocols

### 5.2 Justification for OSPF

#### Scalability
OSPF is inherently designed for large networks and can efficiently handle the Future Vision Smart Campus topology with multiple routers and subnets. Key scalability features include:
- Hierarchical design with areas (future expansion capability)
- Efficient routing table updates using Link State Advertisements (LSAs)
- Support for hundreds of routers in a single area
- Fast convergence even with network topology changes

#### Fast Convergence
In an educational environment where network reliability is critical, OSPF provides:
- Sub-second convergence times (typically 1-5 seconds)
- Immediate detection of link failures
- Automatic route recalculation using Dijkstra's algorithm
- Hello/Dead timers for quick neighbor relationship monitoring (Hello: 10s, Dead: 40s)

#### Open Standard
OSPF is vendor-neutral and widely supported:
- Interoperability between different router vendors
- Industry standard (RFC 2328)
- Well-documented and extensively tested
- Large community support and expertise

#### Additional Benefits
- **Classless Routing**: Native VLSM support for efficient IP addressing
- **Load Balancing**: Equal-cost multi-path routing capability
- **Authentication**: Supports MD5 authentication for routing updates security
- **Bandwidth-based Metrics**: Route selection based on link capacity
- **No Hop Count Limitation**: Unlike RIP's 15-hop limit

### 5.3 OSPF Network Design

#### Area Configuration
```
All routers in Area 0 (Backbone Area)
- Simplifies design for single-site campus
- Eliminates need for ABR (Area Border Routers)
- Optimal for current network size
- Easy to segment into multiple areas in future expansion
```

#### OSPF Configuration Example (ISP Edge Router)
```
router ospf 1
 router-id 1.1.1.1
 log-adjacency-changes
 passive-interface GigabitEthernet 0/0/0
 network 192.168.1.0 0.0.0.3 area 0
 network 192.168.2.0 0.0.0.3 area 0
 network 192.168.3.0 0.0.0.3 area 0
 network 192.168.4.0 0.0.0.3 area 0
```

#### OSPF Configuration Example (Router-A)
```
router ospf 1
 router-id 10.10.10.10
 log-adjacency-changes
 network 192.168.1.0 0.0.0.3 area 0
 network 192.168.10.0 0.0.0.127 area 0
 network 192.168.10.128 0.0.0.63 area 0
 network 192.168.10.192 0.0.0.31 area 0
 default-information originate
```

### 5.6 Router ID Assignment

| Router | Router ID | Purpose | Protocol |
|--------|-----------|---------|----------|
| ISP Edge Router | 1.1.1.1 | Central hub identification | OSPF |
| Router-A (Admin) | 10.10.10.10 | Building A identifier | OSPF |
| Router-B (Academic) | 20.20.20.20 | Building B identifier | OSPF |
| Router-C (Services) | 30.30.30.30 | Building C identifier | OSPF, BGP |
| Router-D (Sports) | 40.40.40.40 | Building D identifier | OSPF |
| ISP_2 | 10.0.0.1 | External ISP router | BGP, EIGRP |
| Router-E1 | 13.0.0.1 | Edge router 1 | EIGRP |
| Router-E2 | 14.0.0.1 | Edge router 2 | EIGRP |

### 5.7 OSPF Configuration Examples

#### Router-CORE (ISP Edge Router)
```cisco
router ospf 1
 router-id 1.1.1.1
 log-adjacency-changes
 network 192.168.1.0 0.0.0.3 area 0
 network 192.168.2.0 0.0.0.3 area 0
 network 192.168.3.0 0.0.0.3 area 0
 network 192.168.4.0 0.0.0.3 area 0
```

#### Router-C with BGP Redistribution
```cisco
router ospf 1
 router-id 30.30.30.30
 log-adjacency-changes
 network 192.168.3.0 0.0.0.3 area 0
 network 192.168.30.0 0.0.0.63 area 0
 network 192.168.30.64 0.0.0.31 area 0
 network 192.168.30.96 0.0.0.31 area 0
 redistribute bgp 65001 subnets

interface GigabitEthernet 0/1/0
 description BGP Link to ISP_2
 ip address 10.0.0.2 255.255.255.252
 no shutdown
```

### 5.8 Routing Protocol Integration

#### Route Redistribution
- **Router-C**: Redistributes OSPF routes into BGP for external advertisement
- **Router-C**: Redistributes BGP routes into OSPF with appropriate metrics
- **ISP_2**: Advertises EIGRP networks via BGP to campus network

#### Metric Adjustment
```cisco
! On Router-C
router ospf 1
 redistribute bgp 65001 subnets metric 100 metric-type 1

router bgp 65001
 redistribute ospf 1 match internal external 1 external 2
```
| Router-C (Services) | 30.30.30.30 | Building C identifier |
| Router-D (Sports) | 40.40.40.40 | Building D identifier |

### 5.5 OSPF Optimization Settings

**Interface Priority Settings**:
- All WAN interfaces: Priority 0 (prevent DR/BDR election on point-to-point links)
- Use point-to-point network type on WAN links for faster convergence

**Timer Optimization** (for faster convergence in small campus):
```
interface GigabitEthernet 0/0/1
 ip ospf hello-interval 5
 ip ospf dead-interval 15
```

**Passive Interfaces**:
- Configure all LAN-facing interfaces as passive to prevent unnecessary OSPF hellos on end-user networks
- Only WAN links between routers should exchange OSPF routing information

---

## 6. ACCESS CONTROL LIST (ACL) PLAN

### 6.1 Updated ACL Security Strategy

**IMPORTANT NOTE**: All previous Access Control Lists have been removed. The network now implements a new security policy focusing on Library access control and inter-building security rules.

Access Control Lists provide granular security enforcement to protect sensitive network resources. The Future Vision Smart Campus now implements the following ACL policies:

1. **Library Access Control**: Only Library PCs permitted to access the network
2. **Admin Building Protection**: All buildings denied access to Admin Building
3. **Services Building Protection**: Student Building denied access to Services Building servers
4. **Admin Building Privilege**: Admin Building has full access to all resources

**⚠️ Packet Tracer Compatibility Note**:
All ACL configurations are compatible with Cisco Packet Tracer. Packet Tracer **does not support the `log` keyword**.

### 6.2 NEW ACL Rule 1: Library Access Control

**Objective**: Permit ONLY Library PCs (VLAN 30) to access the network, deny all other traffic from Library segment

**Implementation Location**: Router-C (Services & Library building router)

**ACL Configuration**:
```cisco
! Extended ACL for Library access control
ip access-list extended LIBRARY-ACCESS-CONTROL
 ! Permit Library PCs to access all networks
 permit ip 192.168.30.0 0.0.0.63 any
 ! Deny all other traffic from Library segment (including servers, monitoring)
 deny ip 192.168.30.64 0.0.0.31 any
 deny ip 192.168.30.96 0.0.0.31 any
 ! Permit return traffic
 permit ip any 192.168.30.0 0.0.0.63
!
! Apply ACL to Library VLAN interface
interface GigabitEthernet 0/0/1.30
 ip access-group LIBRARY-ACCESS-CONTROL in
```

**Security Benefits**:
- Ensures only Library user PCs can initiate connections
- Prevents unauthorized access from server/monitoring segments
- Maintains access control integrity for public access areas

---

### 6.3 NEW ACL Rule 2: Admin Building Full Access

**Objective**: Permit Admin Building (Building A) full access to ALL school resources

**Implementation Location**: Router-A (Administration building router)

**ACL Configuration**:
```cisco
! Standard ACL permitting all Admin traffic
! No ACL restrictions on Admin Building
! Admin users have full access to all networks
! This is implemented by NOT applying any restrictive ACL on Router-A interfaces
```

**Note**: Admin Building requires no restrictive ACLs as it has privileged access to all resources.

---

### 6.4 NEW ACL Rule 3: Protect Admin Building from Other Buildings

**Objective**: Deny ALL buildings access to Admin Building resources (Building A - VLANs 10, 11, 12)

**Implementation Locations**: Router-B, Router-C, Router-D

**ACL Configuration - Router-B (Academic Building)**:
```cisco
! Extended ACL denying access to Admin Building
ip access-list extended DENY-ADMIN-ACCESS
 ! Deny access to Admin Staff VLAN
 deny ip 192.168.20.0 0.0.0.127 192.168.10.0 0.0.0.127
 ! Deny access to Admin Wi-Fi VLAN
 deny ip 192.168.20.0 0.0.0.127 192.168.10.128 0.0.0.63
 ! Deny access to Admin Cameras VLAN
 deny ip 192.168.20.0 0.0.0.127 192.168.10.192 0.0.0.31
 ! Permit all other traffic
 permit ip any any
!
! Apply ACL to Student Lab VLAN interface
interface GigabitEthernet 0/0/1.20
 ip access-group DENY-ADMIN-ACCESS in
!
! Also apply to other VLANs in Building B
interface GigabitEthernet 0/0/1.21
 ip access-group DENY-ADMIN-ACCESS in
!
interface GigabitEthernet 0/0/1.22
 ip access-group DENY-ADMIN-ACCESS in
!
interface GigabitEthernet 0/0/1.23
 ip access-group DENY-ADMIN-ACCESS in
```

**ACL Configuration - Router-C (Services & Library)**:
```cisco
! Extended ACL denying Library/Services access to Admin Building
ip access-list extended DENY-ADMIN-ACCESS
 ! Deny Library users access to Admin VLANs
 deny ip 192.168.30.0 0.0.0.63 192.168.10.0 0.0.0.127
 deny ip 192.168.30.0 0.0.0.63 192.168.10.128 0.0.0.63
 deny ip 192.168.30.0 0.0.0.63 192.168.10.192 0.0.0.31
 ! Permit all other traffic
 permit ip any any
!
! Apply to Library VLAN
interface GigabitEthernet 0/0/1.30
 ip access-group DENY-ADMIN-ACCESS in
```

**ACL Configuration - Router-D (Sports & Events)**:
```cisco
! Extended ACL denying Sports/Events access to Admin Building
ip access-list extended DENY-ADMIN-ACCESS
 ! Deny Sports staff access to Admin VLANs
 deny ip 192.168.40.0 0.0.0.31 192.168.10.0 0.0.0.127
 deny ip 192.168.40.0 0.0.0.31 192.168.10.128 0.0.0.63
 deny ip 192.168.40.0 0.0.0.31 192.168.10.192 0.0.0.31
 ! Deny Guest Wi-Fi access to Admin VLANs
 deny ip 192.168.99.0 0.0.0.255 192.168.10.0 0.0.0.127
 deny ip 192.168.99.0 0.0.0.255 192.168.10.128 0.0.0.63
 deny ip 192.168.99.0 0.0.0.255 192.168.10.192 0.0.0.31
 ! Permit all other traffic
 permit ip any any
!
! Apply to Sports Staff VLAN
interface GigabitEthernet 0/0/1.40
 ip access-group DENY-ADMIN-ACCESS in
!
! Apply to Guest Wi-Fi VLAN
interface GigabitEthernet 0/0/1.99
 ip access-group DENY-ADMIN-ACCESS in
```

**Security Benefits**:
- Protects Admin Building from unauthorized access
- Enforces strict access control to sensitive administrative resources
- Prevents lateral movement from other buildings
- Maintains administrative privilege separation

---

### 6.5 NEW ACL Rule 4: Deny Student Building Access to Services Building Servers

**Objective**: Deny Student Building (Building B) direct access to Services Building servers (VLAN 31) to ensure data integrity

**Implementation Location**: Router-B (Academic Block router)

**ACL Configuration**:
```cisco
! Extended ACL denying student access to servers
ip access-list extended DENY-SERVER-ACCESS
 ! Deny Student Lab access to Server VLAN
 deny ip 192.168.20.0 0.0.0.127 192.168.30.64 0.0.0.31
 ! Deny Teacher access to Server VLAN
 deny ip 192.168.20.128 0.0.0.63 192.168.30.64 0.0.0.31
 ! Permit access to Library (public resources)
 permit ip any 192.168.30.0 0.0.0.63
 ! Permit all other traffic
 permit ip any any
!
! Apply ACL to Student VLAN interface
interface GigabitEthernet 0/0/1.20
 ip access-group DENY-SERVER-ACCESS in
!
! Apply ACL to Teacher VLAN interface
interface GigabitEthernet 0/0/1.21
 ip access-group DENY-SERVER-ACCESS in
```

**Security Benefits**:
- Protects critical servers (NVR, IoT Management) from student access
- Ensures data integrity by preventing direct server access
- Students can still access Library public resources
- Prevents unauthorized data modification or exfiltration

---

### 6.6 ACL Implementation Summary

| Building | ACL Policy | Purpose | Applied On |
|----------|-----------|---------|------------|
| Admin (A) | None (Full Access) | Admin has access to all resources | No restrictions |
| Academic (B) | DENY-ADMIN-ACCESS | Block access to Admin Building | All Building B VLANs |
| Academic (B) | DENY-SERVER-ACCESS | Block access to Services servers | Student & Teacher VLANs |
| Services (C) | LIBRARY-ACCESS-CONTROL | Only Library PCs can access network | VLAN 30 |
| Services (C) | DENY-ADMIN-ACCESS | Block access to Admin Building | VLAN 30 |
| Sports (D) | DENY-ADMIN-ACCESS | Block access to Admin Building | VLANs 40, 99 |

### 6.7 Removed ACLs

The following ACLs from the previous design have been **REMOVED**:
- ❌ GUEST-RESTRICTION (Guest Wi-Fi isolation - removed)
- ❌ STUDENT-RESTRICTION (Old student restrictions - removed)
- ❌ Firewall ACLs (OUTSIDE-IN, INSIDE-OUT - firewall removed)
- ❌ IOT-ISOLATION (IoT device isolation - removed)
- ❌ SSH Access Control ACLs (VTY line protection - removed)

### 6.8 ACL Verification Commands

```cisco
! View configured ACLs
show access-lists

! View ACL statistics
show ip access-lists

! Verify ACL application on interfaces
show ip interface GigabitEthernet 0/0/1.20
show ip interface GigabitEthernet 0/0/1.30

! Test ACL functionality
! From Student PC (should be denied):
ping 192.168.10.10  ! Admin Building - should fail
ping 192.168.30.70  ! Server VLAN - should fail

! From Admin PC (should succeed):
ping 192.168.30.70  ! Server access - should work
ping 192.168.20.10  ! Student access - should work
```
 ip access-group STUDENT-RESTRICTION in
```

---
 class inspection_default
  inspect icmp
  inspect http
  inspect https
  inspect dns
```

**Security Benefits**:
- Application-layer inspection for HTTP/HTTPS
- Prevents inbound attacks from the internet
- NAT/PAT hides internal IP structure
- Logging for security monitoring and compliance
- Protection against common web-based attacks

---

## 7. BILL OF MATERIALS (BUDGET)

### 7.1 Budget Overview

The following Bill of Materials provides a comprehensive cost estimate for implementing the updated Future Vision Smart Campus network infrastructure with the new topology changes. Prices are based on current market rates for educational institutions (with standard educational discounts applied).

**Key Changes**:
- Removed: Cloud and Firewall components
- Added: ISP_2 router, Router-E1, Router-E2, Switch-E1, Switch-E2, and additional end devices

### 7.2 Detailed Equipment List

#### 7.2.1 Core Network Equipment

| Item | Model | Quantity | Unit Price (USD) | Total Price (USD) | Specifications |
|------|-------|----------|------------------|-------------------|----------------|
| **Routers** |
| ISP Edge Router | Cisco ISR 4331 | 1 | $3,500 | $3,500 | 3 GE ports, 2 SFP slots, 4GB RAM |
| Building Router (Large) | Cisco ISR 4321 | 2 | $2,800 | $5,600 | 2 GE ports, 2 SFP slots, 4GB RAM |
| Building Router (Medium) | Cisco ISR 4221 | 2 | $1,900 | $3,800 | 2 GE ports, 2 SFP slots, 4GB RAM |
| **New - External Routers** |
| ISP_2 Router | Cisco ISR 4331 | 1 | $3,500 | $3,500 | BGP + EIGRP capable |
| Edge Router E1 | Cisco ISR 4221 | 1 | $1,900 | $1,900 | EIGRP capable |
| Edge Router E2 | Cisco ISR 4221 | 1 | $1,900 | $1,900 | EIGRP capable |
| **Removed - Firewall** |
| ~~Perimeter Firewall~~ | ~~Cisco ASA 5506-X~~ | ~~1~~ | ~~$650~~ | ~~-$650~~ | Removed from topology |
| **Subtotal Routers** | | | | **$19,550** | |

#### 7.2.2 Switching Infrastructure

| Item | Model | Quantity | Unit Price (USD) | Total Price (USD) | Specifications |
|------|-------|----------|------------------|-------------------|----------------|
| Core Switch (Building B) | Cisco Catalyst 3650-24PD | 1 | $4,200 | $4,200 | 24 PoE+ ports, Layer 3, 10G uplink |
| Access Switches (Buildings) | Cisco Catalyst 2960-24TT | 4 | $1,200 | $4,800 | 24 ports, Layer 2, 2 SFP uplinks |
| **New - Edge Switches** |
| Edge Switch E1 | Cisco Catalyst 2960-24TT | 1 | $1,200 | $1,200 | For Router-E1 LAN segment |
| Edge Switch E2 | Cisco Catalyst 2960-24TT | 1 | $1,200 | $1,200 | For Router-E2 LAN segment |
| **Subtotal Switches** | | | | **$11,400** | |

#### 7.2.3 Wireless Infrastructure

| Item | Model | Quantity | Unit Price (USD) | Total Price (USD) | Specifications |
|------|-------|----------|------------------|-------------------|----------------|
| Staff Wi-Fi AP (Building A) | Cisco Aironet 1852i | 1 | $950 | $950 | 802.11ac Wave 2, Internal Antenna |
| High-Density Guest APs | Cisco Aironet 2802i | 2 | $1,200 | $2,400 | 802.11ac Wave 2, High Density |
| Wireless LAN Controller | Cisco 3504 WLC | 1 | $4,500 | $4,500 | Manages up to 150 APs |
| **Subtotal Wireless** | | | | **$7,850** | |

#### 7.2.4 Security & Surveillance Systems

| Item | Model | Quantity | Unit Price (USD) | Total Price (USD) | Specifications |
|------|-------|----------|------------------|-------------------|----------------|
| IP Security Cameras | Cisco Meraki MV22X | 12 | $600 | $7,200 | 1080p, 128° FoV, 30fps |
| Network Video Recorder | Dedicated NVR Server | 1 | $2,500 | $2,500 | 48TB storage, RAID 6 |
| Security Monitor Station | Dell Precision 3640 | 1 | $1,800 | $1,800 | Dual monitor support |
| **Subtotal Security Systems** | | | | **$11,500** | |

#### 7.2.5 IoT & Smart Devices

| Item | Model | Quantity | Unit Price (USD) | Total Price (USD) | Specifications |
|------|-------|----------|------------------|-------------------|----------------|
| Interactive Smart Boards | Cisco Webex Board 70 | 10 | $5,000 | $50,000 | 70" 4K, Touch, Built-in Camera |
| IoT Digital Scoreboards | Daktronics LED Scoreboard | 3 | $3,500 | $10,500 | Full-color LED, Network controlled |
| IoT Management Server | Dell PowerEdge T440 | 1 | $3,200 | $3,200 | IoT device management platform |
| **Subtotal IoT Devices** | | | | **$63,700** | |

#### 7.2.6 End-User Devices

| Item | Model | Quantity | Unit Price (USD) | Total Price (USD) | Specifications |
|------|-------|----------|------------------|-------------------|----------------|
| Staff Desktop PCs | Dell OptiPlex 3090 | 22 | $750 | $16,500 | i5, 8GB RAM, 256GB SSD |
| Student Lab PCs | HP ProDesk 400 G7 | 75 | $650 | $48,750 | i3, 8GB RAM, 256GB SSD |
| Teacher Laptops | HP ProBook 450 G8 | 20 | $900 | $18,000 | i5, 8GB RAM, 512GB SSD |
| Library Public PCs | Dell OptiPlex 3080 | 15 | $650 | $9,750 | i3, 8GB RAM, 256GB SSD |
| Coach/Staff PCs | Dell OptiPlex 3090 | 3 | $750 | $2,250 | i5, 8GB RAM, 256GB SSD |
| Network Printers | HP LaserJet Enterprise | 2 | $1,800 | $3,600 | 50ppm, Duplex, Network |
| **New - Edge Segment Devices** |
| Edge PCs (E1) | Dell OptiPlex 3080 | 1 | $650 | $650 | For Router-E1 LAN |
| Edge Laptops (E1) | HP ProBook 450 G8 | 1 | $900 | $900 | For Router-E1 LAN |
| Edge Access Point (E1) | Cisco Aironet 1852i | 1 | $950 | $950 | For Router-E1 LAN |
| Edge PCs (E2) | Dell OptiPlex 3080 | 1 | $650 | $650 | For Router-E2 LAN |
| Edge Laptops (E2) | HP ProBook 450 G8 | 1 | $900 | $900 | For Router-E2 LAN |
| Edge Access Point (E2) | Cisco Aironet 1852i | 1 | $950 | $950 | For Router-E2 LAN |
| **Subtotal End-User Devices** | | | | **$103,850** | |

#### 7.2.7 Cabling & Infrastructure

| Item | Description | Quantity | Unit Price (USD) | Total Price (USD) | Specifications |
|------|-------------|----------|------------------|-------------------|----------------|
| Fiber Optic Cable | OS2 Single-Mode Fiber | 2000m | $2.50/m | $5,000 | 9/125μm, LC connectors |
| Cat6a Cable | UTP Ethernet Cable | 5000m | $0.75/m | $3,750 | 10Gbps capable, plenum rated |
| Fiber Patch Panels | 24-Port LC Panels | 4 | $250 | $1,000 | Rack-mountable |
| Cat6a Patch Panels | 48-Port RJ45 Panels | 6 | $150 | $900 | 1U rack-mountable |
| Network Rack Cabinets | 42U Server Racks | 4 | $800 | $3,200 | Lockable, ventilated |
| Cable Management | Trays & Organizers | 1 set | $500 | $500 | Complete cable management |
| Fiber Termination | Professional Installation | 20 hrs | $100/hr | $2,000 | Certified technician |
| **Subtotal Cabling** | | | | **$16,350** | |

#### 7.2.8 Software & Licensing

| Item | Description | Quantity | Unit Price (USD) | Total Price (USD) | Period |
|------|-------------|----------|------------------|-------------------|--------|
| Cisco IOS Licenses | Advanced IP Services | 8 | $400 | $3,200 | Perpetual (includes new routers) |
| ~~ASA Firewall License~~ | ~~FirePOWER Services~~ | ~~1~~ | ~~$500~~ | ~~-$500~~ | Removed |
| Wireless Controller Licenses | Additional AP Licenses | 5 | $150 | $750 | Perpetual (includes new APs) |
| NVR Software License | Recording & Management | 12 cameras | $50 | $600 | Annual |
| Network Management | Cisco DNA Center (Essentials) | 1 | $3,000 | $3,000 | 3 years |
| BGP/EIGRP License | Advanced Routing Protocols | 3 | $300 | $900 | Perpetual |
| **Subtotal Software** | | | | **$7,950** | |

#### 7.2.9 Installation & Professional Services

| Item | Description | Quantity | Unit Price (USD) | Total Price (USD) | Notes |
|------|-------------|----------|------------------|-------------------|-------|
| Network Design Services | Professional Consulting | 50 hrs | $150/hr | $7,500 | Architecture & Multi-protocol planning |
| Installation Services | Equipment Setup | 100 hrs | $100/hr | $10,000 | Router/Switch Configuration (more routers) |
| Testing & Validation | System Testing | 30 hrs | $125/hr | $3,750 | BGP/EIGRP/OSPF testing |
| Documentation | Network Documentation | 25 hrs | $100/hr | $2,500 | As-built drawings, configs |
| Training | Staff Training | 20 hrs | $150/hr | $3,000 | Multi-protocol routing training |
| **Subtotal Professional Services** | | | | **$26,750** | |

#### 7.2.10 Contingency & Maintenance

| Item | Description | Percentage | Base Amount | Total Price (USD) | Purpose |
|------|-------------|------------|-------------|-------------------|---------|
| Contingency Reserve | Unexpected costs | 5% | $248,750 | $12,438 | Equipment changes, issues |
| Annual Maintenance | Support & Warranty | 10% | $234,950 | $23,495 | First year maintenance |
| **Subtotal Contingency** | | | | **$35,933** | |

---

### 7.3 Budget Summary

| Category | Total Cost (USD) | Percentage of Total | Change from Previous |
|----------|------------------|---------------------|----------------------|
| Routers (No Firewall) | $19,550 | 6.5% | +$6,000 (3 new routers) |
| Switches | $11,400 | 3.8% | +$2,400 (2 new switches) |
| Wireless Infrastructure | $7,850 | 2.6% | No change |
| Security & Surveillance | $11,500 | 3.8% | No change |
| IoT & Smart Devices | $63,700 | 21.2% | No change |
| End-User Devices | $103,850 | 34.6% | +$5,000 (6 new devices) |
| Cabling & Infrastructure | $16,350 | 5.4% | No change |
| Software & Licensing | $7,950 | 2.6% | +$1,400 |
| Professional Services | $26,750 | 8.9% | +$5,350 |
| Contingency & Maintenance | $31,250 | 10.4% | Recalculated |
| **GRAND TOTAL** | **$300,150** | **100%** | **+$15,467** |

**Key Changes**:
- ❌ Removed: Cisco ASA Firewall (-$650)
- ✅ Added: ISP_2 Router (+$3,500)
- ✅ Added: 2× Edge Routers (+$3,800)
- ✅ Added: 2× Edge Switches (+$2,400)
- ✅ Added: 6× End devices for edge segments (+$5,000)
- ✅ Added: BGP/EIGRP licensing and training (+$6,750)

---

### 7.4 Funding Recommendations

#### Phase 1 - Core Infrastructure (Priority: Critical)
**Budget**: $70,000
- All routers (including ISP_2, Edge routers)
- All switches (including Edge switches)
- Fiber optic cabling
- BGP/EIGRP/OSPF configuration services

**Timeline**: Months 1-2

#### Phase 2 - Security & Connectivity (Priority: High)
**Budget**: $27,200
- Wireless infrastructure (including edge APs)
- Security cameras and NVR
- Cat6a cabling installation
- Professional installation

**Timeline**: Months 2-3

#### Phase 3 - End-User Deployment (Priority: Medium)
**Budget**: $171,700
- All end-user devices (PCs, laptops, edge devices)
- IoT devices and smart boards
- Printers and peripherals
- Software licensing

**Timeline**: Months 3-4

#### Phase 4 - Optimization & Training (Priority: Low)
**Budget**: $31,250
- Multi-protocol routing training
- Documentation
- Testing and validation
- Contingency reserve

**Timeline**: Month 4-5

### 7.5 Cost-Saving Alternatives

**For Budget-Constrained Scenarios**:

1. **Phased IoT Deployment**: Deploy 5 smart boards initially instead of 10 → Save $25,000
2. **Refurbished Equipment**: Use Cisco Certified Refurbished routers → Save $3,000-$5,000
3. **Gradual Student PC Refresh**: Deploy 50 PCs in Phase 1, remaining 25 later → Spread costs
4. **Open-Source NVR**: Consider open-source NVR software → Save $2,000
5. **Simplified Wireless**: Deploy standard APs instead of high-density → Save $500-$1,000

**Total Potential Savings**: $30,500 - $33,000

---

## CONCLUSION

The Future Vision Smart Campus network design provides a robust, scalable, and secure infrastructure that meets the demanding requirements of a modern K-12 educational institution. Key achievements of this design include:

### Design Strengths

1. **Security First Approach**: 
   - Dedicated Cisco ASA Firewall for perimeter defense
   - Comprehensive ACL policies for network segmentation
   - Guest network complete isolation
   - IoT device security controls

2. **Scalability & Performance**:
   - Fiber optic backbone for future bandwidth growth
   - OSPF routing for efficient path selection
   - VLSM addressing with room for 100% growth
   - Modular design allows building additions

3. **Reliability & Redundancy**:
   - Fast OSPF convergence for minimal downtime
   - Enterprise-grade equipment with high MTBF
   - Centralized monitoring and management
   - Professional installation and documentation

4. **Educational Technology Support**:
   - High-capacity network for 75+ simultaneous lab users
   - Low-latency connections for interactive smart boards
   - Dedicated VLANs for different user groups
   - Guest Wi-Fi for community events

### Implementation Readiness

This design is ready for immediate implementation in Cisco Packet Tracer with:
- Complete IP addressing scheme
- Detailed router and switch configurations
- Clear visual topology layout
- Comprehensive equipment specifications

### Academic Learning Outcomes

Through this project, students will gain hands-on experience with:
- Enterprise network design principles
- VLSM subnetting and IP planning
- OSPF dynamic routing configuration
- Security policy implementation (ACLs, Firewall)
- Network documentation best practices
- Budget planning and project management

---

## APPENDIX A: Configuration Command Summary

### Router-A (Administration Building) Base Configuration
```
hostname Router-A
!
interface GigabitEthernet 0/0/0
 description WAN Link to ISP Router
 ip address 192.168.1.2 255.255.255.252
 no shutdown
!
interface GigabitEthernet 0/0/1
 description LAN Link to Switch-A
 no shutdown
!
interface GigabitEthernet 0/0/1.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.128
!
interface GigabitEthernet 0/0/1.11
 encapsulation dot1Q 11
 ip address 192.168.10.129 255.255.255.192
!
interface GigabitEthernet 0/0/1.12
 encapsulation dot1Q 12
 ip address 192.168.10.193 255.255.255.224
!
router ospf 1
 router-id 10.10.10.10
 network 192.168.1.0 0.0.0.3 area 0
 network 192.168.10.0 0.0.0.127 area 0
 network 192.168.10.128 0.0.0.63 area 0
 network 192.168.10.192 0.0.0.31 area 0
!
```

### Cisco ASA Firewall Base Configuration
```
hostname ASA-Campus-FW
!
interface GigabitEthernet 1/1
 nameif outside
 security-level 0
 ip address dhcp
!
interface GigabitEthernet 1/2
 nameif inside
 security-level 100
 ip address 192.168.100.1 255.255.255.252
!
! Route all campus networks (192.168.0.0/16) through inside interface
route inside 192.168.0.0 255.255.0.0 192.168.100.2
route outside 0.0.0.0 0.0.0.0 dhcp 1
!
```

---

## APPENDIX B: Testing & Verification Checklist

### Connectivity Tests
- [ ] Ping test from each building router to ISP router
- [ ] Ping test between all VLANs (should succeed where ACLs permit)
- [ ] Internet connectivity test from each building
- [ ] Guest Wi-Fi isolation test (should fail to internal networks)

### Routing Tests
- [ ] Verify OSPF neighbor relationships on all routers
- [ ] Check OSPF routing tables for all subnets
- [ ] Test route convergence by disabling/enabling links
- [ ] Verify load balancing on equal-cost paths

### Security Tests
- [ ] Verify ACL blocking guest to admin networks
- [ ] Test student restriction to admin VLAN
- [ ] Confirm firewall blocks unauthorized inbound traffic
- [ ] Validate SSH access restrictions on routers

### Performance Tests
- [ ] Bandwidth test on fiber optic links
- [ ] Latency measurements between buildings
- [ ] DHCP address assignment verification
- [ ] DNS resolution from all VLANs

---

**Project Report Prepared By**: Network Engineering Team
**Date**: December 2025
**Version**: 1.0
**Course**: Computer Networks - Smart Campus Project
**Institution**: Future Vision Smart Campus

---

*This document is formatted for academic submission and can be directly imported into Microsoft Word or similar word processors. All tables, configurations, and diagrams are production-ready for Cisco Packet Tracer implementation.*
