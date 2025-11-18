# CCNA Course 3 - Module 11: Network Design

## การออกแบบเครือข่าย

---

## 11.1 Hierarchical Network Design

### What is Hierarchical Design?

**คำจำกัดความ:**

- **Hierarchical Network Design** = การออกแบบเครือข่ายแบบชั้นลำดับ
- แบ่งเครือข่ายเป็น layers (ชั้น)
- แต่ละชั้นมีหน้าที่เฉพาะ
- Modular, scalable, manageable

**Why Hierarchical Design:**

```
✅ Scalability - ขยายง่าย
✅ Redundancy - สำรอง, high availability
✅ Performance - จัดสรร resources เหมาะสม
✅ Security - จัดการ security ตาม layer
✅ Manageability - จัดการง่าย
✅ Maintainability - บำรุงรักษาง่าย
✅ Cost-effective - ใช้อุปกรณ์เหมาะสมแต่ละชั้น
```

---

### Three-Tier Hierarchical Model

**3 Layers:**

```
┌──────────────────────────────────────┐
│         Core Layer                   │  ← High-speed backbone
│  (High-speed switching/routing)      │
└──────────────────────────────────────┘
              ↕ ↕ ↕
┌──────────────────────────────────────┐
│      Distribution Layer              │  ← Policy, aggregation
│  (Routing, filtering, QoS)           │
└──────────────────────────────────────┘
         ↕      ↕      ↕      ↕
┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐
│Access│ │Access│ │Access│ │Access│  ← End devices
│Layer │ │Layer │ │Layer │ │Layer │
└─────┘ └─────┘ └─────┘ └─────┘
   ↕       ↕       ↕       ↕
[Users] [Users] [Users] [Users]
```

---

### Access Layer

**Purpose:**

```
- เชื่อมต่อ end devices (PCs, phones, printers)
- Grant network access
- Port security, VLANs
- PoE (Power over Ethernet)
```

**Characteristics:**

```
Devices:     Layer 2 switches (access switches)
Density:     High port density (24/48 ports)
Speed:       1 Gbps (access ports), 10 Gbps (uplinks)
Features:    Port security, DHCP snooping, DAI, VLANs, PoE
Redundancy:  Dual uplinks to distribution layer
```

**Functions:**

```
✅ Connect end devices
✅ VLAN assignment
✅ Port security (MAC filtering)
✅ 802.1X authentication
✅ Spanning Tree (prevent loops)
✅ PoE (IP phones, APs)
✅ QoS trust boundary
```

**Example Devices:**

```
- Cisco Catalyst 2960 series
- Cisco Catalyst 9200 series
- Cisco Catalyst 3650 series (stackable)
```

---

### Distribution Layer

**Purpose:**

```
- Aggregate access layer switches
- Routing between VLANs
- Policy enforcement (ACLs, QoS)
- Redundancy
```

**Characteristics:**

```
Devices:     Layer 3 switches or routers
Speed:       10 Gbps (to access), 10/40 Gbps (to core)
Features:    Inter-VLAN routing, ACLs, QoS, redundancy protocols
Redundancy:  Dual distribution switches (HSRP/VRRP)
```

**Functions:**

```
✅ Aggregate access layer
✅ Inter-VLAN routing (Layer 3)
✅ Policy enforcement:
   - ACLs (security)
   - QoS (traffic prioritization)
   - Route filtering
✅ Redundancy (HSRP, VRRP, GLBP)
✅ Route summarization
✅ Broadcast domain boundary
```

**Example Devices:**

```
- Cisco Catalyst 3850 series
- Cisco Catalyst 9300 series
- Cisco Catalyst 6500 series (large campus)
```

---

### Core Layer

**Purpose:**

```
- High-speed backbone
- Forward traffic quickly
- Connect distribution layers
- Minimal latency
```

**Characteristics:**

```
Devices:     High-end Layer 3 switches or routers
Speed:       40/100 Gbps
Features:    High throughput, redundancy, low latency
Redundancy:  Full redundancy, dual core devices
```

**Functions:**

