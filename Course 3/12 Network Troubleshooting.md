# CCNA Course 3 - Module 12: Network Troubleshooting

## การแก้ไขปัญหาเครือข่าย

---

## 12.1 Network Documentation

### Importance of Documentation

**Why Document:**

```
✅ Troubleshooting (faster problem resolution)
✅ Planning (upgrades, changes)
✅ Training (new staff onboarding)
✅ Compliance (regulatory requirements)
✅ Knowledge transfer (prevent single point of knowledge)
✅ Change management (track modifications)
✅ Disaster recovery (rebuild network)
```

---

### Types of Network Documentation

#### 1. Network Topology Diagrams

**Physical Topology:**

```
- Shows physical layout
- Devices, cables, connections
- Rack locations, building layouts

Information:
  - Device models (Catalyst 2960, ISR 4321)
  - Cable types (Cat6, fiber)
  - Port numbers (Gi0/1, Fa0/24)
  - Distances
  - Rack locations (Building A, Rack 5)
```

**Logical Topology:**

```
- Shows logical relationships
- IP addresses, VLANs, routing
- Does not show physical details

Information:
  - IP addressing (192.168.1.0/24)
  - VLANs (VLAN 10, 20, 30)
  - Routing protocols (OSPF areas)
  - Security zones (DMZ, Internal, External)
  - Subnets
```

---

#### 2. Network Configuration Files

**Configuration Backups:**

```
✅ Startup-config
✅ Running-config
✅ Version control (dated copies)
✅ Secure storage (encrypted, offsite)

Best Practices:
  - Backup before changes
  - Regular automated backups (daily/weekly)
  - Test restoration procedures
  - Keep multiple versions (30/60/90 days)
```

**Configuration Templates:**

```
- Standard configurations
- Baseline configs for each device type
- Access switch template
- Distribution switch template
- Router template

Benefits:
  ✅ Consistency
  ✅ Faster deployment
  ✅ Reduced errors
```

---

#### 3. Baseline Configuration

**Purpose:**

```
- Document normal network behavior
- Reference for troubleshooting
- Compare current vs baseline
- Identify deviations
```

**What to Baseline:**

```
Network Performance:
  - Bandwidth utilization (%)
  - Latency (ms)
  - Packet loss (%)
  - CPU utilization (%)
  - Memory usage (%)
  - Interface errors
  - Broadcast/multicast rates

Protocol Information:
  - Routing tables
  - ARP tables
  - MAC address tables
  - Spanning Tree states
  - DHCP bindings

Time Periods:
  - Peak hours (9 AM - 5 PM)
  - Off-peak (nights, weekends)
  - Seasonal variations
```

**Tools for Baselining:**

```
- SNMP monitoring (Cacti, PRTG, SolarWinds)
- NetFlow (traffic analysis)
- show commands (snapshots)
- Protocol analyzers (Wireshark)
```

---

#### 4. End-System Documentation

**Inventory:**

```
Device Information:
  - Hostname/Computer name
  - IP address (static/DHCP)
  - MAC address
  - Operating system
  - Location (building, floor, room)
  - User assignment
  - Purchase date
  - Warranty information
  - Serial number
```

**Network Settings:**

```
- IP address
- Subnet mask
- Default gateway
- DNS servers
- VLAN membership
- Switch port
```

---

## 12.2 Troubleshooting Process

### Structured Troubleshooting Methods

#### 1. Bottom-Up Approach

**Process:**

```
Start from Layer 1 → Work up to Layer 7

Layer 1 (Physical):
  ☐ Cable plugged in?
  ☐ Link lights on?
  ☐ Correct cable type?
  
Layer 2 (Data Link):
  ☐ Switch port enabled?
  ☐ VLAN configuration correct?
  ☐ Spanning Tree blocking?
  
Layer 3 (Network):
  ☐ IP address correct?
  ☐ Subnet mask correct?
  ☐ Default gateway reachable?
  ☐ Routing correct?
  
Layers 4-7:
  ☐ Firewall rules?
  ☐ ACLs?
  ☐ Application issues?
```

**When to Use:**

```
✅ Physical problems suspected
✅ New installations
✅ After cable work
✅ "Nothing works" scenarios
```

**Advantages:**

```
✅ Systematic (nothing missed)
✅ Good for beginners
✅ Finds physical problems quickly
```

