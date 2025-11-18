# CCNA 2: Module 6 - EtherChannel

## Link Aggregation

---

## สารบัญ

1. [วัตถุประสงค์ของ Module](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#%E0%B8%A7%E0%B8%B1%E0%B8%95%E0%B8%96%E0%B8%B8%E0%B8%9B%E0%B8%A3%E0%B8%B0%E0%B8%AA%E0%B8%87%E0%B8%84%E0%B9%8C%E0%B8%82%E0%B8%AD%E0%B8%87-module)
2. [EtherChannel Operation](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#1-etherchannel-operation)
3. [Configure EtherChannel](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#2-configure-etherchannel)
4. [Verify and Troubleshoot EtherChannel](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#3-verify-and-troubleshoot-etherchannel)
5. [สรุป](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#%E0%B8%AA%E0%B8%A3%E0%B8%B8%E0%B8%9B)

---

## วัตถุประสงค์ของ Module

เมื่อจบ Module นี้ คุณจะสามารถ:

- ✅ อธิบายวัตถุประสงค์และประโยชน์ของ EtherChannel
- ✅ เข้าใจการทำงานของ EtherChannel
- ✅ Configure EtherChannel ด้วย LACP
- ✅ Configure EtherChannel ด้วย PAgP
- ✅ Configure EtherChannel แบบ manual (on mode)
- ✅ Verify EtherChannel configuration
- ✅ Troubleshoot common EtherChannel problems
- ✅ เข้าใจ load balancing methods

---

## 1. EtherChannel Operation

### การทำงานของ EtherChannel

### 1.1 Link Aggregation Concepts

**ปัญหา: Bandwidth Limitations**

```
สถานการณ์:
Access Layer มี users เยอะ → traffic มาก
Uplink ระหว่าง Access และ Distribution Switch คับคั่ง

Traditional Solution:
[Access Switch]
      │ 1 Gbps link (congested!)
      │
[Distribution Switch]

Problem:
- Bottleneck ที่ uplink
- Users ได้ bandwidth ไม่เพียงพอ
- Network performance แย่
```

**วิธีแก้ที่ไม่ดี:**

```
Option 1: อัพเกรด link เป็น 10 Gbps
- แพงมาก
- อาจไม่จำเป็น
- Overkill

Option 2: เพิ่ม link แต่ไม่ aggregate
- STP block redundant links
- ได้ redundancy แต่ไม่ได้ bandwidth
- Waste resources
```

**วิธีแก้ที่ดี: EtherChannel (Link Aggregation)**

```
[Access Switch]
      ║ EtherChannel (4 × 1 Gbps = 4 Gbps)
      ║ Multiple physical links act as one
      ║
[Distribution Switch]

Benefits:
✅ Increased bandwidth (รวม links)
✅ Redundancy (ถ้า link หนึ่งเสีย ยังมีอีก)
✅ Load balancing (traffic กระจายไป links)
✅ STP ถือเป็น 1 logical link (ไม่ block)
✅ Cost-effective (ถูกกว่าอัพเกรด)
```

### 1.2 EtherChannel คืออะไร?

**EtherChannel (Port Channel):**

- รวมหลาย physical links เป็น 1 logical link
- STP มองเป็น 1 link (ไม่ block)
- Bandwidth รวมกัน
- Cisco technology (IEEE 802.3ad, 802.1AX)

**Terminology:**

```
EtherChannel = Cisco term
Port Channel = Generic term
LAG (Link Aggregation Group) = IEEE term
Port Aggregation = Alternative term
```

**จำนวน Links:**

```
- Maximum: 8 active links
- Additional: 8 standby links (LACP only)
- Minimum: 2 links (typically)
- Common: 2, 4, or 8 links
```

**ประเภท EtherChannel:**

```
1. Layer 2 EtherChannel
   - Switch-to-Switch
   - Access/Trunk mode

2. Layer 3 EtherChannel
   - Routed EtherChannel
   - No switchport
   - IP address assigned
```

### 1.3 EtherChannel Advantages

**1. Increased Bandwidth:**

```
Single Link: 1 Gbps
EtherChannel (4 links): 4 Gbps

Example:
[Switch A] ═══════════ [Switch B]
           4 × 1 Gbps
           = 4 Gbps total bandwidth
```

**2. Redundancy:**

```
ถ้า 1 link fail:
- Links อื่นยังทำงาน
- No network downtime
- Automatic failover
- Bandwidth ลดลงแต่ยังทำงานได้

Example: 4-link EtherChannel
- 1 link fails → 3 Gbps remaining
- 2 links fail → 2 Gbps remaining
- 3 links fail → 1 Gbps remaining
```

**3. Load Balancing:**

```
Traffic กระจายไปทุก links ใน EtherChannel
- ไม่มี link ใด idle
- Efficient bandwidth utilization
- Multiple algorithms available
```

**4. STP Benefits:**

```
Without EtherChannel:
[SW1] ─── Link1 ─── [SW2]
      ─── Link2 ───      (STP blocks Link2)

Total bandwidth: 1 Gbps

With EtherChannel:
[SW1] ═══ EtherChannel ═══ [SW2]
      (Link1 + Link2)

Total bandwidth: 2 Gbps
STP sees 1 logical link
```

**5. Configuration Simplicity:**

```
Configure once on port-channel interface
- ใช้กับ member interfaces ทั้งหมด
- Easier management
- Consistent configuration
```

### 1.4 EtherChannel Restrictions

**Requirements (Links ต้องเหมือนกัน):**

```
✅ Same speed (all 1 Gbps or all 10 Gbps)
✅ Same duplex (all full duplex)
✅ Same VLAN configuration
   - Access: same VLAN
   - Trunk: same native VLAN, same allowed VLANs
✅ Same interface type
✅ Connected to same pair of switches

❌ Mixed speeds not allowed
❌ Different VLAN configs not allowed
❌ Half duplex not allowed (typically)
```

**EtherChannel Modes Compatibility:**

```
Protocols:
- LACP (Link Aggregation Control Protocol) - IEEE 802.3ad
- PAgP (Port Aggregation Protocol) - Cisco proprietary
- Static (on mode) - No protocol

Compatibility:
LACP ↔ LACP ✅
PAgP ↔ PAgP ✅
Static ↔ Static ✅
LACP ↔ PAgP ❌
LACP ↔ Static ❌
PAgP ↔ Static ❌
```

### 1.5 EtherChannel Protocols

**1. PAgP (Port Aggregation Protocol):**

```
- Cisco proprietary
- Automatic negotiation
- Works only between Cisco switches
- Deprecated (legacy)

Modes:
- Desirable: Actively negotiates
- Auto: Passively waits
- On: No negotiation
```

**2. LACP (Link Aggregation Control Protocol):**

```
- IEEE 802.3ad / 802.1AX
- Industry standard
- Works with any vendor (if supported)
- Preferred method

Modes:
- Active: Actively negotiates
- Passive: Passively waits
- On: No negotiation

Advantages:
+ IEEE standard
+ Vendor interoperability
+ Up to 16 links (8 active + 8 standby)
+ Better failover
```

**3. Static (On Mode):**

```
- No protocol
- No negotiation
- Manual configuration
- Links forced to channel

Disadvantages:
- No detection of misconfig
- No automatic recovery
- Not recommended
```

### 1.6 PAgP และ LACP Modes

**PAgP Modes:**

```
Desirable:
- Actively sends PAgP packets
- Initiates negotiation
- จะเป็น EtherChannel ถ้าอีกฝั่งเป็น:
  * Desirable
  * Auto

Auto:
- Passively listens for PAgP
- Responds to negotiation
- จะเป็น EtherChannel ถ้าอีกฝั่งเป็น:
  * Desirable

On:
- No PAgP
- Forced bundling
- จะเป็น EtherChannel ถ้าอีกฝั่งเป็น:
  * On
```

**PAgP Combination Table:**

```
           │ Desirable │  Auto  │   On   │
───────────┼───────────┼────────┼────────┤
Desirable  │    Yes    │  Yes   │   No   │
Auto       │    Yes    │   No   │   No   │
On         │    No     │   No   │  Yes   │
```

**LACP Modes:**

```
Active:
- Actively sends LACP packets
- Initiates negotiation
- จะเป็น EtherChannel ถ้าอีกฝั่งเป็น:
  * Active
  * Passive

Passive:
- Passively listens for LACP
- Responds to negotiation
- จะเป็น EtherChannel ถ้าอีกฝั่งเป็น:
  * Active

On:
- No LACP
- Forced bundling
- จะเป็น EtherChannel ถ้าอีกฝั่งเป็น:
  * On
```

**LACP Combination Table:**

```
           │ Active │ Passive │  On  │
───────────┼────────┼─────────┼──────┤
Active     │  Yes   │   Yes   │  No  │
Passive    │  Yes   │   No    │  No  │
On         │  No    │   No    │ Yes  │
```

**Best Practice:**

```
Recommended: LACP Active-Active
- IEEE standard
- Vendor independent
- Better detection
- Active negotiation both sides

Example:
Switch1(config-if-range)# channel-group 1 mode active
Switch2(config-if-range)# channel-group 1 mode active
```

### 1.7 Load Balancing

**Load Balancing Methods:**

EtherChannel กระจาย traffic โดยใช้ hash algorithm

**Hash based on:**

```
1. Source MAC address
2. Destination MAC address
3. Source and Destination MAC
4. Source IP address
5. Destination IP address
6. Source and Destination IP
7. Source port number
8. Destination port number
9. Source and Destination port
```

**Default:**

```
Cisco switches (typically): src-dst-ip
- Hash based on source และ destination IP addresses
```

**ตัวอย่าง Load Balancing:**

```
EtherChannel: 4 links (Link0, Link1, Link2, Link3)

Flow 1: PC1 → Server1
Hash = 0 → Link0

Flow 2: PC2 → Server1
Hash = 2 → Link2

Flow 3: PC1 → Server2
Hash = 1 → Link1

Flow 4: PC3 → Server1
Hash = 3 → Link3

Result: Traffic กระจายไปทุก links
```

**สำคัญ:**

```
⚠️ Load balancing ทำงานแบบ per-flow, NOT per-packet

Per-Flow:
- Flow เดียวกันใช้ link เดียวกัน
- Packets ใน flow ไม่เรียงผิด
- Better performance

NOT Per-Packet:
- แต่ละ packet อาจไป link ต่างกัน
- Packets arrive out of order
- Reordering overhead
- Poor performance
```

**Configure Load Balancing:**

```cisco
! ดู current load balance method
Switch# show etherchannel load-balance

EtherChannel Load-Balancing Configuration:
        src-dst-ip

! เปลี่ยน load balance method
Switch(config)# port-channel load-balance ?
  dst-ip       Dst IP Addr
  dst-mac      Dst Mac Addr
  src-dst-ip   Src XOR Dst IP Addr
  src-dst-mac  Src XOR Dst Mac Addr
  src-ip       Src IP Addr
  src-mac      Src Mac Addr

Switch(config)# port-channel load-balance src-dst-ip
```

---

## 2. Configure EtherChannel

### การตั้งค่า EtherChannel

### 2.1 Configuration Guidelines

**Pre-Configuration Checklist:**

```
✅ เลือก interfaces ที่จะรวม (2-8 interfaces)
✅ ตรวจสอบว่า interfaces มี configuration เหมือนกัน:
   - Speed and duplex
   - VLAN assignment (access/trunk)
   - Native VLAN (for trunks)
   - Allowed VLANs (for trunks)
✅ Shutdown interfaces ก่อน configure
✅ เลือก protocol: LACP (recommended) หรือ PAgP
✅ เลือก mode: active/passive (LACP) หรือ desirable/auto (PAgP)
✅ กำหนด channel-group number (1-64)
```

**Configuration Steps:**

```
1. Shutdown member interfaces
2. Configure same settings on all interfaces
3. Create channel-group
4. Configure port-channel interface (optional)
5. No shutdown member interfaces
6. Verify EtherChannel
```

### 2.2 Configure Layer 2 EtherChannel with LACP

**Scenario:**

```
Topology:
[Switch1] ═══════════ [Switch2]
        Fa0/1, Fa0/2
        EtherChannel (LACP)
```

**Switch 1 Configuration:**

```cisco
! Step 1: เข้าสู่ interfaces และ shutdown
Switch1(config)# interface range fastethernet 0/1 - 2
Switch1(config-if-range)# shutdown

! Step 2: Configure basic settings
Switch1(config-if-range)# switchport mode trunk
Switch1(config-if-range)# switchport trunk native vlan 99
Switch1(config-if-range)# switchport trunk allowed vlan 10,20,30,99

! Step 3: Create EtherChannel with LACP
Switch1(config-if-range)# channel-group 1 mode active
Creating a port-channel interface Port-channel 1

! Step 4: No shutdown
Switch1(config-if-range)# no shutdown
Switch1(config-if-range)# exit

! Step 5: Configure Port-Channel interface (optional but recommended)
Switch1(config)# interface port-channel 1
Switch1(config-if)# description EtherChannel to Switch2
Switch1(config-if)# switchport mode trunk
Switch1(config-if)# switchport trunk native vlan 99
Switch1(config-if)# switchport trunk allowed vlan 10,20,30,99
```

**Switch 2 Configuration:**

```cisco
Switch2(config)# interface range fastethernet 0/1 - 2
Switch2(config-if-range)# shutdown
Switch2(config-if-range)# switchport mode trunk
Switch2(config-if-range)# switchport trunk native vlan 99
Switch2(config-if-range)# switchport trunk allowed vlan 10,20,30,99
Switch2(config-if-range)# channel-group 1 mode active
Switch2(config-if-range)# no shutdown
Switch2(config-if-range)# exit

Switch2(config)# interface port-channel 1
Switch2(config-if)# description EtherChannel to Switch1
Switch2(config-if)# switchport mode trunk
Switch2(config-if)# switchport trunk native vlan 99
Switch2(config-if)# switchport trunk allowed vlan 10,20,30,99
```

**คำอธิบายคำสั่ง:**

```cisco
channel-group <number> mode {active | passive}
- สร้าง EtherChannel
- <number>: 1-64 (ต้องไม่ซ้ำบน switch เดียวกัน)
- active: LACP actively negotiates
- passive: LACP passively listens

interface port-channel <number>
- Configure logical port-channel interface
- <number> ต้องตรงกับ channel-group number
```

### 2.3 Configure Layer 2 EtherChannel with PAgP

**Configuration (คล้าย LACP แต่ใช้ PAgP modes):**

```cisco
! Switch 1
Switch1(config)# interface range gigabitethernet 0/1 - 2
Switch1(config-if-range)# shutdown
Switch1(config-if-range)# switchport mode trunk
Switch1(config-if-range)# channel-group 2 mode desirable
Switch1(config-if-range)# no shutdown

! Switch 2
Switch2(config)# interface range gigabitethernet 0/1 - 2
Switch2(config-if-range)# shutdown
Switch2(config-if-range)# switchport mode trunk
Switch2(config-if-range)# channel-group 2 mode auto
Switch2(config-if-range)# no shutdown
```

**PAgP Modes:**

```
desirable: Active negotiation
auto: Passive negotiation

Combination:
desirable + desirable = EtherChannel ขึ้น ✅
desirable + auto = EtherChannel ขึ้น ✅
auto + auto = EtherChannel ไม่ขึ้น ❌
```

### 2.4 Configure Static EtherChannel (On Mode)

**Configuration:**

```cisco
! Switch 1
Switch1(config)# interface range gigabitethernet 0/1 - 2
Switch1(config-if-range)# shutdown
Switch1(config-if-range)# switchport mode trunk
Switch1(config-if-range)# channel-group 3 mode on
Switch1(config-if-range)# no shutdown

! Switch 2
Switch2(config)# interface range gigabitethernet 0/1 - 2
Switch2(config-if-range)# shutdown
Switch2(config-if-range)# switchport mode trunk
Switch2(config-if-range)# channel-group 3 mode on
Switch2(config-if-range)# no shutdown
```

**⚠️ Warning:**

```
Mode "on" ไม่มี protocol negotiation
- ไม่ detect misconfigurations
- ไม่มี automatic failover detection
- Not recommended
- ใช้เฉพาะกรณีพิเศษ
```

### 2.5 Configure Layer 3 EtherChannel

**Layer 3 EtherChannel (Routed EtherChannel):**

- ไม่ใช่ switchport
- กำหนด IP address ได้
- ใช้สำหรับ Layer 3 routing

**Configuration:**

```cisco
! Switch 1 (Layer 3 Switch)
Switch1(config)# interface range gigabitethernet 1/0/1 - 2
Switch1(config-if-range)# no switchport
Switch1(config-if-range)# no ip address
Switch1(config-if-range)# channel-group 1 mode active
Switch1(config-if-range)# no shutdown
Switch1(config-if-range)# exit

! Configure IP address บน Port-Channel
Switch1(config)# interface port-channel 1
Switch1(config-if)# no switchport
Switch1(config-if)# ip address 10.1.1.1 255.255.255.252
Switch1(config-if)# no shutdown

! Switch 2 (Layer 3 Switch)
Switch2(config)# interface range gigabitethernet 1/0/1 - 2
Switch2(config-if-range)# no switchport
Switch2(config-if-range)# no ip address
Switch2(config-if-range)# channel-group 1 mode active
Switch2(config-if-range)# no shutdown
Switch2(config-if-range)# exit

Switch2(config)# interface port-channel 1
Switch2(config-if)# no switchport
Switch2(config-if)# ip address 10.1.1.2 255.255.255.252
Switch2(config-if)# no shutdown
```

**Use Cases:**

```
- Switch-to-Router connections
- Core-to-Distribution layer (routed)
- WAN aggregation
```

### 2.6 Advanced Configuration Examples

**Example 1: Access Mode EtherChannel**

```cisco
! Configuration สำหรับ access ports
Switch1(config)# interface range fastethernet 0/1 - 4
Switch1(config-if-range)# shutdown
Switch1(config-if-range)# switchport mode access
Switch1(config-if-range)# switchport access vlan 10
Switch1(config-if-range)# channel-group 1 mode active
Switch1(config-if-range)# no shutdown

Switch1(config)# interface port-channel 1
Switch1(config-if)# description Access EtherChannel
Switch1(config-if)# switchport mode access
Switch1(config-if)# switchport access vlan 10
```

**Example 2: 8-Link EtherChannel**

```cisco
! Maximum 8 active links
Switch(config)# interface range gigabitethernet 1/0/1 - 8
Switch(config-if-range)# shutdown
Switch(config-if-range)# switchport mode trunk
Switch(config-if-range)# channel-group 1 mode active
Switch(config-if-range)# no shutdown

! Total bandwidth: 8 Gbps (8 × 1 Gbps)
```

**Example 3: LACP Priority (Active/Standby)**

```cisco
! Configure LACP priority สำหรับ 16 links (8 active + 8 standby)
Switch(config)# interface range gigabitethernet 1/0/1 - 8
Switch(config-if-range)# channel-group 1 mode active
Switch(config-if-range)# lacp port-priority 32768

Switch(config)# interface range gigabitethernet 1/0/9 - 16
Switch(config-if-range)# channel-group 1 mode active
Switch(config-if-range)# lacp port-priority 65535

! Links 1-8: Active (lower priority)
! Links 9-16: Standby (higher priority)
```

---

## 3. Verify and Troubleshoot EtherChannel

### การตรวจสอบและแก้ไขปัญหา EtherChannel

### 3.1 Verification Commands

**1. Show EtherChannel Summary:**

```cisco
Switch# show etherchannel summary

Flags:  D - down        P - bundled in port-channel
        I - stand-alone s - suspended
        H - Hot-standby (LACP only)
        R - Layer3      S - Layer2
        U - in use      N - not in use, no aggregation
        f - failed to allocate aggregator
        M - not in use, minimum links not met
        m - not in use, port not aggregated due to minimum links not met
        u - unsuitable for bundling
        w - waiting to be aggregated
        d - default port

Number of channel-groups in use: 1
Number of aggregators:           1

Group  Port-channel  Protocol    Ports
------+-------------+-----------+-----------------------------------------------
1      Po1(SU)       LACP        Fa0/1(P)    Fa0/2(P)

Key:
Po1(SU): Port-channel 1, S=Layer2, U=in use
Fa0/1(P): P=bundled in port-channel
```

**2. Show EtherChannel Detail:**

```cisco
Switch# show etherchannel 1 detail

Group state = L2
Ports: 2   Maxports = 8
Port-channels: 1 Max Port-channels = 1
Protocol:   LACP
Minimum Links: 0

Ports in the group:
-------------------
Port: Fa0/1
------------
Port state    = Up Mstr Assoc In-Bndl
Channel group = 1           Mode = Active          Gcchange = -
Port-channel  = Po1         GC   =   -             Pseudo port-channel = Po1
Port index    = 0           Load = 0x00            Protocol =   LACP

Flags:  S - Device is sending Slow LACPDUs   F - Device is sending fast LACPDUs
        A - Device is in active mode          P - Device is in passive mode

Local information:
                            LACP port     Admin     Oper    Port        Port
Port      Flags   State     Priority      Key       Key     Number      State
Fa0/1     SA      bndl      32768         0x1       0x1     0x102       0x3D
```

**3. Show Port-Channel:**

```cisco
Switch# show interfaces port-channel 1

Port-channel1 is up, line protocol is up (connected)
  Hardware is EtherChannel, address is 0cd9.96d2.4801
  Description: EtherChannel to Switch2
  MTU 1500 bytes, BW 200000 Kbit/sec, DLY 100 usec
  Encapsulation ARPA, loopback not set
  Full-duplex, 100Mb/s, media type is unknown
  ...
  Members in this channel: Fa0/1 Fa0/2
```

**4. Show Interface Status:**

```cisco
Switch# show interfaces status

Port      Name               Status       Vlan       Duplex  Speed Type
Fa0/1                        connected    trunk      a-full  a-100 10/100BaseTX
Fa0/2                        connected    trunk      a-full  a-100 10/100BaseTX
Po1       EtherChannel       connected    trunk      a-full  a-100
```

**5. Show EtherChannel Load-Balance:**

```cisco
Switch# show etherchannel load-balance

EtherChannel Load-Balancing Configuration:
        src-dst-ip

EtherChannel Load-Balancing Addresses Used Per-Protocol:
Non-IP: Source XOR Destination MAC address
  IPv4: Source XOR Destination IP address
  IPv6: Source XOR Destination IP address
```

**6. Show LACP Neighbor:**

```cisco
Switch# show lacp neighbor

Flags:  S - Device is requesting Slow LACPDUs
        F - Device is requesting Fast LACPDUs
        A - Device is in Active mode       P - Device is in Passive mode

Channel group 1 neighbors

Partner's information:

                  LACP port                        Admin  Oper   Port    Port
Port      Flags   Priority  Dev ID          Age    key    Key    Number  State
Fa0/1     SA      32768     0cd9.96d2.3f00  29s    0x0    0x1    0x102   0x3D
Fa0/2     SA      32768     0cd9.96d2.3f00  9s     0x0    0x1    0x103   0x3D
```

**7. Show PAgP Neighbor:**

```cisco
Switch# show pagp neighbor

Flags:  S - Device is sending Slow hello
        C - Device is in Consistent state
        A - Device is in Auto mode        P - Device learns on physical port

Channel group 1 neighbors

Partner              Partner          Partner         Partner Group
Port                 Name             Port            Age  Flags   Cap.
Fa0/1                Switch2          Fa0/1            9s  SC      10001
Fa0/2                Switch2          Fa0/2           24s  SC      10001
```

### 3.2 Common Problems

**Problem 1: Mismatched Configuration**

```
Symptom:
- EtherChannel ไม่ขึ้น (suspended)
- show etherchannel summary แสดง (s)

Cause:
- Speed mismatch
- Duplex mismatch
- VLAN mismatch
- Mode mismatch (access vs trunk)

Example:
Switch1 Fa0/1: speed 100
Switch1 Fa0/2: speed 1000
→ EtherChannel failed!

Solution:
ทำให้ configuration เหมือนกันทุก interfaces
```

**Problem 2: Protocol Mismatch**

```
Symptom:
- EtherChannel ไม่ขึ้น
- Ports เป็น standalone

Cause:
- ฝั่งหนึ่งใช้ LACP, อีกฝั่งใช้ PAgP
- ฝั่งหนึ่งใช้ on, อีกฝั่งใช้ LACP/PAgP
- Mode incompatible (passive + passive)

Example:
Switch1: channel-group 1 mode active (LACP)
Switch2: channel-group 1 mode desirable (PAgP)
→ EtherChannel failed!

Solution:
ใช้ protocol เดียวกันทั้งสองฝั่ง
```

**Problem 3: Native VLAN Mismatch**

```
Symptom:
- EtherChannel ขึ้นแต่ trunk ทำงานผิดพลาด
- CDP warnings

Cause:
- Native VLAN ไม่ตรงกันทั้งสองฝั่ง

Example:
Switch1: native vlan 1
Switch2: native vlan 99
→ Traffic issues!

Solution:
Switch1(config-if-range)# switchport trunk native vlan 99
Switch2(config-if-range)# switchport trunk native vlan 99
```

**Problem 4: Interface in Wrong State**

```
Symptom:
- บาง interfaces ไม่อยู่ใน EtherChannel
- show etherchannel summary แสดง interfaces น้อยกว่าที่ควร

Cause:
- Interface shutdown
- Interface error-disabled
- Cable problem
- Hardware failure

Troubleshooting:
Switch# show interfaces status
Switch# show interfaces fa0/1

Solution:
Switch(config)# interface fa0/1
Switch(config-if)# no shutdown
```

**Problem 5: Mode Mismatch**

```
Symptom:
- EtherChannel ไม่ขึ้น

Cause:
LACP:
- passive + passive ❌
- active + active ✅
- active + passive ✅

PAgP:
- auto + auto ❌
- desirable + desirable ✅
- desirable + auto ✅

Solution:
อย่างน้อยฝั่งหนึ่งต้องเป็น active mode
```

**Problem 6: VLAN Allowed Mismatch**

```
Symptom:
- EtherChannel ขึ้นแต่ VLAN บางตัวไม่ทำงาน

Cause:
- Allowed VLANs ไม่ตรงกัน

Example:
Switch1: allowed vlan 10,20,30
Switch2: allowed vlan 10,20
→ VLAN 30 ไม่ทำงาน!

Solution:
ตรวจสอบและตั้งให้เหมือนกัน
Switch# show interfaces trunk
```

### 3.3 Troubleshooting Process

**Step 1: Verify Physical Connectivity**

```cisco
! ตรวจสอบสถานะ interfaces
Switch# show interfaces status

! ตรวจสอบ errors
Switch# show interfaces fa0/1
Switch# show interfaces fa0/2
```

**Step 2: Verify EtherChannel Status**

```cisco
! ดู summary
Switch# show etherchannel summary

Flags ที่สำคัญ:
(SU) = Success, Layer 2, in Use ✅
(s) = Suspended ❌
(I) = Standalone ❌
(D) = Down ❌
```

**Step 3: Verify Configuration Consistency**

```cisco
! ตรวจสอบ configuration ของแต่ละ interface
Switch# show running-config interface fa0/1
Switch# show running-config interface fa0/2
Switch# show running-config interface port-channel 1

! เปรียบเทียบ:
- Speed and duplex
- Switchport mode (access/trunk)
- VLAN configuration
- Native VLAN (trunk)
```

**Step 4: Verify Protocol Operation**

```cisco
! สำหรับ LACP
Switch# show lacp neighbor
Switch# show lacp internal

! สำหรับ PAgP
Switch# show pagp neighbor
Switch# show pagp internal
```

**Step 5: Check for Error Messages**

```cisco
! ดู logs
Switch# show logging

! ดู CDP messages
Switch# show cdp neighbors detail
```

**Step 6: Systematic Fix**

```cisco
! 1. Shutdown EtherChannel
Switch(config)# interface range fa0/1 - 2
Switch(config-if-range)# shutdown

! 2. Remove channel-group
Switch(config-if-range)# no channel-group 1

! 3. Verify configuration
! ตรวจสอบว่า settings เหมือนกันทุก interfaces

! 4. Recreate EtherChannel
Switch(config-if-range)# channel-group 1 mode active

! 5. No shutdown
Switch(config-if-range)# no shutdown

! 6. Verify
Switch# show etherchannel summary
```

### 3.4 Troubleshooting Example

**Scenario:**

```
Problem:
EtherChannel between Switch1 and Switch2 ไม่ขึ้น

Topology:
[Switch1] Fa0/1, Fa0/2 ═══ [Switch2] Fa0/1, Fa0/2
```

**Troubleshooting Steps:**

```cisco
! Step 1: Check summary
Switch1# show etherchannel summary

Group  Port-channel  Protocol    Ports
------+-------------+-----------+-----------------------------------------------
1      Po1(SD)       LACP        Fa0/1(s)    Fa0/2(s)

Analysis: (SD) = Down, interfaces suspended

! Step 2: Check interfaces
Switch1# show interfaces status | include Fa0/[12]

Port      Name     Status       Vlan       Duplex  Speed Type
Fa0/1              connected    1          a-full  a-100 10/100BaseTX
Fa0/2              connected    1          a-full  a-1000 10/100BaseTX

Problem Found: Speed mismatch! (100 vs 1000)

! Step 3: Check configuration
Switch1# show running-config interface fa0/1
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 10
 channel-group 1 mode active

Switch1# show running-config interface fa0/2
interface FastEthernet0/2
 switchport mode trunk
 channel-group 1 mode active

Problem Found: Mode mismatch! (access vs trunk)

! Step 4: Fix configuration
Switch1(config)# interface fa0/2
Switch1(config-if)# shutdown
Switch1(config-if)# switchport mode access
Switch1(config-if)# switchport access vlan 10
Switch1(config-if)# no shutdown

! Step 5: Verify
Switch1# show etherchannel summary

Group  Port-channel  Protocol    Ports
------+-------------+-----------+-----------------------------------------------
1      Po1(SU)       LACP        Fa0/1(P)    Fa0/2(P)

Success! (SU) = Up and working ✅
```

---

## สรุป

### สิ่งที่ได้เรียนรู้ใน Module 6:

✅ **EtherChannel Concepts:**

- Link Aggregation
- Increased bandwidth
- Redundancy และ load balancing
- STP benefits

✅ **EtherChannel Protocols:**

- LACP (IEEE 802.3ad) - Recommended
- PAgP (Cisco proprietary) - Legacy
- Static (On mode) - Not recommended
- Protocol modes และ compatibility

✅ **Configuration:**

- Layer 2 EtherChannel (access/trunk)
- Layer 3 EtherChannel (routed)
- LACP configuration
- PAgP configuration
- Load balancing methods

✅ **Verification:**

- show etherchannel summary
- show etherchannel detail
- show interfaces port-channel
- show lacp/pagp neighbor

✅ **Troubleshooting:**

- Common problems
- Misconfigurations
- Systematic approach
- Resolution steps

---

## คำสั่งสำคัญสรุป

### Configuration:

```cisco
! Create EtherChannel (LACP - Recommended)
interface range <interfaces>
 channel-group <number> mode active

! Create EtherChannel (PAgP)
interface range <interfaces>
 channel-group <number> mode desirable

! Configure Port-Channel
interface port-channel <number>
 description <text>
 switchport mode trunk
 switchport trunk native vlan <vlan>
 switchport trunk allowed vlan <vlan-list>

! Layer 3 EtherChannel
interface range <interfaces>
 no switchport
 channel-group <number> mode active

interface port-channel <number>
 no switchport
 ip address <ip> <mask>
```

### Verification:

```cisco
show etherchannel summary
show etherchannel <number> detail
show etherchannel load-balance
show interfaces port-channel <number>
show lacp neighbor
show pagp neighbor
show interfaces trunk
```

### Load Balancing:

```cisco
port-channel load-balance {src-mac | dst-mac | src-dst-mac | 
                          src-ip | dst-ip | src-dst-ip}
show etherchannel load-balance
```

### Troubleshooting:

```cisco
show etherchannel summary
show running-config interface <interface>
show interfaces status
show interfaces <interface>
show lacp internal
show pagp internal
```

---

## Best Practices

### Design:

```
✅ Use LACP (IEEE standard, vendor independent)
✅ Use active-active mode (both sides active)
✅ Maximum 8 active links
✅ Same speed และ duplex ทุก links
✅ Same configuration ทุก member interfaces
✅ Configure port-channel interface (easier management)
✅ Use descriptive descriptions
```

### Configuration:

```
✅ Shutdown interfaces ก่อน configure
✅ Configure interfaces พร้อมกัน (interface range)
✅ Verify configuration ก่อน no shutdown
✅ Test failover (disconnect 1 link)
✅ Monitor EtherChannel status
✅ Document configuration
```

### Avoid:

```
❌ ไม่ใช้ "on" mode (no protocol)
❌ ไม่ mix speeds (all 1G or all 10G)
❌ ไม่ mix duplex (all full-duplex)
❌ ไม่ mix VLANs (access/trunk)
❌ ไม่ใช้ PAgP (legacy, Cisco only)
❌ ไม่ leave default load-balance (อาจไม่เหมาะ)
```

---

## EtherChannel Modes Reference

### LACP Modes:

```
Active + Active = EtherChannel ✅ (Recommended)
Active + Passive = EtherChannel ✅
Passive + Passive = No EtherChannel ❌
Active/Passive + On = No EtherChannel ❌
```

### PAgP Modes:

```
Desirable + Desirable = EtherChannel ✅
Desirable + Auto = EtherChannel ✅
Auto + Auto = No EtherChannel ❌
Desirable/Auto + On = No EtherChannel ❌
```

### Static Mode:

```
On + On = EtherChannel ✅ (Not recommended)
On + LACP/PAgP = No EtherChannel ❌
```

---

## Lab Activities

### Lab 6.1: Configure EtherChannel with LACP

**วัตถุประสงค์:**

- Configure 2-link EtherChannel
- Use LACP active-active
- Verify operation
- Test redundancy

### Lab 6.2: Configure EtherChannel with PAgP

**วัตถุประสงค์:**

- Configure EtherChannel with PAgP
- Test different mode combinations
- Compare with LACP
- Understand limitations

### Lab 6.3: Troubleshoot EtherChannel

**วัตถุประสงค์:**

- Identify misconfigurations
- Use troubleshooting commands
- Fix problems
- Verify solutions

### Lab 6.4: Configure Load Balancing

**วัตถุประสงค์:**

- Test different load balance methods
- Observe traffic distribution
- Choose appropriate method
- Verify with show commands

---

## Packet Tracer Activities

### PT 6.1: Basic EtherChannel

**Tasks:**

1. Configure 2-port EtherChannel
2. Use LACP active mode
3. Configure as trunk
4. Verify with show commands
5. Test connectivity

### PT 6.2: Multi-Switch EtherChannel

**Tasks:**

1. Configure EtherChannel topology
2. Multiple EtherChannels
3. Different channel-groups
4. Verify all channels
5. Test redundancy

### PT 6.3: Troubleshooting Challenge

**Tasks:**

1. Identify problems
2. Fix configuration errors
3. Verify EtherChannel comes up
4. Document issues found

---

## คำถามทบทวน

1. EtherChannel คืออะไร? มีประโยชน์อย่างไร?
2. ต่างระหว่าง LACP และ PAgP อย่างไร?
3. EtherChannel สามารถมีได้กี่ links?
4. ทำไม passive + passive (LACP) ไม่เป็น EtherChannel?
5. Interface configuration ต้องเหมือนกันอย่างไรบ้าง?
6. Load balancing ทำงานอย่างไร?
7. Layer 3 EtherChannel ต่างจาก Layer 2 อย่างไร?
8. Mode "on" มีข้อเสียอะไร?
9. จะ troubleshoot EtherChannel ที่ suspended ได้อย่างไร?
10. BPDU Guard สำคัญอย่างไรกับ EtherChannel?

---

## เฉลยคำถาม

1. **EtherChannel:** รวมหลาย physical links เป็น 1 logical link, ประโยชน์: bandwidth เพิ่ม, redundancy, load balancing
2. **LACP vs PAgP:** LACP = IEEE standard (vendor independent), PAgP = Cisco proprietary; LACP รองรับ 16 links, PAgP รองรับ 8 links
3. **Links:** Maximum 8 active links (LACP รองรับ +8 standby)
4. **Passive + Passive:** ทั้งคู่รอกัน ไม่มีฝั่งใดเริ่ม negotiate = EtherChannel ไม่ขึ้น
5. **Configuration:** Speed, duplex, mode (access/trunk), VLAN, native VLAN ต้องเหมือนกันทุก interfaces
6. **Load Balancing:** ใช้ hash algorithm กระจาย traffic, per-flow (not per-packet), methods: src-dst-ip, src-dst-mac, etc.
7. **Layer 3:** no switchport, กำหนด IP address ได้, ใช้สำหรับ routing; Layer 2: switchport, ใช้สำหรับ switching
8. **Mode On:** ไม่มี protocol, ไม่ detect misconfig, ไม่มี automatic recovery, hard to troubleshoot
9. **Troubleshoot Suspended:** show etherchannel summary/detail, ตรวจสอบ speed/duplex/VLAN, แก้ให้เหมือนกันทุก interfaces
10. **BPDU Guard:** ไม่เกี่ยวกับ EtherChannel โดยตรง, แต่ควรใช้กับ access ports ที่อาจต่อ switches ผิด

---

**หมายเหตุ:** EtherChannel เป็นเทคโนโลยีสำคัญในการเพิ่ม bandwidth และ redundancy ใน switched networks การเข้าใจ EtherChannel จำเป็นสำหรับการออกแบบและ troubleshoot enterprise networks

---

**เอกสารจัดทำโดย:** Claude (Anthropic AI)  
**Module:** CCNA 2 Module 6 - EtherChannel  
**Version:** v7.02 SRWE  
**วันที่อัพเดท:** พฤศจิกายน 2025