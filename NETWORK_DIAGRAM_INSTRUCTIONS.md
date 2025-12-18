# Network Diagram Instructions - Cisco Packet Tracer

## Overview

This document provides detailed instructions for creating the **Future Vision Smart Campus** network topology in Cisco Packet Tracer. The visual diagram is a critical component of the network design, allowing you to visualize, configure, and test the entire infrastructure.

---

## Prerequisites

### Software Requirements
- **Cisco Packet Tracer 8.0 or later** (Download from [netacad.com](https://www.netacad.com))
- Minimum 4GB RAM
- 1GB free disk space

### Knowledge Requirements
- Basic understanding of network topology
- Familiarity with Cisco Packet Tracer interface
- Basic IP addressing concepts

---

## Step-by-Step Topology Creation

### Phase 1: Canvas Setup and Organization

#### Step 1.1: Prepare the Workspace
1. Open Cisco Packet Tracer
2. Click **File → New** to create a new project
3. **Save the project** as `Future_Vision_Smart_Campus.pkt`
4. Set the canvas size: View → Zoom → 100%

#### Step 1.2: Add Background Containers (Optional but Recommended)
To organize the topology visually, use containers or notes:

1. Select **Place Note** tool from the toolbar
2. Create 5 labeled areas on canvas:
   - **Top Center**: Internet & Firewall Zone
   - **Center**: Core Router Zone
   - **Top Left**: Building A - Administration
   - **Top Right**: Building B - Academic
   - **Bottom Left**: Building C - Services & Library
   - **Bottom Right**: Building D - Sports & Events

---

### Phase 2: Add Network Devices

#### Step 2.1: Add Internet Cloud
1. Click **Network Devices → WAN Emulation**
2. Select **Cloud-PT** and place at top center of canvas
3. Label it: **Internet Cloud**

#### Step 2.2: Add Firewall
1. Click **Network Devices → Security**
2. Select **ASA5506-X** (or ASA 5505 if 5506-X not available)
3. Place below the Internet Cloud
4. Label it: **ASA-Campus-FW**

**Device Specifications:**
- Model: Cisco ASA 5506-X
- Purpose: Perimeter security and NAT
- Interfaces: 8 GigabitEthernet ports

#### Step 2.3: Add Central ISP Edge Router
1. Click **Network Devices → Routers**
2. Select **4331** router
3. Place below the Firewall (center of canvas)
4. Label it: **Router-CORE (ISP Edge)**

**Device Specifications:**
- Model: Cisco ISR 4331
- Purpose: Central aggregation point
- Memory: 4GB RAM
- Interfaces: Multiple GE and Serial interfaces

**Add Required Modules:**
- Power on the router first (if powered off)
- Turn off the router (click on it, go to Physical tab, click power button)
- Drag **NIM-2T** module to an empty slot for Serial interfaces (if needed)
- Turn on the router

#### Step 2.4: Add Building Routers

**Router-A (Administration - Building A):**
1. Select **ISR 4321** router
2. Place in top-left area
3. Label: **Router-A (Admin)**

**Router-B (Academic - Building B):**
1. Select **ISR 4331** router
2. Place in top-right area
3. Label: **Router-B (Academic)**

**Router-C (Services & Library - Building C):**
1. Select **ISR 4321** router
2. Place in bottom-left area
3. Label: **Router-C (Services)**

**Router-D (Sports & Events - Building D):**
1. Select **ISR 4221** router
2. Place in bottom-right area
3. Label: **Router-D (Sports)**

#### Step 2.5: Add Switches

**Switch-A (Building A):**
1. Click **Network Devices → Switches**
2. Select **2960-24TT**
3. Place below Router-A
4. Label: **Switch-A (Admin)**

**Switch-B (Building B) - Hierarchical Architecture:**

**Important**: Building B uses a **hierarchical switch design** with 4 switches total (1 core + 3 access switches) to accommodate all devices.

1. **Core Switch-B**:
   - Select **3650-24PS** (or 2960-24TT if unavailable)
   - Place below Router-B (center position)
   - Label: **Switch-B (Core/Distribution)**

2. **Access Switch-B1** (Student Labs):
   - Select **2960-24TT**
   - Place to the left below Switch-B
   - Label: **Switch-B1 (VLAN 20 - Students)**

3. **Access Switch-B2** (Teachers):
   - Select **2960-24TT**
   - Place in the center below Switch-B
   - Label: **Switch-B2 (VLAN 21 - Teachers)**

4. **Access Switch-B3** (Smart Boards):
   - Select **2960-24TT**
   - Place to the right below Switch-B
   - Label: **Switch-B3 (VLAN 22 - Smart Boards)**

**Connection Diagram**:
```
        Router-B
            |
        Switch-B (Core)
     _______|________
    |       |        |
Switch-B1 B2      B3
```

**Switch-C (Building C):**
1. Select **2960-24TT**
2. Place below Router-C
3. Label: **Switch-C (Services)**

**Switch-D (Building D):**
1. Select **2960-24TT**
2. Place below Router-D
3. Label: **Switch-D (Sports)**

---

### Phase 3: Add End Devices

#### Step 3.1: Building A - Administration Devices

**Add PCs (20 Staff PCs):**
1. Click **End Devices → End Devices**
2. Select **PC-PT**
3. Place 20 PCs below Switch-A (arrange in rows)
4. Label first few: **Admin-PC-1**, **Admin-PC-2**, etc.

**Add Printers (2):**
1. Select **Printer**
2. Place 2 printers near the PCs
3. Label: **Admin-Printer-1**, **Admin-Printer-2**

**Add Wireless Access Point (1):**
1. Click **Network Devices → Wireless Devices**
2. Select **AccessPoint-PT** (or Aironet 1852i if available)
3. Place 1 AP near Switch-A
4. Label: **Admin-WiFi-AP**

**Add IP Cameras (2):**
1. Select **IP Camera** from IoT Devices (or use PC as camera placeholder)
2. Place 2 cameras
3. Label: **Admin-Cam-1**, **Admin-Cam-2**

#### Step 3.2: Building B - Academic Devices

**Add Student Lab PCs (75):**
- Arrange in 3 lab groups of 25 each
- Labels: **Lab1-PC-1** to **Lab1-PC-25**, **Lab2-PC-1** to **Lab2-PC-25**, etc.
- **Tip**: Clone devices to save time (Ctrl+C, Ctrl+V)

**Add Teacher Laptops (20):**
- Use **Laptop-PT** devices
- Labels: **Teacher-Laptop-1** to **Teacher-Laptop-20**

**Add Smart Boards (10):**
- Use **IoT Device** or **Tablet** as placeholder
- Labels: **SmartBoard-1** to **SmartBoard-10**

**Add IP Cameras (4):**
- Labels: **Academic-Cam-1** to **Academic-Cam-4**

#### Step 3.3: Building C - Services & Library Devices

**Add Library PCs (15):**
- Labels: **Library-PC-1** to **Library-PC-15**

**Add Staff PCs (2):**
- Labels: **Library-Staff-PC-1**, **Library-Staff-PC-2**

**Add Servers (3):**
1. Click **End Devices**
2. Select **Server-PT**
3. Add 3 servers:
   - **NVR-Server** (Network Video Recorder)
   - **IoT-Management-Server**
   - **Security-Monitor-Station**

#### Step 3.4: Building D - Sports & Events Devices

**Add Coach/Staff PCs (3):**
- **Coach-PC-1**, **Coach-PC-2**
- **Event-Management-PC**

**Add Scoreboards (3):**
- Use **IoT Device** or custom device
- Labels: **Scoreboard-1** to **Scoreboard-3**

**Add Guest Wi-Fi APs (2):**
- **Guest-WiFi-AP-1**, **Guest-WiFi-AP-2**

---

### Phase 4: Cable Connections

#### Step 4.1: WAN Backbone Connections (Fiber Optic)

**Internet to Firewall:**
1. Click **Connections** (lightning bolt icon)
2. Select **Copper Straight-Through** for Internet
3. Connect **Internet Cloud → ASA Firewall**
   - Cloud: **Ethernet 6**
   - Firewall: **GigabitEthernet 1/1 (Outside)**

**Firewall to ISP Router:**
1. Select **Fiber Optic** cable (for visual distinction)
2. Connect **ASA Firewall → Router-CORE**
   - Firewall: **GigabitEthernet 1/2 (Inside)** → Router-CORE: **GigabitEthernet 0/0/0**

**ISP Router to Building Routers:**
Use **Fiber Optic** cables to represent fiber backbone:

1. **Router-CORE → Router-A:**
   - CORE: **GigabitEthernet 0/0/1** → Router-A: **GigabitEthernet 0/0/0**

2. **Router-CORE → Router-B:**
   - CORE: **GigabitEthernet 0/1/0** → Router-B: **GigabitEthernet 0/0/0**

3. **Router-CORE → Router-C:**
   - CORE: **GigabitEthernet 0/1/1** → Router-C: **GigabitEthernet 0/0/0**

4. **Router-CORE → Router-D:**
   - CORE: **Serial 0/2/0** → Router-D: **GigabitEthernet 0/0/0**
   - *Note: If Serial interface not available, use another GE interface*

#### Step 4.2: Router to Switch Connections

Use **Copper Straight-Through** cables:

1. **Router-A → Switch-A:**
   - Router-A: **GigabitEthernet 0/0/1** → Switch-A: **GigabitEthernet 0/1**

2. **Router-B → Switch-B (Core):**
   - Router-B: **GigabitEthernet 0/0/1** → Switch-B: **GigabitEthernet 1/0/1**

2a. **Switch-B (Core) → Access Switches (Hierarchical Design):**
   - Switch-B: **Gi1/0/2** → Switch-B1: **Gi0/1** (Student Labs trunk)
   - Switch-B: **Gi1/0/3** → Switch-B2: **Gi0/1** (Teachers trunk)
   - Switch-B: **Gi1/0/4** → Switch-B3: **Gi0/1** (Smart Boards trunk)

3. **Router-C → Switch-C:**
   - Router-C: **GigabitEthernet 0/0/1** → Switch-C: **GigabitEthernet 0/1**

4. **Router-D → Switch-D:**
   - Router-D: **GigabitEthernet 0/0/1** → Switch-D: **GigabitEthernet 0/1**

#### Step 4.3: Switch to End Device Connections

Use **Copper Straight-Through** cables for all LAN connections:

**Building A:**
- Connect all 20 Admin PCs to Switch-A ports (FastEthernet 0/2 through 0/21)
- Connect 2 Printers to ports 0/22, 0/23
- Connect 1 AP to port 0/24
- Connect 2 Cameras to any available ports (or use VLAN trunk)

**Building B (Hierarchical Architecture):**
- **Switch-B1** (Student Labs - VLAN 20):
  - Connect 24 Student Lab PCs to Switch-B1 ports (FastEthernet 0/2-24)
  - Remaining student PCs can be distributed across additional ports or switches
- **Switch-B2** (Teachers - VLAN 21):
  - Connect 20 Teacher Laptop docking stations to Switch-B2 ports (FastEthernet 0/2-21)
  - Connect Academic WiFi AP to port 0/24
- **Switch-B3** (Smart Boards - VLAN 22):
  - Connect 10 Smart Boards to Switch-B3 ports (FastEthernet 0/2-11)
- **Switch-B Core** (Direct connections):
  - Connect 4 Academic Cameras to Switch-B ports (Gi1/0/22-24) - VLAN 23

**Building C:**
- Connect Library PCs to Switch-C
- Connect 3 Servers to Switch-C (ports 0/22, 0/23, 0/24 recommended)
- Connect Staff PCs

**Building D:**
- Connect Coach PCs to Switch-D
- Connect Scoreboards to switch ports
- Connect Guest WiFi APs to switch ports

---

### Phase 5: Visual Enhancements

#### Step 5.1: Improve Readability

**Add Labels:**
1. Use **Place Note** tool for major components:
   - "Internet Zone"
   - "Security Layer - Cisco ASA Firewall"
   - "Core Routing Layer"
   - "Building A - Administration (VLAN 10, 11, 12)"
   - "Building B - Academic (VLAN 20, 21, 22, 23)"
   - "Building C - Services (VLAN 30, 31, 32)"
   - "Building D - Sports (VLAN 40, 41, 99)"

**Add IP Address Labels:**
Add notes showing critical IP addresses:
- Firewall Inside: 192.168.100.1/30
- Router-CORE WAN: 192.168.100.2/30
- Router-A Link: 192.168.1.2/30
- Router-B Link: 192.168.2.2/30
- Router-C Link: 192.168.3.2/30
- Router-D Link: 192.168.4.2/30

#### Step 5.2: Color Coding (Optional)

Use different cable colors to distinguish:
- **Orange/Yellow**: Fiber optic backbone (WAN links)
- **Red**: Connections to Firewall
- **Green**: Building A connections
- **Blue**: Building B connections
- **Purple**: Building C connections
- **Gray**: Building D connections

To change cable color:
1. Right-click on cable
2. Select **Color** → Choose color

#### Step 5.3: Arrange and Organize

1. **Align devices** using grid snapping (Options → Preferences → Enable Grid)
2. **Space evenly** for clarity
3. **Minimize cable crossings**
4. **Use label font** size for readability (12-14pt recommended)

---

### Phase 6: Device Configuration Preparation

#### Step 6.1: Configure Hostnames

Click on each device and go to **CLI** tab to configure hostnames:

**Example for Router-CORE:**
```
Router>enable
Router#configure terminal
Router(config)#hostname Router-CORE
Router-CORE(config)#
```

Repeat for all routers and switches with their respective names.

#### Step 6.2: Save Initial Topology

1. Click **File → Save**
2. Save as: **Future_Vision_Smart_Campus_Topology.pkt**
3. Create a backup: Save a copy to external storage

---

## Device Count Verification

Use this checklist to ensure all devices are added:

### Network Infrastructure
- [ ] 1 Internet Cloud
- [ ] 1 Cisco ASA 5506-X Firewall
- [ ] 1 ISP Edge Router (Cisco ISR 4331)
- [ ] 4 Building Routers (2×4321, 1×4331, 1×4221)
- [ ] **7 Cisco Catalyst Switches** (updated for hierarchical Building B design):
  - [ ] 1 Switch-A (Building A)
  - [ ] 4 Building B Switches (1 core + 3 access: Switch-B, B1, B2, B3)
  - [ ] 1 Switch-C (Building C)
  - [ ] 1 Switch-D (Building D)

### Building A Devices (Administration)
- [ ] 20 Staff Desktop PCs
- [ ] 2 Network Printers
- [ ] 1 Staff Wi-Fi Access Point
- [ ] 2 IP Security Cameras

### Building B Devices (Academic)
- [ ] 75 Student Lab PCs (3 labs × 25)
- [ ] 20 Teacher Laptops
- [ ] 10 Smart Interactive Boards
- [ ] 4 IP Security Cameras

### Building C Devices (Services & Library)
- [ ] 15 Library PCs
- [ ] 2 Library Staff PCs
- [ ] 1 NVR Server
- [ ] 1 IoT Management Server
- [ ] 1 Security Monitor Station

### Building D Devices (Sports & Events)
- [ ] 2 Coach Desktop PCs
- [ ] 1 Event Management PC
- [ ] 3 IoT Digital Scoreboards
- [ ] 2 High-Density Guest Wi-Fi APs

**Total Device Count:**
- Routers: 5
- **Switches: 7** (updated for hierarchical Building B design)
- Firewall: 1
- End Devices: 135+
- IoT Devices: 13+
- Servers: 3
- APs: 3
- **Grand Total: 167+ devices** (updated)

---

## Cable Connection Verification

### WAN Links (Fiber Backbone)
- [ ] Internet Cloud → Firewall (Outside)
- [ ] Firewall (Inside) → Router-CORE (Gi0/0/0)
- [ ] Router-CORE (Gi0/0/1) → Router-A (Gi0/0/0)
- [ ] Router-CORE (Gi0/1/0) → Router-B (Gi0/0/0)
- [ ] Router-CORE (Gi0/1/1) → Router-C (Gi0/0/0)
- [ ] Router-CORE (Se0/2/0) → Router-D (Gi0/0/0)

### Building Distribution
- [ ] Router-A (Gi0/0/1) → Switch-A (Gi0/1)
- [ ] Router-B (Gi0/0/1) → Switch-B (Gi0/1)
- [ ] Router-C (Gi0/0/1) → Switch-C (Gi0/1)
- [ ] Router-D (Gi0/0/1) → Switch-D (Gi0/1)

### End Device Connections
- [ ] All PCs connected to respective switches
- [ ] All servers connected to Switch-C
- [ ] All printers connected to Switch-A
- [ ] All cameras connected to switches
- [ ] All IoT devices connected
- [ ] All APs connected

---

## Best Practices for Packet Tracer Topology

### Performance Optimization
1. **Minimize unnecessary animations** - Turn off device animations if experiencing lag
2. **Save frequently** - Save after completing each building section
3. **Use device groups** - Group related devices for easier management
4. **Backup regularly** - Keep multiple versions during development

### Documentation in Packet Tracer
1. **Add comprehensive notes** explaining each section
2. **Label all links** with VLAN IDs and IP addresses
3. **Use consistent naming** conventions for all devices
4. **Color-code VLANs** for visual identification

### Professional Presentation
1. **Clean layout** - Avoid overlapping devices or cables
2. **Logical flow** - Arrange devices hierarchically (Internet → Core → Distribution → Access)
3. **Proper spacing** - Leave adequate space between building sections
4. **Clear labels** - Ensure all text is readable at 100% zoom

---

## Next Steps After Creating Topology

1. **Proceed to Configuration**: Use the **[Packet Tracer Implementation Guide](./Packet_Tracer_Implementation_Guide.md)** to configure all devices

2. **Implement VLANs**: Configure VLANs on switches according to the design

3. **Configure Routing**: Set up OSPF routing protocol on all routers

4. **Apply Security**: Implement ACLs and firewall rules

5. **Test Connectivity**: Verify end-to-end connectivity and security policies

---

## Troubleshooting Common Issues

### Issue: Routers don't have required interfaces
**Solution**: 
- Turn off router
- Add network modules (NIM-2T for serial, etc.)
- Turn on router

### Issue: Cables show red dots (connection down)
**Solution**:
- Verify correct cable type (straight-through vs crossover)
- Check interface status with `show ip interface brief`
- Ensure interfaces are not administratively down

### Issue: Too many devices causing performance issues
**Solution**:
- Simplify the topology (reduce number of end devices)
- Use fewer PCs to represent labs (e.g., 10 PCs instead of 75)
- Close unnecessary Packet Tracer windows
- Increase allocated RAM to Packet Tracer

### Issue: Cannot save large topology file
**Solution**:
- Split into multiple files if needed
- Remove unnecessary devices
- Compress the .pkt file

---

## File Naming Convention

When saving, use descriptive names:
- `Future_Vision_Smart_Campus_Complete.pkt` - Full topology
- `Future_Vision_Smart_Campus_Configured.pkt` - Fully configured
- `Future_Vision_Smart_Campus_Tested.pkt` - Tested and verified

---

## Additional Resources

### Cisco Packet Tracer Resources
- **Cisco Networking Academy**: [netacad.com](https://www.netacad.com)
- **Packet Tracer Tutorials**: YouTube Cisco Packet Tracer tutorials
- **Documentation**: Cisco Packet Tracer Help menu

### Network Design References
- [Smart Campus Network Design Report](./Smart_Campus_Network_Design_Report.md)
- [Packet Tracer Implementation Guide](./Packet_Tracer_Implementation_Guide.md)
- Cisco Design Zone: Campus Network Design Guides

---

## Conclusion

This visual diagram serves as the foundation for the Future Vision Smart Campus network. Once completed, you'll have a comprehensive, professional network topology that demonstrates enterprise-level design principles and can be used for configuration, testing, and presentation purposes.

**Estimated Time to Complete**: 2-3 hours for full topology creation

**Next Document**: Proceed to [Packet Tracer Implementation Guide](./Packet_Tracer_Implementation_Guide.md) for device configuration instructions.

---

**Created By**: Network Engineering Team  
**Version**: 1.0  
**Last Updated**: December 2025  
**Course**: Computer Networks - Smart Campus Project