**Disadvantages:**

```
❌ Time-consuming
❌ May check unnecessary layers
```

---

#### 2. Top-Down Approach

**Process:**

```
Start from Layer 7 → Work down to Layer 1

Layer 7 (Application):
  ☐ Application error messages?
  ☐ Correct URL/server?
  ☐ Authentication working?
  
Layers 5-6 (Session/Presentation):
  ☐ Encryption working?
  
Layer 4 (Transport):
  ☐ TCP connection established?
  ☐ Port accessible?
  ☐ Firewall blocking?
  
Layer 3 (Network):
  ☐ Can ping server?
  ☐ Routing correct?
  
Layer 2/1:
  ☐ Physical connectivity?
```

**When to Use:**

```
✅ Application-specific problems
✅ Software issues suspected
✅ Physical layer verified
```

**Advantages:**

```
✅ Focuses on user issues
✅ Faster for application problems
```

**Disadvantages:**

```
❌ May miss lower-layer issues
❌ Requires application knowledge
```

---

#### 3. Divide-and-Conquer

**Process:**

```
Start in middle (usually Layer 3) → Narrow down

Step 1: Test Layer 3 (Network)
  Can ping default gateway?
    ↓ Yes              ↓ No
  Problem is        Problem is
  higher layers     lower layers
  (L4-L7)           (L1-L2)
```

**Example:**

```
Problem: Cannot access web server

Test: ping server
  Success → Problem in L4-L7 (firewall, application)
  Fail → Test ping default gateway
    Success → Problem is routing/remote
    Fail → Problem in L1-L3 (cable, IP config)
```

**When to Use:**

```
✅ Intermittent issues
✅ Experienced troubleshooters
✅ Time-critical situations
```

**Advantages:**

```
✅ Fastest method (when skilled)
✅ Efficient
✅ Flexible
```

**Disadvantages:**

```
❌ Requires experience
❌ May miss issues
```

---

#### 4. Follow-the-Path

**Process:**

```
Trace packet path from source to destination
Check each hop

Example (PC to Server):
  PC → Access Switch → Distribution Switch → Core → 
  Router → WAN → Remote Router → Remote Switch → Server
  
Check each device:
  ☐ Is it forwarding?
  ☐ Correct route/VLAN?
  ☐ No errors?
```

**When to Use:**

```
✅ Connectivity issues
✅ Routing problems
✅ Inter-VLAN issues
```

**Advantages:**

```
✅ Visualizes problem location
✅ Good for routing issues
✅ Systematic
```

---

#### 5. Substitution

**Process:**

```
Replace suspected component with known-good

Examples:
  - Swap cable
  - Swap switch port
  - Swap PC
  - Swap module
  
If problem disappears → Replaced component was faulty
If problem persists → Look elsewhere
```

**When to Use:**

```
✅ Hardware problems suspected
✅ Spare parts available
✅ Component easily replaceable
```

---

#### 6. Comparison

**Process:**

```
Compare working vs non-working

Compare:
  - Configuration
  - IP settings
  - Hardware
  - Software version
  
Find differences → Investigate
```

**Example:**

```
User A: Can access internet ✅
User B: Cannot access internet ❌

Compare:
  - IP addresses (same subnet?) ✅
  - Default gateway (same?) ✅
  - DNS servers (same?) ❌ Different!
  
Solution: Fix User B's DNS settings
```

---

### Troubleshooting Steps

**Seven-Step Process:**

```
1. Define the Problem
   - Gather information
   - Ask questions (5 Ws: Who, What, When, Where, Why)
   - Understand symptoms

2. Gather Information
   - Interview users
   - Check logs (syslog)
   - Run show commands
   - Check documentation/baseline

3. Analyze Information
   - Identify symptoms
   - Determine scope (one user? entire network?)
   - Look for patterns

4. Eliminate Possible Causes
   - Narrow down
   - Use appropriate troubleshooting method
   - Question assumptions

5. Propose Hypothesis
   - Most likely cause
   - Create action plan
   - Consider impact

6. Test Hypothesis
   - Implement solution (test environment first if possible)
   - Document changes
   - One change at a time

7. Solve Problem and Document
   - Verify solution works
   - Document problem, cause, solution
   - Update documentation
   - Communicate to stakeholders
```

---

## 12.3 Troubleshooting Tools

### Software Tools

