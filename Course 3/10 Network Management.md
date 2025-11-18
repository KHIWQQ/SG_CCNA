# CCNA Course 3 - Module 10: Network Management

## การจัดการเครือข่าย

---

## 10.1 Device Discovery with CDP

### What is CDP?

**คำจำกัดความ:**

- **Cisco Discovery Protocol (CDP)** = Cisco proprietary protocol
- Layer 2 protocol
- Discover directly connected Cisco devices
- ทำงานโดยอัตโนมัติ (enabled by default)
- ไม่ต้อง IP connectivity

**Purpose:**

```
- ค้นหา neighboring Cisco devices
- รวบรวมข้อมูล: hostname, IP, platform, capabilities
- Troubleshooting
- Network topology discovery
- Network documentation
```

---

### How CDP Works

**Mechanism:**

```
1. CDP enabled by default (global and per-interface)
2. Device ส่ง CDP advertisements ทุก 60 seconds
3. Multicast address: 01:00:0C:CC:CC:CC
4. Advertisements ไม่ forwarded (Layer 2 only)
5. Neighbors เก็บข้อมูล 180 seconds (holdtime)
```

**CDP Advertisement Contains:**

```
- Device ID (hostname)
- IP address(es)
- Platform (model: 2960, 3850, ISR 4321)
- Capabilities (Router, Switch, Phone, etc.)
- Interface (local and remote)
- Software version (IOS version)
- Duplex
- Power draw (PoE devices)
- VLAN information
- VTP domain
```

**CDP Timers:**

```
Timer               Default    Description
------------------------------------------------------------------------
Advertisement       60 sec     ส่ง CDP packets ทุก 60 วินาที
Holdtime            180 sec    เก็บข้อมูล neighbor 180 วินาที
                               (ถ้าไม่ได้รับ update → ลบ neighbor)
------------------------------------------------------------------------
```

---

### CDP Configuration

#### Enable/Disable CDP Globally

```
! Enable CDP globally (default)
Router(config)# cdp run

! Disable CDP globally
Router(config)# no cdp run
```

#### Enable/Disable CDP on Interface

```
! Disable on specific interface (security)
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# no cdp enable

! Enable on interface
Router(config-if)# cdp enable
```

#### Configure CDP Timers

```
! Change advertisement timer (default: 60)
Router(config)# cdp timer 30

! Change holdtime (default: 180)
Router(config)# cdp holdtime 120
```

---

### CDP Verification Commands

#### show cdp

**Purpose:** Show CDP status

```
Router# show cdp

Global CDP information:
    Sending CDP packets every 60 seconds
    Sending a holdtime value of 180 seconds
    Sending CDPv2 advertisements is enabled
```

---

#### show cdp neighbors

**Purpose:** Show summary of neighbors

```
Router# show cdp neighbors

Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                  S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone

Device ID        Local Intrfce     Holdtme    Capability  Platform  Port ID
Switch1          Gig 0/0           165        S I         WS-C2960  Gig 0/1
Router2          Gig 0/1           142        R S I       ISR4321   Gig 0/0
SEP001122334455  Gig 0/2           156        H P         IP Phone  Port 1

ฟิลด์สำคัญ:
- Device ID: Hostname ของ neighbor
- Local Intrfce: Interface บน local device
- Holdtme: เวลาคงเหลือก่อนลบข้อมูล (seconds)
- Capability: R=Router, S=Switch, H=Host, P=Phone
- Platform: Model
- Port ID: Interface บน neighbor device
```

---

#### show cdp neighbors detail

**Purpose:** Show detailed information

```
Router# show cdp neighbors detail

-------------------------
Device ID: Switch1
Entry address(es): 
  IP address: 192.168.1.10
Platform: cisco WS-C2960-24TT-L,  Capabilities: Switch IGMP 
Interface: GigabitEthernet0/0,  Port ID (outgoing port): GigabitEthernet0/1
Holdtime : 165 sec

Version :
Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE7
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2014 by Cisco Systems, Inc.

advertisement version: 2
VTP Management Domain: ''
Native VLAN: 1
Duplex: full
Power drawn: 15.400 Watts
Management address(es): 
  IP address: 192.168.1.10

-------------------------
[Additional neighbors...]

ข้อมูลเพิ่มเติม:
- IP address (management)
- IOS version
- VTP domain
- Native VLAN
- Duplex
- Power draw (PoE)
```

---

#### show cdp interface

**Purpose:** Show CDP status on interfaces

```
Router# show cdp interface

GigabitEthernet0/0 is up, line protocol is up
  Encapsulation ARPA
  Sending CDP packets every 60 seconds
  Holdtime is 180 seconds

GigabitEthernet0/1 is up, line protocol is up
  Encapsulation ARPA
  Sending CDP packets every 60 seconds
  Holdtime is 180 seconds

GigabitEthernet0/2 is administratively down, line protocol is down
  Encapsulation ARPA
  Sending CDP packets every 60 seconds
  Holdtime is 180 seconds
```

---

#### show cdp entry

**Purpose:** Show specific neighbor

```
Router# show cdp entry Switch1

-------------------------
Device ID: Switch1
Entry address(es): 
  IP address: 192.168.1.10
Platform: cisco WS-C2960-24TT-L,  Capabilities: Switch IGMP 
Interface: GigabitEthernet0/0,  Port ID (outgoing port): GigabitEthernet0/1
[...]
```

---

