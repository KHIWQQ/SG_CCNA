# CCNA 2: Module 16 - Troubleshoot Static and Default Routes

## การแก้ไขปัญหา Static และ Default Routes

---

## สารบัญ

1. [วัตถุประสงค์ของ Module](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#%E0%B8%A7%E0%B8%B1%E0%B8%95%E0%B8%96%E0%B8%B8%E0%B8%9B%E0%B8%A3%E0%B8%B0%E0%B8%AA%E0%B8%87%E0%B8%84%E0%B9%8C%E0%B8%82%E0%B8%AD%E0%B8%87-module)
2. [Packet Processing with Static Routes](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#1-packet-processing-with-static-routes)
3. [Troubleshoot IPv4 Static and Default Routes](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#2-troubleshoot-ipv4-static-and-default-routes)
4. [Troubleshoot IPv6 Static and Default Routes](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#3-troubleshoot-ipv6-static-and-default-routes)
5. [Common Issues and Solutions](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#4-common-issues-and-solutions)
6. [Troubleshooting Methodology](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#5-troubleshooting-methodology)
7. [สรุป](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#%E0%B8%AA%E0%B8%A3%E0%B8%B8%E0%B8%9B)

---

## วัตถุประสงค์ของ Module

เมื่อจบ Module นี้ คุณจะสามารถ:

- ✅ อธิบายวิธี router processes packets เมื่อมี static route
- ✅ Troubleshoot IPv4 static และ default route configuration issues
- ✅ Troubleshoot IPv6 static และ default route configuration issues
- ✅ ใช้ troubleshooting tools และ commands อย่างมีประสิทธิภาพ
- ✅ ระบุและแก้ไขปัญหา routing ทั่วไป

---

## 1. Packet Processing with Static Routes

### การประมวลผล Packet ด้วย Static Routes

### 1.1 Packet Forwarding Review

**How Router Processes Packet:**

```
Step-by-Step Process:

1. Receive Frame
   - Packet arrives on interface
   - Check destination MAC address
   - If matches interface MAC → process
   - If not → discard

2. De-encapsulate Frame
   - Remove Layer 2 header
   - Check Ethernet Type field (0x0800 = IPv4, 0x86DD = IPv6)
   - Extract Layer 3 packet

3. Examine Destination IP
   - Read destination IP address
   - Check if destined to router itself
   - If not → route it

4. Routing Table Lookup
   - Search routing table for match
   - Apply longest prefix match rule
   - Find best route

5. Determine Next Hop
   - Identify exit interface
   - Determine next-hop IP (if applicable)

6. Check ARP Table (Ethernet)
   - Look up next-hop MAC address
   - If not in table → send ARP request
   - Wait for ARP reply

7. Encapsulate Packet
   - Create new Layer 2 frame
   - Source MAC: outgoing interface MAC
   - Dest MAC: next-hop MAC
   - Appropriate encapsulation for interface type

8. Forward Frame
   - Send frame out exit interface
   - TTL decremented by 1
```

### 1.2 Packet Forwarding Example

**Scenario:**

```
Topology:
[PC1] ─ [R1] ─ [R2] ─ [R3] ─ [PC3]
10.1.1.10  G0/0/0  S0/0/0  S0/0/1  G0/0/0  S0/0/0  S0/0/1  G0/0/0  192.168.2.10
           .1      .1──.2          .1──.2          .1
      10.1.1.0/24  172.16.1.0/24  172.16.2.0/24  192.168.2.0/24

PC1 pings PC3: 10.1.1.10 → 192.168.2.10
```

**At PC1:**

```
PC1 creates packet:
Source IP: 10.1.1.10
Dest IP: 192.168.2.10

Check: Is 192.168.2.10 in local subnet?
10.1.1.10/24 = 10.1.1.0 network
192.168.2.10 = different network

Decision: Send to default gateway (10.1.1.1)

Create Ethernet frame:
Source MAC: PC1 MAC
Dest MAC: R1's G0/0/0 MAC
Encapsulate packet in frame
Send to R1
```

**At R1:**

```
1. Receive frame on G0/0/0
   - Check dest MAC → matches R1's G0/0/0 ✓
   - Accept frame

2. De-encapsulate
   - Remove Ethernet header
   - Check Type: 0x0800 (IPv4)
   - Extract IP packet

3. Check destination IP: 192.168.2.10
   - Not R1's address → route it

4. Routing table lookup
   R1# show ip route
   S*   0.0.0.0/0 [1/0] via 172.16.1.2
   
   Match: Default route (0.0.0.0/0)
   Next hop: 172.16.1.2 (R2)
   Exit interface: Serial0/0/0

5. Encapsulate for serial link
   - Point-to-point: No MAC addresses
   - HDLC or PPP encapsulation
   - All 1s address (0xFFFF)

6. Forward out S0/0/0 to R2
```

**At R2:**

```
1. Receive frame on S0/0/1
   - Point-to-point: No MAC check
   - De-encapsulate

2. Check destination IP: 192.168.2.10

3. Routing table lookup
   R2# show ip route
   S    192.168.2.0/24 [1/0] via 172.16.2.2
   
   Match: 192.168.2.0/24
   Next hop: 172.16.2.2 (R3)
   Exit interface: Serial0/0/0

4. Encapsulate for serial link
   - Point-to-point
   - HDLC/PPP

5. Forward out S0/0/0 to R3
```

**At R3:**

```
1. Receive frame on S0/0/1
   - De-encapsulate

2. Check destination IP: 192.168.2.10

3. Routing table lookup
   R3# show ip route
   C    192.168.2.0/24 is directly connected, GigabitEthernet0/0/0
   
   Match: Directly connected!
   Exit interface: G0/0/0

4. Check ARP table
   R3# show arp
   - Is 192.168.2.10 in ARP table?
   - If yes: use MAC address
   - If no: send ARP request

   ARP Request:
   Who has 192.168.2.10? Tell 192.168.2.1
   
   PC3 ARP Reply:
   192.168.2.10 is at PC3-MAC-ADDRESS

5. Encapsulate in Ethernet frame
   Source MAC: R3's G0/0/0 MAC
   Dest MAC: PC3's MAC

6. Forward out G0/0/0
```

**At PC3:**

```
1. Receive frame on NIC
   - Check dest MAC → matches PC3 ✓
   - Accept frame

2. De-encapsulate
   - Remove Ethernet header
   - Extract IP packet

3. Check destination IP: 192.168.2.10
   - Matches PC3's IP ✓
   - Deliver to application

4. Process ICMP Echo Request
   - Generate ICMP Echo Reply
   - Send back to PC1 (reverse path)

Result: Ping successful!
```

**Key Observations:**

```
✓ Layer 3 packet unchanged (IP addresses stay same)
✓ Layer 2 frame changes at each hop
✓ Each router makes independent decision
✓ TTL decremented at each hop
✓ Different encapsulations per link type:
  - Ethernet: MAC addresses
  - Serial: HDLC/PPP (no MACs)
✓ ARP used only on Ethernet segments
✓ Each router only knows next hop
```

### 1.3 Static Route Processing

**When Router Processes Static Route:**

```
Configuration:
R1(config)# ip route 192.168.2.0 255.255.255.0 172.16.1.2

Router's Actions:

1. Validate Command
   - Correct syntax? ✓
   - Valid IP addresses? ✓
   - Reasonable subnet mask? ✓

2. Check Reachability
   - Is next-hop 172.16.1.2 reachable?
   - Look in routing table
   - Must have route to 172.16.1.0/24
   
   R1# show ip route | include 172.16.1
   C    172.16.1.0/24 is directly connected, Serial0/0/0
   ✓ Reachable

3. Install Route
   - If valid → add to routing table
   - If invalid → reject
   
   R1# show ip route static
   S    192.168.2.0/24 [1/0] via 172.16.1.2

4. Route Active
   - Ready to forward packets
   - Used when packets match destination

If Interface Goes Down:
R1(config)# interface s0/0/0
R1(config-if)# shutdown

Result:
- Route to 172.16.1.0/24 removed
- Static route becomes unreachable
- Static route removed from table
- Packets to 192.168.2.0/24 dropped

When Interface Comes Up:
R1(config-if)# no shutdown

Result:
- Route to 172.16.1.0/24 re-added
- Next-hop reachable again
- Static route re-installed
- Forwarding resumes
```

---

## 2. Troubleshoot IPv4 Static and Default Routes

### การแก้ไขปัญหา IPv4 Static และ Default Routes

### 2.1 Common IPv4 Static Route Problems

**Problem Categories:**

```
1. Configuration Errors
   - Wrong destination network
   - Wrong subnet mask
   - Wrong next-hop IP
   - Wrong exit interface
   - Typos

2. Topology Issues
   - Interface down
   - Cable unplugged
   - Hardware failure
   - Link congestion

3. Routing Logic Errors
   - Missing route on intermediate router
   - Inconsistent routing (asymmetric)
   - Routing loops
   - Wrong administrative distance

4. Next-hop Issues
   - Next-hop not reachable
   - Next-hop on wrong subnet
   - ARP problems

5. Default Route Issues
   - Missing default route
   - Wrong next-hop for default
   - Multiple conflicting defaults
```

### 2.2 Troubleshooting Tools and Commands

**Essential Commands:**

**1. show ip route**

```
R1# show ip route
Purpose: View entire routing table

Check for:
✓ Is route present?
✓ Correct destination network?
✓ Correct next-hop?
✓ Correct administrative distance?
✓ Is default route present (S*)?

Example Output:
Gateway of last resort is 172.16.1.2 to network 0.0.0.0

S*    0.0.0.0/0 [1/0] via 172.16.1.2
      10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C        10.1.1.0/24 is directly connected, GigabitEthernet0/0/0
L        10.1.1.1/32 is directly connected, GigabitEthernet0/0/0
      172.16.0.0/16 is variably subnetted, 2 subnets, 2 masks
C        172.16.1.0/24 is directly connected, Serial0/0/0
L        172.16.1.1/32 is directly connected, Serial0/0/0
```

**2. show ip route static**

```
R1# show ip route static
Purpose: Filter only static routes

Check for:
✓ All expected static routes present?
✓ Correct next-hops?
✓ Floating static routes (higher AD)?

Example Output:
S*    0.0.0.0/0 [1/0] via 172.16.1.2
S     192.168.2.0/24 [1/0] via 172.16.1.2
S     192.168.3.0/24 [1/0] via 172.16.1.2
```

**3. show ip route <network>**

```
R1# show ip route 192.168.2.0
Purpose: Details about specific route

Example Output:
Routing entry for 192.168.2.0/24
  Known via "static", distance 1, metric 0
  Routing Descriptor Blocks:
  * 172.16.1.2
      Route metric is 0, traffic share count is 1

Shows:
- How route learned (static)
- Administrative distance (1)
- Metric (0)
- Next-hop IP
```

**4. show ip interface brief**

```
R1# show ip interface brief
Purpose: Interface status overview

Check for:
✓ Interface up/up?
✓ IP address correct?
✓ administratively down?

Example Output:
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0/0   10.1.1.1        YES manual up                    up
GigabitEthernet0/0/1   unassigned      YES unset  administratively down down
Serial0/0/0            172.16.1.1      YES manual up                    up
Serial0/0/1            unassigned      YES unset  administratively down down

Status Meanings:
- up/up = Working
- down/down = Physical problem
- up/down = Layer 2 problem
- administratively down = shutdown
```

**5. show running-config | section ip route**

```
R1# show running-config | section ip route
Purpose: View actual configuration

Example Output:
ip route 0.0.0.0 0.0.0.0 172.16.1.2
ip route 192.168.2.0 255.255.255.0 172.16.1.2
ip route 192.168.3.0 255.255.255.0 172.16.1.2

Check for:
✓ Configuration matches intent?
✓ Typos in IP addresses?
✓ Correct subnet masks?
✓ Proper AD values?
```

**6. ping**

```
R1# ping 192.168.2.10
Purpose: Test end-to-end connectivity

Example Success:
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.2.10, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/2/4 ms

Example Failure:
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.2.10, timeout is 2 seconds:
.....
Success rate is 0 percent (0/5)

Symbols:
! = Reply received
. = Timeout
U = Destination unreachable
```

**7. traceroute**

```
R1# traceroute 192.168.2.10
Purpose: Show path to destination

Example Output:
Type escape sequence to abort.
Tracing the route to 192.168.2.10
  1 172.16.1.2 4 msec 4 msec 4 msec
  2 172.16.2.2 8 msec 8 msec 8 msec
  3 192.168.2.10 12 msec * 12 msec

Shows:
- Each hop along path
- IP of each router
- Round-trip time
- Where packets stop (* = timeout)

Troubleshooting:
- Packets stop at specific router → problem there
- * * * = routing loop or filtering
- Unexpected path = routing misconfiguration
```

**8. show cdp neighbors**

```
R1# show cdp neighbors
Purpose: Verify physical connectivity to neighbors

Example Output:
Device ID    Local Intrfce  Holdtme  Capability  Platform  Port ID
R2           Ser 0/0/0      150      R S         ISR4331   Ser 0/0/1

Shows:
- Directly connected Cisco devices
- Which interface
- Neighbor's hostname
- Neighbor's interface

Use to verify:
✓ Physical cable correct?
✓ Connected to right device?
✓ Connected to right port?
```

**9. show arp**

```
R1# show arp
Purpose: View ARP table (Ethernet only)

Example Output:
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  10.1.1.1                -   0011.2233.4455  ARPA   GigabitEthernet0/0/0
Internet  10.1.1.10              10   0011.2233.4466  ARPA   GigabitEthernet0/0/0

Check:
✓ Is destination in ARP table?
✓ Correct MAC address?
✓ If missing → ARP request needed
```

### 2.3 Troubleshooting Scenarios

**Scenario 1: Static Route Not in Routing Table**

```
Symptom:
Configured static route not appearing in routing table

Problem:
R1(config)# ip route 192.168.2.0 255.255.255.0 172.16.1.2
R1# show ip route static
(route not shown)

Diagnosis:

Step 1: Check if next-hop reachable
R1# show ip route | include 172.16.1
(no route shown)

Problem Found: Next-hop 172.16.1.2 not reachable!

Step 2: Check interface status
R1# show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol
Serial0/0/0            172.16.1.1      YES manual administratively down down

Problem Found: Interface shutdown!

Solution:
R1(config)# interface serial0/0/0
R1(config-if)# no shutdown

Verification:
R1# show ip route static
S    192.168.2.0/24 [1/0] via 172.16.1.2
✓ Route now in table
```

**Scenario 2: Wrong Next-Hop IP Address**

```
Symptom:
Can't reach destination network

Problem Configuration:
R1(config)# ip route 192.168.2.0 255.255.255.0 172.16.1.3
(should be 172.16.1.2)

Diagnosis:

Step 1: Check connectivity
R1# ping 192.168.2.10
.....
Success rate is 0 percent (0/5)

Step 2: Check routing table
R1# show ip route static
S    192.168.2.0/24 [1/0] via 172.16.1.3

Step 3: Verify next-hop
R1# ping 172.16.1.3
.....
Success rate is 0 percent (0/5)

Problem Found: Next-hop 172.16.1.3 doesn't exist!
(Should be 172.16.1.2)

Step 4: Check configuration
R1# show running-config | section ip route
ip route 192.168.2.0 255.255.255.0 172.16.1.3

Problem Found: Wrong IP address!

Solution:
R1(config)# no ip route 192.168.2.0 255.255.255.0 172.16.1.3
R1(config)# ip route 192.168.2.0 255.255.255.0 172.16.1.2

Verification:
R1# ping 192.168.2.10
!!!!!
Success rate is 100 percent (5/5)
✓ Fixed!
```

**Scenario 3: Wrong Subnet Mask**

```
Symptom:
Some destinations work, others don't

Problem Configuration:
R1(config)# ip route 192.168.2.0 255.255.255.128 172.16.1.2
(should be 255.255.255.0)

Diagnosis:

Step 1: Test connectivity
R1# ping 192.168.2.10
!!!!!
Success rate is 100 percent (5/5)
✓ Works (192.168.2.10 is in 192.168.2.0-127)

R1# ping 192.168.2.150
.....
Success rate is 0 percent (0/5)
✗ Fails (192.168.2.150 is in 192.168.2.128-255)

Step 2: Check routing table
R1# show ip route static
S    192.168.2.0/25 [1/0] via 172.16.1.2

Problem Found: /25 mask (should be /24)
Only covers 192.168.2.0-127
Missing 192.168.2.128-255

Step 3: Check configuration
R1# show running-config | section ip route
ip route 192.168.2.0 255.255.255.128 172.16.1.2

Solution:
R1(config)# no ip route 192.168.2.0 255.255.255.128 172.16.1.2
R1(config)# ip route 192.168.2.0 255.255.255.0 172.16.1.2

Verification:
R1# show ip route static
S    192.168.2.0/24 [1/0] via 172.16.1.2
R1# ping 192.168.2.150
!!!!!
Success rate is 100 percent (5/5)
✓ Fixed!
```

**Scenario 4: Missing Route on Intermediate Router**

```
Symptom:
One direction works, return path fails (asymmetric routing)

Topology:
PC1 (10.1.1.10) → R1 → R2 → R3 → PC3 (192.168.2.10)

Problem:
PC1 can't ping PC3

Diagnosis:

From PC1:
PC> ping 192.168.2.10
Request timed out

Step 1: Check R1
R1# show ip route static
S    192.168.2.0/24 [1/0] via 172.16.1.2
✓ Route present

R1# ping 192.168.2.10
!!!!!
Success rate is 100 percent (5/5)
✓ R1 can reach destination

Step 2: Check R3 (destination side)
R3# show ip route static
(no static routes)

R3# show ip route | include 10.1.1
(no route shown)

Problem Found: R3 has no route back to 10.1.1.0/24!

Forward path (PC1 → PC3):
PC1 → R1 → R2 → R3 → PC3 ✓

Return path (PC3 → PC1):
PC3 → R3 → ??? (no route) ✗

Step 3: Check R2
R2# show ip route static
S    192.168.2.0/24 [1/0] via 172.16.2.2
(no route to 10.1.1.0/24)

Problem Found: R2 also missing return route!

Solution on R2:
R2(config)# ip route 10.1.1.0 255.255.255.0 172.16.1.1

Solution on R3:
R3(config)# ip route 10.1.1.0 255.255.255.0 172.16.2.1

Verification:
PC> ping 192.168.2.10
Reply from 192.168.2.10: bytes=32 time=10ms TTL=125
✓ Both directions work!

Key Lesson:
Need routes in BOTH directions!
```

**Scenario 5: Default Route Problem**

```
Symptom:
Can't reach internet, but internal networks work

Problem Configuration:
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.5
(should be 203.0.113.1 - ISP router)

Diagnosis:

Step 1: Test connectivity
R1# ping 8.8.8.8
.....
Success rate is 0 percent (0/5)

Step 2: Check routing table
R1# show ip route | include Gateway
Gateway of last resort is 203.0.113.5 to network 0.0.0.0

R1# show ip route static
S*   0.0.0.0/0 [1/0] via 203.0.113.5

Step 3: Test next-hop
R1# ping 203.0.113.5
.....
Success rate is 0 percent (0/5)

Problem Found: Next-hop 203.0.113.5 doesn't exist!

Step 4: Verify correct gateway
R1# show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol
Serial0/0/1            203.0.113.2     YES manual up                    up

Check ISP documentation:
ISP gateway: 203.0.113.1

Solution:
R1(config)# no ip route 0.0.0.0 0.0.0.0 203.0.113.5
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.1

Verification:
R1# ping 8.8.8.8
!!!!!
Success rate is 100 percent (5/5)
✓ Internet access restored!
```

### 2.4 Systematic Troubleshooting Approach

**Step-by-Step Method:**

```
Step 1: Define the Problem
- What exactly doesn't work?
- Which destinations?
- Which sources?
- One direction or both?

Step 2: Gather Information
- Check routing tables
- Check interface status
- Test connectivity
- Check configuration

Step 3: Analyze Information
- Compare expected vs actual
- Identify differences
- Determine root cause

Step 4: Eliminate Causes
- Start with most likely
- Test hypotheses
- Narrow down to exact problem

Step 5: Implement Solution
- Make configuration change
- One change at a time
- Document what you did

Step 6: Verify Solution
- Test connectivity
- Verify routing table
- Confirm end-to-end

Step 7: Document
- Record problem
- Record solution
- Update documentation
```

---

## 3. Troubleshoot IPv6 Static and Default Routes

### การแก้ไขปัญหา IPv6 Static และ Default Routes

### 3.1 IPv6-Specific Considerations

**Differences from IPv4:**

```
1. IPv6 Routing Must Be Enabled
   Command: ipv6 unicast-routing
   Without this: Router won't forward IPv6 packets!

2. Link-Local Addresses
   - Often used as next-hop
   - Requires exit interface specification
   - Must use fully specified route

3. Multiple Addresses Per Interface
   - Link-local (fe80::/10)
   - Global unicast
   - Unique local (fc00::/7)

4. No ARP (uses NDP)
   - Neighbor Discovery Protocol
   - ICMPv6-based
   - Different troubleshooting

5. Longer Addresses
   - More typing
   - More chance for typos
   - Harder to spot errors
```

### 3.2 IPv6 Troubleshooting Commands

**Essential IPv6 Commands:**

```
show ipv6 route
show ipv6 route static
show ipv6 route <network>
show ipv6 interface brief
show ipv6 interface <interface>
show running-config | section ipv6 route
ping ipv6 <address>
traceroute ipv6 <address>
show ipv6 neighbors (like show arp for IPv4)
show cdp neighbors (works for IPv6 too)
```

### 3.3 IPv6 Troubleshooting Scenarios

**Scenario 1: IPv6 Routing Not Enabled**

```
Symptom:
IPv6 static routes configured but not working

Problem:
R1(config)# ipv6 route 2001:db8:3::/64 2001:db8:12::2
R1# show ipv6 route
(empty table)

Diagnosis:

Step 1: Check IPv6 routing table
R1# show ipv6 route
% IPv6 routing table does not exist

Problem Found: IPv6 routing not enabled!

Step 2: Check configuration
R1# show running-config | include ipv6
ipv6 route 2001:DB8:3::/64 2001:DB8:12::2
(no ipv6 unicast-routing command)

Solution:
R1(config)# ipv6 unicast-routing

Verification:
R1# show ipv6 route
IPv6 Routing Table - default - 8 entries
Codes: C - Connected, L - Local, S - Static, ...

S   2001:DB8:3::/64 [1/0]
     via 2001:DB8:12::2
✓ Route now in table!

Critical: ALWAYS enable ipv6 unicast-routing first!
```

**Scenario 2: Link-Local Without Exit Interface**

```
Symptom:
IPv6 static route configured but not in table

Problem Configuration:
R1(config)# ipv6 route 2001:db8:3::/64 fe80::2
(missing exit interface)

Diagnosis:

Step 1: Check routing table
R1# show ipv6 route static
(no routes shown)

Step 2: Check configuration
R1# show running-config | section ipv6 route
ipv6 route 2001:DB8:3::/64 FE80::2

Step 3: Why rejected?
Link-local addresses (fe80::) only unique per link
Router doesn't know which interface!

Problem Found: Link-local needs exit interface!

Solution:
R1(config)# no ipv6 route 2001:db8:3::/64 fe80::2
R1(config)# ipv6 route 2001:db8:3::/64 Serial0/0/0 fe80::2

Verification:
R1# show ipv6 route static
S   2001:DB8:3::/64 [1/0]
     via FE80::2, Serial0/0/0
✓ Now shows in table with interface!
```

**Scenario 3: Wrong Next-Hop IPv6 Address**

```
Symptom:
Can't reach IPv6 destination

Problem Configuration:
R1(config)# ipv6 route 2001:db8:3::/64 2001:db8:12::3
(should be ::2)

Diagnosis:

Step 1: Test connectivity
R1# ping ipv6 2001:db8:3::10
% Destination unreachable

Step 2: Check routing table
R1# show ipv6 route 2001:db8:3::/64
Routing entry for 2001:DB8:3::/64
  Known via "static", distance 1, metric 0
  Route count is 1/1, share count 0
  Routing paths:
    2001:DB8:12::3

Step 3: Test next-hop
R1# ping ipv6 2001:db8:12::3
% Destination unreachable

Problem Found: Next-hop doesn't exist!

Step 4: Verify correct next-hop
Check topology/documentation
Correct next-hop: 2001:db8:12::2

Solution:
R1(config)# no ipv6 route 2001:db8:3::/64 2001:db8:12::3
R1(config)# ipv6 route 2001:db8:3::/64 2001:db8:12::2

Verification:
R1# ping ipv6 2001:db8:3::10
!!!!!
Success rate is 100 percent (5/5)
✓ Works!
```

**Scenario 4: Wrong Prefix Length**

```
Symptom:
Some IPv6 addresses work, others don't

Problem Configuration:
R1(config)# ipv6 route 2001:db8:3::/96 2001:db8:12::2
(should be /64)

Diagnosis:

Step 1: Test connectivity
R1# ping ipv6 2001:db8:3::10
!!!!!
Success rate is 100 percent (5/5)
✓ Works

R1# ping ipv6 2001:db8:3::1:10
% Destination unreachable
✗ Fails

Step 2: Check routing table
R1# show ipv6 route static
S   2001:DB8:3::/96 [1/0]
     via 2001:DB8:12::2

Problem Found: /96 prefix too long!
Only covers first 96 bits
2001:db8:3::0 to 2001:db8:3::ffff:ffff (only)
Doesn't cover 2001:db8:3::1:0 and beyond

Should be /64:
Covers 2001:db8:3:0::/64
All 2001:db8:3::<anything>

Solution:
R1(config)# no ipv6 route 2001:db8:3::/96 2001:db8:12::2
R1(config)# ipv6 route 2001:db8:3::/64 2001:db8:12::2

Verification:
R1# ping ipv6 2001:db8:3::1:10
!!!!!
Success rate is 100 percent (5/5)
✓ Now works!
```

**Scenario 5: IPv6 Default Route Issue**

```
Symptom:
Internal IPv6 works, internet doesn't

Problem Configuration:
Missing IPv6 default route

Diagnosis:

Step 1: Test internet connectivity
R1# ping ipv6 2001:4860:4860::8888
(Google DNS)
% Destination unreachable

Step 2: Check default route
R1# show ipv6 route | include default
(nothing shown)

R1# show ipv6 route static
S   2001:DB8:1::/64 [1/0]
     via 2001:DB8:12::2
S   2001:DB8:3::/64 [1/0]
     via 2001:DB8:12::2

Problem Found: No default route!

Step 3: Check ISP interface
R1# show ipv6 interface brief
Serial0/0/1            [up/up]
    FE80::1
    2001:DB8:F:F::1

ISP gateway: 2001:db8:f:f::2

Solution:
R1(config)# ipv6 route ::/0 2001:db8:f:f::2

Verification:
R1# show ipv6 route static
S   ::/0 [1/0]
     via 2001:DB8:F:F::2
S   2001:DB8:1::/64 [1/0]
     via 2001:DB8:12::2
S   2001:DB8:3::/64 [1/0]
     via 2001:DB8:12::2

R1# ping ipv6 2001:4860:4860::8888
!!!!!
Success rate is 100 percent (5/5)
✓ Internet works!
```

---

## 4. Common Issues and Solutions

### ปัญหาทั่วไปและวิธีแก้ไข

### 4.1 Configuration Errors

**Issue: Typos in IP Addresses**

```
Problem:
R1(config)# ip route 192.168.2.0 255.255.255.0 172.16.1.12
(should be 172.16.1.2)

Symptoms:
- Route not in table
- Or route present but doesn't work

Prevention:
✓ Double-check IP addresses
✓ Copy from documentation
✓ Use tab completion (IOS)
✓ Verify with show commands

Detection:
show running-config | section ip route
Compare with topology diagram

Solution:
Remove wrong route, add correct one
```

**Issue: Wrong Subnet Mask**

```
Problem:
255.255.255.128 instead of 255.255.255.0
255.255.0.0 instead of 255.255.255.0

Effect:
- Covers wrong address range
- Some destinations work, others don't

Prevention:
✓ Calculate subnet carefully
✓ Use CIDR notation mentally (/24, /25, etc.)
✓ Verify with subnet calculator

Solution:
Reconfigure with correct mask
```

**Issue: Wrong Exit Interface**

```
Problem:
R1(config)# ip route 192.168.2.0 255.255.255.0 Serial0/0/1
(should be Serial0/0/0)

Effect:
- Packet sent out wrong interface
- Reaches wrong destination
- Or dropped (if interface down)

Detection:
traceroute shows unexpected path
Check show ip route output

Solution:
Correct interface specification
```

### 4.2 Topology Issues

**Issue: Interface Down**

```
Problem:
Interface administratively down or physically down

Symptoms:
- Static route removed from table
- No connectivity

Detection:
R1# show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol
Serial0/0/0            172.16.1.1      YES manual administratively down down

Solutions:

If administratively down:
R1(config)# interface serial0/0/0
R1(config-if)# no shutdown

If physically down:
- Check cable connection
- Try different cable
- Check hardware
- Check other end
```

**Issue: Next-Hop Not Reachable**

```
Problem:
Next-hop router unreachable

Causes:
- No route to next-hop subnet
- Next-hop interface down
- Next-hop router offline

Detection:
- Static route not in table
- Or present but ping fails

Debug:
R1# ping <next-hop>
If fails:
R1# show ip route | include <next-hop-network>
If no route:
- Add route to next-hop subnet
- Or check interface status

Solution:
Ensure route to next-hop subnet exists
```

### 4.3 Routing Logic Errors

**Issue: Missing Return Route**

```
Problem:
Forward path works, return path fails

Example:
PC1 → R1 → R2 → R3 → PC3 ✓
PC3 → R3 → ??? (no route) → PC1 ✗

Detection:
- ping from source fails
- ping from intermediate router works
- Check routing tables on all routers

Solution:
Add static routes in BOTH directions

R1: Route to PC3's network
R3: Route to PC1's network
R2: Routes to both (if needed)
```

**Issue: Routing Loop**

```
Problem:
Packets loop between routers

Example:
R1 routes to R2
R2 routes to R3
R3 routes back to R1
= Loop!

Detection:
traceroute shows repeating routers
Packets TTL expires

Prevention:
✓ Careful route planning
✓ Consistent next-hops
✓ Verify end-to-end path

Solution:
Correct routes to break loop
Use proper topology design
```

**Issue: Wrong Administrative Distance**

```
Problem:
Floating static not activating

Example:
Primary EIGRP (AD=90)
Backup static (AD=85)  ← Wrong! Lower than EIGRP
Should be AD=95

Effect:
Static always used (wrong!)

Solution:
R1(config)# no ip route 10.3.3.0 255.255.255.0 10.1.3.2 85
R1(config)# ip route 10.3.3.0 255.255.255.0 10.1.3.2 95
```

---

## 5. Troubleshooting Methodology

### วิธีการแก้ปัญหา

### 5.1 Structured Approach

**OSI Model Approach (Bottom-Up):**

```
Layer 1 (Physical):
✓ Cable connected?
✓ Interface lights on?
✓ Right cable type?

Layer 2 (Data Link):
✓ Interface up/up?
✓ Correct encapsulation?
✓ No errors on interface?

Layer 3 (Network):
✓ IP address correct?
✓ Route in routing table?
✓ Next-hop reachable?

Layer 4+ (Transport/Application):
✓ Firewall/ACL blocking?
✓ Service running?
✓ Application working?
```

**Divide and Conquer:**

```
PC1 → R1 → R2 → R3 → PC3

Test from middle (R2):
- Can R2 ping PC1? Yes → R1 direction OK
- Can R2 ping PC3? No → Problem R2→R3

Focus on R2-R3 segment
- Check R2's route to PC3
- Check R3's interface
- Check R2-R3 link
```

**Top-Down vs Bottom-Up:**

```
Bottom-Up (Physical first):
1. Check cables and interfaces
2. Check Layer 2
3. Check routing
Pro: Finds physical issues first
Con: Slower if problem is config

Top-Down (Logical first):
1. Check routing table
2. Check configuration
3. Check interfaces if needed
Pro: Faster for config issues
Con: May miss physical issues

Best: Use experience to choose
Physical problem suspected? Bottom-up
Config problem suspected? Top-down
```

### 5.2 Documentation

**What to Document:**

```
Problem Report:
- Date/time
- Reporter/affected users
- Symptoms (exact description)
- Which services affected
- Any recent changes

Troubleshooting Steps:
- Commands used
- Results observed
- Tests performed
- Hypotheses tested

Solution:
- Root cause identified
- Configuration changes made
- Commands used
- Results verified

Follow-up:
- Problem resolved (Y/N)
- Time to resolution
- Lessons learned
- Preventive measures
```

**Example Documentation:**

```
Problem: No connectivity to 192.168.2.0/24 network
Date: 2025-11-18 14:30
Reported by: User at 10.1.1.10

Symptoms:
- Ping to 192.168.2.10 fails
- All 192.168.2.x unreachable
- Local network (10.1.1.x) works fine

Troubleshooting:
14:35 - show ip route on R1
        Static route present: S 192.168.2.0/24 [1/0] via 172.16.1.2

14:37 - ping 192.168.2.10 from R1
        Success! (R1 can reach)

14:40 - show ip route on R3
        No route to 10.1.1.0/24 (return path missing!)

Root Cause:
Missing static route on R3 for return traffic

Solution:
14:45 - R3(config)# ip route 10.1.1.0 255.255.255.0 172.16.2.1

Verification:
14:47 - ping 192.168.2.10 from PC1
        Success rate 100%

Resolved: 14:47
Duration: 17 minutes

Lesson Learned:
Always verify bidirectional routing
Add reminder to routing checklist
```

---

## สรุป

### สิ่งที่ได้เรียนรู้ใน Module 16:

✅ **Packet Processing:**

- How routers process packets with static routes
- Step-by-step forwarding process
- De-encapsulation and re-encapsulation
- Layer 2 changes at each hop

✅ **IPv4 Troubleshooting:**

- Essential troubleshooting commands
- Common configuration errors
- Topology issues
- Systematic approach
- Real-world scenarios

✅ **IPv6 Troubleshooting:**

- IPv6-specific considerations
- ipv6 unicast-routing requirement
- Link-local issues
- IPv6 verification commands

✅ **Common Issues:**

- Configuration errors (typos, wrong masks)
- Topology issues (interfaces down)
- Routing logic errors (missing return routes)
- Next-hop problems

✅ **Methodology:**

- Structured troubleshooting approach
- OSI model (bottom-up)
- Divide and conquer
- Documentation importance

---

## Essential Troubleshooting Commands

### IPv4:

```
show ip route
show ip route static
show ip route <network>
show ip interface brief
show running-config | section ip route
ping <destination>
traceroute <destination>
show cdp neighbors
show arp
```

### IPv6:

```
show ipv6 route
show ipv6 route static
show ipv6 route <network>
show ipv6 interface brief
show running-config | section ipv6 route
ping ipv6 <destination>
traceroute ipv6 <destination>
show ipv6 neighbors
```

---

## Troubleshooting Checklist

### When Static Route Not Working:

```
☑ Is interface up/up?
☑ Is IP address correct?
☑ Is static route in routing table?
☑ Is next-hop reachable?
☑ Is next-hop correct?
☑ Is subnet mask correct?
☑ Is exit interface correct?
☑ Is return route configured?
☑ Any ACLs blocking?
☑ Can intermediate routers reach destination?
```

### IPv6 Specific:

```
☑ Is ipv6 unicast-routing enabled?
☑ If using link-local, is interface specified?
☑ Is prefix length correct?
☑ Is IPv6 enabled on interfaces?
☑ Are IPv6 addresses correct?
```

---

## Best Practices

### Prevention:

```
✅ Double-check all configurations
✅ Use consistent documentation
✅ Test after each change
✅ Configure bidirectional routes
✅ Use meaningful descriptions
✅ Save configurations
✅ Keep topology diagrams updated
```

### Troubleshooting:

```
✅ Use systematic approach
✅ Check one thing at a time
✅ Document everything
✅ Verify before and after
✅ Start with most likely cause
✅ Don't skip basic checks
✅ Test end-to-end
```

### Documentation:

```
✅ Keep accurate topology diagrams
✅ Document all static routes
✅ Record IP addressing scheme
✅ Note any special configurations
✅ Update after changes
✅ Include troubleshooting notes
```

---

## คำถามทบทวน

1. Router ตรวจสอบอะไรก่อนเมื่อรับ packet?
2. Static route ไม่อยู่ใน routing table เพราะอะไรได้บ้าง?
3. Command อะไรใช้ดู routing table?
4. traceroute ช่วย troubleshoot อย่างไร?
5. IPv6 static route ต้อง enable อะไรก่อน?
6. Link-local next-hop ต้องระบุอะไรเพิ่ม?
7. Missing return route มีผลอย่างไร?
8. show cdp neighbors ใช้ทำอะไร?
9. Bottom-up troubleshooting คืออะไร?
10. ควร document อะไรเมื่อแก้ปัญหา?

---

## เฉลยคำถาม

1. **Router checks:** Destination MAC address (Layer 2) first
2. **Route not in table:** Next-hop unreachable, interface down, wrong configuration
3. **View routing table:** show ip route, show ipv6 route
4. **traceroute:** Shows path packets take, identifies where packets stop
5. **IPv6 requirement:** ipv6 unicast-routing (global config)
6. **Link-local:** Must specify exit interface (fully specified)
7. **Missing return route:** Forward works, return fails (one direction only)
8. **show cdp neighbors:** Verify physical connectivity to adjacent Cisco devices
9. **Bottom-up:** Start Layer 1 (physical) work up to Layer 7
10. **Document:** Problem, symptoms, troubleshooting steps, solution, results

---

**Congratulations!** คุณได้จบ Module สุดท้ายของ CCNA 2: Switching, Routing, and Wireless Essentials แล้ว!

ทักษะ troubleshooting ที่ได้เรียนรู้จะเป็นประโยชน์อย่างมากในการทำงานจริง จำไว้ว่า "The best way to gain network troubleshooting skills is simple: always be troubleshooting"

---

**เอกสารจัดทำโดย:** Claude (Anthropic AI)  
**Module:** CCNA 2 Module 16 - Troubleshoot Static and Default Routes  
**Version:** v7.02 SRWE  
**วันที่อัพเดท:** พฤศจิกายน 2025