#### 1. ping

**Purpose:**

```
- Test reachability
- Measure round-trip time (RTT)
- ICMP Echo Request/Reply
```

**Basic Usage:**

```
PC> ping 8.8.8.8

Pinging 8.8.8.8 with 32 bytes of data:
Reply from 8.8.8.8: bytes=32 time=15ms TTL=117
Reply from 8.8.8.8: bytes=32 time=14ms TTL=117
Reply from 8.8.8.8: bytes=32 time=15ms TTL=117
Reply from 8.8.8.8: bytes=32 time=16ms TTL=117

Ping statistics for 8.8.8.8:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
Approximate round trip times in milli-seconds:
    Minimum = 14ms, Maximum = 16ms, Average = 15ms
```

**Extended Ping (Cisco):**

```
Router# ping
Protocol [ip]: 
Target IP address: 8.8.8.8
Repeat count [5]: 100
Datagram size [100]: 1500
Timeout in seconds [2]: 
Extended commands [n]: y
Source address or interface: 192.168.1.1
Type of service [0]: 
Set DF bit in IP header? [no]: 
Validate reply data? [no]: 
Data pattern [0xABCD]: 
Loose, Strict, Record, Timestamp, Verbose[none]: 
Sweep range of sizes [n]: 

Options:
  - Repeat count: Number of packets
  - Datagram size: Test MTU
  - Source: Specify source IP
  - DF bit: Test path MTU
```

**Results Interpretation:**

```
! (exclamation): Success
. (period): Timeout
U: Destination unreachable
C: Congestion experienced
I: User interrupted
?: Unknown packet type
&: Packet lifetime exceeded
```

---

#### 2. traceroute / tracert

**Purpose:**

```
- Trace path to destination
- Identify where packets stop
- Measure latency per hop
```

**Windows (tracert):**

```
C:\> tracert google.com

Tracing route to google.com [142.250.185.78]
over a maximum of 30 hops:

  1    <1 ms    <1 ms    <1 ms  192.168.1.1
  2     2 ms     2 ms     2 ms  10.0.0.1
  3     5 ms     5 ms     5 ms  203.0.113.1
  4    10 ms    11 ms    10 ms  isp-gateway.net [198.51.100.1]
  5    15 ms    14 ms    15 ms  google-peer.net [142.250.185.78]

Trace complete.
```

**Cisco (traceroute):**

```
Router# traceroute 8.8.8.8

Tracing the route to 8.8.8.8

  1 192.168.1.1 4 msec 4 msec 4 msec
  2 10.0.0.1 8 msec 8 msec 8 msec
  3 203.0.113.1 12 msec 12 msec 12 msec
  4 8.8.8.8 16 msec 16 msec 16 msec
```

**Symbols:**

```
* * *: Timeout at hop (possible firewall)
!H: Host unreachable
!N: Network unreachable
!P: Protocol unreachable
!A: Administratively prohibited
```

---

#### 3. ipconfig / ifconfig / ip

**Windows (ipconfig):**

```
C:\> ipconfig /all

Ethernet adapter Local Area Connection:

   Connection-specific DNS Suffix  : company.local
   Description . . . . . . . . . . : Intel Ethernet Adapter
   Physical Address. . . . . . . . : 00-1A-2B-3C-4D-5E
   DHCP Enabled. . . . . . . . . . : Yes
   IPv4 Address. . . . . . . . . . : 192.168.1.100
   Subnet Mask . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . : 192.168.1.1
   DNS Servers . . . . . . . . . . : 8.8.8.8
                                     8.8.4.4
```

**Common Commands:**

```
ipconfig                Show basic info
ipconfig /all           Show detailed info
ipconfig /release       Release DHCP address
ipconfig /renew         Renew DHCP address
ipconfig /flushdns      Clear DNS cache
ipconfig /displaydns    Show DNS cache
```

**Linux (ip):**

```
$ ip addr show

2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500
    link/ether 00:1a:2b:3c:4d:5e brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.100/24 brd 192.168.1.255 scope global eth0
    inet6 fe80::21a:2bff:fe3c:4d5e/64 scope link
```

**Common Commands:**

```
ip addr show            Show IP addresses
ip link show            Show interfaces
ip route show           Show routing table
ip neigh show           Show ARP table
```

---

#### 4. nslookup / dig

**nslookup (Windows/Linux):**

