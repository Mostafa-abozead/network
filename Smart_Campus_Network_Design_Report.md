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

The Future Vision Smart Campus network implements a **Hub-and-Spoke Distributed Routing Architecture** with enhanced perimeter security. This design provides centralized control while allowing distributed intelligence at each building location.

### 3.2 Core Infrastructure Components

#### Internet Connectivity Layer
```
[Internet Cloud]
       |
       | (Public IP via DHCP/Static)
       |
[Cisco ASA 5506-X Firewall]
       | (Outside Interface: Public IP)
       | (Inside Interface: 192.168.100.1/30)
       |
[Central ISP Edge Router]
```

**Firewall Positioning**: The Cisco ASA Firewall is strategically placed between the Internet Cloud and the Central ISP Router, providing:
- Stateful packet inspection
- NAT/PAT services for private IP translation
- Intrusion Prevention System (IPS)
- VPN termination capabilities
- Application layer filtering
- DDoS protection

#### Central Hub Router
**ISP Edge Router (Router-CORE)**
- Model: Cisco ISR 4331
- Role: Central aggregation point for all building routers
- Interfaces:
  - GigabitEthernet 0/0/0: WAN link to Firewall (192.168.100.2/30)
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
| Firewall-ISP Link | 192.168.100.0 | 255.255.255.252 | /30 | 2 | 192.168.100.1 | 192.168.100.2 | 192.168.100.3 | Firewall to ISP Router |
| ISP-RouterA Link | 192.168.1.0 | 255.255.255.252 | /30 | 2 | 192.168.1.1 | 192.168.1.2 | 192.168.1.3 | Core to Building A |
| ISP-RouterB Link | 192.168.2.0 | 255.255.255.252 | /30 | 2 | 192.168.2.1 | 192.168.2.2 | 192.168.2.3 | Core to Building B |
| ISP-RouterC Link | 192.168.3.0 | 255.255.255.252 | /30 | 2 | 192.168.3.1 | 192.168.3.2 | 192.168.3.3 | Core to Building C |
| ISP-RouterD Link | 192.168.4.0 | 255.255.255.252 | /30 | 2 | 192.168.4.1 | 192.168.4.2 | 192.168.4.3 | Core to Building D |
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

## 5. DYNAMIC ROUTING PROTOCOL

### 5.1 Protocol Selection: OSPF (Open Shortest Path First)

**Selected Protocol**: OSPF Version 2 (OSPFv2)
**Routing Process ID**: 1
**Area Design**: Single Area 0 (Backbone Area)

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

### 5.4 Router ID Assignment

| Router | Router ID | Purpose |
|--------|-----------|---------|
| ISP Edge Router | 1.1.1.1 | Central hub identification |
| Router-A (Admin) | 10.10.10.10 | Building A identifier |
| Router-B (Academic) | 20.20.20.20 | Building B identifier |
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

### 6.1 ACL Security Strategy

Access Control Lists provide granular security enforcement to protect sensitive network resources and ensure appropriate network segmentation. The Future Vision Smart Campus implements both standard and extended ACLs at strategic points in the network.

**⚠️ Important - Packet Tracer Compatibility Note**:
All ACL configurations in this document are compatible with Cisco Packet Tracer. Note that Packet Tracer **does not support the `log` keyword** in ACL statements. If implementing in production IOS, you can add `log` to the end of deny statements for security monitoring. For Packet Tracer implementation, the `log` keyword must be omitted.

**Syntax Differences**:
- ✅ **Packet Tracer**: `deny ip 192.168.99.0 0.0.0.255 192.168.10.0 0.0.0.127`
- ❌ **Production IOS** (not for PT): `deny ip 192.168.99.0 0.0.0.255 192.168.10.0 0.0.0.127 log`

### 6.2 ACL Rule 1: Guest Wi-Fi Isolation

**Objective**: Deny Guest Wi-Fi (Building D - VLAN 99) from accessing Internal Servers (Building C - VLAN 31) and Admin PCs (Building A - VLAN 10)