### CDP Security Considerations

**Security Risks:**

```
❌ Information disclosure
   - Reveals device types, IOS versions
   - Attackers use for reconnaissance
   
❌ No authentication
   - Anyone can receive CDP packets
   
❌ Enabled by default
   - May be forgotten
```

**Best Practices:**

```
1. Disable CDP on external/untrusted interfaces
   interface gigabitEthernet 0/0
    description === Internet ===
    no cdp enable

2. Disable globally if not needed
   no cdp run

3. Keep CDP on internal management interfaces
   - Useful for troubleshooting
   - Document network topology

4. Use CDP only in trusted network segments
```

**When to Disable:**

```
✅ Internet-facing interfaces
✅ Guest networks
✅ Untrusted VLANs
✅ DMZ interfaces
✅ Any interface facing untrusted networks
```

**When to Keep Enabled:**

```
✅ Internal management network
✅ Between network devices (switch-to-switch)
✅ Within trusted corporate network
```

---

## 10.2 Device Discovery with LLDP

### What is LLDP?

**คำจำกัดความ:**

- **Link Layer Discovery Protocol (LLDP)** = IEEE 802.1AB
- Open standard (not Cisco proprietary)
- Layer 2 protocol
- Similar to CDP but vendor-neutral
- Interoperates with non-Cisco devices

**CDP vs LLDP Comparison:**

```
Feature          CDP                  LLDP
------------------------------------------------------------------------
Standard         Cisco proprietary    IEEE 802.1AB (open)
Vendor           Cisco only           Multi-vendor
Default          Enabled              Disabled (Cisco devices)
Timers           60/180 sec           30/120 sec (default)
Multicast        01:00:0C:CC:CC:CC    01:80:C2:00:00:0E
Information      Similar              Similar
------------------------------------------------------------------------
```

**Why Use LLDP:**

```
✅ Multi-vendor environments
✅ Standards-based
✅ Interoperability (Cisco, HP, Juniper, etc.)
✅ Required for some features (VoIP, PoE negotiation)
```

---

### How LLDP Works

**Mechanism:**

```
1. LLDP disabled by default (Cisco)
2. When enabled, send advertisements every 30 seconds
3. Multicast address: 01:80:C2:00:00:0E
4. Neighbors เก็บข้อมูล 120 seconds (holdtime)
5. TLVs (Type-Length-Value) carry information
```

**LLDP Advertisement Contains (TLVs):**

```
Mandatory TLVs:
- Chassis ID (MAC address)
- Port ID (interface)
- Time to Live (TTL)

Optional TLVs:
- System name (hostname)
- System description (platform, IOS)
- System capabilities (router, switch, etc.)
- Management address (IP)
- Port description
- VLAN information
- Power (PoE)
```

**LLDP Modes:**

```
TX only:    Transmit advertisements only
RX only:    Receive advertisements only
TX/RX:      Both transmit and receive (normal)
Disabled:   No LLDP
```

---

### LLDP Configuration

#### Enable LLDP Globally

```
! Enable LLDP (required first)
Router(config)# lldp run
```

#### Configure LLDP on Interface

```
! Enable transmit and receive (default when LLDP enabled)
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# lldp transmit
Router(config-if)# lldp receive

! Disable LLDP on interface
Router(config-if)# no lldp transmit
Router(config-if)# no lldp receive
```

#### Configure LLDP Timers

```
! Change advertisement timer (default: 30)
Router(config)# lldp timer 20

! Change holdtime (default: 120)
Router(config)# lldp holdtime 90

! Reinitialize delay after disable/enable (default: 2)
Router(config)# lldp reinit 3
```

---

### LLDP Verification Commands

#### show lldp

**Purpose:** Show LLDP status

```
Router# show lldp

Global LLDP Information:
    Status: ACTIVE
    LLDP advertisements are sent every 30 seconds
    LLDP hold time advertised is 120 seconds
    LLDP interface reinitialisation delay is 2 seconds
```

---

#### show lldp neighbors

**Purpose:** Show summary of neighbors

```
Router# show lldp neighbors

Capability codes:
    (R) Router, (B) Bridge, (T) Telephone, (C) DOCSIS Cable Device
    (W) WLAN Access Point, (P) Repeater, (S) Station, (O) Other

Device ID           Local Intf     Hold-time  Capability      Port ID
Switch1             Gi0/0          120        B               Gi0/1
HP-Switch           Gi0/1          120        B               Port 24
AP3802              Gi0/2          120        W               Gi0

Total entries displayed: 3
```

---

#### show lldp neighbors detail

**Purpose:** Show detailed information

```
Router# show lldp neighbors detail

------------------------------------------------
Chassis id: 0026.0bd0.1234
Port id: Gi0/1
Port Description: GigabitEthernet0/1
System Name: Switch1

System Description: 
Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE7
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2014 by Cisco Systems, Inc.

Time remaining: 97 seconds
System Capabilities: B
Enabled Capabilities: B
Management Addresses:
    IP: 192.168.1.10
Auto Negotiation - supported, enabled
Physical media capabilities:
    1000baseT(FD)
    100base-TX(FD)
    100base-TX(HD)
    10base-T(FD)
    10base-T(HD)
Media Attachment Unit type: 30
Vlan ID: 1

------------------------------------------------
[Additional neighbors...]
```

---

#### show lldp interface

**Purpose:** Show LLDP status on interfaces