```
✅ Fast packet switching
✅ Connect distribution layers
✅ Redundant paths
✅ Minimal latency
✅ No policy processing (keep it simple)
✅ Load balancing
```

**Design Principles:**

```
❌ NO access layer connections (only distribution)
❌ NO ACLs (adds latency)
❌ NO packet manipulation
✅ Keep it simple and fast
✅ High availability
✅ Redundancy
```

**Example Devices:**

```
- Cisco Catalyst 6500 series
- Cisco Catalyst 9400/9500 series
- Cisco Nexus 7000 series (data center)
- Cisco ASR series routers (WAN edge)
```

---

### Layer Comparison

```
Layer         Access              Distribution         Core
------------------------------------------------------------------------
Purpose       Connect users       Aggregation          Backbone
Device        L2 switch           L3 switch            L3 switch/router
Speed         1 Gbps              10 Gbps              40/100 Gbps
Ports         24/48               24/48                Fewer, faster
Features      VLAN, PoE,          Routing, ACL,        Speed, redundancy
              Security            QoS, redundancy
Complexity    Low                 Medium               High
Cost          Low                 Medium               High
------------------------------------------------------------------------
```

---

### Two-Tier Model (Collapsed Core)

**When to Use:**

```
Small to medium networks
Limited budget
Fewer users
```

**Design:**

```
┌────────────────────────────────────┐
│  Core/Distribution Layer           │  ← Combined layer
│  (Routing, switching, policies)    │
└────────────────────────────────────┘
         ↕      ↕      ↕
    ┌─────┐ ┌─────┐ ┌─────┐
    │Access│ │Access│ │Access│  ← Access switches
    │Layer │ │Layer │ │Layer │
    └─────┘ └─────┘ └─────┘
       ↕       ↕       ↕
    [Users] [Users] [Users]

Distribution and Core functions combined into one layer
```

**Characteristics:**

```
✅ Cost-effective (fewer devices)
✅ Simpler (fewer layers)
✅ Suitable for small networks
❌ Less scalable
❌ Single point of congestion
```

---

## 11.2 Scalable Networks

### Design Principles for Scalability

**1. Hierarchical Design:**

```
✅ Use 3-tier model (large networks)
✅ Use 2-tier model (small/medium networks)
✅ Clear separation of functions
```

**2. Modularity:**

```
✅ Design in modules/blocks
✅ Easy to add new blocks
✅ Independent modules

Example:
  Building 1 block: Access + Distribution
  Building 2 block: Access + Distribution
  Building 3 block: Access + Distribution
  Core connects all buildings
```

**3. Resiliency:**

```
✅ Redundancy at all layers
✅ No single point of failure
✅ Fast convergence (FHRP, routing protocols)
✅ Backup links

Example:
  - Dual distribution switches
  - Dual uplinks from access
  - Dual core switches
  - Redundant power supplies
```

**4. Flexibility:**

```
✅ Support new technologies (easy upgrades)
✅ Support new applications
✅ Support growth
✅ Wireless, VoIP, video-ready
```

---

### Network Diameter

**คำจำกัดความ:**

```
- Number of hops between farthest devices
- ยิ่งน้อยยิ่งดี (lower latency)
```

**Best Practices:**

```
✅ Minimize hops
✅ Flat network (fewer layers) where possible
✅ Use Layer 3 at distribution (routing, not spanning tree)

Example:
  3-tier: User → Access → Distribution → Core → Distribution → Access → Server
          (5 hops)
  
  Optimized: User → Access → Core → Access → Server
             (3 hops)
```

---

### Bandwidth Aggregation

**Purpose:**

```
- Combine multiple links
- Increase bandwidth
- Redundancy
```

**Technologies:**

**EtherChannel:**

```
- Bundle multiple physical links into one logical link
- Up to 8 links
- Increased bandwidth, redundancy
- Load balancing

Example:
  Access switch → 4 × 1 Gbps links → Distribution
  = 4 Gbps aggregate bandwidth
  = Redundancy (if one fails, others continue)
```