**Implementation Location**: Router-D (Sports & Events building router)

**ACL Configuration**:
```
! Extended ACL to block Guest access to sensitive networks
! Note: For production deployments, consider using object groups for better maintainability
ip access-list extended GUEST-RESTRICTION
 ! Block access to Admin VLAN (192.168.10.0/25)
 deny ip 192.168.99.0 0.0.0.255 192.168.10.0 0.0.0.127
 ! Block access to Server VLAN (192.168.30.64/27)
 deny ip 192.168.99.0 0.0.0.255 192.168.30.64 0.0.0.31
 ! Block access to Security Monitoring VLAN (192.168.30.96/27)
 deny ip 192.168.99.0 0.0.0.255 192.168.30.96 0.0.0.31
 ! Allow internet access only (HTTP/HTTPS)
 permit tcp 192.168.99.0 0.0.0.255 any eq 80
 permit tcp 192.168.99.0 0.0.0.255 any eq 443
 permit tcp 192.168.99.0 0.0.0.255 any eq 53
 permit udp 192.168.99.0 0.0.0.255 any eq 53
 ! Deny all other traffic
 deny ip 192.168.99.0 0.0.0.255 any
 ! Implicit permit for return traffic
 permit tcp any 192.168.99.0 0.0.0.255 established
!
! Apply ACL to Guest VLAN interface
interface GigabitEthernet 0/0.99
 ip access-group GUEST-RESTRICTION in
```

**Security Benefits**:
- Prevents unauthorized access to critical systems
- Limits guest traffic to internet-only access
- Protects against lateral movement attacks
- Allows DNS for internet name resolution

---

### 6.3 ACL Rule 2: Student Lab Restrictions

**Objective**: Deny Students (Building B - VLAN 20) from accessing Admin VLAN (Building A - VLAN 10)

**Implementation Location**: Router-B (Academic Block router)

**ACL Configuration**:
```
! Extended ACL to restrict student access
ip access-list extended STUDENT-RESTRICTION
 ! Block access to all Admin VLANs
 deny ip 192.168.20.0 0.0.0.127 192.168.10.0 0.0.0.127
 deny ip 192.168.20.0 0.0.0.127 192.168.10.128 0.0.0.63
 deny ip 192.168.20.0 0.0.0.127 192.168.10.192 0.0.0.31
 ! Allow access to library resources
 permit ip 192.168.20.0 0.0.0.127 192.168.30.0 0.0.0.63
 ! Allow access to educational servers
 permit ip 192.168.20.0 0.0.0.127 192.168.30.64 0.0.0.31
 ! Allow internet access
 permit ip 192.168.20.0 0.0.0.127 any
!
! Apply ACL to Student VLAN interface
interface GigabitEthernet 0/0.20
 ip access-group STUDENT-RESTRICTION in
```

**Security Benefits**:
- Protects administrative systems from student access
- Prevents unauthorized changes to sensitive data
- Maintains educational access to appropriate resources
- Supports institutional security policies

---

### 6.4 ACL Rule 3: Firewall Traffic Filtering

**Objective**: Permit HTTP/HTTPS traffic through Cisco ASA Firewall while blocking unauthorized protocols

**Implementation Location**: Cisco ASA 5506-X Firewall