```
Router# show lldp interface

GigabitEthernet0/0:
    Tx: enabled
    Rx: enabled
    Tx state: IDLE
    Rx state: WAIT FOR FRAME

GigabitEthernet0/1:
    Tx: enabled
    Rx: enabled
    Tx state: IDLE
    Rx state: WAIT FOR FRAME
```

---

#### show lldp entry

**Purpose:** Show specific neighbor

```
Router# show lldp entry Switch1

Capability codes:
    (R) Router, (B) Bridge, (T) Telephone, (C) DOCSIS Cable Device
    (W) WLAN Access Point, (P) Repeater, (S) Station, (O) Other

------------------------------------------------
Local Intf: Gi0/0
Chassis id: 0026.0bd0.1234
Port id: Gi0/1
Port Description: GigabitEthernet0/1
System Name: Switch1
[...]
```

---

### CDP and LLDP Together

**Can Run Both:**

```
✅ CDP and LLDP can run simultaneously
✅ Useful in mixed environments

Configuration:
  cdp run              (for Cisco devices)
  lldp run             (for multi-vendor)
  
  Both protocols active
```

**Recommendation:**

```
Cisco-only network:
  → CDP (simpler, enabled by default)

Multi-vendor network:
  → LLDP (standards-based)

Mixed requirements:
  → Both CDP and LLDP
```

---

## 10.3 NTP (Network Time Protocol)

### What is NTP?

**คำจำกัดความ:**

- **Network Time Protocol (NTP)** = Protocol สำหรับ time synchronization
- Synchronize clocks across network
- Critical for logging, security, troubleshooting
- Uses UDP port 123

**Why Time Synchronization Matters:**

```
Important for:
✅ Accurate logs (correlate events across devices)
✅ Security certificates (validity periods)
✅ Authentication (time-based)
✅ Troubleshooting (sequence of events)
✅ Compliance (audit trails)

Example Problem:
  Router A (12:00:00): Link down on Gi0/1
  Router B (14:30:15): Link down on Gi0/0
  
  ❌ Times don't match → Hard to correlate!
  
  With NTP (both synced):
  Router A (12:00:00): Link down on Gi0/1
  Router B (12:00:00): Link down on Gi0/0
  
  ✅ Same time → Easy to see simultaneous failure!
```

---

### NTP Architecture

**NTP Hierarchy (Stratum):**

```
Stratum 0: Atomic clock, GPS receiver (reference clock)
           ↓
Stratum 1: NTP servers connected to Stratum 0 (primary)
           ↓
Stratum 2: NTP servers sync from Stratum 1
           ↓
Stratum 3: NTP servers sync from Stratum 2
           ↓
...
Stratum 15: Maximum (Stratum 16 = unsynchronized)

Lower stratum = More accurate
```

**Example:**

```
[GPS Clock] ← Stratum 0
     ↓
[NTP Server] ← Stratum 1 (time.nist.gov)
     ↓
[Company NTP Server] ← Stratum 2
     ↓
[Routers/Switches] ← Stratum 3
```

---

### NTP Modes

**1. Client Mode:**

```
- Device syncs from NTP server
- Most common mode
- One-way (client → server)

Example:
  Router (client) → NTP Server (stratum 2)
  Router becomes stratum 3
```

**2. Server Mode:**

```
- Device provides time to clients
- Higher stratum devices sync from it

Example:
  NTP Server (stratum 2) → Routers (stratum 3)
```

**3. Peer Mode (Symmetric Active):**

```
- Two devices sync with each other
- Both can be time source
- Redundancy

Example:
  Router A ↔ Router B (peers)
  Both sync from external server
  If external fails, sync from each other
```

**4. Broadcast/Multicast Mode:**

```
- Server broadcasts time
- Clients listen
- Less accurate (one-way)
- Lower overhead
```

---

### NTP Configuration

#### Configure NTP Server (Client Mode)

```
! Set NTP server (client mode)
Router(config)# ntp server 129.6.15.28

! Multiple servers (redundancy)
Router(config)# ntp server 129.6.15.28
Router(config)# ntp server 132.163.97.1

! Prefer one server
Router(config)# ntp server 129.6.15.28 prefer
```

---

#### Configure NTP Master (Server Mode)

```
! Device becomes NTP server (stratum)
Router(config)# ntp master 3
                           ↑
                    Stratum number (default: 8)

Use case: No external NTP server available
Device uses internal clock (less accurate)
```

---

#### Configure NTP Peer

```
! Configure peer relationship
Router(config)# ntp peer 10.1.1.2

Both devices sync with each other
```

---

#### Configure NTP Authentication

**Why Authentication:**

```
Security: Prevent rogue NTP servers
Ensure time from trusted source only
```

**Configuration:**

```
! Enable NTP authentication
Router(config)# ntp authenticate

! Define authentication key
Router(config)# ntp authentication-key 1 md5 MySecretKey123

! Trust the key
Router(config)# ntp trusted-key 1

! Specify server with authentication
Router(config)# ntp server 129.6.15.28 key 1
```

---

#### Configure NTP Access Control

```
! Restrict NTP access
Router(config)# ntp access-group serve-only 10

! ACL to allow specific servers
Router(config)# access-list 10 permit 192.168.1.0 0.0.0.255

Restrictions:
  peer:       Allow sync + provide time (full access)
  serve:      Provide time only (server mode)
  serve-only: Provide time, don't sync
  query-only: Allow queries only (show ntp)
```

