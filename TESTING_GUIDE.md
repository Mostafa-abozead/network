# Future Vision Smart Campus Network Testing Guide

## 📋 Overview

This comprehensive testing guide provides systematic procedures to verify all aspects of the Future Vision Smart Campus network implementation. Use this guide to ensure proper configuration, security, and functionality of the network.

---

## 🎯 Testing Objectives

The testing process validates:
- ✅ DHCP functionality across all VLANs
- ✅ Inter-VLAN routing and connectivity
- ✅ OSPF routing protocol operation
- ✅ ACL security policies enforcement
- ✅ SSH management access control
- ✅ Firewall NAT and security functions
- ✅ Guest WiFi complete isolation
- ✅ IoT device access restrictions
- ✅ Cross-building routing

---

## 🧪 Phase 1: DHCP & Basic Connectivity Tests

### Test 1.1: DHCP Address Assignment

**Purpose**: Verify that all end devices receive correct IP addresses automatically.

**Procedure**:
1. For each end device (PC/Laptop), go to **Desktop** → **IP Configuration**
2. Select **DHCP**
3. Wait for IP assignment (should take 1-5 seconds)

**Expected Results**:

| Device Type | Expected IP Range | Expected Gateway | Expected DNS |
|-------------|------------------|------------------|--------------|
| Admin Staff PC | 192.168.10.11-126 | 192.168.10.1 | 8.8.8.8, 8.8.4.4 |
| Admin WiFi | 192.168.10.130-190 | 192.168.10.129 | 8.8.8.8, 8.8.4.4 |
| Student Lab PC | 192.168.20.11-126 | 192.168.20.1 | 8.8.8.8, 8.8.4.4 |
| Teacher Laptop | 192.168.20.135-190 | 192.168.20.129 | 8.8.8.8, 8.8.4.4 |
| Library PC | 192.168.30.11-62 | 192.168.30.1 | 8.8.8.8, 8.8.4.4 |
| Sports Staff PC | 192.168.40.6-30 | 192.168.40.1 | 8.8.8.8, 8.8.4.4 |
| Guest WiFi | 192.168.99.11-254 | 192.168.99.1 | 8.8.8.8, 8.8.4.4 |

**Pass Criteria**: ✅ All devices receive IP addresses within their respective subnet ranges

**Troubleshooting**:
- If device shows 169.254.x.x (APIPA), check DHCP pool configuration on router
- Verify VLAN assignment on switch port
- Check trunk configuration between switch and router

---

### Test 1.2: Gateway Reachability

**Purpose**: Verify connectivity to the default gateway from each device.

**Procedure**:
1. Open **Command Prompt** on each PC type
2. Run: `ping <gateway-ip>`

**Test Commands**:
```bash
# From Admin Staff PC
ping 192.168.10.1

# From Student Lab PC
ping 192.168.20.1

# From Library PC
ping 192.168.30.1

# From Guest WiFi device
ping 192.168.99.1
```

**Expected Results**: ✅ All pings should succeed with 0% packet loss

**Pass Criteria**: Reply from gateway IP with < 10ms latency

---

### Test 1.3: DNS Configuration Verification

**Purpose**: Verify DNS servers are correctly assigned.

**Procedure**:
1. On each PC, go to **Command Prompt**
2. Run: `ipconfig /all`
3. Check DNS Server entries

**Expected Results**: DNS Servers should show `8.8.8.8` and `8.8.4.4` (or configured DNS)

---

## 🧪 Phase 2: Routing & Inter-VLAN Connectivity Tests

### Test 2.1: Admin PC Access Tests

**Purpose**: Verify that Admin Staff has broad network access as expected.

**Test Device**: Admin-PC-1 (VLAN 10, Building A)

**Test Commands**:
```bash
# Test access to other buildings
ping 192.168.20.1      # Router-B (Academic) - Should SUCCEED
ping 192.168.30.1      # Router-C (Services) - Should SUCCEED
ping 192.168.40.1      # Router-D (Sports) - Should SUCCEED

# Test access to servers
ping 192.168.30.70     # NVR Server - Should SUCCEED
ping 192.168.30.71     # IoT Management Server - Should SUCCEED
ping 192.168.30.98     # Security Monitor - Should SUCCEED

# Test internet access
ping 8.8.8.8           # Google DNS - Should SUCCEED
```