**Oversubscription:**

```
- More bandwidth at access than uplink (acceptable)
- Ratio: Access bandwidth / Uplink bandwidth

Example:
  24 × 1 Gbps access ports
  2 × 10 Gbps uplinks
  Ratio: 24 Gbps / 20 Gbps = 1.2:1 (good)
  
Acceptable ratios:
  Access to Distribution: 20:1 or better
  Distribution to Core: 4:1 or better
```

---

### Redundancy and High Availability

**Redundancy Types:**

**1. Device Redundancy:**

```
✅ Dual distribution switches
✅ Dual core switches
✅ Redundant power supplies
✅ Redundant supervisor modules (chassis switches)
```

**2. Link Redundancy:**

```
✅ Multiple uplinks (EtherChannel)
✅ Alternate paths
✅ Spanning Tree (prevents loops)
```

**3. Path Redundancy:**

```
✅ Multiple routes to destination
✅ Dynamic routing protocols (OSPF, EIGRP)
✅ Fast convergence
```

**4. Module Redundancy:**

```
✅ Spare line cards
✅ Hot-swappable components
```

---

### First Hop Redundancy Protocols (FHRP)

**Purpose:**

```
- Redundant default gateways
- Automatic failover
- Transparent to end devices
```

**Protocols:**

**HSRP (Hot Standby Router Protocol):**

```
- Cisco proprietary
- Active/Standby routers
- Virtual IP address
- Default: Hello 3s, Hold 10s

Example:
  Router A: 192.168.1.2 (Active)
  Router B: 192.168.1.3 (Standby)
  Virtual IP: 192.168.1.1 (used by clients)
  
  If Router A fails → Router B becomes active
```

**VRRP (Virtual Router Redundancy Protocol):**

```
- Industry standard (RFC 5798)
- Master/Backup routers
- Similar to HSRP
```

**GLBP (Gateway Load Balancing Protocol):**

```
- Cisco proprietary
- Load balancing (multiple active routers)
- AVG (Active Virtual Gateway) + AVFs (Active Virtual Forwarders)
```

---

## 11.3 Switch Hardware

### Switch Form Factors

**1. Fixed Configuration:**

```
- Fixed number of ports
- Cannot add/remove modules
- Cost-effective
- Small to medium deployments

Examples:
  - Catalyst 2960 (24/48 ports)
  - Catalyst 9200 (24/48 ports)

Pros:
  ✅ Simple
  ✅ Low cost
  ✅ Plug and play

Cons:
  ❌ Cannot expand
  ❌ Limited flexibility
```

---

**2. Modular:**

```
- Chassis-based
- Add/remove line cards
- Scalable, flexible
- Large deployments

Examples:
  - Catalyst 6500 series
  - Catalyst 9400 series
  - Nexus 7000 series

Components:
  - Chassis (frame)
  - Supervisor module (control plane)
  - Line cards (ports)
  - Power supplies
  - Fans

Pros:
  ✅ Scalable (add modules)
  ✅ Flexible (different module types)
  ✅ High availability (redundant supervisors)
  ✅ Hot-swappable modules

Cons:
  ❌ Expensive
  ❌ Complex
  ❌ Large physical size
```

---

**3. Stackable:**

```
- Multiple switches stacked
- Managed as single switch
- Shared backplane (stacking cables)
- Medium to large deployments

Examples:
  - Catalyst 3850 series
  - Catalyst 9300 series

Pros:
  ✅ Scalable (add switches)
  ✅ Single management IP
  ✅ High bandwidth (stacking cables)
  ✅ Redundancy (master/standby)
  ✅ Cost-effective vs modular

Cons:
  ❌ Limited to ~8-10 switches per stack
  ❌ Requires stacking cables/modules
```

---

### Switch Port Density

**คำจำกัดความ:**

```
- Number of ports on switch
- Higher density = more devices per switch
```

**Common Densities:**

