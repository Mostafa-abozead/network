# Future Vision Smart Campus Network Design

## 📋 Project Overview

This repository contains a comprehensive network design and implementation plan for a K-12 educational institution - **Future Vision Smart Campus**. The project demonstrates enterprise-level network architecture with multi-protocol routing, enhanced security policies, and edge connectivity.

## 🏗️ Architecture Highlights

- **Topology**: Hub-and-Spoke Distributed Routing with External Edge Connectivity
- **Routing Protocols**: Multi-protocol architecture (OSPF + BGP + EIGRP)
- **Security**: NEW ACL policies focused on Library access control and inter-building restrictions
- **IP Addressing**: VLSM-based efficient allocation (192.168.0.0/16, 10.0.0.0/8, 11-14.0.0.0/8)
- **Buildings**: 4 interconnected campus buildings + 2 edge networks
- **Devices**: 141+ end-user devices, 12 security cameras, 10 smart boards

## 🆕 Latest Updates - Network Topology Changes

### Infrastructure Changes
- ❌ **REMOVED**: Internet Cloud and Cisco ASA Firewall components
- ✅ **ADDED**: ISP_2 external router with BGP connectivity
- ✅ **ADDED**: EIGRP segment with Router-E1 and Router-E2
- ✅ **ADDED**: Two edge switches (Switch-E1, Switch-E2) with dedicated LANs
- ✅ **ADDED**: 6 new edge devices (2 PCs, 2 Laptops, 2 Access Points)

### Routing Protocol Updates
- **OSPF**: Internal campus routing (Area 0) for buildings A, B, C, D
- **BGP**: AS 65000 (ISP_2) ↔ AS 65001 (Router-C) via 10.0.0.0/8
- **EIGRP**: AS 100 for edge segment (11.0.0.0/8, 12.0.0.0/8, 13.0.0.0/8, 14.0.0.0/8)

### Security Policy Overhaul
**All previous ACLs removed and replaced with:**
1. **Library Access Control**: Only Library PCs (192.168.30.0/26) permitted
2. **Admin Building Protection**: All buildings denied access to Admin Building
3. **Server Protection**: Student Building denied access to Services Building servers
4. **Admin Privilege**: Admin Building has full access (no restrictions)

## 📁 Repository Structure

```
network/
├── README.md                                  # This file - Project overview
├── QUICK_START_GUIDE.md                       # Quick start guide for the project
├── Smart_Campus_Network_Design_Report.md      # Complete technical documentation
├── Packet_Tracer_Implementation_Guide.md      # Step-by-step implementation guide
├── NETWORK_DIAGRAM_INSTRUCTIONS.md            # Instructions to create .pkt file
├── TESTING_GUIDE.md                           # Comprehensive testing procedures (NEW)
├── DOCUMENTATION_UPDATE_SUMMARY.md            # All configuration changes and enhancements (NEW)
└── PROJECT_REQUIREMENTS_VERIFICATION.md       # Requirements verification checklist
```

## 📖 Documentation

### [Quick Start Guide](./QUICK_START_GUIDE.md) ← **Start Here!**
New to the project? Read this first for a guided walkthrough of all documentation and how to use it.

### [Smart Campus Network Design Report](./Smart_Campus_Network_Design_Report.md)
Complete technical documentation including:
1. **Case Study Overview** - Project requirements and objectives
2. **Buildings & Users Breakdown** - Detailed inventory of 4 campus buildings + 2 edge networks
3. **Visual Diagram Description** - Updated Cisco Packet Tracer topology design
4. **IP Addressing Plan (VLSM)** - Complete subnet allocation tables (includes new 10.0.0.0, 11-14.0.0.0 networks)
5. **Multi-Protocol Routing** - OSPF + BGP + EIGRP configurations and justifications
6. **NEW Access Control Lists (ACL)** - Completely redesigned security policies
7. **Bill of Materials** - Updated equipment list with costs ($300,150 total, +$15,467)

### [Packet Tracer Implementation Guide](./Packet_Tracer_Implementation_Guide.md)
Step-by-step instructions to build the network in Cisco Packet Tracer:
- Core router and building routers configuration
- **NEW**: ISP_2, Router-E1, Router-E2 setup
- **NEW**: BGP configuration between ISP_2 and Router-C
- **NEW**: EIGRP configuration for edge segment
- Switch configurations (campus + edge switches)
- **UPDATED**: ACL security policies (all new)
- DHCP and end device configurations

### [Network Diagram Instructions](./NETWORK_DIAGRAM_INSTRUCTIONS.md)
Detailed guide to create the visual topology in Packet Tracer.

### [Testing Guide](./TESTING_GUIDE.md) ✨ NEEDS UPDATE
Comprehensive testing procedures with 20+ test scenarios covering:
- DHCP & Basic Connectivity
- **NEW**: Multi-protocol routing tests (OSPF, BGP, EIGRP)
- **NEW**: Edge network connectivity validation
- **UPDATED**: ACL Security policy enforcement (new policies)
- Cross-Building Routing validation
- **TO BE UPDATED**: Test scenarios for new topology