**Expected Results**:

| Destination | Expected Result | ACL Rule |
|-------------|----------------|----------|
| Router-B (Academic) | ✅ SUCCEED | Allowed by default |
| Router-C (Services) | ✅ SUCCEED | Allowed by default |
| Router-D (Sports) | ✅ SUCCEED | Allowed by default |
| NVR Server | ✅ SUCCEED | Admin allowed to servers |
| Security Monitor | ✅ SUCCEED | Admin allowed to security |
| Internet | ✅ SUCCEED | Firewall allows outbound |

**Pass Criteria**: All pings succeed

---

### Test 2.2: Student PC Access Tests (Restriction Verification)

**Purpose**: Verify that students are blocked from admin networks but have appropriate access.

**Test Device**: Lab1-PC-1 (VLAN 20, Building B)

**Test Commands**:
```bash
# Test access to BLOCKED networks
ping 192.168.10.1      # Admin gateway - Should FAIL
ping 192.168.10.10     # Admin PC - Should FAIL
ping 192.168.30.97     # Security Monitor - Should FAIL
ping 192.168.40.5      # Sports Staff - Should FAIL

# Test access to ALLOWED networks
ping 192.168.30.1      # Library gateway - Should SUCCEED
ping 192.168.30.70     # NVR Server (HTTP/HTTPS only) - Should SUCCEED
ping 8.8.8.8           # Internet - Should SUCCEED

# Test access to same building
ping 192.168.20.129    # Teacher gateway - Should SUCCEED
```

**Expected Results**:

| Destination | Expected Result | ACL Rule |
|-------------|----------------|----------|
| Admin networks | ❌ BLOCKED | STUDENT-LAB-RESTRICT |
| Security Monitor | ❌ BLOCKED | STUDENT-LAB-RESTRICT |
| Sports Staff | ❌ BLOCKED | STUDENT-LAB-RESTRICT |
| Library | ✅ ALLOWED | STUDENT-LAB-RESTRICT permit |
| Servers | ✅ ALLOWED | SERVER-VLAN-PROTECT (HTTP/HTTPS) |
| Teachers | ✅ ALLOWED | Same building routing |
| Internet | ✅ ALLOWED | Default permit |

**Pass Criteria**: All expected blocks are effective, allowed traffic succeeds

---

### Test 2.3: Teacher Access Tests

**Purpose**: Verify teachers have broader access than students but restricted from admin.

**Test Device**: Teacher-Laptop-1 (VLAN 21, Building B)

**Test Commands**:
```bash
# Test access to BLOCKED networks
ping 192.168.10.1      # Admin Staff - Should FAIL

# Test access to ALLOWED networks
ping 192.168.20.1      # Student Lab - Should SUCCEED
ping 192.168.20.193    # Smart Boards - Should SUCCEED
ping 192.168.30.1      # Library - Should SUCCEED
ping 192.168.30.70     # Servers - Should SUCCEED
ping 8.8.8.8           # Internet - Should SUCCEED
```

**Expected Results**:

| Destination | Expected Result | ACL Rule |
|-------------|----------------|----------|
| Admin Staff | ❌ BLOCKED | TEACHER-RESTRICT |
| Student Labs | ✅ ALLOWED | TEACHER-RESTRICT permit |
| Smart Boards | ✅ ALLOWED | TEACHER-RESTRICT permit |
| Library | ✅ ALLOWED | TEACHER-RESTRICT permit |
| Servers | ✅ ALLOWED | TEACHER-RESTRICT permit |
| Internet | ✅ ALLOWED | Default permit |

---

### Test 2.4: Library User Access Tests

**Purpose**: Verify library PCs have limited access (public access model).

**Test Device**: Library-PC-1 (VLAN 30, Building C)

**Test Commands**:
```bash
# Test access to BLOCKED networks
ping 192.168.10.1      # Admin - Should FAIL
ping 192.168.30.97     # Security Monitor - Should FAIL
ping 192.168.40.1      # Sports Staff - Should FAIL

# Test access to ALLOWED networks
ping 192.168.20.1      # Academic - Should SUCCEED
ping 8.8.8.8           # Internet - Should SUCCEED
```

**Expected Results**:

| Destination | Expected Result | ACL Rule |
|-------------|----------------|----------|
| Admin networks | ❌ BLOCKED | LIBRARY-USER-RESTRICT |
| Security Monitor | ❌ BLOCKED | LIBRARY-USER-RESTRICT |
| Sports Staff | ❌ BLOCKED | LIBRARY-USER-RESTRICT |
| Academic Building | ✅ ALLOWED | Default permit |
| Internet | ✅ ALLOWED | Default permit |

---

### Test 2.5: Guest WiFi Isolation Tests (CRITICAL)

**Purpose**: Verify complete isolation of guest network from ALL internal networks.

**Test Device**: Guest-WiFi device (VLAN 99, Building D)

**Test Commands**:
```bash
# Test access to ALL internal networks (ALL should FAIL)
ping 192.168.10.1      # Admin - Should FAIL
ping 192.168.20.1      # Academic - Should FAIL
ping 192.168.30.1      # Library - Should FAIL
ping 192.168.30.70     # Servers - Should FAIL
ping 192.168.30.97     # Security Monitor - Should FAIL
ping 192.168.40.1      # Sports Staff - Should FAIL

# Test internet access (should SUCCEED)
ping 8.8.8.8           # Internet - Should SUCCEED
```

**Expected Results**:

| Destination | Expected Result | ACL Rule |
|-------------|----------------|----------|
| ANY internal network | ❌ BLOCKED | GUEST-WIFI-ISOLATION |
| Admin networks | ❌ BLOCKED | GUEST-WIFI-ISOLATION |
| Academic networks | ❌ BLOCKED | GUEST-WIFI-ISOLATION |
| Library | ❌ BLOCKED | GUEST-WIFI-ISOLATION |
| Servers | ❌ BLOCKED | GUEST-WIFI-ISOLATION |
| Security systems | ❌ BLOCKED | GUEST-WIFI-ISOLATION |
| Sports Staff | ❌ BLOCKED | GUEST-WIFI-ISOLATION |
| Internet | ✅ ALLOWED | HTTP/HTTPS/DNS permit |

**Pass Criteria**: ❌ ALL internal network access must fail, ✅ Internet access succeeds

---

## 🧪 Phase 3: Management Access (SSH) Tests

### Test 3.1: SSH from Authorized Network

**Purpose**: Verify SSH access works from admin networks.

**Test Device**: Admin-PC-1 (192.168.10.x)

**Procedure**:
1. Open **Command Prompt**
2. Run: `ssh admin@192.168.1.1` (Router-CORE IP)
3. Enter password: `Admin@123`
4. Verify login succeeds

**Test Commands**:
```bash
# Test SSH to all routers
ssh admin@192.168.1.1       # Router-CORE - Should SUCCEED
ssh admin@192.168.1.2       # Router-A - Should SUCCEED
ssh admin@192.168.2.2       # Router-B - Should SUCCEED
ssh admin@192.168.3.2       # Router-C - Should SUCCEED
ssh admin@192.168.4.2       # Router-D - Should SUCCEED
```

**Expected Results**: ✅ All SSH connections succeed with username/password authentication

---

### Test 3.2: SSH from Unauthorized Networks (Should Fail)

**Purpose**: Verify SSH access is blocked from student and guest networks.

**Test Devices**: 
- Student Lab PC (192.168.20.x)
- Guest WiFi device (192.168.99.x)

**Test Commands**:
```bash
# From Student PC (should be blocked by ACL on VTY lines)
ssh admin@192.168.1.1       # Should TIMEOUT or be DENIED

# From Guest WiFi (should be blocked by ACL)
ssh admin@192.168.1.1       # Should TIMEOUT or be DENIED
```

**Expected Results**: ❌ SSH connections should fail or timeout (VTY ACL blocks non-admin sources)

**Pass Criteria**: No SSH access granted to unauthorized networks

---

### Test 3.3: SSH Authentication Test

**Purpose**: Verify username/password authentication is working.

**Procedure**:
1. From Admin PC, SSH to any router
2. Test different privilege levels:
   - `admin` (privilege 15) - Full access
   - `netadmin` (privilege 15) - Full access
   - `monitor` (privilege 1) - Read-only
3. Verify enable mode access with enable secret