```
C:\> nslookup google.com

Server:  dns.company.local
Address:  192.168.1.1

Non-authoritative answer:
Name:    google.com
Addresses:  142.250.185.78
            2404:6800:4003:c00::71
```

**Specify DNS Server:**

```
C:\> nslookup google.com 8.8.8.8

Server:  google-public-dns-a.google.com
Address:  8.8.8.8

Non-authoritative answer:
Name:    google.com
Address:  142.250.185.78
```

---

#### 5. netstat

**Purpose:**

```
- Show active connections
- Show listening ports
- Network statistics
```

**Common Commands:**

```
netstat -a              All connections and listening ports
netstat -n              Numeric (no name resolution)
netstat -r              Routing table
netstat -s              Statistics per protocol

Combined:
netstat -an             All connections, numeric
netstat -ano            + Process ID (Windows)
```

**Example Output:**

```
C:\> netstat -an

Active Connections

  Proto  Local Address          Foreign Address        State
  TCP    0.0.0.0:80            0.0.0.0:0              LISTENING
  TCP    192.168.1.100:49152   142.250.185.78:443     ESTABLISHED
  TCP    192.168.1.100:49153   13.107.42.14:443       ESTABLISHED
  UDP    0.0.0.0:53            *:*
  UDP    192.168.1.100:68      *:*
```

---

#### 6. arp

**Purpose:**

```
- Show/modify ARP cache
- Map IP to MAC addresses
```

**Commands:**

```
Windows:
  arp -a                Show ARP table
  arp -d                Delete entry
  arp -s                Add static entry

Linux:
  ip neigh show         Show ARP table
  ip neigh flush        Clear ARP cache
```

**Example:**

```
C:\> arp -a

Interface: 192.168.1.100 --- 0xa
  Internet Address      Physical Address      Type
  192.168.1.1           00-11-22-33-44-55     dynamic
  192.168.1.50          aa-bb-cc-dd-ee-ff     dynamic
  192.168.1.255         ff-ff-ff-ff-ff-ff     static
```

---

### Cisco IOS Troubleshooting Commands

#### Layer 1 Troubleshooting

**show interfaces:**

```
Router# show interfaces gigabitEthernet 0/0

GigabitEthernet0/0 is up, line protocol is up 
  Hardware is iGbE, address is 0011.2233.4455 (bia 0011.2233.4455)
  Internet address is 192.168.1.1/24
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec
  Encapsulation ARPA, loopback not set
  Keepalive set (10 sec)
  Full-duplex, 1000Mb/s, media type is RJ45
  [... statistics ...]
  5 minute input rate 1000 bits/sec, 2 packets/sec
  5 minute output rate 2000 bits/sec, 3 packets/sec
     123456 packets input, 12345678 bytes, 0 no buffer
     Received 1234 broadcasts (567 multicasts)
     0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 567 multicast, 0 pause input
     234567 packets output, 23456789 bytes, 0 underruns
     0 output errors, 0 collisions, 0 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out

Key Fields:
  "is up, line protocol is up" = Layer 1 and 2 OK
  Input/Output errors = Problems
  CRC errors = Cable issues
  Collisions = Duplex mismatch
  Runts/Giants = Malformed frames
```

**Interface States:**

```
State                          Meaning                  Layer
------------------------------------------------------------------------
up, line protocol is up        Working                 1 & 2 OK
up, line protocol is down      L1 OK, L2 problem       1 OK, 2 fail
down, line protocol is down    Cable/interface issue   1 fail
admin down, line protocol down Shutdown                Disabled
------------------------------------------------------------------------
```

**show controllers:**

```
Router# show controllers gigabitEthernet 0/0

Interface GigabitEthernet0/0
Hardware is 88E1111
DTE V.35 clocks stopped
[... detailed hardware info ...]

Use: Check physical layer details (cable type, DCE/DTE)
```

---

#### Layer 2 Troubleshooting

**show mac address-table:**

```
Switch# show mac address-table

          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    0011.2233.4455    DYNAMIC     Gi0/1
   1    aabb.ccdd.eeff    DYNAMIC     Gi0/2
  10    1122.3344.5566    DYNAMIC     Gi0/10
 All    ffff.ffff.ffff    STATIC      CPU
```

**show spanning-tree:**