### [Documentation Update Summary](./DOCUMENTATION_UPDATE_SUMMARY.md) ✨ TO BE UPDATED
Summary of configuration changes and enhancements:
- **NEW**: Multi-protocol routing architecture
- **NEW**: External connectivity via ISP_2
- **NEW**: Edge network segments
- **NEW**: Completely redesigned ACL policies
- Hierarchical switch architecture for Building B
- SSH configuration fixes for Packet Tracer
- Router-on-a-stick IP overlap resolution

### [Project Requirements Verification](./PROJECT_REQUIREMENTS_VERIFICATION.md)
Comprehensive verification that all project requirements have been met with links to each requirement.

## 🎯 Key Features

### Network Topology
- **1 Central ISP Edge Router** (Cisco ISR 4331) - Campus core
- **4 Building Routers** (Cisco ISR series) - Buildings A, B, C, D
- **1 External ISP_2 Router** (Cisco ISR 4331) - BGP + EIGRP connectivity
- **2 Edge Routers** (Cisco ISR 4221) - Router-E1, Router-E2
- ❌ **REMOVED**: Cisco ASA Firewall (no longer in topology)
- **9 Cisco Catalyst Switches** (2960/3650 series)
  - **Building B**: Hierarchical 3-tier architecture (1 core + 3 access switches)
  - **Edge Networks**: 2 additional switches (Switch-E1, Switch-E2)
- **Fiber Optic Backbone** for inter-building connectivity

### Multi-Protocol Routing
- **OSPF Area 0**: Internal campus routing for buildings (fast convergence, link-state)
- **BGP (AS 65000 ↔ AS 65001)**: External routing between ISP_2 and campus
- **EIGRP AS 100**: Edge network segment routing (hybrid protocol)
- **Route Redistribution**: OSPF ↔ BGP on Router-C for seamless connectivity

### NEW Security Features
- **Library Access Control**: Only Library PCs can access network from VLAN 30
- **Admin Building Protection**: Complete isolation - NO building can access Admin
- **Server Protection**: Student Building denied direct access to Services servers
- **Admin Privilege**: Admin Building has unrestricted access to all resources
- VLAN-based network segmentation (14 VLANs)
- **Enhanced Authentication**: Username/password with privilege levels (15, 7, 1)
- **SSH-only Management**: Telnet disabled, SSH v2 with 2048-bit RSA keys

### IP Addressing Scheme
| Network Segment | Purpose | IP Range | Routing Protocol |
|----------------|---------|----------|------------------|
| Building A | Administration | 192.168.10.0/24 | OSPF |
| Building B | Academic | 192.168.20.0/24 | OSPF |
| Building C | Services & Library | 192.168.30.0/24 | OSPF + BGP |
| Building D | Sports & Events | 192.168.40.0/24, 192.168.99.0/24 | OSPF |
| ISP_2 ↔ Router-C | BGP Link | 10.0.0.0/8 | BGP |
| ISP_2 ↔ Router-E1 | EIGRP Link | 11.0.0.0/8 | EIGRP |
| ISP_2 ↔ Router-E2 | EIGRP Link | 12.0.0.0/8 | EIGRP |
| Edge Network 1 | Router-E1 LAN | 13.0.0.0/8 | EIGRP |
| Edge Network 2 | Router-E2 LAN | 14.0.0.0/8 | EIGRP |

## 🛠️ Equipment Summary

### Core Infrastructure
- **Routers**: 8 Cisco ISR routers (4331, 4321, 4221)
  - 1× ISR 4331 (Campus core)
  - 1× ISR 4331 (ISP_2 external)
  - 2× ISR 4321 (Buildings A, C)
  - 2× ISR 4221 (Buildings D)
  - 2× ISR 4221 (Edge routers E1, E2)
- **Switches**: 9 Cisco Catalyst switches (2960/3650)
  - 1× Catalyst 3650 (Building B core)
  - 6× Catalyst 2960 (Building access switches)
  - 2× Catalyst 2960 (Edge switches E1, E2)
- ❌ **Firewall**: Removed (was 1× Cisco ASA 5506-X)
- **Wireless**: 5 Cisco Aironet APs + Wireless Controller

### End-User Devices
- **141 Total End-User Devices**:
  - 75 Student Lab PCs
  - 20 Teacher Laptops
  - 22 Staff Desktop PCs
  - 15 Library PCs
  - 3 Sports/Events PCs
  - 6 Edge Network Devices (2 PCs, 2 Laptops, 2 APs)

### IoT & Smart Devices
- 12 IP Security Cameras (Cisco Meraki MV22X)
- 10 Interactive Smart Boards (Cisco Webex Board 70)
- 3 Digital Scoreboards
- 1 NVR Server + 1 IoT Management Server

