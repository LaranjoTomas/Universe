---
tags:
  - "#Security"
  - "#SRC"
  - "#CyberSecurity"
  - Networks
  - "#Communications"
---
> [!todo] TODO
> - [x] Analyze project requirements from PDF files
>- [x] Extract key network security concepts
>- [ ] Design network topology based on requirements
>- [ ] Identify required tools and equipment
>- [ ] Create security implementation plan
>- [ ] Generate visual network diagram
>- [ ] Compile comprehensive report
>- [ ] Deliver final documentation

## Project Requirements Analysis
- Network with redundant load-balancers and firewalls
- Specific network addressing scheme
- Security policies implementation
- Firewall and load-balancer placement
- Zone definition based on security policies
- Testing in GNS3 environment

## Policy
Traffic incoming to building A need tunnels since only a firewall between those 2 right most layer 3s arent enough. The tunnel must force traffic to redirect packets from vlan 10 and vlan20 to go to throught the firewalls. place firewall between router processers.

--- 
## Network Security Concepts for Communications Network Project
### Network Segmentation and Zones
Based on the project requirements, the following network zones need to be established:
1. **Internet Zone** - External untrusted network (100.0.0.0/24 for testing)
2. **DMZ (Demilitarized Zone)** - 200.0.0.0/24 for public-facing services
3. **Internal Datacenter** - 10.100.0.0/16 for internal services
4. **VLAN 10** - 10.10.0.0/24 for specific internal clients
5. **VLAN 20** - 10.20.0.0/24 for specific internal clients
6. **Internal Network** - 10.0.0.0/16 for internal connections
#### Services and Required Ports

The network must support the following services:
#### DMZ Services (Public-Facing)
- Web services (HTTPS UDP/TCP 443)
- Email (IMAP TCP 993 and SMTP TCP 25)
- DNS (UDP 53)
#### Internal Datacenter Services
- Intranet/Storage (TCP 443)
- Internal DNS (UDP 53)
- Databases (TCP 3306)
### Security Policies
The following security policies must be implemented:
1. **DDoS Protection** - The network should be able to handle high-rate DDoS attacks from the Internet
2. **Internet Access Control** - Internal devices may only access Internet services using ports TCP/UDP 80 and 443
3. **DMZ Access** - All DMZ services must be accessible from both the Internet and inside the network
4. **Intranet/Storage and Internal DNS Access** - May only be accessible by devices on VLAN 10 and 20
5. **Database Access** - Internal Databases may only be accessible by devices on VLAN 20
6. **Network Management** - A specific device on VLAN 1 (Building A) can PING and use SSH (port TCP 22) to access the console of all network devices
7. **Inter-VLAN Communication** - VLAN 10 devices may only use VoIP (SIP, port UDP 5060) to communicate directly with VLAN 20 devices, and vice-versa
### Firewall Concepts
From the documentation, the following firewall concepts are important:
1. **Zone-Based Firewall** - Defining security zones and applying policies between zones
2. **Stateful Inspection** - Using state tables to track established and related connections
3. **NAT/PAT** - Network Address Translation for internal-to-external communication
4. **Rule Chains** - Ordered sets of rules that determine traffic flow between zones
5. **High Availability** - Redundant firewall configurations for fault tolerance
### Load Balancing Concepts
The following load balancing concepts are important:
1. **Active-Active Configuration** - Multiple load balancers sharing traffic load
2. **State Synchronization** - Sharing connection state information between load balancers
3. **Health Monitoring** - Checking the health of backend servers
4. **Sticky Sessions** - Maintaining client connections to the same backend server
5. **Load Distribution Algorithms** - Methods for distributing traffic across multiple servers
### High Availability Concepts
The following high availability concepts are important:
1. **VRRP (Virtual Router Redundancy Protocol)** - Protocol for automatic assignment of available routers
2. **Connection State Synchronization** - Sharing connection state information between firewalls
3. **Failover Mechanisms** - Automatic switching to backup systems when primary systems fail
4. **Active-Active vs. Active-Passive** - Different redundancy configurations
### Authentication and Access Control
The following authentication concepts are important:
1. **802.1X** - Port-based Network Access Control
2. **RADIUS** - Remote Authentication Dial-In User Service for centralized authentication
3. **EAP (Extensible Authentication Protocol)** - Authentication framework used in wireless networks and point-to-point connections
### Network Monitoring and Management
The following monitoring concepts are important:
1. **Traffic Monitoring** - Using tools like Wireshark to analyze network traffic
2. **Logging** - Recording security events and authentication attempts
3. **Centralized Management** - Managing network devices from a central location

---
## Network Topology Design
### Overview

Based on the project requirements and security concepts, this document outlines the network topology design including the placement of firewalls, load balancers, and security zones.