```
Switch# show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0011.2233.4455
             This bridge is the root
  
  Bridge ID  Priority    32769 (priority 32768 sys-id-ext 1)
             Address     0011.2233.4455

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- ----
Gi0/1               Desg FWD 4         128.1    P2p 
Gi0/2               Desg FWD 4         128.2    P2p

States: FWD (Forwarding), BLK (Blocking), LIS (Listening)
```

**show vlan:**

```
Switch# show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- ------------------------
1    default                          active    Gi0/1, Gi0/2
10   Users                            active    Gi0/10, Gi0/11
20   Servers                          active    Gi0/20
99   Management                       active    
```

---

#### Layer 3 Troubleshooting

**show ip interface brief:**

```
Router# show ip interface brief

Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0     192.168.1.1     YES manual up                    up      
GigabitEthernet0/1     10.0.0.1        YES manual up                    up      
Serial0/0/0            203.0.113.1     YES manual up                    up      
Serial0/0/1            unassigned      YES unset  administratively down down    

Status: up/down (Layer 1)
Protocol: up/down (Layer 2)
```

**show ip route:**

```
Router# show ip route

Codes: C - connected, S - static, O - OSPF, D - EIGRP
       * - candidate default

Gateway of last resort is 203.0.113.254 to network 0.0.0.0

S*    0.0.0.0/0 [1/0] via 203.0.113.254
      10.0.0.0/8 is variably subnetted, 5 subnets, 2 masks
C        10.1.1.0/24 is directly connected, GigabitEthernet0/1
L        10.1.1.1/32 is directly connected, GigabitEthernet0/1
O        10.2.2.0/24 [110/20] via 10.1.1.2, 00:05:23, GigabitEthernet0/1
C     192.168.1.0/24 is directly connected, GigabitEthernet0/0
L     192.168.1.1/32 is directly connected, GigabitEthernet0/0

Codes:
  C = Directly connected
  L = Local (interface address)
  S = Static route
  O = OSPF learned
  [AD/Metric] = Administrative Distance / Metric
```

**show arp / show ip arp:**

```
Router# show ip arp

Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  192.168.1.1             -   0011.2233.4455  ARPA   GigabitEthernet0/0
Internet  192.168.1.10            5   aabb.ccdd.eeff  ARPA   GigabitEthernet0/0
Internet  192.168.1.20           10   1122.3344.5566  ARPA   GigabitEthernet0/0

Age "-" means local interface
```

**show ip protocols:**

```
Router# show ip protocols

*** IP Routing is NSF aware ***

Routing Protocol is "ospf 1"
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Router ID 1.1.1.1
  Number of areas in this router is 1. 1 normal 0 stub 0 nssa
  Maximum path: 4
  Routing for Networks:
    192.168.1.0 0.0.0.255 area 0
    10.0.0.0 0.0.0.255 area 0
  Routing Information Sources:
    Gateway         Distance      Last Update
    2.2.2.2              110      00:05:23
  Distance: (default is 110)
```

---

#### ACL Troubleshooting

**show access-lists:**

```
Router# show access-lists

Standard IP access list 10
    10 permit 192.168.1.0, wildcard bits 0.0.0.255
    20 deny   any (5 matches)

Extended IP access list 100
    10 permit tcp any any eq www (1234 matches)
    20 permit tcp any any eq 443 (5678 matches)
    30 permit icmp any any (100 matches)
    40 deny ip any any (50 matches)

(matches) = Hit count
```

**show ip interface:**

```
Router# show ip interface gigabitEthernet 0/0

GigabitEthernet0/0 is up, line protocol is up
  Internet address is 192.168.1.1/24
  [...]
  Outgoing access list is not set
  Inbound  access list is 100
  [...]

Shows which ACL applied to interface
```

---

#### NAT Troubleshooting

**show ip nat translations:**

```
Router# show ip nat translations

Pro Inside global      Inside local       Outside local      Outside global
tcp 203.0.113.5:50001 192.168.1.10:50001 8.8.8.8:80         8.8.8.8:80
tcp 203.0.113.5:50002 192.168.1.20:50002 1.1.1.1:443        1.1.1.1:443
--- 203.0.113.10      192.168.1.100      ---                ---

---: Static NAT
tcp/udp/icmp: Dynamic NAT/PAT
```

**show ip nat statistics:**

