# Future Vision Smart Campus Network Design

## 📋 Project Overview

This repository contains a comprehensive network design and implementation plan for a K-12 educational institution - **Future Vision Smart Campus**. The project demonstrates enterprise-level network architecture with security, scalability, and performance considerations.

## 🏗️ Architecture Highlights

- **Topology**: Hub-and-Spoke Distributed Routing Architecture
- **Security**: Cisco ASA Firewall with comprehensive ACL policies
- **Routing Protocol**: OSPF (Open Shortest Path First)
- **IP Addressing**: VLSM-based efficient allocation (192.168.0.0/16)
- **Buildings**: 4 interconnected buildings via fiber optic backbone
- **Devices**: 135+ end-user devices, 12 security cameras, 10 smart boards

## 📁 Repository Structure

```
network/
├── README.md                                  # This file - Project overview
├── Smart_Campus_Network_Design_Report.md      # Complete technical documentation
├── Packet_Tracer_Implementation_Guide.md      # Step-by-step implementation guide
└── NETWORK_DIAGRAM_INSTRUCTIONS.md            # Instructions to create .pkt file
```

## 📖 Documentation

### [Smart Campus Network Design Report](./Smart_Campus_Network_Design_Report.md)
Complete technical documentation including:
1. **Case Study Overview** - Project requirements and objectives
2. **Buildings & Users Breakdown** - Detailed inventory of 4 buildings
3. **Visual Diagram Description** - Cisco Packet Tracer topology design
4. **IP Addressing Plan (VLSM)** - Complete subnet allocation tables
5. **Dynamic Routing Protocol** - OSPF configuration and justification
6. **Access Control Lists (ACL)** - Security policies and traffic control
7. **Bill of Materials** - Equipment list with costs ($284,683 total)

### [Packet Tracer Implementation Guide](./Packet_Tracer_Implementation_Guide.md)
Step-by-step instructions to build the network in Cisco Packet Tracer.

### [Network Diagram Instructions](./NETWORK_DIAGRAM_INSTRUCTIONS.md)
Detailed guide to create the visual topology in Packet Tracer.

## 🎯 Key Features

### Network Topology
- **1 Central ISP Edge Router** (Cisco ISR 4331)
- **4 Building Routers** (Cisco ISR series)
- **1 Cisco ASA 5506-X Firewall** for perimeter security
- **4 Cisco Catalyst Switches** (2960/3650 series)
- **Fiber Optic Backbone** for inter-building connectivity

### Security Features
- Perimeter firewall with stateful inspection
- VLAN-based network segmentation (14 VLANs)
- Granular ACL policies for traffic control
- Complete guest Wi-Fi isolation
- IoT device security controls

### IP Addressing Scheme
| Building | Purpose | VLANs | IP Range |
|----------|---------|-------|----------|
| Building A | Administration | 3 VLANs | 192.168.10.0/24 |
| Building B | Academic | 4 VLANs | 192.168.20.0/24 |
| Building C | Services & Library | 3 VLANs | 192.168.30.0/24 |
| Building D | Sports & Events | 3 VLANs | 192.168.40.0/24, 192.168.99.0/24 |

## 🛠️ Equipment Summary

### Core Infrastructure
- **Routers**: 5 Cisco ISR routers (4331, 4321, 4221)
- **Switches**: 4 Cisco Catalyst switches (2960/3650)
- **Firewall**: 1 Cisco ASA 5506-X
- **Wireless**: 3 Cisco Aironet APs + Wireless Controller

### End-User Devices
- **135 Total End-User Devices**:
  - 75 Student Lab PCs
  - 20 Teacher Laptops
  - 22 Staff Desktop PCs
  - 15 Library PCs
  - 3 Sports/Events PCs

### IoT & Smart Devices
- 12 IP Security Cameras (Cisco Meraki MV22X)
- 10 Interactive Smart Boards (Cisco Webex Board 70)
- 3 Digital Scoreboards
- 1 NVR Server + 1 IoT Management Server

## 💰 Budget Overview

| Category | Cost (USD) | % of Total |
|----------|------------|------------|
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
- VLSM subnetting and IP planning
- OSPF dynamic routing configuration
- Security policy implementation (ACLs, Firewall)
- Network documentation best practices
- Budget planning and project management

## 🔧 Technologies Used

- **Cisco Packet Tracer** - Network simulation and visualization
- **Cisco IOS** - Router and switch configuration
- **Cisco ASA** - Firewall configuration
- **OSPF** - Dynamic routing protocol
- **VLANs** - Network segmentation
- **802.1Q** - VLAN tagging
- **VLSM** - Variable Length Subnet Masking

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