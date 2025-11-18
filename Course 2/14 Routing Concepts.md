# CCNA 2: Module 14 - Routing Concepts

## แนวคิดเรื่อง Routing

---

## สารบัญ

1. [วัตถุประสงค์ของ Module](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#%E0%B8%A7%E0%B8%B1%E0%B8%95%E0%B8%96%E0%B8%B8%E0%B8%9B%E0%B8%A3%E0%B8%B0%E0%B8%AA%E0%B8%87%E0%B8%84%E0%B9%8C%E0%B8%82%E0%B8%AD%E0%B8%87-module)
2. [Path Determination](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#1-path-determination)
3. [Packet Forwarding](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#2-packet-forwarding)
4. [Basic Router Configuration Review](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#3-basic-router-configuration-review)
5. [IP Routing Table](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#4-ip-routing-table)
6. [Static and Dynamic Routing](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#5-static-and-dynamic-routing)
7. [สรุป](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#%E0%B8%AA%E0%B8%A3%E0%B8%B8%E0%B8%9B)

---

## วัตถุประสงค์ของ Module

เมื่อจบ Module นี้ คุณจะสามารถ:

- ✅ อธิบายวิธี routers กำหนด best path
- ✅ อธิบายวิธี routers forward packets ไปยัง destination
- ✅ Configure basic settings บน router
- ✅ อธิบายโครงสร้างของ routing table
- ✅ เปรียบเทียบ static และ dynamic routing concepts

---

## 1. Path Determination

### การกำหนดเส้นทาง

### 1.1 Router Functions

**Primary Router Functions:**

```
1. Path Determination (Best Path Selection)
   - ตัดสินใจว่า packet ควรส่งไปทางไหน
   - ใช้ข้อมูลใน routing table
   - เลือก best path จาก available routes

2. Packet Forwarding
   - ส่ง packet ออกไปที่ outgoing interface ที่เหมาะสม
   - Encapsulate ใน appropriate data link frame
   - ส่งไปยัง next-hop router หรือ destination
```

**Router vs Switch:**

```
Switch (Layer 2):
- เชื่อมต่อ devices ใน same network
- Forward frames based on MAC address
- Operates in one broadcast domain
- Simpler operation

Router (Layer 3):
- เชื่อมต่อ multiple networks
- Forward packets based on IP address
- Separates broadcast domains
- More intelligent path selection

Key Difference:
- Switch: Connect devices within network
- Router: Connect networks together
```

### 1.2 Router Packet Forwarding Decision

**Router Operation Process:**

```
Step 1: Receive Packet
Router receives packet on incoming interface

Step 2: De-encapsulate
Remove Layer 2 frame header
Examine Layer 3 packet (IP header)
Read destination IP address

Step 3: Routing Table Lookup
Compare destination IP with routing table
Find matching route (longest prefix match)

Step 4: Determine Next Hop
Identify:
- Outgoing interface
- Next-hop IP address (if applicable)

Step 5: Encapsulate
Encapsulate packet in new Layer 2 frame
Appropriate for outgoing interface type

Step 6: Forward
Send frame out outgoing interface
```

**Example Scenario:**

```
Topology:
PC1 (10.1.1.10) → [R1] → [R2] → PC2 (10.3.3.10)
              G0/0/0   S0/0/0 S0/0/1   G0/0/0
              10.1.1.1        10.2.2.1-2        10.3.3.1

PC1 sends packet to PC2:

At PC1:
- Source IP: 10.1.1.10
- Dest IP: 10.3.3.10
- Dest MAC: R1's G0/0/0 MAC (default gateway)

At R1:
1. Receives Ethernet frame on G0/0/0
2. Strips Ethernet header
3. Checks dest IP: 10.3.3.10
4. Looks up routing table
5. Finds route via S0/0/0
6. Encapsulates in HDLC/PPP frame
7. Forwards out S0/0/0 to R2

At R2:
1. Receives serial frame on S0/0/1
2. Strips serial header
3. Checks dest IP: 10.3.3.10
4. Looks up routing table
5. Finds directly connected via G0/0/0
6. Encapsulates in Ethernet frame
7. Forwards out G0/0/0 to PC2

Key Point:
Layer 3 packet (IP) stays same
Layer 2 frame changes at each hop
```

### 1.3 Routing Table Basics

**What is a Routing Table?**

```
Definition:
Data structure stored in RAM
Contains network paths and directions
Used to determine where to forward packets

Purpose:
- Store best paths to destinations
- Provide next-hop information
- Enable packet forwarding decisions

Location: Router's RAM
Updated by:
- Directly connected networks (automatic)
- Static routes (manual)
- Dynamic routing protocols (automatic)
```

**Three Types of Routes:**

**1. Directly Connected Networks (C)**

```
คืออะไร:
- Networks directly attached to router interfaces
- Automatically added when:
  * Interface configured with IP
  * Interface is active (up/up)

Example:
Interface GigabitEthernet0/0/0
ip address 10.1.1.1 255.255.255.0
no shutdown

Routing Table Entry:
C    10.1.1.0/24 is directly connected, GigabitEthernet0/0/0

Benefits:
- Automatic (no configuration needed)
- Always accurate
- Zero administrative distance
```

**2. Remote Networks**

```
คืออะไร:
- Networks NOT directly connected
- Must be learned in one of two ways:
  a) Static Routes (manual)
  b) Dynamic Routing Protocols (automatic)

Example:
Remote network: 10.3.3.0/24
Location: Behind another router
Must configure path to reach it

Methods:
Static: ip route 10.3.3.0 255.255.255.0 10.2.2.2
Dynamic: Enable OSPF, EIGRP, etc.
```

**3. Default Route (Gateway of Last Resort)**

```
คืออะไร:
- Route used when no specific match
- "Catch-all" route
- Typically points to ISP

Notation: 0.0.0.0/0 or ::/0

Purpose:
- Forward unknown destinations
- Reduce routing table size
- Common in edge routers

Configuration:
IPv4: ip route 0.0.0.0 0.0.0.0 <next-hop>
IPv6: ipv6 route ::/0 <next-hop>

Example:
ip route 0.0.0.0 0.0.0.0 192.168.1.1

Routing Table:
S*   0.0.0.0/0 [1/0] via 192.168.1.1

* = Candidate default route
```

### 1.4 Best Path = Longest Match

**Longest Prefix Match Rule:**

```
Definition:
When multiple routes match a destination,
router selects route with LONGEST prefix
(most specific route)

Why Longest?
- More specific = better accuracy
- Ensures packets take optimal path
- Prevents incorrect routing

Process:
1. Compare destination IP with all routes
2. Find all matching routes
3. Select route with longest subnet mask
4. Forward packet accordingly
```

**Example 1: Simple Longest Match:**

```
Routing Table:
1. 192.168.0.0/16    via 10.1.1.2
2. 192.168.20.0/24   via 10.1.1.3
3. 192.168.20.0/27   via 10.1.1.4

Packet Destination: 192.168.20.25

Matching Routes:
- 192.168.0.0/16     ✓ matches (16 bits)
- 192.168.20.0/24    ✓ matches (24 bits)
- 192.168.20.0/27    ✓ matches (27 bits)

Best Match: 192.168.20.0/27 (longest prefix = 27 bits)
Next Hop: 10.1.1.4

Why?
/27 is most specific (covers 192.168.20.0-31)
/24 covers 192.168.20.0-255 (less specific)
/16 covers entire 192.168.x.x (least specific)
```

**Example 2: Binary Comparison:**

```
Destination IP: 192.168.20.50

Binary: 11000000.10101000.00010100.00110010

Routes:
1. 192.168.20.0/24
   Network: 11000000.10101000.00010100.00000000
   Mask:    11111111.11111111.11111111.00000000
   Match:   First 24 bits ✓

2. 192.168.20.32/27
   Network: 11000000.10101000.00010100.00100000
   Mask:    11111111.11111111.11111111.11100000
   Match:   First 27 bits ✓

3. 192.168.20.0/16
   Network: 11000000.10101000.00000000.00000000
   Mask:    11111111.11111111.00000000.00000000
   Match:   First 16 bits ✓

Selection:
Route 2 (192.168.20.32/27) = Longest match (27 bits)
```

**Example 3: Multiple Overlapping Routes:**

```
Routing Table:
10.0.0.0/8           via 10.1.1.1
10.10.0.0/16         via 10.1.1.2
10.10.10.0/24        via 10.1.1.3
10.10.10.128/25      via 10.1.1.4

Packet to: 10.10.10.150

Check each route:
10.0.0.0/8           ✓ (8 bits match)
10.10.0.0/16         ✓ (16 bits match)
10.10.10.0/24        ✓ (24 bits match)
10.10.10.128/25      ✓ (25 bits match)

Best: 10.10.10.128/25 via 10.1.1.4

Packet to: 10.10.10.50

Check each route:
10.0.0.0/8           ✓ (8 bits match)
10.10.0.0/16         ✓ (16 bits match)
10.10.10.0/24        ✓ (24 bits match)
10.10.10.128/25      ✗ (doesn't match - 50 < 128)

Best: 10.10.10.0/24 via 10.1.1.3
```

### 1.5 Routing Table Principles

**Three Routing Table Principles:**

**Principle 1: Every Router Makes Independent Decision**

```
คืออะไร:
- Each router decides based on its own routing table
- Doesn't know entire path
- Only knows next hop

Example:
PC1 → R1 → R2 → R3 → PC2

R1 only knows: Send to R2
R2 only knows: Send to R3
R3 only knows: Directly connected to PC2

No router knows complete path!

Implication:
- Each router must have correct routes
- Routing loops possible if misconfigured
- Importance of consistent routing information
```

**Principle 2: Routing Table Contains Next-Hop Information Only**

```
คืออะไร:
- Router only knows next hop
- Doesn't know complete path to destination
- Doesn't know what's beyond next hop

Example:
R1's routing table:
10.3.3.0/24 via 10.2.2.2

R1 knows:
✓ To reach 10.3.3.0/24, send to 10.2.2.2
✗ How 10.2.2.2 reaches 10.3.3.0/24
✗ How many hops to 10.3.3.0/24
✗ What routers are in between

Trust:
Router trusts next-hop router knows how to forward
```

**Principle 3: Routing Information May Not Provide Complete Path**

```
คืออะไร:
- Routing table shows best next hop
- May not show all possible paths
- May not show backup paths

Example:
Actual topology:
     R2
    /  \
R1 —    — R4
    \  /
     R3

R1's routing table may only show:
10.4.4.0/24 via R2

But path via R3 also exists (not in table)

Why?
- Only best path installed (by default)
- Load balancing: ECMP (Equal Cost Multi-Path)
  can install multiple equal routes
```

---

## 2. Packet Forwarding

### การส่ง Packet

### 2.1 Packet Forwarding Mechanisms

**Three Forwarding Mechanisms:**

**1. Process Switching (Oldest)**

```
คืออะไร:
- CPU examines every packet
- Looks up routing table for each packet
- Slowest method

Process:
1. Packet arrives
2. Interrupt CPU
3. CPU looks up destination in routing table
4. CPU determines outgoing interface
5. CPU forwards packet

Performance:
Speed: Very slow
CPU Usage: High
Use: Legacy, not used today

Disadvantage:
- Not scalable
- High CPU utilization
- Slow forwarding
```

**2. Fast Switching**

```
คืออะไร:
- Use cache for subsequent packets
- First packet: process switched
- Subsequent packets: use cache

Process:
1. First packet to destination:
   - Process switched
   - Create cache entry

2. Subsequent packets:
   - Check cache
   - If match: forward immediately
   - If no match: process switch

Performance:
Speed: Fast (for cached flows)
CPU Usage: Medium
Use: Older routers

Advantage:
✓ Faster than process switching
✓ Reduced CPU load
```

**3. Cisco Express Forwarding (CEF) - Current**

```
คืออะไร:
- Pre-built forwarding table (FIB)
- Fastest method
- Default on modern Cisco routers

Components:
a) FIB (Forwarding Information Base)
   - Copy of routing table
   - Optimized for forwarding
   - Pre-built, not on-demand

b) Adjacency Table
   - Layer 2 next-hop information
   - MAC addresses cached
   - No ARP lookup needed

Process:
1. Packet arrives
2. Check FIB (one lookup)
3. Check Adjacency Table
4. Forward immediately

Performance:
Speed: Fastest
CPU Usage: Low
Use: Default on modern routers

Advantages:
✓ Consistent forwarding performance
✓ Low CPU usage
✓ Scalable
✓ Pre-built tables (no cache delays)

Command to verify:
R1# show ip cef
```

### 2.2 Packet Forwarding Process Details

**Step-by-Step Forwarding:**

```
Scenario:
PC1 (172.16.1.10) wants to send to PC2 (172.16.3.10)
via R1, R2

Step 1: PC1 creates packet
Source IP: 172.16.1.10
Dest IP: 172.16.3.10
Dest not in local subnet → send to default gateway

Step 2: PC1 encapsulates in Ethernet frame
Source MAC: PC1's MAC
Dest MAC: R1's G0/0/0 MAC (default gateway)
Sends frame to R1

Step 3: R1 receives frame on G0/0/0
- Checks dest MAC (its own) → accept
- De-encapsulates (removes Ethernet header)
- Examines IP header
- Dest IP: 172.16.3.10

Step 4: R1 routing decision
- Looks up 172.16.3.10 in routing table
- Finds: 172.16.3.0/24 via 172.16.2.2 out S0/0/0
- Next hop: R2 (172.16.2.2)
- Outgoing interface: S0/0/0 (Serial)

Step 5: R1 encapsulates packet
- Encapsulate in serial frame (HDLC/PPP)
- No MAC addresses (point-to-point)
- Source: R1's S0/0/0
- Dest: R2's S0/0/1

Step 6: R1 forwards frame out S0/0/0

Step 7: R2 receives frame on S0/0/1
- De-encapsulates serial frame
- Examines IP header
- Dest IP: 172.16.3.10

Step 8: R2 routing decision
- Looks up 172.16.3.10 in routing table
- Finds: 172.16.3.0/24 directly connected on G0/0/1
- Destination is local!

Step 9: R2 checks ARP table
- Does R2 have MAC for 172.16.3.10?
- If yes: use it
- If no: send ARP request

Step 10: R2 encapsulates packet
- Encapsulate in Ethernet frame
- Source MAC: R2's G0/0/1 MAC
- Dest MAC: PC2's MAC

Step 11: R2 forwards frame out G0/0/1

Step 12: PC2 receives frame
- Checks dest MAC (its own) → accept
- De-encapsulates
- Delivers to application

Summary:
- IP packet unchanged (source/dest IPs same)
- Layer 2 frame changed at each hop
- Each router makes independent decision
```

### 2.3 Forwarding Decision Example

**Example Topology:**

```
Network Diagram:
[PC1] - [R1] - [R2] - [R3] - [PC2]
10.1.1.10  G0/0/0  S0/0/0  S0/0/1  G0/0/0  S0/0/0  S0/0/1  G0/0/0  10.3.3.10
           10.1.1.1        10.2.2.1-2             10.2.3.1-2       10.3.3.1

R1's Routing Table:
C    10.1.1.0/24 is directly connected, G0/0/0
C    10.2.2.0/24 is directly connected, S0/0/0
S    10.2.3.0/24 [1/0] via 10.2.2.2
S    10.3.3.0/24 [1/0] via 10.2.2.2

R2's Routing Table:
S    10.1.1.0/24 [1/0] via 10.2.2.1
C    10.2.2.0/24 is directly connected, S0/0/1
C    10.2.3.0/24 is directly connected, S0/0/0
S    10.3.3.0/24 [1/0] via 10.2.3.2

R3's Routing Table:
S    10.1.1.0/24 [1/0] via 10.2.3.1
S    10.2.2.0/24 [1/0] via 10.2.3.1
C    10.2.3.0/24 is directly connected, S0/0/1
C    10.3.3.0/24 is directly connected, G0/0/0
```

**Packet Flow: PC1 → PC2:**

```
At R1 (receives from PC1):
Dest IP: 10.3.3.10
Routing table lookup: 10.3.3.0/24
Match found: S 10.3.3.0/24 via 10.2.2.2
Decision: Forward to 10.2.2.2 out S0/0/0

At R2 (receives from R1):
Dest IP: 10.3.3.10
Routing table lookup: 10.3.3.0/24
Match found: S 10.3.3.0/24 via 10.2.3.2
Decision: Forward to 10.2.3.2 out S0/0/0

At R3 (receives from R2):
Dest IP: 10.3.3.10
Routing table lookup: 10.3.3.0/24
Match found: C 10.3.3.0/24 directly connected
Decision: Forward out G0/0/0 to PC2

Key Points:
- Each router makes independent decision
- Each router only knows next hop
- Static routes configured manually
- Works if all routers have correct routes
```

---

## 3. Basic Router Configuration Review

### ทบทวน Router Configuration พื้นฐาน

### 3.1 Initial Router Configuration

**Basic Router Setup:**

```
Step 1: Enter Global Config Mode
Router> enable
Router# configure terminal
Router(config)#

Step 2: Set Hostname
Router(config)# hostname R1
R1(config)#

Step 3: Secure Privileged EXEC Mode
R1(config)# enable secret Cisco123!

Step 4: Secure Console Line
R1(config)# line console 0
R1(config-line)# password Cisco123!
R1(config-line)# login
R1(config-line)# exit

Step 5: Secure VTY Lines (Telnet/SSH)
R1(config)# line vty 0 4
R1(config-line)# password Cisco123!
R1(config-line)# login
R1(config-line)# exit

Step 6: Encrypt Passwords
R1(config)# service password-encryption

Step 7: Banner MOTD
R1(config)# banner motd #
Unauthorized access prohibited!
#

Step 8: Save Configuration
R1(config)# exit
R1# copy running-config startup-config
или
R1# write memory
```

### 3.2 Configure Router Interfaces

**Interface Configuration:**

```
Step 1: Enter Interface Config Mode
R1(config)# interface gigabitethernet 0/0/0
R1(config-if)#

Step 2: Configure IP Address
R1(config-if)# ip address 10.1.1.1 255.255.255.0

Step 3: Add Description (optional but recommended)
R1(config-if)# description Link to LAN

Step 4: Enable Interface
R1(config-if)# no shutdown
*Interface comes up*

Step 5: Exit and Save
R1(config-if)# exit
R1(config)# exit
R1# copy running-config startup-config
```

**Example: Configure All Interfaces:**

```
R1(config)# interface gigabitethernet 0/0/0
R1(config-if)# description Connection to LAN 1
R1(config-if)# ip address 10.1.1.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit

R1(config)# interface gigabitethernet 0/0/1
R1(config-if)# description Connection to LAN 2
R1(config-if)# ip address 10.2.2.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit

R1(config)# interface serial 0/0/0
R1(config-if)# description WAN Link to R2
R1(config-if)# ip address 10.3.3.1 255.255.255.0
R1(config-if)# clock rate 128000  (if DCE side)
R1(config-if)# no shutdown
R1(config-if)# exit

Serial Interface Notes:
- clock rate: Only on DCE side
- DTE side: No clock rate needed
- Check cable type: show controllers s0/0/0
```

### 3.3 IPv6 Configuration

**Enable IPv6 Routing:**

```
R1(config)# ipv6 unicast-routing

Note: Required to enable IPv6 routing on Cisco router
Without this command, router won't forward IPv6 packets
```

**Configure IPv6 Address:**

```
Method 1: Manual (Static) IPv6 Address
R1(config)# interface g0/0/0
R1(config-if)# ipv6 address 2001:db8:acad:1::1/64
R1(config-if)# no shutdown

Method 2: Link-Local Only
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# no shutdown

Method 3: EUI-64
R1(config-if)# ipv6 address 2001:db8:acad:1::/64 eui-64
R1(config-if)# no shutdown

Complete Example:
R1(config)# ipv6 unicast-routing
R1(config)# interface g0/0/0
R1(config-if)# description IPv6 LAN
R1(config-if)# ipv6 address 2001:db8:acad:1::1/64
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# no shutdown
R1(config-if)# exit
```

### 3.4 Verification Commands

**Interface Verification:**

```
Command: show ip interface brief
Purpose: Quick interface status overview

R1# show ip interface brief
Interface         IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0/0 10.1.1.1    YES manual up                    up
GigabitEthernet0/0/1 10.2.2.1    YES manual up                    up
Serial0/0/0       10.3.3.1        YES manual up                    up
Serial0/0/1       unassigned      YES unset  administratively down down

Status:
- up/up = working
- down/down = cable/hardware issue
- administratively down = shutdown command active
```

**Detailed Interface Information:**

```
Command: show ip interface <interface>

R1# show ip interface g0/0/0
GigabitEthernet0/0/0 is up, line protocol is up
  Internet address is 10.1.1.1/24
  Broadcast address is 255.255.255.255
  MTU is 1500 bytes
  Helper address is not set
  Directed broadcast forwarding is disabled
  Outgoing access list is not set
  Inbound  access list is not set
  Proxy ARP is enabled
  ...

Shows:
- Interface status
- IP address and mask
- MTU
- Access lists
- ARP settings
- Much more
```

**IPv6 Interface Verification:**

```
Command: show ipv6 interface brief

R1# show ipv6 interface brief
GigabitEthernet0/0/0     [up/up]
    FE80::1
    2001:DB8:ACAD:1::1
GigabitEthernet0/0/1     [up/up]
    FE80::2
    2001:DB8:ACAD:2::1

Command: show ipv6 interface <interface>

R1# show ipv6 interface g0/0/0
GigabitEthernet0/0/0 is up, line protocol is up
  IPv6 is enabled, link-local address is FE80::1
  No Virtual link-local address(es):
  Global unicast address(es):
    2001:DB8:ACAD:1::1, subnet is 2001:DB8:ACAD:1::/64
  Joined group address(es):
    FF02::1
    FF02::2
    FF02::1:FF00:1
  MTU is 1500 bytes
  ...
```

**Running Configuration:**

```
Command: show running-config

R1# show running-config
Building configuration...

Current configuration : 1123 bytes
!
version 15.4
!
hostname R1
!
enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0
!
interface GigabitEthernet0/0/0
 description Connection to LAN 1
 ip address 10.1.1.1 255.255.255.0
!
interface Serial0/0/0
 description WAN Link to R2
 ip address 10.3.3.1 255.255.255.0
 clock rate 128000
!
...
```

---

## 4. IP Routing Table

### ตาราง IP Routing

### 4.1 Routing Table Structure

**View Routing Table:**

```
Command: show ip route

R1# show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       + - replicated route, % - next hop override

Gateway of last resort is 10.2.2.2 to network 0.0.0.0

S*    0.0.0.0/0 [1/0] via 10.2.2.2
      10.0.0.0/8 is variably subnetted, 5 subnets, 2 masks
C        10.1.1.0/24 is directly connected, GigabitEthernet0/0/0
L        10.1.1.1/32 is directly connected, GigabitEthernet0/0/0
C        10.2.2.0/24 is directly connected, Serial0/0/0
L        10.2.2.1/32 is directly connected, Serial0/0/0
S        10.3.3.0/24 [1/0] via 10.2.2.2
```

### 4.2 Route Sources (Codes)

**Route Source Codes:**

```
L - Local
- IP address configured on router interface
- /32 for IPv4, /128 for IPv6
- Always directly connected

Example:
L    10.1.1.1/32 is directly connected, GigabitEthernet0/0/0

C - Connected
- Network directly connected to router interface
- Automatically added when interface is up
- Uses subnet mask from interface config

Example:
C    10.1.1.0/24 is directly connected, GigabitEthernet0/0/0

S - Static
- Manually configured by administrator
- Doesn't change unless admin changes it
- Reliable but not scalable

Example:
S    10.3.3.0/24 [1/0] via 10.2.2.2

D - EIGRP
- Learned via EIGRP dynamic routing protocol
- Cisco proprietary (but open since 2013)
- Fast convergence

Example:
D    10.4.4.0/24 [90/2172416] via 10.2.2.2

O - OSPF
- Learned via OSPF dynamic routing protocol
- Open standard (most common IGP)
- Link-state protocol

Example:
O    10.5.5.0/24 [110/65] via 10.2.2.2

R - RIP
- Learned via RIP (Routing Information Protocol)
- Legacy protocol (distance-vector)
- Rarely used today

Example:
R    10.6.6.0/24 [120/2] via 10.2.2.2

S* - Default Static Route
- Static default route
- Gateway of last resort
- Matches all destinations

Example:
S*   0.0.0.0/0 [1/0] via 10.2.2.2
```

### 4.3 Route Entry Components

**Anatomy of Route Entry:**

```
Example Route:
D    172.16.3.0/24 [90/2172416] via 172.16.2.2, 00:05:32, Serial0/0/0
│    │             │            │               │         │
│    │             │            │               │         └─ Outgoing Interface
│    │             │            │               └─────────── Last Update Time
│    │             │            └─────────────────────────── Next-Hop IP
│    │             └──────────────────────────────────────── [AD/Metric]
│    └────────────────────────────────────────────────────── Network/Prefix
└─────────────────────────────────────────────────────────── Route Source

Components Explained:

1. Route Source: D (EIGRP)
   How route was learned

2. Destination Network: 172.16.3.0/24
   Target network and prefix length

3. Administrative Distance: 90
   Trustworthiness (lower = better)

4. Metric: 2172416
   Cost to destination (protocol-specific)

5. Next Hop: 172.16.2.2
   IP of next-hop router

6. Timestamp: 00:05:32
   How long since update

7. Outgoing Interface: Serial0/0/0
   Interface to use for forwarding
```

### 4.4 Administrative Distance (AD)

**What is Administrative Distance?**

```
Definition:
Trustworthiness of routing information source
Lower value = more trustworthy
Used when multiple sources provide same route

Range: 0-255
0 = Most trustworthy
255 = Never used (untrustworthy)

Purpose:
Route Selection between different sources
NOT used for path selection within same protocol
```

**Default Administrative Distances:**

```
Route Source               AD Value    Trust Level
─────────────────────────────────────────────────
Directly Connected         0           Highest
Static Route               1           Very High
EIGRP Summary Route        5           High
External BGP (eBGP)        20          High
Internal EIGRP             90          Medium-High
IGRP                       100         Medium
OSPF                       110         Medium
IS-IS                      115         Medium
RIP                        120         Medium-Low
External EIGRP             170         Low
Internal BGP (iBGP)        200         Very Low
Unknown                    255         Never Use
```

**AD Example:**

```
Scenario:
Router learns same network from multiple sources:

10.5.5.0/24 via OSPF (AD=110)
10.5.5.0/24 via RIP  (AD=120)
10.5.5.0/24 via EIGRP (AD=90)

Decision:
Router selects EIGRP route (AD=90 = lowest)

Routing Table Entry:
D    10.5.5.0/24 [90/2172416] via 10.2.2.2

Only EIGRP route installed
OSPF and RIP routes rejected
```

**Modifying AD (Static Route):**

```
Default Static Route AD: 1

Change AD:
R1(config)# ip route 10.5.5.0 255.255.255.0 10.2.2.2 5

Syntax:
ip route <network> <mask> <next-hop> <AD>

Result:
S    10.5.5.0/24 [5/0] via 10.2.2.2

Use Case: Floating Static Route
- Backup route with higher AD
- Used only when primary fails

Example:
Primary: EIGRP route (AD=90)
Backup: Static route (AD=95)

R1(config)# ip route 10.5.5.0 255.255.255.0 10.2.2.3 95

Normal: EIGRP route used
EIGRP fails: Static route activated
```

### 4.5 Routing Table Lookup Process

**Lookup Process Steps:**

```
Step 1: Receive Packet
Router receives packet on interface

Step 2: Extract Destination IP
Read destination IP from packet header

Step 3: Search Routing Table
Compare destination IP with routing table entries

Step 4: Find All Matches
Identify all routes that match destination

Step 5: Select Longest Match
Choose route with longest prefix match

Step 6: Forward Packet
Send packet out appropriate interface

If No Match:
- Check for default route (0.0.0.0/0)
- If exists: use default route
- If not exists: drop packet, send ICMP unreachable
```

**Lookup Example:**

```
Routing Table:
10.0.0.0/8           via 10.1.1.2   (Route A)
10.1.0.0/16          via 10.1.1.3   (Route B)
10.1.1.0/24          via 10.1.1.4   (Route C)
10.1.1.0/25          via 10.1.1.5   (Route D)

Packet 1: Dest = 10.1.1.50
Matches: A (8), B (16), C (24), D (25)
Selection: D - 10.1.1.0/25 (longest)
Next Hop: 10.1.1.5

Packet 2: Dest = 10.1.1.150
Matches: A (8), B (16), C (24), NOT D
Selection: C - 10.1.1.0/24 (longest)
Next Hop: 10.1.1.4

Packet 3: Dest = 10.1.2.50
Matches: A (8), B (16), NOT C or D
Selection: B - 10.1.0.0/16 (longest)
Next Hop: 10.1.1.3

Packet 4: Dest = 10.2.3.50
Matches: A (8), NOT others
Selection: A - 10.0.0.0/8 (longest)
Next Hop: 10.1.1.2

Packet 5: Dest = 11.1.1.1
Matches: NONE
Selection: Default route (if exists) or DROP
```

---

## 5. Static and Dynamic Routing

### Static และ Dynamic Routing

### 5.1 Static Routing

**What is Static Routing?**

```
Definition:
Routes manually configured by administrator
Don't change unless admin changes them
Simple but not scalable

Configuration:
IPv4: ip route <network> <mask> <next-hop | exit-interface>
IPv6: ipv6 route <network/prefix> <next-hop | exit-interface>

Example:
R1(config)# ip route 10.5.5.0 255.255.255.0 10.2.2.2
R1(config)# ipv6 route 2001:db8:acad:5::/64 2001:db8:acad:2::2
```

**Static Route Types:**

**1. Standard Static Route:**

```
Syntax:
ip route <destination-network> <mask> <next-hop-ip>

Example:
R1(config)# ip route 10.3.3.0 255.255.255.0 10.2.2.2

Meaning:
To reach 10.3.3.0/24, send packets to 10.2.2.2

Routing Table:
S    10.3.3.0/24 [1/0] via 10.2.2.2
```

**2. Default Static Route:**

```
Syntax:
ip route 0.0.0.0 0.0.0.0 <next-hop-ip>

Example:
R1(config)# ip route 0.0.0.0 0.0.0.0 192.168.1.1

Meaning:
Send all unknown destinations to 192.168.1.1
Gateway of last resort

Routing Table:
S*   0.0.0.0/0 [1/0] via 192.168.1.1

Use Case:
- Edge routers (connect to ISP)
- Stub networks
- Reduce routing table size
```

**3. Floating Static Route:**

```
Syntax:
ip route <network> <mask> <next-hop> <AD>

Example:
Primary route: EIGRP (AD=90)
Backup route:
R1(config)# ip route 10.3.3.0 255.255.255.0 10.2.2.3 95

Meaning:
Static route with AD=95
Only used if EIGRP route fails

Normal Operation:
D    10.3.3.0/24 [90/2172416] via 10.2.2.2

EIGRP Fails:
S    10.3.3.0/24 [95/0] via 10.2.2.3
```

**4. Directly Connected Static Route:**

```
Syntax:
ip route <network> <mask> <exit-interface>

Example:
R1(config)# ip route 10.3.3.0 255.255.255.0 Serial0/0/0

Meaning:
Forward via specified interface
No next-hop IP needed

Use:
- Point-to-point links
- When next-hop unknown
- Faster lookup (no recursion)
```

**5. Fully Specified Static Route:**

```
Syntax:
ip route <network> <mask> <exit-interface> <next-hop-ip>

Example:
R1(config)# ip route 10.3.3.0 255.255.255.0 s0/0/0 10.2.2.2

Meaning:
Out interface S0/0/0 to next-hop 10.2.2.2
Most specific

Use:
- Multiaccess networks (Ethernet)
- When both interface and next-hop needed
- No recursion, faster lookup
```

**Static Routing Advantages:**

```
✅ Simple to configure (small networks)
✅ No routing protocol overhead (CPU, bandwidth)
✅ No routing advertisements (more secure)
✅ Predictable path (no dynamic changes)
✅ No routing protocol complexity
✅ Deterministic behavior

Use Cases:
- Small networks (few routers)
- Stub networks (one exit point)
- Default routes (to ISP)
- Backup routes (floating static)
- Lab environments
```

**Static Routing Disadvantages:**

```
❌ Not scalable (manual config each router)
❌ No automatic failover
❌ Administrative overhead (many changes)
❌ Prone to configuration errors
❌ Doesn't adapt to topology changes
❌ Requires knowledge of entire network

Problems in Large Networks:
- 100 routes = 100 commands per router
- Topology change = reconfigure all routers
- No load balancing (by default)
- Human error likely
```

### 5.2 Dynamic Routing

**What is Dynamic Routing?**

```
Definition:
Routes learned automatically via routing protocols
Routers share information with each other
Automatically adapt to changes

How it Works:
1. Routers run routing protocol
2. Exchange routing information
3. Calculate best paths
4. Update routing tables automatically
5. Detect failures and reroute

Example Protocols:
- RIP (old, rarely used)
- EIGRP (Cisco, widely used)
- OSPF (industry standard)
- IS-IS (service provider)
- BGP (internet routing)
```

**Dynamic Routing Components:**

```
1. Data Structures
   - Routing tables
   - Neighbor tables
   - Topology databases
   Stored in: RAM

2. Routing Protocol Messages
   - Hello packets (discover neighbors)
   - Update packets (share routes)
   - Query/Reply (EIGRP)
   - LSAs (OSPF)

3. Algorithm
   - Bellman-Ford (RIP)
   - DUAL (EIGRP)
   - Dijkstra/SPF (OSPF, IS-IS)
   - Path Vector (BGP)
```

**Dynamic Routing Protocol Types:**

**IGP (Interior Gateway Protocol):**

```
Used within an organization

Distance Vector:
- RIP (Routing Information Protocol)
  * Hop count metric
  * Max 15 hops
  * Slow convergence
  * Legacy

- EIGRP (Enhanced Interior Gateway Routing Protocol)
  * Cisco proprietary (now open)
  * Fast convergence
  * Multiple metrics
  * Scalable

Link State:
- OSPF (Open Shortest Path First)
  * Industry standard
  * Fast convergence
  * Link-state database
  * Most common IGP

- IS-IS (Intermediate System to Intermediate System)
  * Similar to OSPF
  * Used by service providers
  * Scalable
```

**EGP (Exterior Gateway Protocol):**

```
Used between organizations (ASes)

Path Vector:
- BGP (Border Gateway Protocol)
  * Internet routing protocol
  * Policy-based routing
  * Very scalable
  * Complex
```

**Dynamic Routing Advantages:**

```
✅ Automatic route discovery
✅ Scales well (large networks)
✅ Automatic failover
✅ Adapts to topology changes
✅ Load balancing (ECMP)
✅ Less administrative overhead
✅ Reduced configuration errors

Use Cases:
- Medium to large networks
- Redundant topology
- Need automatic failover
- Enterprise networks
- Service provider networks
```

**Dynamic Routing Disadvantages:**

```
❌ More complex to configure
❌ CPU and memory overhead
❌ Bandwidth consumption (updates)
❌ Security concerns (routing updates)
❌ Convergence time
❌ Troubleshooting complexity

Considerations:
- Requires proper design
- Need skilled administrators
- More overhead than static
- Potential routing loops (if misconfigured)
```

### 5.3 Static vs Dynamic Routing

**Comparison Table:**

```
┌──────────────────┬──────────────────┬─────────────────────┐
│ Feature          │ Static Routing   │ Dynamic Routing     │
├──────────────────┼──────────────────┼─────────────────────┤
│ Configuration    │ Manual           │ Automatic           │
│ Scalability      │ Poor             │ Excellent           │
│ Complexity       │ Simple           │ Complex             │
│ CPU Usage        │ Minimal          │ Moderate-High       │
│ Memory Usage     │ Minimal          │ Moderate-High       │
│ Bandwidth Usage  │ None             │ Routing updates     │
│ Convergence      │ N/A              │ Protocol-dependent  │
│ Failover         │ Manual/Floating  │ Automatic           │
│ Security         │ More secure      │ Updates visible     │
│ Maintenance      │ High (large net) │ Lower               │
│ Best For         │ Small/Stub nets  │ Large networks      │
└──────────────────┴──────────────────┴─────────────────────┘
```

**When to Use Static Routing:**

```
✓ Small network (few routers)
✓ Stub network (one exit path)
✓ Default route to ISP
✓ Backup path (floating static)
✓ Specific traffic engineering
✓ Maximum security required
✓ No dynamic routing protocol available

Example Scenario:
Branch office with single link to HQ
- Static default route to HQ
- Simple, no routing protocol needed
```

**When to Use Dynamic Routing:**

```
✓ Large network (many routers)
✓ Redundant paths available
✓ Need automatic failover
✓ Frequent topology changes
✓ Load balancing required
✓ Scalability important
✓ Reduced administrative overhead

Example Scenario:
Corporate network with 50+ routers
- OSPF for automatic updates
- Fast convergence
- Load balancing
```

**Hybrid Approach (Common):**

```
Combination of both:

Example:
1. Dynamic routing (OSPF) within organization
   - Automatic updates
   - Redundancy
   - Scalability

2. Static default route to ISP
   - No need to exchange routes with ISP
   - Simple
   - Secure

3. Static routes for special cases
   - Backup paths
   - Traffic engineering
   - Specific requirements

Configuration Example:
! Dynamic routing internally
router ospf 1
 network 10.0.0.0 0.255.255.255 area 0

! Static default route to ISP
ip route 0.0.0.0 0.0.0.0 203.0.113.1
```

---

## สรุป

### สิ่งที่ได้เรียนรู้ใน Module 14:

✅ **Path Determination:**

- Router functions (path determination & forwarding)
- Longest prefix match rule
- Routing table principles
- Best path selection

✅ **Packet Forwarding:**

- Forwarding mechanisms (Process, Fast, CEF)
- Step-by-step forwarding process
- Layer 2 encapsulation changes
- Independent routing decisions

✅ **Router Configuration:**

- Basic router setup
- Interface configuration (IPv4/IPv6)
- Verification commands
- Troubleshooting

✅ **IP Routing Table:**

- Routing table structure
- Route sources (C, L, S, D, O, R, etc.)
- Route entry components
- Administrative Distance
- Routing table lookup process

✅ **Static vs Dynamic Routing:**

- Static routing types and configuration
- Dynamic routing protocols (IGP/EGP)
- Advantages and disadvantages
- When to use each approach
- Hybrid approach

---

## Key Concepts สรุป

### Routing Fundamentals:

```
Router Functions:
1. Path Determination (best path selection)
2. Packet Forwarding (send to next hop)

Key Principle:
Longest Prefix Match = Best Path
More specific route always preferred
```

### Routing Table:

```
Route Types:
C - Directly Connected (automatic)
L - Local (interface IP)
S - Static (manual config)
D - EIGRP (dynamic)
O - OSPF (dynamic)
R - RIP (dynamic)
S* - Default route

Path Selection Order:
1. Administrative Distance (between sources)
2. Metric (within protocol)
3. Longest Prefix Match (for forwarding)
```

### Static vs Dynamic:

```
Static:
- Manual configuration
- Small networks
- No overhead
- Predictable

Dynamic:
- Automatic learning
- Large networks
- Protocol overhead
- Adaptive

Best Practice:
Use hybrid approach (both)
```

---

## Important Commands

### Verification Commands:

```
show ip route                    # Show routing table
show ip route <network>          # Check specific route
show ip interface brief          # Interface status
show ipv6 route                  # IPv6 routing table
show ip protocols                # Routing protocols
show running-config              # Current configuration
```

### Configuration Commands:

```
# Static Route
ip route <network> <mask> <next-hop>
ip route 0.0.0.0 0.0.0.0 <next-hop>    # Default route

# Interface
interface <type> <number>
ip address <ip> <mask>
no shutdown

# IPv6
ipv6 unicast-routing
ipv6 address <ipv6>/prefix
```

---

## Best Practices

### Routing Design:

```
✅ Plan IP addressing scheme
✅ Document network topology
✅ Use dynamic routing for large networks
✅ Use static for stub networks/default routes
✅ Implement redundancy
✅ Monitor routing table size
✅ Use route summarization
```

### Configuration:

```
✅ Use descriptions on interfaces
✅ Save configurations (copy run start)
✅ Test before production
✅ Document all static routes
✅ Use consistent naming
✅ Plan for scalability
```

---

## คำถามทบทวน

1. Router มีหน้าที่หลักอะไรบ้าง?
2. Longest Prefix Match คืออะไร?
3. ความแตกต่างระหว่าง Static และ Dynamic Routing?
4. Administrative Distance คืออะไร?
5. Route source codes (C, L, S, D, O) หมายถึงอะไร?
6. Default route คืออะไร? ใช้เมื่อไหร่?
7. Floating Static Route คืออะไร?
8. CEF คืออะไร?
9. เมื่อไหร่ควรใช้ Static Routing?
10. IGP และ EGP ต่างกันอย่างไร?

---

## เฉลยคำถาม

1. **Router Functions:** Path Determination (เลือก best path) และ Packet Forwarding (ส่ง packet)
2. **Longest Prefix Match:** เลือก route ที่มี prefix ยาวที่สุดที่ match กับ destination (most specific)
3. **Static vs Dynamic:** Static = manual config, no overhead; Dynamic = automatic, adapts to changes
4. **Administrative Distance:** ความน่าเชื่อถือของ route source (0-255, lower = better)
5. **Route Codes:** C=Connected, L=Local, S=Static, D=EIGRP, O=OSPF
6. **Default Route:** 0.0.0.0/0, ใช้เมื่อไม่มี specific route, ส่งไป ISP
7. **Floating Static:** Backup route with higher AD, ใช้เมื่อ primary route fail
8. **CEF:** Cisco Express Forwarding, fastest forwarding method, pre-built FIB
9. **ใช้ Static:** Small networks, stub networks, default routes, specific control
10. **IGP vs EGP:** IGP = within organization (OSPF, EIGRP); EGP = between organizations (BGP)

---

**หมายเหตุ:** Routing Concepts เป็นพื้นฐานสำคัญของ networking การเข้าใจวิธีที่ routers ทำงานจะช่วยในการ design, implement และ troubleshoot networks ได้อย่างมีประสิทธิภาพ

---

**เอกสารจัดทำโดย:** Claude (Anthropic AI)  
**Module:** CCNA 2 Module 14 - Routing Concepts  
**Version:** v7.02 SRWE  
**วันที่อัพเดท:** พฤศจิกายน 2025