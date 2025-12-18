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

**Project Goal**: Design and implement a comprehensive, secure, and scalable network infrastructure for a K-12 educational institution.

**Key Objectives:**
- ✅ Connect 4 buildings with high-speed fiber optic backbone
- ✅ Support 135+ end-user devices
- ✅ Implement multi-layered security architecture
- ✅ Provide guest WiFi with complete isolation
- ✅ Enable centralized camera monitoring and IoT management
- ✅ Ensure 99.9% uptime with fast convergence routing

**Implementation Platform:** Cisco Packet Tracer

**Total Budget:** $284,683

> 📸 *[IMAGE PLACEHOLDER: Campus overview map showing 4 buildings]*

---

## Slide 2: Network Architecture

### Hub-and-Spoke Distributed Routing Architecture

```
                    [Internet Cloud]
                          |
                    [ASA Firewall]
                          |
                   [Router-CORE]
              ________|________
             |     |     |     |
         Router-A B    C      D
             |     |     |     |
         Building 1-4 (with switches)
```

**Core Components:**
- **5 Routers**: 1 Central Hub + 4 Building Routers
- **7 Switches**: 4 buildings (hierarchical design in Building B)
- **1 Firewall**: Cisco ASA 5506-X for perimeter security
- **14 VLANs**: Segmented for different user groups

**Connection Type:**
- Inter-building: Fiber optic (1-10 Gbps capable)
- Intra-building: Cat6a UTP (10 Gbps capable)

> 📸 *[IMAGE PLACEHOLDER: Full network topology diagram showing all routers, switches, and connections]*

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

> 📸 *[IMAGE PLACEHOLDER: Building C server room with NVR and security monitoring setup]*

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

**Private IP Space:** 192.168.0.0/16

**VLAN Distribution Across 4 Buildings:**

| Building | VLANs | IP Range | Total IPs | Usage |
|----------|-------|----------|-----------|-------|
| **A (Admin)** | 3 | 192.168.10.0/24 | 256 | 22 devices |
| **B (Academic)** | 4 | 192.168.20.0/24 | 256 | 109 devices |
| **C (Services)** | 3 | 192.168.30.0/24 | 256 | 20 devices |
| **D (Sports)** | 3 | 192.168.40.0/27 + 99.0/24 | 280+ | 8 devices + guests |

**WAN Links:** Point-to-point /30 subnets (2 usable IPs each)

**Design Benefits:**
- ✅ Minimal IP waste with VLSM
- ✅ 100% growth capacity in each subnet
- ✅ Clear hierarchical structure
- ✅ Easy troubleshooting