```
Access switches:    24 or 48 ports
Distribution:       24 or 48 ports (+ uplinks)
Core:               Fewer ports, higher speed

Planning:
  Calculate required ports:
    Users + IP phones + APs + printers + servers + growth
  
  Example:
    50 users × 2 ports (PC + phone) = 100 ports
    Add 20% growth = 120 ports
    Need: 3 × 48-port switches (144 ports)
```

---

### Forwarding Rates

**คำจำกัดความ:**

```
- Speed ที่ switch forwards frames
- วัดเป็น packets per second (pps) or frames per second (fps)
- สูงกว่า = ดีกว่า
```

**Calculation:**

```
Formula:
  Forwarding rate = (Port speed × Number of ports) / Frame size

Example (48-port Gigabit switch):
  Line rate = 48 × 1 Gbps = 48 Gbps
  
  Frame size: 64 bytes (minimum Ethernet frame)
  = (64 bytes × 8 bits) + 20 bytes overhead = 672 bits/frame
  
  Forwarding rate = (48 Gbps × 10^9) / 672 bits
                  = 71,428,571 fps
                  ≈ 71.4 Mpps (million packets per second)

Wire speed: Switch can forward at full line rate (no bottleneck)
```

**Importance:**

```
✅ Ensure switch can handle full traffic
✅ No dropped frames due to processing limitations
✅ Look for "wire speed" or "line rate" capability
```

---

### Power over Ethernet (PoE)

**Purpose:**

```
- Deliver power and data over Ethernet cable
- Eliminate need for separate power adapters
- Simplify deployment
```

**PoE Standards:**

```
Standard    Power          Use Case
------------------------------------------------------------------------
PoE         15.4W (actual: IP phones, basic APs, cameras
(802.3af)   12.95W)

PoE+        30W (actual:   High-power APs, PTZ cameras,
(802.3at)   25.5W)         video phones

UPoE        60W            High-end APs, thin clients
(Cisco)

PoE++       Up to 90W      Laptops, displays, high-power
(802.3bt)   (Type 4)       devices
------------------------------------------------------------------------
```

**PoE Power Budget:**

```
Switch PoE capacity vs. required power

Example:
  Switch: 48 ports, 740W PoE budget
  Devices: 40 × IP phones (7W each) = 280W
           8 × APs (20W each) = 160W
  Total: 440W (within budget ✅)
  
  If over budget → Devices may not power on
```

**Configuration:**

```
! Enable PoE on interface
Switch(config)# interface gigabitEthernet 0/1
Switch(config-if)# power inline auto

! Verify PoE
Switch# show power inline
Interface   Admin  Oper  Power   Device              Class
---------------------------------------------------------------------
Gi0/1       auto   on    15.4    IP Phone 7841       2
Gi0/2       auto   on    20.0    Cisco AP            4

! Check PoE budget
Switch# show power inline
Available: 740W  Used: 440W  Remaining: 300W
```

---

## 11.4 Router Hardware

### Router Form Factors

**1. Branch Routers:**

```
- Small offices, branches
- Fixed configuration
- Integrated services (firewall, VPN)

Examples:
  - Cisco ISR 1000 series (small branch)
  - Cisco ISR 4000 series (medium branch)

Features:
  - WAN connectivity (Ethernet, DSL, 4G/5G)
  - Firewall
  - VPN
  - VoIP gateway
```

---

**2. Edge Routers:**

```
- Enterprise edge (WAN connection)
- Internet gateway
- High performance

Examples:
  - Cisco ASR 1000 series
  - Cisco ASR 9000 series

Features:
  - High throughput (Gbps)
  - Carrier-class
  - Redundancy
  - Advanced services
```

---

**3. Service Provider Routers:**

```
- Core/edge of service provider network
- Very high performance (Tbps)
- Massive scalability

Examples:
  - Cisco CRS series
  - Cisco NCS series

Features:
  - Terabit switching
  - Carrier Ethernet
  - MPLS
```

---

### Router Interfaces

**WAN Interfaces:**

```
- Serial (legacy)
- T1/E1
- DSL
- Cable (DOCSIS)
- Ethernet (Metro Ethernet)
- Cellular (4G/5G)
```