**Test Commands**:
```cisco
# Test full access user
Router> enable
Router# configure terminal  ← Should work for privilege 15

# Test read-only user (monitor)
Router> enable
Router# configure terminal  ← Should be DENIED for privilege 1
```

---

## 🧪 Phase 4: Service Access Tests

### Test 4.1: DNS Resolution Test

**Purpose**: Verify DNS resolution is working for internet access.

**Test Devices**: Any PC with internet access

**Test Commands**:
```bash
# Test DNS resolution
nslookup www.google.com
nslookup www.cisco.com

# Expected output: Should resolve to IP addresses
```

**Expected Results**: Domain names resolve to IP addresses

---

### Test 4.2: HTTP/HTTPS Access Test

**Purpose**: Verify web traffic filtering (if web server configured).

**Test Commands**:
```bash
# From Student PC - should have HTTP/HTTPS access to servers
ping 192.168.30.70          # NVR Server

# From Guest WiFi - should only access internet
# (Internal HTTP blocked, external HTTP allowed)
```

---

### Test 4.3: Traceroute Path Verification

**Purpose**: Verify routing paths through the network.

**Test Commands**:
```bash
# From Admin PC to Library
tracert 192.168.30.1

# Expected path:
# 1. 192.168.10.1 (Router-A gateway)
# 2. 192.168.1.1 (Router-CORE)
# 3. 192.168.3.1 (Router-CORE to Router-C link)
# 4. 192.168.30.1 (Router-C gateway)

# From Student PC to Sports Staff
tracert 192.168.40.1

# Expected path:
# 1. 192.168.20.1 (Router-B gateway)
# 2. 192.168.2.1 (Router-CORE)
# 3. 192.168.4.1 (Router-D)
```

**Expected Results**: Traceroute shows proper multi-hop routing through Router-CORE

---

## 🧪 Phase 5: Camera & IoT Access Tests

### Test 5.1: Security Monitor Access to Cameras

**Purpose**: Verify security monitor station can access all cameras.

**Test Device**: Security-Monitor-Station (192.168.30.98)

**Test Commands**:
```bash
# Test access to all camera networks
ping 192.168.10.194    # Admin Camera (VLAN 12)
ping 192.168.20.226    # Academic Camera (VLAN 23)

# Should SUCCEED - Security monitor has access to cameras
```

**Expected Results**: ✅ Security monitor can reach all camera VLANs

---

### Test 5.2: Student/Guest Blocked from Cameras

**Purpose**: Verify students and guests cannot access security cameras.

**Test Devices**:
- Student Lab PC
- Guest WiFi device

**Test Commands**:
```bash
# From Student PC
ping 192.168.10.194    # Admin Camera - Should FAIL

# From Guest WiFi
ping 192.168.10.194    # Any Camera - Should FAIL
```

**Expected Results**: ❌ Students and guests blocked from camera access

---

### Test 5.3: Smart Board Access (Teachers Only)

**Purpose**: Verify only teachers can control smart boards.

**Test Commands**:
```bash
# From Teacher Laptop
ping 192.168.20.193    # Smart Board gateway - Should SUCCEED

# From Student PC
ping 192.168.20.193    # Smart Board - Should SUCCEED (same subnet)
# But control access would be via application-level security
```

---

## 🧪 Phase 6: Cross-Building Routing Tests

### Test 6.1: Inter-Building Connectivity

**Purpose**: Verify OSPF routing enables communication between all buildings.

**Test Commands**:
```bash
# From Building A to Building B
ping 192.168.20.1

# From Building A to Building C
ping 192.168.30.1

# From Building A to Building D
ping 192.168.40.1

# From Building B to Building C
ping 192.168.30.1

# From Building B to Building D
ping 192.168.40.1

# From Building C to Building D
ping 192.168.40.1
```

**Expected Results**: ✅ All inter-building pings succeed (subject to ACL permissions)

---

### Test 6.2: OSPF Neighbor Verification

**Purpose**: Verify OSPF routing protocol is functioning properly.

**Procedure**: On each router, run:

```cisco
show ip ospf neighbor

! Expected output on Router-CORE:
! Neighbor ID     Pri   State           Dead Time   Address         Interface
! 10.10.10.10     0     FULL/  -        00:00:39    192.168.1.2     Gi0/0/1
! 20.20.20.20     0     FULL/  -        00:00:35    192.168.2.2     Gi0/1/0
! 30.30.30.30     0     FULL/  -        00:00:38    192.168.3.2     Gi0/1/1
! 40.40.40.40     0     FULL/  -        00:00:32    192.168.4.2     Se0/2/0

! Expected output on building routers (e.g., Router-A):
! Neighbor ID     Pri   State           Dead Time   Address         Interface
! 1.1.1.1         0     FULL/  -        00:00:37    192.168.1.1     Gi0/0/0
```

**Pass Criteria**: All routers show FULL state with their neighbors

---

### Test 6.3: OSPF Routing Table Verification

**Purpose**: Verify all subnets are learned via OSPF.

**Procedure**: On each router, run:

```cisco
show ip route ospf

! Expected: See 'O' routes for all remote subnets
! O    192.168.20.0/25 [110/2] via 192.168.2.2, GigabitEthernet0/1/0
! O    192.168.30.0/26 [110/2] via 192.168.3.2, GigabitEthernet0/1/1
! etc.
```

**Pass Criteria**: All VLANs from other buildings appear in routing table with 'O' designation

---

## 📊 Comprehensive Test Results Matrix

| Source Device | Destination | Expected Result | ACL/Security Rule | Priority |
|--------------|-------------|-----------------|-------------------|----------|
| **Admin PC** | Any internal network | ✅ ALLOW | Default permit | HIGH |
| **Admin PC** | Internet | ✅ ALLOW | Firewall NAT | HIGH |
| **Admin PC** | Security Monitor | ✅ ALLOW | Admin access | HIGH |
| **Student PC** | Admin networks | ❌ BLOCK | STUDENT-LAB-RESTRICT | CRITICAL |
| **Student PC** | Security Monitor | ❌ BLOCK | STUDENT-LAB-RESTRICT | CRITICAL |
| **Student PC** | Library | ✅ ALLOW | Default permit | MEDIUM |
| **Student PC** | Servers (HTTP/HTTPS) | ✅ ALLOW | SERVER-VLAN-PROTECT | MEDIUM |
| **Student PC** | Internet | ✅ ALLOW | Firewall NAT | HIGH |
| **Teacher** | Student Labs | ✅ ALLOW | TEACHER-RESTRICT | MEDIUM |
| **Teacher** | Admin networks | ❌ BLOCK | TEACHER-RESTRICT | HIGH |
| **Teacher** | Smart Boards | ✅ ALLOW | SMARTBOARD-PROTECT | MEDIUM |
| **Library PC** | Admin networks | ❌ BLOCK | LIBRARY-USER-RESTRICT | HIGH |
| **Library PC** | Security Monitor | ❌ BLOCK | LIBRARY-USER-RESTRICT | CRITICAL |
| **Library PC** | Internet | ✅ ALLOW | Firewall NAT | MEDIUM |
| **Guest WiFi** | ANY internal network | ❌ BLOCK | GUEST-WIFI-ISOLATION | CRITICAL |
| **Guest WiFi** | Internet only | ✅ ALLOW | HTTP/HTTPS/DNS | CRITICAL |
| **Sports Staff** | Academic networks | ❌ BLOCK | SPORTS-STAFF-PROTECT | MEDIUM |
| **Sports Staff** | Scoreboards | ✅ ALLOW | SCOREBOARD-PROTECT | MEDIUM |
| **Security Monitor** | All cameras | ✅ ALLOW | CAMERA-RESTRICT | HIGH |
| **Any PC** | SSH to routers | ✅/❌ ALLOW/BLOCK | VTY ACL (admin only) | HIGH |

---

## 🔧 Troubleshooting Guide

### Issue: DHCP Not Working

**Symptoms**: Device shows 169.254.x.x (APIPA address)

**Troubleshooting Steps**:
1. Check DHCP pool configuration: `show ip dhcp pool`
2. Verify DHCP bindings: `show ip dhcp binding`
3. Check excluded addresses: `show ip dhcp excluded-address`
4. Verify VLAN configuration on switch
5. Check trunk port between switch and router

**Solution**:
```cisco
! Clear DHCP bindings
clear ip dhcp binding *

! Verify pool matches subnet
show run | section dhcp
```

---

### Issue: Ping Fails Between VLANs

**Symptoms**: Cannot ping across VLANs but same-VLAN pings work