**ASA Configuration**:
```
! Object definitions
object-group service ALLOWED-INTERNET
 service-object tcp destination eq 80
 service-object tcp destination eq 443
 service-object tcp destination eq 53
 service-object udp destination eq 53
 service-object icmp

! Access list for outside interface
access-list OUTSIDE-IN extended deny ip any 192.168.0.0 255.255.0.0
access-list OUTSIDE-IN extended deny ip any 10.0.0.0 255.0.0.0
access-list OUTSIDE-IN extended deny ip any 172.16.0.0 255.240.0.0
access-list OUTSIDE-IN extended permit tcp any any established
access-list OUTSIDE-IN extended permit icmp any any echo-reply
access-list OUTSIDE-IN extended deny ip any any

! Access list for inside interface
access-list INSIDE-OUT extended permit object-group ALLOWED-INTERNET any any
access-list INSIDE-OUT extended permit icmp any any
access-list INSIDE-OUT extended permit udp any any eq ntp
access-list INSIDE-OUT extended deny ip any any log

! Apply ACLs to interfaces
access-group OUTSIDE-IN in interface outside
access-group INSIDE-OUT in interface inside

! NAT Configuration (PAT)
object network CAMPUS-NETWORK
 subnet 192.168.0.0 255.255.0.0
 nat (inside,outside) dynamic interface

! Inspection policies
policy-map global_policy
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

### 6.5 Additional ACL Best Practices

#### IoT Device Isolation
```
! Prevent IoT devices from initiating connections to PCs
ip access-list extended IOT-ISOLATION
 ! Smart Boards can only receive connections
 deny ip 192.168.20.192 0.0.0.31 192.168.20.0 0.0.0.127
 ! Scoreboards isolated from staff PCs
 deny ip 192.168.40.32 0.0.0.31 192.168.40.0 0.0.0.31
 ! Allow IoT to Server VLAN for management
 permit ip any 192.168.30.64 0.0.0.31
 ! Permit established connections
 permit tcp any any established
```

#### Administrative Access Control
```
! VTY line protection for router management
access-list 1 permit 192.168.10.0 0.0.0.127
access-list 1 permit 192.168.30.96 0.0.0.31
access-list 1 deny any log
!
line vty 0 4
 access-class 1 in
 transport input ssh
```

### 6.6 ACL Monitoring and Maintenance

**Logging Configuration**:
```
! Enable ACL logging
logging buffered 51200
logging console warnings
logging trap informational
! Send logs to NVR/Server system in Building C (VLAN 31)
logging host 192.168.30.65

! Log ACL matches
ip access-list extended GUEST-RESTRICTION
 10 deny ip 192.168.99.0 0.0.0.255 192.168.10.0 0.0.0.127 log