```
Router# show ip nat statistics

Total active translations: 3 (1 static, 2 dynamic; 2 extended)
Peak translations: 10, occurred 00:05:23 ago
Outside interfaces:
  GigabitEthernet0/1
Inside interfaces:
  GigabitEthernet0/0
Hits: 1234  Misses: 5
Dynamic mappings:
-- Inside Source
[Id: 1] access-list 1 interface GigabitEthernet0/1 refcount 2

Hits: Successful translations
Misses: Failed translations (pool exhausted)
```

---

### Hardware Troubleshooting Tools

#### 1. Cable Tester

**Purpose:**

```
- Test cable continuity
- Identify cable faults
- Wire map (correct wiring?)
- Cable length
```

**Tests:**

```
✅ Continuity (wires connected?)
✅ Wire map (correct order?)
✅ Shorts (wires touching?)
✅ Opens (broken wires?)
✅ Split pairs (wrong pairing?)
✅ Cable length
```

---

#### 2. Multimeter

**Purpose:**

```
- Measure voltage, current, resistance
- Check power supplies
- Test cables (continuity)
```

**Uses:**

```
- PoE power delivery
- Power supply voltages
- Grounding issues
```

---

#### 3. Network Analyzer (Wireshark)

**Purpose:**

```
- Capture packets
- Analyze protocols
- Troubleshoot application issues
- Security analysis
```

**Common Uses:**

```
- See actual packet contents
- Identify protocol errors
- Measure latency
- Find broadcast storms
- Detect malicious traffic
```

**Example Filters:**

```
ip.addr == 192.168.1.100    Packets to/from this IP
tcp.port == 80              HTTP traffic
icmp                        Ping traffic
arp                         ARP packets
dns                         DNS queries
```

---

## 12.4 Common Network Issues

### Layer 1 Issues

**Physical Cable Problems:**

```
Symptoms:
  - Link down
  - Intermittent connection
  - High error rate

Causes:
  - Damaged cable
  - Wrong cable type (crossover vs straight-through)
  - Exceeds distance limits (100m for Cat6)
  - Poor connectors
  - EMI interference

Troubleshooting:
  ☐ Check cable plugged in
  ☐ Check link lights (LEDs)
  ☐ Try different cable
  ☐ Cable tester
  ☐ Check show interfaces (errors)
```

**Speed/Duplex Mismatch:**

```
Symptoms:
  - Slow performance
  - High collision rate
  - Late collisions

Causes:
  - One side auto, other fixed
  - Different duplex settings

Example:
  Switch: auto (negotiates to full-duplex)
  Device: Fixed 100/half-duplex
  Result: Mismatch, poor performance

Solution:
  - Both auto (recommended)
  - Or both manually set to same
```

---

### Layer 2 Issues

**VLAN Mismatch:**

```
Symptoms:
  - Cannot communicate with devices on same switch

Cause:
  - Devices in different VLANs

Troubleshooting:
  ☐ show vlan brief
  ☐ Verify port VLAN assignment
  ☐ Check trunk allowed VLANs

Solution:
  - Move port to correct VLAN
  - Or configure inter-VLAN routing
```

**Spanning Tree Issues:**

```
Symptoms:
  - Port in blocking state
  - Slow convergence
  - Loops

Troubleshooting:
  ☐ show spanning-tree
  ☐ Check root bridge
  ☐ Verify port states
  ☐ Check port priorities/costs

Common Issues:
  - Wrong root bridge
  - Port blocked (by design or error)
  - Portfast not enabled (delays)
```

**MAC Address Table Issues:**

```
Symptoms:
  - Traffic flooded
  - CAM table full

Troubleshooting:
  ☐ show mac address-table
  ☐ Check for excessive entries
  ☐ Look for MAC flapping

Causes:
  - MAC address flood attack
  - Loop (without STP)
```

---

### Layer 3 Issues

**IP Address Issues:**

```
1. Wrong IP Address:
   Symptom: Cannot communicate
   Check: ipconfig, show ip interface brief
   Solution: Correct IP address

2. Wrong Subnet Mask:
   Symptom: Can reach some devices, not others
   Example:
     Correct: 255.255.255.0 (/24)
     Wrong:   255.255.0.0 (/16)
     Impact: Wrong network calculation
   Solution: Correct subnet mask

3. Duplicate IP:
   Symptom: Intermittent connectivity
   Check: ARP table (one IP, multiple MACs)
   Solution: Change IP on one device

4. Wrong Default Gateway:
   Symptom: Cannot reach outside network
   Check: ipconfig, ping gateway
   Solution: Correct gateway address

5. Default Gateway Not Reachable:
   Symptom: Cannot reach outside network
   Check: ping gateway (fails)
   Solution: Fix Layer 1/2 to gateway
```