### Network Addressing Scheme

- **Internet Testing Network**: 100.0.0.0/24
- **DMZ (Demilitarized Zone)**: 200.0.0.0/24
- **Internal Datacenter**: 10.100.0.0/16
- **VLAN 10**: 10.10.0.0/24
- **VLAN 20**: 10.20.0.0/24
- **Internal Connections**: 10.0.0.0/16

### Network Zones

1. **Internet Zone** 
   - External untrusted network
   - Connected to the external routers (Router E1 and Router E2)
   - Represents the public internet

1. **DMZ Zone**
   - Semi-trusted zone for public-facing services
   - Contains servers for Web (HTTPS), Email (IMAP/SMTP), and DNS services
   - Protected by redundant firewalls
   - Accessible from both Internet and internal networks

3. **Datacenter Zone** - Highly secured zone for internal services
   - Contains servers for Intranet/Storage, internal DNS, and Databases
   - Accessible only by authorized internal networks (VLAN 10 and VLAN 20)
   - Database servers only accessible by VLAN 20

4. **Internal Network Zone (Building A)** - Trusted internal network
   - Contains VLAN 10 and VLAN 20
   - VLAN 10 can access Intranet/Storage and internal DNS
   - VLAN 20 can access Intranet/Storage, internal DNS, and Databases
   - Inter-VLAN communication limited to VoIP (SIP, UDP 5060)
   - Management device in VLAN 1 can access all network devices via PING and SSH

### Firewall Placement

1. **Internet-DMZ Firewalls** (Redundant Pair)
   - Positioned between Internet routers (E1/E2) and DMZ switches
   - Configured in high-availability mode (Active-Active with state synchronization)
   - Implements security policies for Internet-to-DMZ traffic
   - Provides DDoS protection for incoming traffic
   - Allows access to DMZ services from Internet

1. **DMZ-Internal Firewalls** (Redundant Pair)
   - Positioned between DMZ switches and internal network switches
   - Configured in high-availability mode (Active-Active with state synchronization)
   - Implements security policies for DMZ-to-Internal traffic
   - Controls access from internal networks to DMZ services
   - Prevents unauthorized access from DMZ to internal networks
  
2. **Internal-Datacenter Firewalls** (Redundant Pair)
   - Positioned between internal network switches and datacenter switches
   - Configured in high-availability mode (Active-Active with state synchronization)
   - Implements security policies for Internal-to-Datacenter traffic
   - Controls access to Intranet/Storage, internal DNS, and Databases
   - Enforces VLAN-specific access restrictions
 
### Load Balancer Placement

1. **DMZ Load Balancers** (Redundant Pair)
   - Positioned in front of DMZ servers
   - Distributes traffic to multiple instances of Web, Email, and DNS servers
   - Configured in high-availability mode with state synchronization
   - Provides health monitoring for DMZ services
1. **Datacenter Load Balancers** (Redundant Pair)
   - Positioned in front of Datacenter servers
   - Distributes traffic to multiple instances of Intranet/Storage, internal DNS, and Database servers
   - Configured in high-availability mode with state synchronization
   - Provides health monitoring for Datacenter services
 
### Traffic Flow Paths
1. **Internet to DMZ Services**
   - Internet → Router E1/E2 → Internet-DMZ Firewalls → DMZ Load Balancers → DMZ Servers
1. **Internal to DMZ Services**
   - Internal Network → DMZ-Internal Firewalls → DMZ Load Balancers → DMZ Servers
2. **Internal to Internet**
   - Internal Network → DMZ-Internal Firewalls → Internet-DMZ Firewalls → Router E1/E2 → Internet
   - Limited to TCP/UDP ports 80 and 443
3. **VLAN 10 to Intranet/Storage and Internal DNS**
   - VLAN 10 → Internal-Datacenter Firewalls → Datacenter Load Balancers → Intranet/Storage and Internal DNS Servers
4. **VLAN 20 to Intranet/Storage, Internal DNS, and Databases**
   - VLAN 20 → Internal-Datacenter Firewalls → Datacenter Load Balancers → Intranet/Storage, Internal DNS, and Database Servers
4. **VLAN 10 to VLAN 20 (VoIP)**
   - Direct communication through internal switches using UDP port 5060 only
4. **Management Access**
   - Management device (VLAN 1) → All network devices via PING and SSH (TCP 22)
 
### High Availability Design

1. **Firewall High Availability**
   - All firewalls deployed in redundant pairs
   - Active-Active configuration with state synchronization
   - VRRP for virtual IP management
   - Conntrack-sync for connection state synchronization

1. **Load Balancer High Availability**
   - All load balancers deployed in redundant pairs
   - Active-Active configuration with state synchronization
   - Sticky connections for session persistence
   - Health monitoring for backend servers

---