**Troubleshooting Steps**:
1. Check router subinterfaces: `show ip interface brief`
2. Verify routing table: `show ip route`
3. Check ACLs: `show access-lists`
4. Verify trunk port: `show interfaces trunk`

---

### Issue: SSH Access Fails

**Symptoms**: Cannot SSH to router

**Troubleshooting Steps**:
1. Verify RSA keys generated: `show crypto key mypubkey rsa`
2. Check SSH configuration: `show ip ssh`
3. Check VTY ACL: `show access-class`
4. Verify domain name configured: `show run | include domain-name`

**Solution**:
```cisco
! Generate RSA keys if missing
crypto key generate rsa
! Enter 2048 when prompted

! Verify SSH enabled
ip ssh version 2
```

---

### Issue: Guest WiFi Can Access Internal Networks

**Symptoms**: Guest devices can ping internal IPs (SECURITY ISSUE!)

**Troubleshooting Steps**:
1. Check ACL applied to VLAN 99: `show ip interface Gi0/0/1.99`
2. Verify ACL content: `show access-lists GUEST-WIFI-ISOLATION`
3. Check ACL hit counts: `show access-lists GUEST-WIFI-ISOLATION`

**Solution**:
```cisco
! Verify ACL is applied inbound
interface GigabitEthernet 0/0/1.99
 ip access-group GUEST-WIFI-ISOLATION in
```

---

### Issue: OSPF Neighbors Not Forming

**Symptoms**: `show ip ospf neighbor` shows no neighbors

**Troubleshooting Steps**:
1. Check interface status: `show ip interface brief`
2. Verify OSPF configuration: `show ip ospf`
3. Check network statements: `show run | section ospf`
4. Verify OSPF area matches on both sides

**Solution**:
```cisco
! Ensure network statement includes interface
router ospf 1
 network 192.168.1.0 0.0.0.3 area 0
```

---

## ✅ Final Testing Checklist

### Core Functionality
- [ ] All devices receive DHCP addresses
- [ ] Gateway reachability from all VLANs
- [ ] DNS resolution working
- [ ] Inter-VLAN routing functional
- [ ] Internet access through firewall

### Security Policies
- [ ] Guest WiFi completely isolated from internal networks
- [ ] Students blocked from admin networks
- [ ] Teachers blocked from admin but have academic access
- [ ] Library PCs have limited access
- [ ] SSH only accessible from admin networks
- [ ] Camera access restricted to security monitoring

### Routing Protocol
- [ ] OSPF neighbors in FULL state
- [ ] All subnets in routing tables
- [ ] Fast convergence on link failure
- [ ] Proper routing paths verified with traceroute

### Performance
- [ ] Ping latency < 10ms within building
- [ ] Ping latency < 50ms cross-building
- [ ] No packet loss in steady-state
- [ ] DHCP lease assignment < 5 seconds

---

## 📈 Test Report Template

```
Future Vision Smart Campus Network Test Report
==============================================

Date: ________________
Tester: ______________
Version: _____________

Test Summary:
- Total Tests: _____
- Passed: _____
- Failed: _____
- Pass Rate: _____%

Critical Issues Found:
1. _____________________
2. _____________________

Medium Issues Found:
1. _____________________
2. _____________________

Recommendations:
1. _____________________
2. _____________________

Sign-off:
Network Engineer: ________________  Date: ________
Project Manager: ________________  Date: ________
```

---

## 🎯 Success Criteria

The network implementation is considered successful when:

1. ✅ **100% DHCP Success Rate**: All devices receive proper IP addresses
2. ✅ **Zero ACL Violations**: All security policies enforced correctly
3. ✅ **Full OSPF Convergence**: All routers in FULL state
4. ✅ **Complete Guest Isolation**: Zero internal network access from guest WiFi
5. ✅ **SSH Access Control**: Only admin networks can SSH to infrastructure
6. ✅ **Low Latency**: < 50ms cross-building, < 10ms within building
7. ✅ **No Packet Loss**: 0% loss in steady-state operation

---

**Testing Guide Version**: 1.0  
**Last Updated**: December 2025  
**Repository**: Mostafa-abozead/network  
**Total Test Scenarios**: 20+

---

*Use this guide systematically to ensure the Future Vision Smart Campus network meets all functional and security requirements before deployment.* 🚀