> 📸 *[IMAGE PLACEHOLDER: Visual VLAN diagram showing color-coded network segments]*

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
Layer 1: Perimeter → Cisco ASA Firewall
Layer 2: Network   → 17 Access Control Lists (ACLs)
Layer 3: Access    → VLAN Segmentation (14 VLANs)
Layer 4: Device    → SSH-only Management + Authentication
```

**Key Security Features:**

1. **Firewall Protection**
   - Stateful packet inspection
   - NAT/PAT for IP hiding
   - Intrusion prevention
   - DDoS protection

2. **Access Control Lists (17 Total)**
   - Guest WiFi complete isolation
   - Student restrictions from admin networks
   - Server protection
   - Management access control

3. **Enhanced Authentication**
   - 35 user accounts with privilege levels
   - SSH v2 with 2048-bit RSA encryption
   - Telnet disabled network-wide
   - Individual accountability

4. **Network Segmentation**
   - 14 VLANs for different user groups
   - IoT device isolation
   - Guest network complete isolation

> 📸 *[IMAGE PLACEHOLDER: Security layers diagram showing defense in depth]*

---

## Slide 7: Routing Protocol - OSPF

### Why OSPF (Open Shortest Path First)?

**Selected Protocol:** OSPFv2, Process ID 1, Area 0

**Justification:**

| Criteria | OSPF Advantage |
|----------|----------------|
| **Scalability** | Handles hundreds of routers efficiently |
| **Convergence** | Sub-second (1-5 seconds) vs RIP's 30+ seconds |
| **Bandwidth** | No hop count limit, bandwidth-based metrics |
| **Standards** | Open standard (RFC 2328), vendor-neutral |
| **Features** | VLSM support, authentication, load balancing |

**Network Design:**
- All routers in Area 0 (Backbone)
- Router IDs: 1.1.1.1 (CORE), 10.10.10.10 (A), 20.20.20.20 (B), etc.
- Passive interfaces on LAN-facing ports
- Active OSPF on WAN links only

**Verification:**
```cisco
show ip ospf neighbor
! Expected: FULL state on all neighbors
! Router-CORE: 4 neighbors (A, B, C, D)
! Building routers: 1 neighbor (CORE)
```

> 📸 *[IMAGE PLACEHOLDER: OSPF topology showing Area 0 and neighbor relationships]*

---

## Slide 8: Access Control Lists

### Security Policy Enforcement

**17 ACLs Deployed (All Packet Tracer Compatible)**

**Critical Security Rules:**

### 1. Guest WiFi Isolation (CRITICAL)
```cisco
! Block ALL internal networks
deny ip 192.168.99.0 0.0.0.255 192.168.10.0 0.0.0.127
deny ip 192.168.99.0 0.0.0.255 192.168.30.64 0.0.0.31
! Allow only HTTP/HTTPS/DNS
permit tcp any any eq 80
permit tcp any any eq 443
```
**Result:** ❌ Guests BLOCKED from internal networks, ✅ Internet allowed

### 2. Student Lab Restrictions
```cisco
! Block admin networks
deny ip 192.168.20.0 0.0.0.127 192.168.10.0 0.0.0.127
! Allow library and servers
permit ip 192.168.20.0 0.0.0.127 192.168.30.0 0.0.0.63
```
**Result:** ❌ Students BLOCKED from admin, ✅ Library/Servers allowed

### 3. Management Access Control
```cisco
access-list 1 permit 192.168.10.0 0.0.0.127
access-list 1 deny any
! Applied to VTY lines
```
**Result:** ✅ Only admin staff can SSH to routers

> 📸 *[IMAGE PLACEHOLDER: ACL security matrix showing allowed/blocked traffic]*

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

### Comprehensive 6-Phase Testing Strategy

**Phase 1: DHCP & Basic Connectivity**
- ✅ All devices receive correct IP addresses
- ✅ Gateway reachability verified
- ✅ DNS configuration confirmed

**Phase 2: Routing & Inter-VLAN**
- ✅ Admin PC: Broad access verified
- ✅ Student PC: Restrictions enforced
- ✅ Teacher PC: Moderate access confirmed
- ✅ Guest WiFi: Complete isolation verified

**Phase 3: Management Access (SSH)**
- ✅ SSH from admin networks: SUCCESS
- ❌ SSH from student/guest networks: BLOCKED
- ✅ Username/password authentication working

**Phase 4: Service Access**
- ✅ DNS resolution functional
- ✅ HTTP/HTTPS traffic flows correctly
- ✅ Traceroute shows proper routing paths

**Phase 5: Camera & IoT Access**
- ✅ Security monitor accesses all cameras
- ❌ Students/guests blocked from cameras
- ✅ Teachers control smart boards

**Phase 6: Cross-Building Routing**
- ✅ OSPF neighbors in FULL state
- ✅ All routes learned and propagated
- ✅ Traceroute shows multi-hop routing through CORE

**Test Results:** 20+ scenarios executed, 100% pass rate

> 📸 *[IMAGE PLACEHOLDER: Testing dashboard showing green checkmarks for all phases]*

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
- **Routers:** 5 (Cisco ISR 4331, 4321, 4221)
- **Switches:** 7 (Cisco Catalyst 3650-24PS, 2960-24TT)
- **Firewall:** 1 (Cisco ASA 5506-X)
- **Wireless:** 3 APs + 1 Controller

**End-User Devices:**
- **PCs/Laptops:** 135
- **Printers:** 2
- **IP Cameras:** 12
- **Smart Boards:** 10
- **IoT Scoreboards:** 3
- **Servers:** 3

**Network Configuration:**
- **VLANs:** 14
- **ACLs:** 17 (Extended & Standard)
- **DHCP Pools:** 14
- **User Accounts:** 35
- **OSPF Area:** Single Area 0

**Performance Metrics:**
- **Fiber Backbone:** 1-10 Gbps capable
- **LAN Speed:** 10 Gbps (Cat6a)
- **OSPF Convergence:** 1-5 seconds
- **Network Uptime Target:** 99.9%

> 📸 *[IMAGE PLACEHOLDER: Technical specifications infographic with icons]*

---

## Slide 14: Budget Overview

### Total Project Investment: $284,683

**Budget Breakdown:**

| Category | Cost (USD) | % of Total |
|----------|-----------|------------|
| **IoT & Smart Devices** | $63,700 | 22.6% |
| **End-User Devices** | $98,850 | 35.1% |
| **Professional Services** | $21,400 | 7.6% |
| **Cabling & Infrastructure** | $16,350 | 5.8% |
| **Routers & Firewall** | $13,550 | 4.8% |
| **Security & Surveillance** | $11,500 | 4.1% |
| **Switches** | $9,000 | 3.2% |
| **Wireless Infrastructure** | $7,850 | 2.8% |
| **Software & Licensing** | $6,550 | 2.3% |
| **Contingency & Maintenance** | $35,933 | 12.7% |

**Implementation Phases:**
1. **Phase 1:** Core Infrastructure ($60,300) - Months 1-2
2. **Phase 2:** Security & Connectivity ($27,200) - Months 2-3
3. **Phase 3:** End-User Deployment ($161,350) - Months 3-4
4. **Phase 4:** Optimization & Training ($35,933) - Month 4-5

> 📸 *[IMAGE PLACEHOLDER: Pie chart showing budget distribution]*

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
4. Ping internet: `ping 8.8.8.8`
5. **Result:** ✅ Reply received (Internet access allowed)

**Demo 3: SSH Access Control**
1. From Admin PC, SSH to Router-CORE: `ssh admin@192.168.1.1`
2. **Result:** ✅ Login successful (Admin network allowed)
3. From Student PC, SSH to Router-CORE: `ssh admin@192.168.1.1`
4. **Result:** ❌ Connection timeout (VTY ACL blocks student network)

**Demo 4: OSPF Routing**
1. On Router-CORE, run: `show ip ospf neighbor`
2. **Result:** 4 neighbors in FULL state (A, B, C, D)
3. Traceroute from Building A to Building C: `tracert 192.168.30.1`
4. **Result:** Path shown through Router-CORE (multi-hop routing)

**Demo 5: ACL Security**
1. From Student PC, ping Admin gateway: `ping 192.168.10.1`
2. **Result:** ❌ Request timeout (Student ACL blocks admin)
3. From Student PC, ping Library: `ping 192.168.30.1`
4. **Result:** ✅ Reply received (Students allowed to library)

> 📸 *[IMAGE PLACEHOLDER: Screenshot of Packet Tracer showing ping tests]*

---

## Slide 16: Q&A

### Common Questions & Expert Answers

**Q1: Why OSPF instead of RIP or EIGRP?**
**A:** OSPF provides faster convergence (1-5 seconds vs RIP's 30+ seconds), no hop count limitation, and is an open standard supporting vendor interoperability. EIGRP is Cisco proprietary. For our 4-building campus with growth potential, OSPF is the optimal choice.

**Q2: Why hierarchical switches only in Building B?**
**A:** Building B has 109+ devices (75 students + 20 teachers + 10 smart boards + cameras), exceeding a single 24-port switch capacity. Other buildings have fewer devices that fit within single switch port counts. This demonstrates scalable design when needed.

**Q3: Can we add more buildings in the future?**
**A:** Yes! The hub-and-spoke architecture with Router-CORE as the central hub makes expansion straightforward. Simply add a new building router, connect via fiber to Router-CORE, configure OSPF, and the new building integrates seamlessly.

**Q4: How do you handle password management with 35 accounts?**
**A:** We implement strong password policies (complexity, expiration), use privilege levels for role-based access, and employ SSH encryption. In production, integrate with RADIUS/TACACS+ for centralized authentication and audit trails.

**Q5: What happens if Router-CORE fails?**
**A:** In this design, Router-CORE is a single point of failure. For high availability, we'd implement redundant core routers with HSRP/VRRP for failover. Current design prioritizes educational value and budget constraints.

**Q6: Why Packet Tracer instead of physical equipment?**
**A:** Packet Tracer provides risk-free testing, easy configuration rollback, and cost savings ($284K budget is for physical deployment). It's ideal for learning, prototyping, and demonstrating concepts before physical implementation.

**Q7: How long does full implementation take?**
**A:** Physical deployment: 4-5 months across 4 phases. Packet Tracer simulation: 4-6 hours for complete configuration and testing. Documentation and design: Already complete!

**Q8: What's the most critical security feature?**
**A:** Guest WiFi complete isolation (GUEST-WIFI-ISOLATION ACL). Preventing external users from accessing internal resources is paramount. Secondary: SSH-only management with username/password authentication.

**Q9: Can this design scale to a university campus?**
**A:** Yes, with modifications. Add multiple Area 0 routers for redundancy, implement multi-area OSPF for larger scale, add distribution layer switches, and enhance with QoS for voice/video. Core principles remain the same.

**Q10: What monitoring tools are used?**
**A:** NVR server monitors all 12 cameras 24/7, Security Monitor Station provides live feeds, OSPF logs track routing changes, ACL counters show denied traffic, and DHCP bindings track device connections.

---

## Thank You!

### Future Vision Smart Campus Network

**Project Highlights:**
- ✅ 4 Buildings Connected
- ✅ 7 Switches (Hierarchical Design)
- ✅ 17 Security ACLs
- ✅ 35 User Accounts
- ✅ 100% Test Pass Rate
- ✅ $284,683 Budget

**Key Achievements:**
- Multi-layered security architecture
- Scalable hierarchical design
- Fast-converging OSPF routing
- Complete guest isolation
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