---

#### Configure Time Zone and DST

```
! Set time zone
Router(config)# clock timezone PST -8

! Configure daylight saving time (DST)
Router(config)# clock summer-time PDT recurring

Full example:
Router(config)# clock timezone EST -5
Router(config)# clock summer-time EDT recurring
```

---

### NTP Verification

#### show ntp status

```
Router# show ntp status

Clock is synchronized, stratum 3, reference is 129.6.15.28
nominal freq is 250.0000 Hz, actual freq is 249.9995 Hz, precision is 2**6
ntp uptime is 36500 (1/100 of seconds), resolution is 4016
reference time is E0E0E0E0.A1234567 (12:30:15.123 UTC Mon Jan 1 2024)
clock offset is -2.1234 msec, root delay is 50.12 msec
root dispersion is 60.45 msec, peer dispersion is 1.23 msec
loopfilter state is 'CTRL' (Normal Controlled Loop), drift is -0.000001234 s/s
system poll interval is 64, last update was 32 sec ago.

ฟิลด์สำคัญ:
- synchronized: ✅ Synced with NTP server
- stratum 3: Accuracy level
- reference: NTP server IP
- clock offset: Difference from server (ms)
```

---

#### show ntp associations

```
Router# show ntp associations

  address         ref clock       st   when   poll reach  delay  offset   disp
*~129.6.15.28    .GPS.           1    32     64   377     50.1   -2.1     1.2
 ~132.163.97.1   .GPS.           1    45     64   377     75.3   -5.4     2.1
 * sys.peer, # selected, + candidate, - outlyer, x falseticker, ~ configured

ฟิลด์:
- *: System peer (currently syncing)
- ~: Configured server
- st: Stratum
- when: Seconds since last update
- poll: Poll interval (seconds)
- reach: Reachability (octal, 377 = 255 = all attempts successful)
- delay: Round-trip delay (ms)
- offset: Time offset (ms)
```

---

#### show clock

```
Router# show clock
*12:30:15.123 UTC Mon Jan 1 2024

Router# show clock detail
*12:30:15.123 UTC Mon Jan 1 2024
Time source is NTP
```

---

#### debug ntp

```
Router# debug ntp all
NTP debugging is on

*Jan 1 12:30:15.123: NTP: xmit packet to 129.6.15.28
*Jan 1 12:30:15.234: NTP: rcv packet from 129.6.15.28
*Jan 1 12:30:15.235: NTP: synced to 129.6.15.28, stratum 3

Router# no debug all
(Turn off when done - CPU intensive!)
```

---

### NTP Best Practices

**1. Use Multiple Servers:**

```
✅ At least 3 NTP servers (redundancy)
✅ Geographically diverse
✅ Reliable sources

Router(config)# ntp server 129.6.15.28 prefer
Router(config)# ntp server 132.163.97.1
Router(config)# ntp server 216.239.35.0
```

**2. Use Stratum 1/2 Sources:**

```
Public NTP servers:
  time.nist.gov (129.6.15.28)
  time.google.com
  pool.ntp.org
  
Organization:
  Deploy internal NTP servers (stratum 2)
  All devices sync from internal (stratum 3)
```

**3. Enable Authentication:**

```
✅ Prevent rogue NTP servers
✅ Ensure trusted time source

ntp authenticate
ntp authentication-key 1 md5 SecurePassword
ntp trusted-key 1
ntp server 192.168.1.10 key 1
```

**4. Configure Access Control:**

```
✅ Limit who can query/sync
✅ Prevent NTP amplification attacks

ntp access-group serve-only 10
access-list 10 permit 192.168.0.0 0.0.255.255
```

**5. Monitor NTP Status:**

```
✅ Verify synchronization
✅ Check stratum level
✅ Monitor offset (should be low, < 10ms)

show ntp status
show ntp associations
```

---

## 10.4 SNMP (Simple Network Management Protocol)

### What is SNMP?

**คำจำกัดความ:**

- **Simple Network Management Protocol** = Protocol สำหรับ network management
- Monitor and manage network devices
- Collect information (CPU, memory, interface stats)
- Configure devices remotely
- Send alerts (traps)

**SNMP Components:**

```
1. SNMP Manager (NMS - Network Management Station)
   - Software: SolarWinds, PRTG, Nagios, LibreNMS
   - Collects data from devices
   - Displays dashboards, alerts

2. SNMP Agent
   - Software on network devices (routers, switches)
   - Responds to manager queries
   - Sends traps (alerts)

3. MIB (Management Information Base)
   - Database of manageable objects
   - Tree structure (OIDs - Object Identifiers)
```

**How SNMP Works:**

```
[SNMP Manager] ──────────→ [Router (Agent)]
                GET request
                (What's your CPU?)

[SNMP Manager] ←────────── [Router (Agent)]
                GET response
                (CPU: 25%)

[SNMP Manager] ←────────── [Router (Agent)]
                TRAP
                (Alert: Link down!)
```

---

### SNMP Versions

**SNMPv1:**

```
- Original version (1988)
- Simple
- Security: Community strings (plaintext)
- ❌ No encryption
- ❌ Weak authentication
- ❌ Obsolete
```

**SNMPv2c:**

```
- Improved (1996)
- More efficient
- Bulk operations (GetBulk)
- Security: Community strings (still plaintext)
- ❌ No encryption
- ❌ Weak authentication
- ✅ Most commonly used (legacy)
```

