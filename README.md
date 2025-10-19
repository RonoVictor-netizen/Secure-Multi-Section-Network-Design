# Secure Multi-Section Enterprise Network Design

## 📋 Project Overview

This comprehensive enterprise network design implements a secure three-section architecture with advanced VLAN segmentation, OSPF dynamic routing, and multi-layered security controls. Built using Cisco Packet Tracer, this project demonstrates enterprise-grade network infrastructure with robust security policies and efficient routing protocols.

## 🌐 Network Architecture

### Physical Topology
- **3x Cisco 2911 Routers** (R1, R2, R3) - Each serving as a section gateway
- **3x Cisco 2960 Switches** (SW1, SW2, SW3) - Distribution and access layer switching
- **9x End-user PCs** (3 per section, distributed across VLANs)
- **Full mesh inter-router connectivity** for redundancy and optimal routing

### VLAN Architecture & Segmentation

| Section | Management VLAN | User VLAN | Guest VLAN | Native VLAN |
|---------|-----------------|-----------|------------|-------------|
| Section 1 (R1) | VLAN 10: 192.168.10.1/24 | VLAN 20: 192.168.110.1/24 | VLAN 30: 192.168.210.1/24 | VLAN 99 |
| Section 2 (R2) | VLAN 10: 192.168.20.1/24 | VLAN 20: 192.168.120.1/24 | VLAN 30: 192.168.220.1/24 | VLAN 99 |
| Section 3 (R3) | VLAN 10: 192.168.30.1/24 | VLAN 20: 192.168.130.1/24 | VLAN 30: 192.168.230.1/24 | VLAN 99 |

### VLAN Purpose Description
- **VLAN 10 (Management)**: Network infrastructure management access
- **VLAN 20 (Users)**: Regular employee workstations and corporate devices
- **VLAN 30 (Guests)**: Visitor and contractor network access with restricted permissions
- **VLAN 99 (Native)**: Untagged VLAN for trunk ports security

## 🛡️ Comprehensive Security Implementation

### Layer 2 Security Features

#### Port Security
- **Strict MAC Address Control**: 1 MAC address per access port maximum
- **Violation Policy**: Immediate shutdown on unauthorized MAC detection
- **Sticky MAC Learning**: Dynamic learning with automatic configuration saving

#### Spanning Tree Protections
- **BPDU Guard**: Enabled on all access ports to prevent topology changes
- **PortFast**: Configured on access ports for rapid transition to forwarding state
- **Root Guard**: Protection against unauthorized root bridge elections

#### DHCP Security
- **DHCP Snooping**: Enabled globally with trusted uplink ports
- **Trust Configuration**: Only trunk ports and authorized DHCP servers marked as trusted
- **Rate Limiting**: DHCP packet rate limiting to prevent exhaustion attacks

#### Switch Port Hardening
- **Unused Port Administration**: All unused switch ports disabled
- **Manual Enablement**: Ports require explicit enablement for device connection
- **Trunk Port Security**: Explicit VLAN allowed lists on all trunk interfaces

### Layer 3 Security Features

#### Access Control Lists (ACLs)

**Guest VLAN ACL Policy (VLAN 30)**
- Deny access to all internal corporate VLANs (10, 20, 110, 120, 130, 210, 220, 230)
- Permit internet-bound traffic only
- Restrict inter-guest communication between sections

**User VLAN ACL Policy (VLAN 20)**
- Deny access to Management VLANs (10, 20, 30)
- Permit access to other User VLANs across sections
- Allow necessary inter-VLAN communication for business applications

**Management VLAN ACL Policy (VLAN 10)**
- Full administrative access to all network segments
- Restricted to authorized IT personnel only
- Logging enabled for security monitoring

#### Management Plane Security
- **SSH-Only Access**: Telnet completely disabled network-wide
- **VLAN Restriction**: Management access permitted only from Management VLAN
- **User Authentication**: Local username/password authentication required
- **Session Timeouts**: Automatic session termination after inactivity periods

## 🔄 Dynamic Routing Configuration