**LAN Interfaces:**

```
- Ethernet (RJ-45)
- Gigabit Ethernet
- 10 Gigabit Ethernet
- SFP/SFP+ (fiber)
```

**Management Interfaces:**

```
- Console (RJ-45 or USB)
- Auxiliary (dial-in, legacy)
- Management Ethernet (out-of-band)
```

---

## 11.5 Network Design Best Practices

### Security Best Practices

**1. Defense in Depth:**

```
Multiple layers of security:
  ✅ Perimeter (firewall)
  ✅ Network (ACLs, VLANs)
  ✅ Host (antivirus, personal firewall)
  ✅ Application (authentication, authorization)
  ✅ Data (encryption)
```

**2. Access Layer Security:**

```
✅ Port security (MAC limiting)
✅ DHCP snooping
✅ Dynamic ARP Inspection (DAI)
✅ 802.1X authentication
✅ VLAN segmentation
```

**3. Distribution Layer Security:**

```
✅ ACLs (filter traffic between VLANs)
✅ Private VLANs
✅ VLAN ACLs (VACLs)
```

**4. Management Security:**

```
✅ SSH (not Telnet)
✅ HTTPS (not HTTP)
✅ SNMPv3 (not v1/v2c)
✅ AAA (RADIUS/TACACS+)
✅ Disable CDP/LLDP on untrusted interfaces
```

---

### Manageability Best Practices

**1. Documentation:**

```
✅ Network diagrams (physical, logical)
✅ IP addressing scheme
✅ VLAN assignments
✅ Configuration standards
✅ Change logs
```

**2. Naming Conventions:**

```
✅ Consistent device names
✅ Meaningful interface descriptions
✅ VLAN naming

Example:
  Hostname: BKK-B1-SW-01 (Location-Building-Type-Number)
  Interface: "User VLAN 10 - Building 1 Floor 2"
  VLAN 10: "Users"
```

**3. IP Addressing:**

```
✅ Structured addressing plan
✅ Summarizable (VLSM)
✅ Reserved addresses (network, broadcast, gateway)

Example:
  Building 1: 10.1.0.0/16
    Floor 1: 10.1.1.0/24 (VLAN 11)
    Floor 2: 10.1.2.0/24 (VLAN 12)
  Building 2: 10.2.0.0/16
    Floor 1: 10.2.1.0/24 (VLAN 21)
```

**4. Configuration Management:**

```
✅ Configuration backups (regular)
✅ Version control
✅ Configuration templates
✅ Automated deployment (Ansible, Puppet)
```

**5. Monitoring:**

```
✅ SNMP monitoring (CPU, memory, interfaces)
✅ Syslog (centralized logging)
✅ NetFlow (traffic analysis)
✅ Alerts (thresholds)
```

---

### Performance Best Practices

**1. Bandwidth Management:**

```
✅ QoS (prioritize critical traffic)
✅ Traffic shaping (prevent congestion)
✅ Policing (enforce limits)
```

**2. Minimize Broadcast Domains:**

```
✅ Use VLANs (segment networks)
✅ Route between VLANs (Layer 3)
✅ Limit broadcast traffic
```

**3. Optimize Routing:**

```
✅ Use dynamic routing protocols (OSPF, EIGRP)
✅ Route summarization (reduce routing table)
✅ Load balancing (ECMP)
```

**4. Use Appropriate Hardware:**

```
✅ Match device capability to role:
  - Access: Layer 2 switches (cost-effective)
  - Distribution: Layer 3 switches (routing, policies)
  - Core: High-end switches (speed, redundancy)
```

---

### Availability Best Practices

**1. Redundancy:**

```
✅ Dual devices (distribution, core)
✅ Dual uplinks (EtherChannel)
✅ Dual power supplies
✅ Dual WAN connections (ISPs)
```

**2. Fast Convergence:**

```
✅ FHRP (HSRP, VRRP, GLBP) - subsecond failover
✅ Rapid PVST+ or MST (fast spanning tree)
✅ OSPF/EIGRP timers tuning
✅ BFD (Bidirectional Forwarding Detection)
```