**SNMPv3:**

```
- Secure version (1998)
- Authentication (username/password)
- Encryption (DES, 3DES, AES)
- Integrity (MD5, SHA)
- ✅ Strong security
- ✅ Recommended
- More complex configuration
```

**Version Comparison:**

```
Feature          v1          v2c         v3
------------------------------------------------------------------------
Security         Weak        Weak        Strong
Authentication   Community   Community   Username/Password
Encryption       No          No          Yes (DES, AES)
Integrity        No          No          Yes (MD5, SHA)
Bulk operations  No          Yes         Yes
Recommended      ❌          ⚠️          ✅
------------------------------------------------------------------------
```

---

### SNMP Operations

**GET:**

```
Manager → Agent: "What's the value of this OID?"
Agent → Manager: "Here's the value"

Example: Get interface status
```

**GET-NEXT:**

```
Manager → Agent: "What's the next OID after this?"
Agent → Manager: "Here's the next OID and value"

Example: Walk through MIB tree
```

**GET-BULK (v2c, v3):**

```
Manager → Agent: "Give me multiple values at once"
Agent → Manager: "Here are multiple values"

Example: Get all interface stats (faster)
```

**SET:**

```
Manager → Agent: "Set this OID to this value"
Agent → Manager: "Done" or "Error"

Example: Change interface description
Note: Requires write community (v1/v2c) or write permissions (v3)
```

**TRAP:**

```
Agent → Manager: "Alert! Something happened!"
No response expected (unreliable)

Example: Link down, high CPU
```

**INFORM (v2c, v3):**

```
Agent → Manager: "Alert! Something happened!"
Manager → Agent: "Acknowledged"

Like TRAP but reliable (requires acknowledgment)
```

---

### MIB (Management Information Base)

**คำจำกัดความ:**

```
- Database of objects that can be managed
- Hierarchical tree structure
- Each object = OID (Object Identifier)
```

**MIB Tree Structure:**

```
                    .iso (1)
                       |
                    .org (3)
                       |
                    .dod (6)
                       |
                  .internet (1)
                       |
        ┌──────────────┼──────────────┐
      mgmt (2)     private (4)    experimental (3)
        |              |
     mib-2 (1)    enterprises (1)
        |              |
    ┌───┴───┐         cisco (9)
  system  interfaces
   (1)       (2)

OID Example:
  Interface status: 1.3.6.1.2.1.2.2.1.8.1
  = .iso.org.dod.internet.mgmt.mib-2.interfaces.ifTable.ifEntry.ifOperStatus.1
```

**Common OIDs:**

```
Object                  OID                         Description
------------------------------------------------------------------------
sysDescr                1.3.6.1.2.1.1.1             System description
sysUpTime               1.3.6.1.2.1.1.3             Uptime
sysName                 1.3.6.1.2.1.1.5             Hostname
ifDescr                 1.3.6.1.2.1.2.2.1.2         Interface description
ifOperStatus            1.3.6.1.2.1.2.2.1.8         Interface status
ifInOctets              1.3.6.1.2.1.2.2.1.10        Bytes in
ifOutOctets             1.3.6.1.2.1.2.2.1.16        Bytes out
------------------------------------------------------------------------
```

---

### SNMP Configuration

#### SNMPv2c Configuration

**Basic Setup:**

```
! Set read-only community string
Router(config)# snmp-server community Public RO

! Set read-write community string
Router(config)# snmp-server community Private RW

! Set contact and location
Router(config)# snmp-server contact admin@company.com
Router(config)# snmp-server location Building-A, Floor-3
```

**With ACL (Security):**

```
! Restrict access to specific NMS
Router(config)# access-list 10 permit 192.168.1.100
Router(config)# access-list 10 deny any

Router(config)# snmp-server community Public RO 10
Router(config)# snmp-server community Private RW 10

Only 192.168.1.100 can query via SNMP
```

**Enable Traps:**

```
! Enable all traps
Router(config)# snmp-server enable traps

! Enable specific traps
Router(config)# snmp-server enable traps snmp linkdown linkup
Router(config)# snmp-server enable traps config
Router(config)# snmp-server enable traps cpu threshold

! Specify trap receiver (NMS)
Router(config)# snmp-server host 192.168.1.100 version 2c Public
```

---

#### SNMPv3 Configuration

**Create User with Authentication and Encryption:**

```
! Create SNMPv3 user
Router(config)# snmp-server group ADMIN v3 priv
Router(config)# snmp-server user admin1 ADMIN v3 auth sha AuthPass123 priv aes 128 PrivPass456

Parameters:
  - Group: ADMIN
  - Version: v3
  - Security: priv (authentication + encryption)
  - Auth: SHA with password "AuthPass123"
  - Priv: AES-128 with password "PrivPass456"
```

**View Configuration (Limit Access):**

```
! Create view (limit to specific MIB subtree)
Router(config)# snmp-server view READONLY iso included

! Assign view to group
Router(config)# snmp-server group ADMIN v3 priv read READONLY
```

**Enable Traps (v3):**

```
! Trap receiver with v3
Router(config)# snmp-server host 192.168.1.100 version 3 priv admin1
```

---

### SNMP Verification

#### show snmp

