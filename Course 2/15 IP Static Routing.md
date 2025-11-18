# CCNA 2: Module 15 - IP Static Routing

## การ Configure Static Routes

---

## สารบัญ

1. [วัตถุประสงค์ของ Module](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#%E0%B8%A7%E0%B8%B1%E0%B8%95%E0%B8%96%E0%B8%B8%E0%B8%9B%E0%B8%A3%E0%B8%B0%E0%B8%AA%E0%B8%87%E0%B8%84%E0%B9%8C%E0%B8%82%E0%B8%AD%E0%B8%87-module)
2. [Static Routes](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#1-static-routes)
3. [Configure IPv4 Static Routes](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#2-configure-ipv4-static-routes)
4. [Configure IPv6 Static Routes](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#3-configure-ipv6-static-routes)
5. [Configure Floating Static Routes](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#4-configure-floating-static-routes)
6. [Configure Static Host Routes](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#5-configure-static-host-routes)
7. [สรุป](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#%E0%B8%AA%E0%B8%A3%E0%B8%B8%E0%B8%9B)

---

## วัตถุประสงค์ของ Module

เมื่อจบ Module นี้ คุณจะสามารถ:

- ✅ อธิบาย command syntax สำหรับ static routes
- ✅ Configure IPv4 static routes
- ✅ Configure IPv6 static routes
- ✅ Configure floating static routes
- ✅ Configure static host routes
- ✅ Verify static routes

---

## 1. Static Routes

### แนวคิด Static Routes

### 1.1 Static Route Overview

**What is a Static Route?**

```
Definition:
Route manually configured by administrator
Explicitly defines path to specific destination network
Does not change unless manually modified

Format:
ip route <destination-network> <subnet-mask> <next-hop | exit-interface>

Purpose:
- Provide connectivity to remote networks
- Simple configuration for small networks
- Backup routes (floating static)
- Default routes to ISP
```

**When to Use Static Routes:**

```
✅ Small network (few routers)
✅ Stub network (only one exit path)
✅ Default route to ISP
✅ Backup route (floating static)
✅ Security requirement (no routing updates)
✅ Specific traffic engineering
✅ Bandwidth conservation

Example Scenarios:
1. Branch office with single link to HQ
2. Small office connecting to internet
3. Backup path when dynamic routing fails
4. Lab environment
```

### 1.2 Types of Static Routes

**Four Main Types:**

**1. Standard Static Route**

```
Uses next-hop IP address

Configuration:
R1(config)# ip route 192.168.2.0 255.255.255.0 172.16.2.2

Components:
- Destination: 192.168.2.0/24
- Next hop: 172.16.2.2

Routing Table:
S    192.168.2.0/24 [1/0] via 172.16.2.2

Advantages:
✓ Works on any network type
✓ Most common method
✓ Clear next-hop specified

Disadvantages:
❌ Recursive lookup (router must look up next-hop)
❌ Slightly slower than directly connected
```

**2. Directly Connected Static Route**

```
Uses exit interface only

Configuration:
R1(config)# ip route 192.168.2.0 255.255.255.0 Serial0/0/0

Components:
- Destination: 192.168.2.0/24
- Exit interface: Serial0/0/0

Routing Table:
S    192.168.2.0/24 is directly connected, Serial0/0/0

Advantages:
✓ No recursive lookup
✓ Faster forwarding
✓ Good for point-to-point links

Disadvantages:
❌ Only for point-to-point interfaces
❌ Not recommended for Ethernet (multi-access)

Best Use:
- Serial interfaces (point-to-point)
- PPP, HDLC links
```

**3. Fully Specified Static Route**

```
Uses both exit interface AND next-hop

Configuration:
R1(config)# ip route 192.168.2.0 255.255.255.0 GigabitEthernet0/0/0 172.16.2.2

Components:
- Destination: 192.168.2.0/24
- Exit interface: GigabitEthernet0/0/0
- Next hop: 172.16.2.2

Routing Table:
S    192.168.2.0/24 [1/0] via 172.16.2.2, GigabitEthernet0/0/0

Advantages:
✓ No recursive lookup
✓ Works on multi-access networks
✓ Fastest forwarding
✓ Most specific

Use When:
- Multi-access network (Ethernet)
- Need both interface and next-hop
- Maximum performance required
```

**4. Default Static Route**

```
Matches all destinations (0.0.0.0/0)

Configuration:
R1(config)# ip route 0.0.0.0 0.0.0.0 172.16.2.2
or
R1(config)# ip route 0.0.0.0 0.0.0.0 Serial0/0/0

Routing Table:
S*   0.0.0.0/0 [1/0] via 172.16.2.2

Purpose:
- Gateway of last resort
- Send all unknown traffic to ISP
- Reduce routing table size

Use Case:
- Edge routers connecting to Internet
- Stub routers
- Any router with single exit path

Symbol: * (asterisk) indicates candidate default route
```

### 1.3 Static Route Command Syntax

**IPv4 Static Route Command:**

```
Full Syntax:
Router(config)# ip route <network-address> <subnet-mask> 
                {<next-hop-ip> | <exit-interface> | <exit-interface> <next-hop-ip>}
                [<administrative-distance>]

Parameters:

network-address:
- Destination network
- Example: 192.168.2.0

subnet-mask:
- Subnet mask of destination
- Example: 255.255.255.0
- Can be used for route summarization

next-hop-ip:
- IP address of next-hop router
- Must be reachable
- Example: 172.16.2.2

exit-interface:
- Outgoing interface
- Example: Serial0/0/0, GigabitEthernet0/0/0

administrative-distance (optional):
- Override default AD (1)
- Used for floating static routes
- Range: 1-255
- Example: 5, 95, 120

Examples:

1. Next-hop route:
ip route 192.168.2.0 255.255.255.0 172.16.2.2

2. Directly connected route:
ip route 192.168.2.0 255.255.255.0 Serial0/0/0

3. Fully specified route:
ip route 192.168.2.0 255.255.255.0 GigabitEthernet0/0/0 172.16.2.2

4. Default route:
ip route 0.0.0.0 0.0.0.0 172.16.2.2

5. Floating static route:
ip route 192.168.2.0 255.255.255.0 172.16.2.3 95
```

### 1.4 Static Route Processing

**How Router Processes Static Route:**

```
Step 1: Administrator Enters Command
R1(config)# ip route 192.168.2.0 255.255.255.0 172.16.2.2

Step 2: Router Validates
- Is next-hop reachable?
- Is exit interface up?
- Is syntax correct?

Step 3: Route Added to Routing Table
If valid → Add to routing table
If invalid → Reject, show error

Step 4: Route Becomes Active
Ready to forward packets to 192.168.2.0/24

Validation Requirements:

Next-hop Route:
✓ Next-hop IP must be reachable
✓ Next-hop must have route in table
✓ If next-hop unreachable → route inactive

Directly Connected Route:
✓ Exit interface must be up/up
✓ If interface down → route removed from table

Fully Specified Route:
✓ Exit interface must be up
✓ Next-hop must be on that interface's network
```

**Example: Route Validation:**

```
Scenario:
R1(config)# ip route 192.168.2.0 255.255.255.0 172.16.2.2

R1 checks:
1. Do I have route to 172.16.2.2?
   - Yes: 172.16.2.0/24 is directly connected
   ✓ Valid

2. Is interface to 172.16.2.2 up?
   - Serial0/0/0 is up/up
   ✓ Valid

3. Add route to table
S    192.168.2.0/24 [1/0] via 172.16.2.2

If Serial0/0/0 goes down:
- Route to 172.16.2.2 lost
- Static route removed from table
- Packets to 192.168.2.0/24 dropped
```

---

## 2. Configure IPv4 Static Routes

### การ Configure IPv4 Static Routes

### 2.1 Configuration Scenario

**Network Topology:**

```
Topology:
[PC1] ─ [R1] ─ [R2] ─ [R3] ─ [PC3]
10.1.1.10  G0/0/0  S0/0/0  S0/0/1  G0/0/0  S0/0/0  S0/0/1  G0/0/0  10.3.3.10
           .1      .1──.2          .1──.2          .1      
      10.1.1.0/24  10.1.2.0/24    10.2.3.0/24    10.3.3.0/24

Interface Addresses:

R1:
G0/0/0: 10.1.1.1/24
S0/0/0: 10.1.2.1/24

R2:
S0/0/1: 10.1.2.2/24
S0/0/0: 10.2.3.1/24

R3:
S0/0/1: 10.2.3.2/24
G0/0/0: 10.3.3.1/24

Goal:
Full connectivity between PC1 and PC3
using static routes
```

### 2.2 Configure Next-Hop Static Routes

**R1 Configuration:**

```
Objective:
R1 needs routes to reach networks behind R2 and R3

Networks to reach:
- 10.2.3.0/24 (R2-R3 link)
- 10.3.3.0/24 (R3 LAN)

Configuration:
R1(config)# ip route 10.2.3.0 255.255.255.0 10.1.2.2
R1(config)# ip route 10.3.3.0 255.255.255.0 10.1.2.2

Explanation:
- Both networks reachable via R2 (10.1.2.2)
- Next-hop is directly connected neighbor

Verify:
R1# show ip route static
S    10.2.3.0/24 [1/0] via 10.1.2.2
S    10.3.3.0/24 [1/0] via 10.1.2.2
```

**R2 Configuration:**

```
Objective:
R2 needs routes to reach R1 and R3 LANs

Networks to reach:
- 10.1.1.0/24 (R1 LAN) → via R1 (10.1.2.1)
- 10.3.3.0/24 (R3 LAN) → via R3 (10.2.3.2)

Configuration:
R2(config)# ip route 10.1.1.0 255.255.255.0 10.1.2.1
R2(config)# ip route 10.3.3.0 255.255.255.0 10.2.3.2

Verify:
R2# show ip route static
S    10.1.1.0/24 [1/0] via 10.1.2.1
S    10.3.3.0/24 [1/0] via 10.2.3.2
```

**R3 Configuration:**

```
Objective:
R3 needs routes to reach networks behind R1 and R2

Networks to reach:
- 10.1.2.0/24 (R1-R2 link)
- 10.1.1.0/24 (R1 LAN)

Configuration:
R3(config)# ip route 10.1.2.0 255.255.255.0 10.2.3.1
R3(config)# ip route 10.1.1.0 255.255.255.0 10.2.3.1

Explanation:
- Both networks reachable via R2 (10.2.3.1)

Verify:
R3# show ip route static
S    10.1.2.0/24 [1/0] via 10.2.3.1
S    10.1.1.0/24 [1/0] via 10.2.3.1
```

### 2.3 Configure Directly Connected Static Routes

**Point-to-Point Example:**

```
Topology:
R1 ──Serial0/0/0── R2
   10.1.2.1/24    10.1.2.2/24

R1 to reach 10.3.3.0/24 behind R2:

Configuration:
R1(config)# ip route 10.3.3.0 255.255.255.0 Serial0/0/0

Routing Table:
S    10.3.3.0/24 is directly connected, Serial0/0/0

Advantage:
- No next-hop lookup needed
- Faster than next-hop route

When to Use:
✓ Point-to-point serial links
✓ Only one device on other end
✓ PPP or HDLC encapsulation

When NOT to Use:
✗ Ethernet (multi-access)
✗ Multiple devices on segment
✗ Frame Relay
```

**Configuration Example:**

```
R1(config)# ip route 10.2.3.0 255.255.255.0 Serial0/0/0
R1(config)# ip route 10.3.3.0 255.255.255.0 Serial0/0/0

R2(config)# ip route 10.1.1.0 255.255.255.0 Serial0/0/1
R2(config)# ip route 10.3.3.0 255.255.255.0 Serial0/0/0

R3(config)# ip route 10.1.2.0 255.255.255.0 Serial0/0/1
R3(config)# ip route 10.1.1.0 255.255.255.0 Serial0/0/1
```

### 2.4 Configure Fully Specified Static Routes

**Multi-Access Network Example:**

```
Topology:
       10.1.2.0/24 (Ethernet - Multi-access)
R1 ─────┬───────────┬─────── Switch
  G0/0/0│           │G0/0/0
   .1   │           │.2
        │           R2
        │
     Other devices possible

Problem with exit-interface only:
R1(config)# ip route 10.3.3.0 255.255.255.0 GigabitEthernet0/0/0
- Which device on Ethernet?
- ARP for every destination
- Performance issue

Solution: Fully Specified
R1(config)# ip route 10.3.3.0 255.255.255.0 GigabitEthernet0/0/0 10.1.2.2

Benefits:
✓ Specifies both interface and next-hop
✓ No recursive lookup
✓ Single ARP (for next-hop)
✓ Fastest forwarding on multi-access
```

**Configuration Example:**

```
Network: Ethernet between R1 and R2

R1:
R1(config)# ip route 10.2.3.0 255.255.255.0 GigabitEthernet0/0/0 10.1.2.2
R1(config)# ip route 10.3.3.0 255.255.255.0 GigabitEthernet0/0/0 10.1.2.2

R2:
R2(config)# ip route 10.1.1.0 255.255.255.0 GigabitEthernet0/0/0 10.1.2.1
R2(config)# ip route 10.3.3.0 255.255.255.0 GigabitEthernet0/0/1 10.2.3.2

Verify:
R1# show ip route static
S    10.2.3.0/24 [1/0] via 10.1.2.2, GigabitEthernet0/0/0
S    10.3.3.0/24 [1/0] via 10.1.2.2, GigabitEthernet0/0/0
```

### 2.5 Configure Default Static Route

**Edge Router Configuration:**

```
Scenario:
R1 connects to ISP
Send all unknown traffic to ISP

Topology:
[Internet] ─ [ISP] ─ [R1] ─ [LAN]
              .1      .2
             203.0.113.0/30

Configuration on R1:
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.1
or
R1(config)# ip route 0.0.0.0 0.0.0.0 Serial0/0/1

Verify:
R1# show ip route static
S*   0.0.0.0/0 [1/0] via 203.0.113.1

Meaning:
- Any destination not in routing table
- Forward to 203.0.113.1 (ISP)
- Gateway of last resort

Check Gateway of Last Resort:
R1# show ip route | include Gateway
Gateway of last resort is 203.0.113.1 to network 0.0.0.0
```

**Stub Network Example:**

```
Topology:
[Main Network] ─ [R1] ─ [R2] ─ [Branch LAN]
                  .1     .2
               10.1.2.0/24

R2 is stub router (only one exit):

R2(config)# ip route 0.0.0.0 0.0.0.0 10.1.2.1

Benefits:
✓ One route instead of many
✓ Reduces routing table size
✓ Simpler configuration
✓ Automatic coverage of new networks

R2 Routing Table:
C    10.2.2.0/24 is directly connected, GigabitEthernet0/0/0
C    10.1.2.0/24 is directly connected, Serial0/0/0
S*   0.0.0.0/0 [1/0] via 10.1.2.1

All traffic except local → send to R1
```

### 2.6 Verification Commands

**show ip route**

```
R1# show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, ...

Gateway of last resort is 10.1.2.2 to network 0.0.0.0

S*    0.0.0.0/0 [1/0] via 10.1.2.2
      10.0.0.0/8 is variably subnetted, 6 subnets, 2 masks
C        10.1.1.0/24 is directly connected, GigabitEthernet0/0/0
L        10.1.1.1/32 is directly connected, GigabitEthernet0/0/0
C        10.1.2.0/24 is directly connected, Serial0/0/0
L        10.1.2.1/32 is directly connected, Serial0/0/0
S        10.2.3.0/24 [1/0] via 10.1.2.2
S        10.3.3.0/24 [1/0] via 10.1.2.2

Shows:
- All routes (connected, local, static)
- Gateway of last resort
- Next-hop information
- Administrative distance and metric [AD/metric]
```

**show ip route static**

```
R1# show ip route static
S*    0.0.0.0/0 [1/0] via 10.1.2.2
S     10.2.3.0/24 [1/0] via 10.1.2.2
S     10.3.3.0/24 [1/0] via 10.1.2.2

Shows:
- Only static routes
- Filters out connected and dynamic routes
- Quick view of static configuration
```

**show ip route <network>**

```
R1# show ip route 10.3.3.0
Routing entry for 10.3.3.0/24
  Known via "static", distance 1, metric 0
  Routing Descriptor Blocks:
  * 10.1.2.2
      Route metric is 0, traffic share count is 1

Shows:
- Specific route details
- How route was learned
- Next-hop information
- Metric and AD
```

**show running-config | section ip route**

```
R1# show running-config | section ip route
ip route 0.0.0.0 0.0.0.0 10.1.2.2
ip route 10.2.3.0 255.255.255.0 10.1.2.2
ip route 10.3.3.0 255.255.255.0 10.1.2.2

Shows:
- Actual configuration commands
- Exactly what was entered
- Easy to see all static routes
- Good for documentation
```

**ping and traceroute**

```
Test Connectivity:

R1# ping 10.3.3.10
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.3.10, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/2/4 ms

R1# traceroute 10.3.3.10
Type escape sequence to abort.
Tracing the route to 10.3.3.10
  1 10.1.2.2 4 msec 4 msec 4 msec
  2 10.2.3.2 8 msec 8 msec 8 msec
  3 10.3.3.10 12 msec 12 msec 12 msec

Shows:
- End-to-end connectivity
- Path taken by packets
- Hop-by-hop verification
- Latency at each hop
```

---

## 3. Configure IPv6 Static Routes

### การ Configure IPv6 Static Routes

### 3.1 IPv6 Static Route Command Syntax

**Basic Syntax:**

```
Router(config)# ipv6 route <ipv6-prefix/prefix-length> 
                {<ipv6-next-hop> | <exit-interface> | <exit-interface> <ipv6-next-hop>}
                [<administrative-distance>]

Parameters:

ipv6-prefix/prefix-length:
- Destination IPv6 network
- Example: 2001:db8:acad:2::/64

ipv6-next-hop:
- IPv6 address of next-hop router
- Can be global unicast or link-local
- Example: 2001:db8:acad:1::2 or fe80::2

exit-interface:
- Outgoing interface
- Example: Serial0/0/0, GigabitEthernet0/0/0

administrative-distance (optional):
- Override default AD (1)
- Range: 1-255

Examples:

1. Next-hop route (global unicast):
ipv6 route 2001:db8:acad:3::/64 2001:db8:acad:2::2

2. Next-hop route (link-local):
ipv6 route 2001:db8:acad:3::/64 fe80::2

3. Directly connected route:
ipv6 route 2001:db8:acad:3::/64 Serial0/0/0

4. Fully specified route:
ipv6 route 2001:db8:acad:3::/64 Serial0/0/0 fe80::2

5. Default route:
ipv6 route ::/0 2001:db8:acad:2::2
```

### 3.2 IPv6 Next-Hop Static Routes

**Using Global Unicast Address:**

```
Topology:
R1 ────────── R2 ────────── R3
2001:db8:acad:1::1/64    2001:db8:acad:2::1/64
           :2/64              :2/64

Configuration:

R1 needs route to 2001:db8:acad:3::/64 (R3 LAN):
R1(config)# ipv6 unicast-routing
R1(config)# ipv6 route 2001:db8:acad:3::/64 2001:db8:acad:1::2

R3 needs route to 2001:db8:acad:1::/64 (R1-R2 link):
R3(config)# ipv6 unicast-routing
R3(config)# ipv6 route 2001:db8:acad:1::/64 2001:db8:acad:2::1

Verify:
R1# show ipv6 route static
S   2001:DB8:ACAD:3::/64 [1/0]
     via 2001:DB8:ACAD:1::2

Note: Must enable IPv6 routing first!
```

**Using Link-Local Address:**

```
Link-local addresses more common in IPv6

Configuration:
R1(config)# ipv6 route 2001:db8:acad:3::/64 fe80::2

Problem:
- Link-local only unique per link
- Which interface?
- Ambiguous!

Solution: Must specify exit interface
R1(config)# ipv6 route 2001:db8:acad:3::/64 Serial0/0/0 fe80::2

This is fully specified route (required with link-local)

Verify:
R1# show ipv6 route static
S   2001:DB8:ACAD:3::/64 [1/0]
     via FE80::2, Serial0/0/0
```

### 3.3 IPv6 Directly Connected Static Routes

**Configuration:**

```
Point-to-Point Links:

R1(config)# ipv6 route 2001:db8:acad:3::/64 Serial0/0/0

Routing Table:
S   2001:DB8:ACAD:3::/64 [1/0]
     via Serial0/0/0, directly connected

Use:
- Point-to-point serial links
- Simple configuration
- No ambiguity on P2P

Not Recommended:
- Ethernet (multi-access)
- Must use fully specified
```

### 3.4 IPv6 Fully Specified Static Routes

**Configuration:**

```
Required for:
- Link-local next-hop
- Multi-access networks
- Best practice

Example:
R1(config)# ipv6 route 2001:db8:acad:3::/64 GigabitEthernet0/0/0 fe80::2
R1(config)# ipv6 route 2001:db8:acad:3::/64 Serial0/0/0 fe80::2

Benefits:
✓ No ambiguity
✓ Works with link-local
✓ Fastest forwarding
✓ Most explicit

Routing Table:
S   2001:DB8:ACAD:3::/64 [1/0]
     via FE80::2, GigabitEthernet0/0/0
```

### 3.5 IPv6 Default Static Route

**Configuration:**

```
Default route: ::/0 (all zeros)

Syntax:
R1(config)# ipv6 route ::/0 {next-hop | exit-interface}

Examples:

1. Next-hop (global unicast):
R1(config)# ipv6 route ::/0 2001:db8:acad:1::2

2. Fully specified (link-local):
R1(config)# ipv6 route ::/0 Serial0/0/0 fe80::2

3. Exit interface only:
R1(config)# ipv6 route ::/0 Serial0/0/0

Verify:
R1# show ipv6 route static
S   ::/0 [1/0]
     via 2001:DB8:ACAD:1::2

Use Case:
- Edge router to ISP
- Stub networks
- Default gateway
```

### 3.6 IPv6 Configuration Example

**Complete Scenario:**

```
Topology:
[PC1] ─ [R1] ─ [R2] ─ [R3] ─ [PC3]
2001:db8:1::/64  2001:db8:12::/64  2001:db8:23::/64  2001:db8:3::/64

R1 Configuration:
R1(config)# ipv6 unicast-routing
R1(config)# interface g0/0/0
R1(config-if)# ipv6 address 2001:db8:1::1/64
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)# interface s0/0/0
R1(config-if)# ipv6 address 2001:db8:12::1/64
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)# ipv6 route 2001:db8:23::/64 2001:db8:12::2
R1(config)# ipv6 route 2001:db8:3::/64 2001:db8:12::2

R2 Configuration:
R2(config)# ipv6 unicast-routing
R2(config)# interface s0/0/1
R2(config-if)# ipv6 address 2001:db8:12::2/64
R2(config-if)# no shutdown
R2(config-if)# exit
R2(config)# interface s0/0/0
R2(config-if)# ipv6 address 2001:db8:23::1/64
R2(config-if)# no shutdown
R2(config-if)# exit
R2(config)# ipv6 route 2001:db8:1::/64 2001:db8:12::1
R2(config)# ipv6 route 2001:db8:3::/64 2001:db8:23::2

R3 Configuration:
R3(config)# ipv6 unicast-routing
R3(config)# interface s0/0/1
R3(config-if)# ipv6 address 2001:db8:23::2/64
R3(config-if)# no shutdown
R3(config-if)# exit
R3(config)# interface g0/0/0
R3(config-if)# ipv6 address 2001:db8:3::1/64
R3(config-if)# no shutdown
R3(config-if)# exit
R3(config)# ipv6 route 2001:db8:12::/64 2001:db8:23::1
R3(config)# ipv6 route 2001:db8:1::/64 2001:db8:23::1
```

### 3.7 IPv6 Verification

**Verification Commands:**

```
show ipv6 route:
R1# show ipv6 route
IPv6 Routing Table - default - 8 entries
Codes: C - Connected, L - Local, S - Static, ...

S   2001:DB8:23::/64 [1/0]
     via 2001:DB8:12::2
S   2001:DB8:3::/64 [1/0]
     via 2001:DB8:12::2
C   2001:DB8:1::/64 [0/0]
     via GigabitEthernet0/0/0, directly connected
L   2001:DB8:1::1/128 [0/0]
     via GigabitEthernet0/0/0, receive
C   2001:DB8:12::/64 [0/0]
     via Serial0/0/0, directly connected
L   2001:DB8:12::1/128 [0/0]
     via Serial0/0/0, receive

show ipv6 route static:
R1# show ipv6 route static
S   2001:DB8:23::/64 [1/0]
     via 2001:DB8:12::2
S   2001:DB8:3::/64 [1/0]
     via 2001:DB8:12::2

show ipv6 route <network>:
R1# show ipv6 route 2001:db8:3::/64
Routing entry for 2001:DB8:3::/64
  Known via "static", distance 1, metric 0
  Route count is 1/1, share count 0
  Routing paths:
    2001:DB8:12::2
      Last updated 00:05:23 ago

ping ipv6:
R1# ping ipv6 2001:db8:3::10
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 2001:DB8:3::10, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 12/14/16 ms

traceroute ipv6:
R1# traceroute ipv6 2001:db8:3::10
Type escape sequence to abort.
Tracing the route to 2001:DB8:3::10
  1 2001:DB8:12::2 4 msec 4 msec 4 msec
  2 2001:DB8:23::2 8 msec 8 msec 8 msec
  3 2001:DB8:3::10 12 msec 12 msec 12 msec
```

---

## 4. Configure Floating Static Routes

### การ Configure Floating Static Routes

### 4.1 Floating Static Route Concept

**What is a Floating Static Route?**

```
Definition:
Backup static route with higher AD
Only used when primary route fails

Purpose:
- Provide redundancy
- Automatic failover
- Backup path

How it Works:
1. Primary route: Lower AD (installed in table)
2. Backup route: Higher AD (not in table)
3. Primary fails: Removed from table
4. Backup route: Now best AD → installed
5. Traffic uses backup path

Key: Administrative Distance
Primary must have lower AD than backup
```

### 4.2 Configure IPv4 Floating Static Route

**Scenario:**

```
Topology:
         Primary Link
    R1 ────────────── R2
     │                │
     │  Backup Link  │
     └────────────────┘

Primary: Serial0/0/0 (fast, expensive)
Backup: Serial0/0/1 (slow, cheap)

Goal:
- Use primary normally
- Failover to backup if primary fails

Networks to reach: 10.3.3.0/24
```

**Configuration:**

```
Default AD values:
- EIGRP: 90
- OSPF: 110
- Static: 1

Example 1: Static Primary, Static Backup

Primary route (AD = 1):
R1(config)# ip route 10.3.3.0 255.255.255.0 Serial0/0/0

Backup route (AD = 5):
R1(config)# ip route 10.3.3.0 255.255.255.0 Serial0/0/1 5

Normal Operation:
R1# show ip route static
S    10.3.3.0/24 [1/0] via Serial0/0/0

Primary Fails:
R1# show ip route static
S    10.3.3.0/24 [5/0] via Serial0/0/1

Example 2: Dynamic Primary, Static Backup

EIGRP route (AD = 90) - automatic
R1(config)# router eigrp 100
R1(config-router)# network 10.0.0.0

Floating static backup (AD = 95):
R1(config)# ip route 10.3.3.0 255.255.255.0 Serial0/0/1 95

Normal Operation (EIGRP working):
R1# show ip route
D    10.3.3.0/24 [90/2172416] via 10.1.2.2

Backup route NOT in table (AD 95 > 90)

EIGRP Fails:
R1# show ip route
S    10.3.3.0/24 [95/0] via Serial0/0/1

Static route now installed (only route available)
```

**AD Selection Guidelines:**

```
Floating Static AD should be:
- Higher than primary
- But not too high (not 255)

Recommendations:

Primary: EIGRP (90)
Backup: Static (95-100)

Primary: OSPF (110)
Backup: Static (115-120)

Primary: RIP (120)
Backup: Static (125-130)

Primary: Static (1)
Backup: Static (5-10)

Rule: Backup AD > Primary AD
```

### 4.3 Configure IPv6 Floating Static Route

**Configuration:**

```
Primary route (default AD = 1):
R1(config)# ipv6 route 2001:db8:3::/64 2001:db8:12::2

Backup route (AD = 5):
R1(config)# ipv6 route 2001:db8:3::/64 Serial0/0/1 fe80::2 5

Verify Normal Operation:
R1# show ipv6 route static
S   2001:DB8:3::/64 [1/0]
     via 2001:DB8:12::2

Primary Fails:
R1# show ipv6 route static
S   2001:DB8:3::/64 [5/0]
     via FE80::2, Serial0/0/1

Example with OSPF:

OSPFv3 route (AD = 110):
R1(config)# ipv6 router ospf 1
R1(config-rtr)# router-id 1.1.1.1
R1(config-rtr)# exit
R1(config)# interface s0/0/0
R1(config-if)# ipv6 ospf 1 area 0

Floating static backup (AD = 115):
R1(config)# ipv6 route 2001:db8:3::/64 Serial0/0/1 fe80::2 115
```

### 4.4 Testing Floating Static Routes

**Test Failover:**

```
Step 1: Verify Primary Active
R1# show ip route static
S    10.3.3.0/24 [1/0] via 10.1.2.2

Step 2: Test Connectivity
R1# ping 10.3.3.10
!!!!!
Success rate is 100 percent (5/5)

Step 3: Simulate Primary Failure
R1(config)# interface serial0/0/0
R1(config-if)# shutdown

Step 4: Verify Backup Active
R1# show ip route static
S    10.3.3.0/24 [5/0] via 10.1.3.2

Step 5: Test Connectivity via Backup
R1# ping 10.3.3.10
!!!!!
Success rate is 100 percent (5/5)

Step 6: Restore Primary
R1(config)# interface serial0/0/0
R1(config-if)# no shutdown

Step 7: Verify Primary Restored
R1# show ip route static
S    10.3.3.0/24 [1/0] via 10.1.2.2

Primary route back in table (lower AD)
Backup removed (higher AD)
```

---

## 5. Configure Static Host Routes

### การ Configure Static Host Routes

### 5.1 Host Route Concept

**What is a Host Route?**

```
Definition:
Route to specific host (/32 for IPv4, /128 for IPv6)
Most specific route possible

Characteristics:
- IPv4: /32 subnet mask (255.255.255.255)
- IPv6: /128 prefix length
- Matches single IP address only

Use Cases:
- Specific server access
- Traffic engineering for one host
- Testing and troubleshooting
- Security (granular control)

Example:
Network route: 10.1.1.0/24 (256 addresses)
Host route:    10.1.1.10/32 (1 address only)
```

### 5.2 Configure IPv4 Host Route

**Syntax:**

```
Router(config)# ip route <host-ip> 255.255.255.255 <next-hop | exit-interface>

Examples:

1. Host route to specific server:
R1(config)# ip route 10.3.3.100 255.255.255.255 10.1.2.2

2. Host route to management interface:
R1(config)# ip route 192.168.1.1 255.255.255.255 GigabitEthernet0/0/0

3. Host route for loopback:
R1(config)# ip route 10.10.10.1 255.255.255.255 Serial0/0/0
```

**Example Scenario:**

```
Topology:
[R1] ── [R2] ── [Server Farm]
                10.3.3.0/24
                └─ Web Server: 10.3.3.50
                └─ DB Server: 10.3.3.51

Requirement:
- Web Server via primary link
- DB Server via different link
- Rest of network via normal route

Configuration:

Network route (normal traffic):
R1(config)# ip route 10.3.3.0 255.255.255.0 10.1.2.2

Host route (web server - specific path):
R1(config)# ip route 10.3.3.50 255.255.255.255 10.1.3.2

Host route (db server - specific path):
R1(config)# ip route 10.3.3.51 255.255.255.255 10.1.4.2

Result:
- 10.3.3.50 → via 10.1.3.2 (specific)
- 10.3.3.51 → via 10.1.4.2 (specific)
- Other 10.3.3.x → via 10.1.2.2 (general)

Routing Table:
S    10.3.3.50/32 [1/0] via 10.1.3.2
S    10.3.3.51/32 [1/0] via 10.1.4.2
S    10.3.3.0/24 [1/0] via 10.1.2.2

Longest Match:
Packet to 10.3.3.50 → matches both /32 and /24
Router selects /32 (longer prefix)
```

### 5.3 Configure IPv6 Host Route

**Syntax:**

```
Router(config)# ipv6 route <host-ipv6>/128 <next-hop | exit-interface>

Examples:

1. Host route to specific server:
R1(config)# ipv6 route 2001:db8:3::100/128 2001:db8:12::2

2. Host route with link-local:
R1(config)# ipv6 route 2001:db8:3::100/128 Serial0/0/0 fe80::2

3. Loopback host route:
R1(config)# ipv6 route 2001:db8::1/128 Serial0/0/0
```

**Configuration Example:**

```
Network route:
R1(config)# ipv6 route 2001:db8:3::/64 2001:db8:12::2

Host route (specific server):
R1(config)# ipv6 route 2001:db8:3::50/128 2001:db8:13::2

Routing Table:
S   2001:DB8:3::50/128 [1/0]
     via 2001:DB8:13::2
S   2001:DB8:3::/64 [1/0]
     via 2001:DB8:12::2

Traffic to 2001:db8:3::50 → via 2001:db8:13::2 (/128 more specific)
Traffic to other 2001:db8:3:: → via 2001:db8:12::2 (/64)
```

### 5.4 Automatically Created Host Routes

**Local Routes (L):**

```
Router automatically creates /32 host routes
for its own interface IP addresses

Example:
R1(config)# interface g0/0/0
R1(config-if)# ip address 10.1.1.1 255.255.255.0
R1(config-if)# no shutdown

Routing Table:
C    10.1.1.0/24 is directly connected, GigabitEthernet0/0/0
L    10.1.1.1/32 is directly connected, GigabitEthernet0/0/0

Explanation:
C = Network route (for forwarding to LAN)
L = Local route (for packets to router itself)

Local route (/32):
- Matches packets destined to router
- More efficient processing
- Router knows "this is for me"

IPv6 Example:
R1(config)# interface g0/0/0
R1(config-if)# ipv6 address 2001:db8:1::1/64

Routing Table:
C   2001:DB8:1::/64 [0/0]
     via GigabitEthernet0/0/0, directly connected
L   2001:DB8:1::1/128 [0/0]
     via GigabitEthernet0/0/0, receive
L   FE80::1/128 [0/0]
     via GigabitEthernet0/0/0, receive
```

---

## สรุป

### สิ่งที่ได้เรียนรู้ใน Module 15:

✅ **Static Routes:**

- Four types: next-hop, directly connected, fully specified, default
- When to use each type
- Command syntax and configuration

✅ **IPv4 Static Routes:**

- Next-hop routes (most common)
- Directly connected routes (point-to-point)
- Fully specified routes (multi-access)
- Default routes (0.0.0.0/0)
- Verification commands

✅ **IPv6 Static Routes:**

- Global unicast vs link-local next-hop
- Must enable ipv6 unicast-routing
- Fully specified required with link-local
- Default routes (::/0)
- IPv6-specific verification

✅ **Floating Static Routes:**

- Backup routes with higher AD
- Automatic failover
- AD configuration strategies
- Testing procedures

✅ **Static Host Routes:**

- /32 (IPv4) or /128 (IPv6)
- Specific host routing
- Longest match principle
- Automatic local routes

---

## Command Summary

### IPv4 Static Routes:

```
# Standard static route
ip route <network> <mask> <next-hop>
ip route 10.3.3.0 255.255.255.0 10.1.2.2

# Directly connected
ip route <network> <mask> <exit-interface>
ip route 10.3.3.0 255.255.255.0 Serial0/0/0

# Fully specified
ip route <network> <mask> <exit-interface> <next-hop>
ip route 10.3.3.0 255.255.255.0 GigabitEthernet0/0/0 10.1.2.2

# Default route
ip route 0.0.0.0 0.0.0.0 <next-hop>

# Floating static route
ip route <network> <mask> <next-hop> <AD>
ip route 10.3.3.0 255.255.255.0 10.1.3.2 95

# Host route
ip route <host> 255.255.255.255 <next-hop>
ip route 10.3.3.50 255.255.255.255 10.1.2.2
```

### IPv6 Static Routes:

```
# Enable IPv6 routing (required first!)
ipv6 unicast-routing

# Standard static route
ipv6 route <prefix/length> <next-hop>
ipv6 route 2001:db8:3::/64 2001:db8:12::2

# Fully specified (with link-local)
ipv6 route <prefix/length> <exit-interface> <link-local>
ipv6 route 2001:db8:3::/64 Serial0/0/0 fe80::2

# Default route
ipv6 route ::/0 <next-hop>

# Floating static route
ipv6 route <prefix/length> <next-hop> <AD>
ipv6 route 2001:db8:3::/64 2001:db8:13::2 95

# Host route
ipv6 route <host>/128 <next-hop>
ipv6 route 2001:db8:3::50/128 2001:db8:12::2
```

### Verification:

```
show ip route [static | <network>]
show ipv6 route [static | <network>]
show running-config | section ip route
show running-config | section ipv6 route
ping <destination>
traceroute <destination>
```

---

## Best Practices

### Configuration:

```
✅ Document all static routes
✅ Use descriptions where possible
✅ Plan IP addressing for easy summarization
✅ Use default routes for stub networks
✅ Configure floating static for redundancy
✅ Test failover scenarios
✅ Save configurations after changes
```

### Route Selection:

```
Next-Hop Route:
- Most common
- Works on all network types
- Requires recursive lookup

Directly Connected:
- Only for point-to-point
- Faster than next-hop
- NOT for Ethernet

Fully Specified:
- Best for Ethernet
- Required for link-local IPv6
- Fastest forwarding
- Most explicit

Use Case Determines Type!
```

### Administrative Distance:

```
Static Route Default: 1

Floating Static Guidelines:
- Primary EIGRP (90) → Backup 95-100
- Primary OSPF (110) → Backup 115-120
- Primary RIP (120) → Backup 125-130
- Primary Static (1) → Backup 5-10

Rule: Backup AD > Primary AD
```

---

## Common Issues and Solutions

### Issue 1: Static Route Not in Table

```
Problem: Route configured but not showing

Causes:
❌ Next-hop not reachable
❌ Exit interface down
❌ Syntax error

Solutions:
✓ Verify next-hop has route in table
✓ Check interface status (show ip interface brief)
✓ Verify command syntax
✓ Check for typos in IP addresses
```

### Issue 2: Traffic Not Using Static Route

```
Problem: Route in table but traffic not forwarded

Causes:
❌ More specific route exists
❌ Different route with lower AD
❌ ARP issues (Ethernet)

Solutions:
✓ Check routing table (show ip route)
✓ Verify longest match selection
✓ Check AD values
✓ Verify ARP table (show arp)
```

### Issue 3: IPv6 Static Route Not Working

```
Problem: IPv6 route configured but no connectivity

Causes:
❌ Forgot ipv6 unicast-routing
❌ Link-local without exit interface
❌ Wrong next-hop address

Solutions:
✓ Enable ipv6 unicast-routing
✓ Use fully specified with link-local
✓ Verify next-hop address
✓ Check interface status
```

---

## คำถามทบทวน

1. Static route มี types อะไรบ้าง 4 แบบ?
2. ความแตกต่างระหว่าง next-hop และ directly connected static route?
3. เมื่อไหร่ต้องใช้ fully specified static route?
4. Default route คืออะไร? syntax เป็นอย่างไร?
5. Floating static route คืออะไร? ทำงานอย่างไร?
6. AD ของ static route default คือเท่าไหร่?
7. Host route คืออะไร? subnet mask เท่าไหร่?
8. ทำไมต้อง enable ipv6 unicast-routing ก่อน config IPv6 static route?
9. Link-local address ใน IPv6 static route ต้องระบุอะไรเพิ่ม?
10. Command อะไรใช้ verify static routes?

---

## เฉลยคำถาม

1. **4 Types:** Next-hop, Directly connected, Fully specified, Default route
2. **Next-hop vs Directly connected:** Next-hop = ระบุ IP ของ router ถัดไป; Directly connected = ระบุ exit interface เท่านั้น
3. **Fully specified:** Multi-access networks (Ethernet), IPv6 with link-local, fastest forwarding
4. **Default route:** ส่งทุก destination ที่ไม่รู้จัก; IPv4: 0.0.0.0/0, IPv6: ::/0
5. **Floating static:** Backup route with higher AD, ใช้เมื่อ primary route fail
6. **Static AD:** 1 (default)
7. **Host route:** Route ไปยัง host เดียว; IPv4: /32, IPv6: /128
8. **ipv6 unicast-routing:** Enable IPv6 routing function บน router, ไม่งั้นไม่ forward IPv6 packets
9. **Link-local:** ต้องระบุ exit interface (fully specified) เพราะ link-local ไม่ unique globally
10. **Verification:** show ip route static, show ipv6 route static, ping, traceroute

---

**หมายเหตุ:** Static routing เป็นพื้นฐานสำคัญของ routing การเข้าใจ concepts และ configuration จะช่วยในการ implement และ troubleshoot networks ได้อย่างมีประสิทธิภาพ

---

**เอกสารจัดทำโดย:** Claude (Anthropic AI)  
**Module:** CCNA 2 Module 15 - IP Static Routing  
**Version:** v7.02 SRWE  
**วันที่อัพเดท:** พฤศจิกายน 2025