**3. High Availability Features:**

```
✅ NSF (Non-Stop Forwarding)
✅ SSO (Stateful Switchover)
✅ Redundant supervisors (chassis switches)
✅ Hot-standby routing
```

**4. Disaster Recovery:**

```
✅ Configuration backups (offsite)
✅ Spare devices
✅ Documented procedures
✅ Regular testing
```

---

## Summary (สรุป)

Module 11 นี้เราได้เรียนรู้:

1. ✅ **Hierarchical Network Design**:
    
    - **Access Layer**: Connect end devices, VLANs, port security
    - **Distribution Layer**: Aggregation, routing, policies (ACLs, QoS)
    - **Core Layer**: High-speed backbone, minimal latency
    - **Two-Tier Model**: Collapsed core (small networks)
2. ✅ **Scalable Networks**:
    
    - Hierarchical design
    - Modularity (blocks/modules)
    - Resiliency (redundancy)
    - Minimize network diameter
    - Bandwidth aggregation (EtherChannel)
    - Proper oversubscription ratios
3. ✅ **Switch Hardware**:
    
    - **Fixed**: Simple, low cost, cannot expand
    - **Modular**: Chassis-based, scalable, expensive
    - **Stackable**: Multiple switches as one, flexible
    - Port density planning
    - Forwarding rates (wire speed)
    - PoE standards (802.3af, 802.3at, 802.3bt)
4. ✅ **Router Hardware**:
    
    - Branch routers (ISR)
    - Edge routers (ASR)
    - Service provider routers
    - WAN/LAN interfaces
5. ✅ **Best Practices**:
    
    - Security: Defense in depth, port security, ACLs
    - Manageability: Documentation, naming, monitoring
    - Performance: QoS, VLAN segmentation, routing optimization
    - Availability: Redundancy, FHRP, fast convergence

**สิ่งสำคัญที่ต้องจำ:**

- **3-Tier Model**: Access → Distribution → Core
- **2-Tier Model**: Access → Collapsed Core/Distribution
- **Access Layer**: L2 switches, connect users, VLANs, PoE
- **Distribution Layer**: L3 switches, routing, ACLs, QoS
- **Core Layer**: Fast switching, no policies, redundancy
- **FHRP**: HSRP (Cisco), VRRP (standard), GLBP (load balancing)
- **Switch Types**: Fixed (simple), Modular (scalable), Stackable (flexible)
- **PoE**: 802.3af (15.4W), 802.3at (30W), 802.3bt (90W)
- **Redundancy**: Devices, links, paths
- **Oversubscription**: Access-Distribution 20:1, Distribution-Core 4:1

**Design Checklist:**

```
Planning:
  ☐ Requirements analysis (users, apps, growth)
  ☐ IP addressing scheme
  ☐ VLAN design
  ☐ Hierarchical model (2-tier or 3-tier)

Access Layer:
  ☐ Port density calculation
  ☐ PoE requirements
  ☐ Port security
  ☐ VLAN assignment
  ☐ Dual uplinks

Distribution Layer:
  ☐ Inter-VLAN routing
  ☐ ACLs
  ☐ QoS policies
  ☐ Redundancy (dual switches, FHRP)
  ☐ Route summarization

Core Layer (if 3-tier):
  ☐ High-speed links (10/40/100G)
  ☐ Full redundancy
  ☐ Minimal configuration
  ☐ Fast convergence

Security:
  ☐ Defense in depth
  ☐ Access control (802.1X)
  ☐ Management security (SSH, SNMPv3)

Availability:
  ☐ Device redundancy
  ☐ Link redundancy (EtherChannel)
  ☐ FHRP (HSRP/VRRP)
  ☐ Backup/restore procedures

Monitoring:
  ☐ SNMP monitoring
  ☐ Syslog server
  ☐ NetFlow
  ☐ Documentation
```

**Next Module:** Module 12 - Network Troubleshooting

---

**[ไฟล์ Module 11 สมบูรณ์แล้ว!]**