```
Router# show snmp

Chassis: FOC1234567
Contact: admin@company.com
Location: Building-A, Floor-3
451234 SNMP packets input
    0 Bad SNMP version errors
    0 Unknown community name
    0 Illegal operation for community name supplied
    0 Encoding errors
    1234 Number of requested variables
    0 Number of altered variables
    567 Get-request PDUs
    234 Get-next PDUs
    0 Set-request PDUs
123456 SNMP packets output
    0 Too big errors (Maximum packet size 1500)
    0 No such name errors
    0 Bad values errors
    0 General errors
    890 Response PDUs
    567 Trap PDUs
```

---

#### show snmp community

```
Router# show snmp community

Community name: Public
Community Index: Public
Community SecurityName: Public
storage-type: nonvolatile        active
```

---

#### show snmp user

```
Router# show snmp user

User name: admin1
Engine ID: 800000090300001A2B3C4D5E
storage-type: nonvolatile        active
Authentication Protocol: SHA
Privacy Protocol: AES128
Group-name: ADMIN
```

---

#### show snmp host

```
Router# show snmp host

Notification host: 192.168.1.100  udp-port: 162  type: trap
user: admin1  security model: v3 priv
```

---

### SNMP Security Best Practices

**1. Use SNMPv3:**

```
✅ Authentication (SHA)
✅ Encryption (AES)
✅ Integrity
✅ Strong security

Avoid SNMPv1/v2c in production (if possible)
```

**2. Strong Community Strings (v1/v2c):**

```
❌ Bad: "public", "private", "community"
✅ Good: "xK9mP2qR7nL5vT3w" (long, random)

Change default communities!
```

**3. Read-Only Access:**

```
✅ Use RO (read-only) community for monitoring
❌ Avoid RW (read-write) unless necessary

snmp-server community ComplexString123 RO
```

**4. Restrict Access (ACL):**

```
✅ Limit to specific NMS IP addresses

access-list 10 permit 192.168.1.100
access-list 10 deny any
snmp-server community String123 RO 10
```

**5. Disable SNMP if Not Needed:**

```
If not using SNMP:

Router(config)# no snmp-server

Reduces attack surface
```

**6. Separate Management Network:**

```
✅ Dedicated management VLAN
✅ Out-of-band management
✅ Isolated from production
```

**7. Monitor SNMP Activity:**

```
✅ Log SNMP access
✅ Alert on unauthorized access
✅ Review logs regularly

show snmp
show logging
```

---

## 10.5 Syslog

### What is Syslog?

**คำจำกัดความ:**

- **Syslog** = Protocol for logging system messages
- Standard logging mechanism
- UDP port 514 (unreliable)
- Centralized logging server
- Essential for troubleshooting, security, compliance

**Why Syslog:**

```
✅ Centralized logs (all devices → one server)
✅ Historical data (long-term storage)
✅ Correlation (compare logs across devices)
✅ Troubleshooting (what happened?)
✅ Security (detect attacks, unauthorized access)
✅ Compliance (audit trails)
```

---

### Syslog Architecture

**Components:**

```
1. Syslog Client (Device)
   - Routers, switches, firewalls
   - Generates log messages
   - Sends to syslog server

2. Syslog Server
   - Receives messages
   - Stores logs
   - Examples: Kiwi Syslog, Splunk, ELK Stack

3. Syslog Protocol
   - UDP 514 (default)
   - TCP 514 (reliable)
   - TLS (encrypted)
```

**Flow:**

```
[Router] ───────→ [Syslog Server]
          Syslog
         UDP 514
  
Router generates event → Sends syslog → Server stores
```

---

### Syslog Message Format

**Syslog Message Structure:**

```
%FACILITY-SEVERITY-MNEMONIC: Message Text

Example:
%SYS-5-CONFIG_I: Configured from console by admin on vty0 (192.168.1.10)

Parts:
  FACILITY:  SYS (system)
  SEVERITY:  5 (notification)
  MNEMONIC:  CONFIG_I
  Message:   Details
```

---

### Syslog Severity Levels

**8 Levels (0-7):**

```
Level  Name              Keyword       Description
------------------------------------------------------------------------
0      Emergency         emergencies   System unusable (highest)
1      Alert             alerts        Immediate action needed
2      Critical          critical      Critical condition
3      Error             errors        Error condition
4      Warning           warnings      Warning condition
5      Notification      notifications Normal but significant
6      Informational     informational Informational message
7      Debug             debugging     Debug message (lowest)
------------------------------------------------------------------------
```

**Severity Levels Explained:**

**Level 0 - Emergency:**

```
System unusable, complete failure

Example:
  %SYS-0-CPUHOG: System shut down due to overheating
```

**Level 1 - Alert:**

```
Immediate action required

Example:
  %LINK-1-FAIL: All links to critical server failed
```

**Level 2 - Critical:**

```
Critical conditions

Example:
  %SYS-2-MALLOCFAIL: Memory allocation failure
```

**Level 3 - Error:**

```
Error conditions, functionality impaired

Example:
  %LINEPROTO-3-UPDOWN: Line protocol on Interface Gi0/1, changed state to down
```

**Level 4 - Warning:**

```
Warning, potential problem

Example:
  %SYS-4-CPURISINGTHRESHOLD: CPU utilization rising above 75%
```

**Level 5 - Notification:**

```
Normal but significant, noteworthy

Example:
  %SYS-5-CONFIG_I: Configured from console by admin
  %LINK-5-CHANGED: Interface Gi0/1, changed state to up
```

**Level 6 - Informational:**