## 💰 Budget Overview

| Category | Total Cost (USD) | Change from Previous | % of Total |
|----------|------------------|----------------------|------------|
| Routers (No Firewall) | $19,550 | +$6,000 | 6.5% |
| Switches | $11,400 | +$2,400 | 3.8% |
| Wireless Infrastructure | $7,850 | - | 2.6% |
| Security & Surveillance | $11,500 | - | 3.8% |
| IoT & Smart Devices | $63,700 | - | 21.2% |
| End-User Devices | $103,850 | +$5,000 | 34.6% |
| Cabling & Infrastructure | $16,350 | - | 5.4% |
| Software & Licensing | $7,950 | +$1,400 | 2.6% |
| Professional Services | $26,750 | +$5,350 | 8.9% |
| Contingency & Maintenance | $31,250 | Recalculated | 10.4% |
| **GRAND TOTAL** | **$300,150** | **+$15,467** | **100%** |

**Key Budget Changes**:
- ❌ Removed Cisco ASA Firewall: -$650
- ✅ Added ISP_2 Router: +$3,500
- ✅ Added 2× Edge Routers: +$3,800
- ✅ Added 2× Edge Switches: +$2,400
- ✅ Added Edge Devices: +$5,000
- ✅ Added BGP/EIGRP Licensing & Training: +$6,750
- **Net Increase**: +$15,467 (5.4% increase)

## 🚀 Implementation Phases

### Phase 1 - Core Infrastructure ($60,300)
- All routers, firewall, and switches
- Fiber optic cabling
- Basic configuration services
- **Timeline**: Months 1-2

### Phase 2 - Security & Connectivity ($27,200)
- Wireless infrastructure
- Security cameras and NVR
- Cat6a cabling
- **Timeline**: Months 2-3

### Phase 3 - End-User Deployment ($161,350)
- All end-user devices
- IoT devices and smart boards
- Software licensing
- **Timeline**: Months 3-4

### Phase 4 - Optimization & Training ($35,933)
- Staff training
- Testing and validation
- Final documentation
- **Timeline**: Month 4-5

## 🔐 Security Policies

### ACL Rules Implemented
1. **Guest Wi-Fi Isolation**: Blocks access to internal networks
2. **Student Lab Restrictions**: Denies access to admin resources
3. **Firewall Traffic Filtering**: HTTP/HTTPS with inspection
4. **IoT Device Isolation**: Limits IoT-to-PC communication
5. **Administrative Access Control**: VTY line protection

### OSPF Routing
- **Protocol**: OSPF Version 2 (OSPFv2)
- **Area Design**: Single Area 0 (Backbone Area)
- **Convergence**: Sub-second (1-5 seconds)
- **Benefits**: Scalability, fast convergence, open standard

## 📚 Academic Learning Outcomes

This project provides hands-on experience with:
- Enterprise network design principles
- **Multi-protocol routing** (OSPF, BGP, EIGRP)
- **Route redistribution** and protocol integration
- **AS-based routing** and BGP peering
- VLSM subnetting and IP planning
- Security policy implementation (NEW ACL approach)
- Network documentation best practices
- Budget planning and project management
- **Edge network connectivity** and expansion planning

## 🔧 Technologies Used

- **Cisco Packet Tracer** - Network simulation and visualization
- **Cisco IOS** - Router and switch configuration
- **OSPF** - Internal campus dynamic routing (link-state)
- **BGP** - External inter-AS routing (path vector)
- **EIGRP** - Edge segment routing (hybrid)
- **VLANs** - Network segmentation
- **802.1Q** - VLAN tagging
- **VLSM** - Variable Length Subnet Masking
- **Route Redistribution** - OSPF ↔ BGP integration

## 📝 Getting Started

To implement this network design:

1. **Review Documentation**: Read the [Smart Campus Network Design Report](./Smart_Campus_Network_Design_Report.md)
2. **Open Cisco Packet Tracer**: Install Cisco Packet Tracer (v8.0+)
3. **Follow Implementation Guide**: Use the [Packet Tracer Implementation Guide](./Packet_Tracer_Implementation_Guide.md)
4. **Create Topology**: Follow [Network Diagram Instructions](./NETWORK_DIAGRAM_INSTRUCTIONS.md)
5. **Configure Devices**: Use configuration examples from the report
6. **Test & Verify**: Use the testing checklist in Appendix B

## 📞 Project Information

- **Project Name**: Future Vision Smart Campus
- **Course**: Computer Networks
- **Institution**: Educational Institution Network Design
- **Version**: 1.0
- **Date**: December 2025

## 📄 License

This project is for educational purposes.

---

**Note**: This is a comprehensive network design project suitable for computer networking courses, certifications (CCNA, CCNP), and real-world implementation planning.