**Routing Issues:**

```
1. Missing Route:
   Symptom: Cannot reach specific network
   Check: show ip route (no route to destination)
   Solution: Add static route or configure routing protocol

2. Wrong Route:
   Symptom: Traffic goes wrong path
   Check: show ip route (check next-hop)
   Solution: Correct route entry

3. Routing Loop:
   Symptom: Packet TTL expires
   Check: traceroute (loops back)
   Solution: Fix routing configuration

4. Routing Protocol Issues:
   Symptom: Routes not learned
   Check:
     ☐ show ip protocols
     ☐ show ip ospf neighbor / show ip eigrp neighbors
     ☐ Verify network statements
     ☐ Check areas (OSPF)
   Solution: Fix routing protocol config
```

---

### Layer 4-7 Issues

**ACL Issues:**

```
Symptoms:
  - Traffic blocked unexpectedly

Troubleshooting:
  ☐ show access-lists (check hit counts)
  ☐ Check ACL logic (order matters)
  ☐ Implicit deny at end
  ☐ Check ACL applied direction (in/out)

Common Errors:
  - ACL too restrictive
  - Permit statement after deny all
  - Applied wrong direction
  - Applied to wrong interface
```

**NAT Issues:**

```
Symptoms:
  - Cannot reach Internet
  - Inbound connections fail

Troubleshooting:
  ☐ show ip nat translations (check mappings)
  ☐ show ip nat statistics (check misses)
  ☐ Verify inside/outside interfaces
  ☐ Check ACL for NAT

Common Issues:
  - Pool exhausted (misses > 0)
  - Wrong ACL (traffic not matched)
  - Inside/outside reversed
  - NAT before/after ACL (order)
```

**DNS Issues:**

```
Symptoms:
  - Cannot resolve names (www.google.com)
  - Can ping IP but not hostname

Troubleshooting:
  ☐ nslookup / dig
  ☐ Check DNS server IP (ipconfig /all)
  ☐ Ping DNS server (reachable?)
  ☐ Check firewall (UDP 53)

Solutions:
  - Fix DNS server address
  - Use alternate DNS (8.8.8.8, 1.1.1.1)
  - Clear DNS cache (ipconfig /flushdns)
```

---

## 12.5 Troubleshooting Scenarios

### Scenario 1: Cannot Access Internet

**Symptoms:**

```
- User cannot browse websites
- Can access local resources
```

**Troubleshooting:**

```
Step 1: Check IP configuration
  C:\> ipconfig /all
  ✅ IP address correct?
  ✅ Subnet mask correct?
  ✅ Default gateway correct?
  ✅ DNS servers configured?

Step 2: Test default gateway
  C:\> ping 192.168.1.1
  ✅ Success → Gateway reachable
  ❌ Fail → Layer 1/2 problem to gateway

Step 3: Test outside connectivity
  C:\> ping 8.8.8.8
  ✅ Success → Routing works, check DNS
  ❌ Fail → Check router/NAT

Step 4: Test DNS
  C:\> nslookup google.com
  ✅ Success → Problem solved (was DNS cache?)
  ❌ Fail → Fix DNS server address

Step 5: Check router NAT
  Router# show ip nat translations
  Router# show ip nat statistics
  Check: Translations created? Misses?
```

---

### Scenario 2: Slow Network Performance

**Symptoms:**

```
- Network very slow
- High latency
```

**Troubleshooting:**

```
Step 1: Verify physical layer
  ☐ Check interface errors (show interfaces)
  ☐ Duplex mismatch? (show interfaces)
  ☐ Cable issues?

Step 2: Check utilization
  ☐ Interface bandwidth saturated? (show interfaces)
  ☐ CPU high? (show processes cpu)
  ☐ Broadcast storm? (show interfaces - broadcasts)

Step 3: Compare to baseline
  ☐ Normal utilization vs current
  ☐ What changed recently?

Step 4: Check for loops
  ☐ show spanning-tree (look for TCN)
  ☐ Look for MAC flapping

Step 5: QoS issues
  ☐ Critical traffic getting priority?
  ☐ show policy-map interface
```