```
Informational messages, normal operations

Example:
  %LINEPROTO-6-UPDOWN: Line protocol on Interface Gi0/1, changed state to up
```

**Level 7 - Debug:**

```
Debug messages, very detailed

Example:
  %OSPF-7-DEBUG: OSPF packet received from 10.1.1.2

⚠️ CPU intensive, use carefully!
```

---

### Logging Locations

**Console (Default):**

```
- Logs to console port (physical)
- Default: All messages (level 7)
- Useful during initial setup
- ⚠️ Can be disruptive

Router(config)# logging console 6
                              ↑
                     Informational and above
```

**Terminal (VTY Lines):**

```
- Logs to Telnet/SSH sessions
- Default: Disabled
- Useful for remote troubleshooting

Router(config)# logging monitor 5
                              ↑
                    Notifications and above

Router# terminal monitor
(Enable logging for current session)
```

**Buffered (RAM):**

```
- Logs stored in router memory (RAM)
- Limited size (volatile, lost on reload)
- Fast access

Router(config)# logging buffered 16384
                              ↑
                         Buffer size (bytes)

Router(config)# logging buffered warnings
                              ↑
                        Severity level

View logs:
Router# show logging
```

**Syslog Server (External):**

```
- Logs sent to external server
- Persistent storage
- Centralized
- ✅ Recommended for production

Router(config)# logging host 192.168.1.100
Router(config)# logging trap 5
                            ↑
                  Notifications and above
```

**File (Flash/Disk):**

```
- Logs stored on device storage
- Persistent (survives reload)
- Limited space

Router(config)# logging file flash:logfile.txt 4096
                                                ↑
                                         Max size (bytes)
```

---

### Syslog Configuration

#### Basic Syslog Configuration

```
! Enable logging
Router(config)# logging on

! Set logging severity for console
Router(config)# logging console warnings

! Set logging severity for buffer
Router(config)# logging buffered 16384 informational

! Configure syslog server
Router(config)# logging host 192.168.1.100

! Set logging severity for syslog server
Router(config)# logging trap notifications

! Add timestamps to messages
Router(config)# service timestamps log datetime msec

! Add sequence numbers
Router(config)# service sequence-numbers
```

---

#### Configure Multiple Syslog Servers

```
! Primary syslog server
Router(config)# logging host 192.168.1.100

! Backup syslog server
Router(config)# logging host 192.168.1.101

! Different severity per server
Router(config)# logging host 192.168.1.100
Router(config)# logging trap errors

Router(config)# logging host 192.168.1.200
Router(config)# logging trap debugging
```

---

#### Configure Syslog Source Interface

```
! Use specific interface as source
Router(config)# logging source-interface loopback 0

Why:
  ✅ Consistent source IP (even if physical interface down)
  ✅ Easier filtering on syslog server
```

---

#### Configure Timestamps

```
! Datetime with milliseconds
Router(config)# service timestamps log datetime msec

! Datetime with timezone
Router(config)# service timestamps log datetime localtime show-timezone

! Just datetime
Router(config)# service timestamps log datetime

Example output:
  *Jan 1 12:30:45.123 UTC: %SYS-5-CONFIG_I: Configured from console
                      ↑↑↑
                  milliseconds
```

---

#### Configure Sequence Numbers

```
! Add sequence numbers to logs
Router(config)# service sequence-numbers

Example output:
  000012: *Jan 1 12:30:45: %SYS-5-CONFIG_I: Configured from console
  ↑↑↑↑↑↑
  Sequence number (helps identify missing messages)
```

---

#### Disable Logging Temporarily

```
! Disable all logging
Router(config)# no logging on

! Disable console logging (but keep others)
Router(config)# no logging console

! Re-enable
Router(config)# logging on
Router(config)# logging console
```

---

### Syslog Verification

#### show logging

```
Router# show logging

Syslog logging: enabled (0 messages dropped, 3 messages rate-limited,
                0 flushes, 0 overruns, xml disabled, filtering disabled)

No Active Message Discriminator.

No Inactive Message Discriminator.

    Console logging: level warnings, 45 messages logged, xml disabled,
                     filtering disabled
    Monitor logging: level informational, 0 messages logged, xml disabled,
                     filtering disabled
    Buffer logging:  level informational, 234 messages logged, xml disabled,
                    filtering disabled
    Exception Logging: size (4096 bytes)
    Count and timestamp logging messages: enabled
    Persistent logging: disabled

No active filter modules.

    Trap logging: level notifications, 167 message lines logged
        Logging to 192.168.1.100  (udp port 514, audit disabled,
              link up), 167 message lines logged, 
              0 message lines rate-limited, 
              0 message lines dropped-by-MD, 
              xml disabled, sequence number disabled
              filtering disabled
        Logging Source-Interface:       VRF Name:

Log Buffer (16384 bytes):

*Jan  1 12:25:30.123: %SYS-5-CONFIG_I: Configured from console by admin
*Jan  1 12:26:15.456: %LINK-3-UPDOWN: Interface GigabitEthernet0/1, changed state to up
*Jan  1 12:26:16.789: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up
[...]
```

---

#### show logging history

```
Router# show logging history

Syslog History Table:  1 maximum table entries, 
                       saving level warnings or higher
1 messages ignored, 0 dropped, 0 recursion drops
0 table entries flushed

    SNMP notifications not enabled
entry number 1 : LINK-3-UPDOWN  
    Interface GigabitEthernet0/1, changed state to down
    timestamp: 1234
```