### OSPF Routing Protocol
- **Process ID**: 1 (consistent across all routers)
- **Area Design**: All networks in Area 0 for simplified topology
- **Network Statements**: All internal networks advertised via OSPF
- **Router IDs**: Manually configured for stability (R1: 1.1.1.1, R2: 2.2.2.2, R3: 3.3.3.3)

### Route Redistribution
- Directly connected networks automatically redistributed
- Equal-cost load balancing enabled for redundant paths
- Route summarization at section boundaries where applicable

### Neighbor Relationships
- **Full Mesh Adjacencies**: Each router maintains OSPF neighbors with both peers
- **Hello Timers**: Default 10-second intervals with 40-second dead timer
- **Authentication**: OSPF MD5 authentication for secure neighbor relationships

## 🚀 Implementation Guide

### Prerequisites
- **Cisco Packet Tracer 8.2** or later recommended
- Basic understanding of Cisco IOS commands
- Fundamental knowledge of VLANs, OSPF, and ACLs

### Installation Steps
1. **Download** the `Secure-Multi-Section-Network.pkt` file
2. **Open** in Cisco Packet Tracer 8.x or compatible version
3. **Verify** all devices are powered on and connections established
4. **Review** the pre-configured settings before testing

### Initial Configuration Verification
```
! Check device status
show version
show ip interface brief
show running-config
```

## 🧪 Comprehensive Testing Procedures

### Connectivity Validation Tests

#### Intra-VLAN Communication
1. **Same VLAN, Same Switch**: Ping between PCs in same VLAN on same switch
2. **Same VLAN, Different Switches**: Verify cross-switch same VLAN connectivity
3. **VLAN Isolation**: Confirm devices in different VLANs cannot communicate directly

#### Inter-VLAN Routing Tests
1. **Management to User VLAN**: Verify administrative access capabilities
2. **User to Guest VLAN**: Confirm restriction policies are functioning
3. **Cross-section Routing**: Test communication between same-purpose VLANs across sections

#### OSPF Routing Verification
1. **Neighbor Adjacencies**: Confirm all routers establish OSPF relationships
2. **Route Propagation**: Verify routes appear in all routing tables
3. **Path Selection**: Test redundant path utilization during link failures

### Security Feature Validation

#### Port Security Testing
1. **Authorized Device**: Connect approved device - should function normally
2. **Unauthorized Device**: Connect different device - port should disable
3. **Recovery Procedure**: Test port recovery after security violation

#### ACL Policy Enforcement
1. **Guest Restrictions**: Attempt guest access to internal resources - should be blocked
2. **User Limitations**: Test user access to management segments - should be denied
3. **Management Privileges**: Verify full administrative access from management VLAN

#### SSH Access Control
1. **Management VLAN SSH**: Confirm successful SSH connections
2. **Non-Management VLAN SSH**: Verify connection attempts are rejected
3. **Telnet Attempts**: Confirm telnet is completely disabled network-wide

## 📊 Verification Command Reference

### Switch Verification Commands

#### VLAN Configuration
```bash
show vlan brief                    # VLAN membership and status
show vlan id [vlan-id]            # Detailed VLAN information
show interfaces switchport        # Switch port VLAN assignments
```

#### Trunk Configuration
```bash
show interfaces trunk              # Trunk status and allowed VLANs
show interfaces [interface] trunk  # Specific trunk details
```

#### Security Status
```bash
show port-security                 # Port security status summary
show port-security address         # Learned secure MAC addresses
show port-security interface [int] # Detailed port security info
show dhcp snooping                 # DHCP snooping binding table
```

### Router Verification Commands

#### OSPF Status
```bash
show ip ospf neighbor              # OSPF neighbor relationships
show ip ospf database              # OSPF link-state database
show ip ospf interface             # OSPF interface configurations
```

#### Routing Information
```bash
show ip route                      # Complete routing table
show ip route ospf                 # OSPF-learned routes only
show ip protocols                  # All routing protocol status
```

#### Security Policies
```bash
show access-lists                  # All configured ACLs
show ip interface                  # ACL application points
show ssh                           # SSH connection status
```

#### General Diagnostics
```bash
show ip interface brief           # Interface IP status
show running-config               # Current configuration
show logging                      # System log messages
```

*Note: This configuration represents enterprise best practices and should be adapted to specific organizational requirements and security policies before production deployment.*