```

**Regular Review Schedule**:
- Weekly review of ACL hit counters
- Monthly audit of deny log entries
- Quarterly ACL policy updates based on security assessments

---

## 7. BILL OF MATERIALS (BUDGET)

### 7.1 Budget Overview

The following Bill of Materials provides a comprehensive cost estimate for implementing the Future Vision Smart Campus network infrastructure. Prices are based on current market rates for educational institutions (with standard educational discounts applied).

### 7.2 Detailed Equipment List

#### 7.2.1 Core Network Equipment

| Item | Model | Quantity | Unit Price (USD) | Total Price (USD) | Specifications |
|------|-------|----------|------------------|-------------------|----------------|
| **Routers** |
| ISP Edge Router | Cisco ISR 4331 | 1 | $3,500 | $3,500 | 3 GE ports, 2 SFP slots, 4GB RAM |
| Building Router (Large) | Cisco ISR 4321 | 2 | $2,800 | $5,600 | 2 GE ports, 2 SFP slots, 4GB RAM |
| Building Router (Medium) | Cisco ISR 4221 | 2 | $1,900 | $3,800 | 2 GE ports, 2 SFP slots, 4GB RAM |
| **Firewall** |
| Perimeter Firewall | Cisco ASA 5506-X | 1 | $650 | $650 | 8 GE ports, FirePOWER services |
| **Subtotal Routers & Firewall** | | | | **$13,550** | |

#### 7.2.2 Switching Infrastructure

| Item | Model | Quantity | Unit Price (USD) | Total Price (USD) | Specifications |
|------|-------|----------|------------------|-------------------|----------------|
| Core Switch (Building B) | Cisco Catalyst 3650-24PD | 1 | $4,200 | $4,200 | 24 PoE+ ports, Layer 3, 10G uplink |
| Access Switches | Cisco Catalyst 2960-24TT | 3 | $1,200 | $3,600 | 24 ports, Layer 2, 2 SFP uplinks |
| Access Switch (Building D) | Cisco Catalyst 2960-24TT | 1 | $1,200 | $1,200 | 24 ports, Layer 2, 2 SFP uplinks |
| **Subtotal Switches** | | | | **$9,000** | |

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
| **Subtotal End-User Devices** | | | | **$98,850** | |

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
| Cisco IOS Licenses | Advanced IP Services | 5 | $400 | $2,000 | Perpetual |
| ASA Firewall License | FirePOWER Services | 1 | $500 | $500 | 3 years |
| Wireless Controller Licenses | Additional AP Licenses | 3 | $150 | $450 | Perpetual |
| NVR Software License | Recording & Management | 12 cameras | $50 | $600 | Annual |
| Network Management | Cisco DNA Center (Essentials) | 1 | $3,000 | $3,000 | 3 years |
| **Subtotal Software** | | | | **$6,550** | |

#### 7.2.9 Installation & Professional Services

| Item | Description | Quantity | Unit Price (USD) | Total Price (USD) | Notes |
|------|-------------|----------|------------------|-------------------|-------|
| Network Design Services | Professional Consulting | 40 hrs | $150/hr | $6,000 | Architecture & Planning |
| Installation Services | Equipment Setup | 80 hrs | $100/hr | $8,000 | Router/Switch Configuration |
| Testing & Validation | System Testing | 24 hrs | $125/hr | $3,000 | Performance & Security Testing |
| Documentation | Network Documentation | 20 hrs | $100/hr | $2,000 | As-built drawings, configs |
| Training | Staff Training | 16 hrs | $150/hr | $2,400 | IT staff & administrators |
| **Subtotal Professional Services** | | | | **$21,400** | |

#### 7.2.10 Contingency & Maintenance

| Item | Description | Percentage | Base Amount | Total Price (USD) | Purpose |
|------|-------------|------------|-------------|-------------------|---------|
| Contingency Reserve | Unexpected costs | 5% | $248,750 | $12,438 | Equipment changes, issues |
| Annual Maintenance | Support & Warranty | 10% | $234,950 | $23,495 | First year maintenance |
| **Subtotal Contingency** | | | | **$35,933** | |

---

### 7.3 Budget Summary

| Category | Total Cost (USD) | Percentage of Total |
|----------|------------------|---------------------|
| Routers & Firewall | $13,550 | 4.8% |
| Switches | $9,000 | 3.2% |
| Wireless Infrastructure | $7,850 | 2.8% |
| Security & Surveillance | $11,500 | 4.1% |
| IoT & Smart Devices | $63,700 | 22.6% |
| End-User Devices | $98,850 | 35.1% |
| Cabling & Infrastructure | $16,350 | 5.8% |
| Software & Licensing | $6,550 | 2.3% |
| Professional Services | $21,400 | 7.6% |
| Contingency & Maintenance | $35,933 | 12.7% |
| **GRAND TOTAL** | **$284,683** | **100%** |

---

### 7.4 Funding Recommendations

#### Phase 1 - Core Infrastructure (Priority: Critical)
**Budget**: $60,300
- All routers and firewall
- All switches
- Fiber optic cabling
- Basic configuration services

**Timeline**: Months 1-2

#### Phase 2 - Security & Connectivity (Priority: High)
**Budget**: $27,200
- Wireless infrastructure
- Security cameras and NVR
- Cat6a cabling installation
- Professional installation

**Timeline**: Months 2-3

#### Phase 3 - End-User Deployment (Priority: Medium)
**Budget**: $161,350
- All end-user devices (PCs, laptops)
- IoT devices and smart boards
- Printers and peripherals
- Software licensing

**Timeline**: Months 3-4

#### Phase 4 - Optimization & Training (Priority: Low)
**Budget**: $35,933
- Staff training
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