---

### Scenario 3: Intermittent Connectivity

**Symptoms:**

```
- Works sometimes, not always
- Ping packet loss
```

**Troubleshooting:**

```
Step 1: Extended ping
  Router# ping 8.8.8.8 repeat 1000
  Check: Packet loss percentage? Pattern?

Step 2: Physical issues
  ☐ Loose cable?
  ☐ Intermittent link down? (show interfaces)
  ☐ Input/output errors increasing?

Step 3: Duplicate IP
  Router# show ip arp | include 192.168.1.100
  Check: Multiple MAC addresses for one IP?

Step 4: Spanning Tree reconvergence
  ☐ show spanning-tree (topology changes?)
  ☐ Enable portfast on access ports

Step 5: Route flapping
  ☐ show ip route (route appears/disappears?)
  ☐ Check routing protocol neighbors
```

---

## Summary (สรุป)

Module 12 นี้เราได้เรียนรู้:

1. ✅ **Network Documentation**:
    
    - Topology diagrams (physical, logical)
    - Configuration backups
    - Baseline configuration
    - End-system documentation
    - Importance: Troubleshooting, planning, compliance
2. ✅ **Troubleshooting Methods**:
    
    - **Bottom-Up**: Layer 1 → Layer 7 (physical problems)
    - **Top-Down**: Layer 7 → Layer 1 (application problems)
    - **Divide-and-Conquer**: Start middle, narrow down (fastest)
    - **Follow-the-Path**: Trace packet path
    - **Substitution**: Replace component
    - **Comparison**: Compare working vs non-working
3. ✅ **Seven-Step Process**:
    
    1. Define problem
    2. Gather information
    3. Analyze information
    4. Eliminate possible causes
    5. Propose hypothesis
    6. Test hypothesis
    7. Solve and document
4. ✅ **Troubleshooting Tools**:
    
    - **ping**: Reachability, RTT
    - **traceroute**: Path tracing
    - **ipconfig/ifconfig**: Interface configuration
    - **nslookup/dig**: DNS queries
    - **netstat**: Connections, statistics
    - **arp**: ARP table
    - **show commands**: Cisco device info
    - **Cable tester**: Cable faults
    - **Wireshark**: Packet analysis
5. ✅ **Cisco IOS Commands**:
    
    - **Layer 1**: show interfaces, show controllers
    - **Layer 2**: show mac address-table, show spanning-tree, show vlan
    - **Layer 3**: show ip interface brief, show ip route, show ip arp
    - **ACL**: show access-lists, show ip interface
    - **NAT**: show ip nat translations, show ip nat statistics
6. ✅ **Common Issues**:
    
    - **Layer 1**: Cable problems, speed/duplex mismatch
    - **Layer 2**: VLAN mismatch, spanning tree, MAC table
    - **Layer 3**: Wrong IP/subnet/gateway, routing issues
    - **Layer 4-7**: ACL blocks, NAT problems, DNS failures

**สิ่งสำคัญที่ต้องจำ:**

- **Document everything** (topology, configs, baselines)
- **Systematic approach** (use structured methods)
- **One change at a time** (know what worked)
- **Verify solution** (test thoroughly)
- **Document solution** (help future troubleshooting)

**Troubleshooting Mindset:**

```
✅ Stay calm, methodical
✅ Gather facts (don't assume)
✅ Question the obvious (check basics)
✅ Use appropriate method for situation
✅ Verify fix (ensure problem really solved)
✅ Document for future
```

**Quick Reference - Cannot Connect:**

```
1. ping 127.0.0.1 → Test TCP/IP stack
2. ping default gateway → Test local connectivity
3. ping remote host (IP) → Test routing
4. ping remote host (name) → Test DNS
5. traceroute → Find where it stops
```

**Interface State Meanings:**

```
up, up               ✅ Working (L1 & L2 OK)
up, down             ⚠️  L1 OK, L2 problem (encapsulation, keepalive)
down, down           ❌ L1 problem (cable, interface)
admin down, down     🔌 Shutdown (disabled)
```

**Next Module:** Module 13 - Network Virtualization

---

**[ไฟล์ Module 12 สมบูรณ์แล้ว!]**