---

#### clear logging

```
! Clear buffered logs (RAM)
Router# clear logging

! Confirm
Clear logging buffer [confirm]
[OK]

Note: Only clears local buffer, doesn't affect syslog server
```

---

### Syslog Best Practices

**1. Use External Syslog Server:**

```
✅ Centralized storage
✅ Persistent (survives device reload/crash)
✅ Large capacity
✅ Searchable
✅ Correlation across devices

Router(config)# logging host 192.168.1.100
```

**2. Use Appropriate Severity Levels:**

```
Console:      warnings (4) - Important messages only
Terminal:     informational (6) - For troubleshooting
Buffered:     informational (6) - Keep recent history
Syslog Server: notifications (5) or informational (6)

Router(config)# logging console warnings
Router(config)# logging buffered informational
Router(config)# logging trap notifications
```

**3. Enable Timestamps:**

```
✅ Correlate events
✅ Troubleshooting

Router(config)# service timestamps log datetime msec localtime show-timezone
Router(config)# service sequence-numbers
```

**4. Configure NTP:**

```
✅ Accurate timestamps
✅ Essential for correlation

Router(config)# ntp server 129.6.15.28
(See NTP section)
```

**5. Use Source Interface:**

```
✅ Consistent source IP

Router(config)# logging source-interface loopback 0
```

**6. Multiple Syslog Servers:**

```
✅ Redundancy
✅ High availability

Router(config)# logging host 192.168.1.100
Router(config)# logging host 192.168.1.101
```

**7. Secure Syslog:**

```
✅ Dedicated management network
✅ ACLs (restrict access)
✅ Encryption (TLS, if supported)
✅ Integrity (prevent tampering)
```

**8. Monitor Syslog Server:**

```
✅ Ensure messages received
✅ Disk space monitoring
✅ Alerting on critical messages
```

**9. Regular Review:**

```
✅ Review logs regularly
✅ Look for anomalies
✅ Proactive troubleshooting
✅ Security monitoring
```

**10. Avoid Debug on Production:**

```
❌ Debug (level 7) very verbose
❌ CPU intensive
❌ Can crash device

Use debug only when troubleshooting, then disable:
Router# undebug all
```

---

## Summary (สรุป)

Module 10 นี้เราได้เรียนรู้:

1. ✅ **CDP (Cisco Discovery Protocol)**:
    
    - Layer 2, Cisco proprietary
    - Discover neighboring Cisco devices
    - Default: Enabled
    - Commands: show cdp neighbors [detail]
    - Security: Disable on untrusted interfaces
2. ✅ **LLDP (Link Layer Discovery Protocol)**:
    
    - Layer 2, IEEE standard (802.1AB)
    - Multi-vendor support
    - Default: Disabled
    - Commands: show lldp neighbors [detail]
    - Use in multi-vendor environments
3. ✅ **NTP (Network Time Protocol)**:
    
    - Time synchronization
    - Critical for logs, security
    - Stratum hierarchy (0-15)
    - Commands: show ntp status, show ntp associations
    - Best practice: Multiple servers, authentication
4. ✅ **SNMP (Simple Network Management Protocol)**:
    
    - Network monitoring and management
    - Versions: v1 (obsolete), v2c (common), v3 (secure)
    - Operations: GET, SET, TRAP
    - MIB and OIDs
    - Commands: show snmp user, show snmp community
    - Best practice: Use SNMPv3, ACLs, strong communities
5. ✅ **Syslog**:
    
    - Centralized logging
    - Severity levels: 0-7 (emergencies to debugging)
    - Locations: Console, Terminal, Buffer, Server
    - Commands: show logging, clear logging
    - Best practice: External server, timestamps, NTP

**สิ่งสำคัญที่ต้องจำ:**

- **CDP**: Cisco only, Layer 2, default enabled
- **LLDP**: Multi-vendor, Layer 2, default disabled
- **NTP**: Time sync, UDP 123, use stratum 1/2 servers
- **SNMP**: v3 preferred (secure), UDP 161/162
- **Syslog**: UDP 514, severity 0-7, external server recommended

**Configuration Quick Reference:**

```
! CDP
cdp run
show cdp neighbors detail

! LLDP
lldp run
show lldp neighbors detail

! NTP
ntp server 129.6.15.28
show ntp status
show ntp associations

! SNMP v3
snmp-server group ADMIN v3 priv
snmp-server user admin1 ADMIN v3 auth sha AuthPass priv aes 128 PrivPass
show snmp user

! Syslog
logging host 192.168.1.100
logging trap notifications
service timestamps log datetime msec
show logging
```

**Best Practices Summary:**

```
Discovery:
  ✅ Disable CDP/LLDP on external interfaces
  ✅ Use LLDP in multi-vendor environments

Time:
  ✅ Use NTP (multiple servers)
  ✅ Enable authentication
  ✅ Verify synchronization

Monitoring:
  ✅ Use SNMPv3 (security)
  ✅ Strong community strings (v2c)
  ✅ ACLs to restrict access

Logging:
  ✅ External syslog server
  ✅ Appropriate severity levels
  ✅ Timestamps + sequence numbers
  ✅ NTP for accurate times
  ✅ Regular log review
```

**Next Module:** Module 11 - Network Design

---

**[ไฟล์ Module 10 สมบูรณ์แล้ว!]**