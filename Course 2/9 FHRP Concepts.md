# CCNA 2: Module 9 - FHRP Concepts

## First Hop Redundancy Protocols

---

## สารบัญ

1. [วัตถุประสงค์ของ Module](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#%E0%B8%A7%E0%B8%B1%E0%B8%95%E0%B8%96%E0%B8%B8%E0%B8%9B%E0%B8%A3%E0%B8%B0%E0%B8%AA%E0%B8%87%E0%B8%84%E0%B9%8C%E0%B8%82%E0%B8%AD%E0%B8%87-module)
2. [First Hop Redundancy Protocols](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#1-first-hop-redundancy-protocols)
3. [HSRP (Hot Standby Router Protocol)](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#2-hsrp-hot-standby-router-protocol)
4. [VRRP and GLBP](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#3-vrrp-and-glbp)
5. [สรุป](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#%E0%B8%AA%E0%B8%A3%E0%B8%B8%E0%B8%9B)

---

## วัตถุประสงค์ของ Module

เมื่อจบ Module นี้ คุณจะสามารถ:

- ✅ อธิบายความจำเป็นของ first hop redundancy
- ✅ เข้าใจวัตถุประสงค์ของ FHRP
- ✅ อธิบายการทำงานของ HSRP
- ✅ Configure HSRP
- ✅ Verify HSRP operation
- ✅ เข้าใจ VRRP และ GLBP
- ✅ เปรียบเทียบ HSRP, VRRP, และ GLBP

---

## 1. First Hop Redundancy Protocols

### โปรโตคอลสำหรับ Default Gateway Redundancy

### 1.1 Default Gateway Limitations

**ปัญหา: Single Point of Failure**

```
Traditional Network (No Redundancy):

     [R1] 192.168.1.1 (Default Gateway)
       │
   [Switch]
     / │ \
   PC1 PC2 PC3

Configuration:
- PCs default gateway: 192.168.1.1

Problem:
ถ้า R1 fail → PCs ไม่สามารถออกไปข้างนอก LAN ได้!
= Single Point of Failure
= Network Down!
```

**ผลกระทบ:**

```
❌ Network outage
❌ Users ไม่สามารถเข้า Internet
❌ ไม่สามารถเข้าถึง resources ข้าง LANs อื่น
❌ Business impact
❌ Loss of productivity
```

### 1.2 Router Redundancy

**วิธีแก้: ใช้ 2 Routers**

```
Network with Redundant Routers:

    [R1] 192.168.1.1      [R2] 192.168.1.2
       \                    /
        \                  /
            [Switch]
           /   |   \
        PC1   PC2  PC3

Problem:
- PCs ต้องกำหนด default gateway เป็น 192.168.1.1 หรือ 192.168.1.2?
- ถ้าเลือก R1 และ R1 fail → ต้อง manually เปลี่ยนเป็น R2
- ไม่ automatic failover!
```

**ปัญหาของ Manual Configuration:**

```
❌ ต้องเปลี่ยน default gateway manual บนทุก PC
❌ Downtime ระหว่างการเปลี่ยน
❌ User ไม่รู้ว่าต้องเปลี่ยน
❌ Scalability issue (หลายร้อย/พัน PCs)
```

### 1.3 FHRP Solution

**First Hop Redundancy Protocol (FHRP):**

- กลุ่ม routers ทำงานเป็นทีม
- แสดงตัวเป็น virtual router เดียว
- มี virtual IP address (default gateway)
- มี virtual MAC address
- Automatic failover

**FHRP Benefits:**

```
✅ Automatic failover (ไม่ต้อง manual)
✅ Transparent to end devices
✅ Load balancing (บาง protocols)
✅ High availability
✅ No single point of failure
✅ Fast convergence
```

**FHRP Operation:**

```
    Virtual Router: 192.168.1.254 (Virtual IP)
        /                    \
    [R1] Active          [R2] Standby
  192.168.1.1          192.168.1.2
       \                    /
            [Switch]
           /   |   \
        PC1   PC2  PC3
        
PCs Configuration:
- Default Gateway: 192.168.1.254 (Virtual IP)

Normal Operation:
- R1 (Active) forwards traffic
- R2 (Standby) monitors R1

When R1 Fails:
- R2 detects failure
- R2 becomes Active
- Takes over virtual IP/MAC
- Traffic continues through R2
- Automatic! No manual intervention!
```

### 1.4 FHRP Options

**Three Main FHRP Protocols:**

**1. HSRP (Hot Standby Router Protocol)**

```
- Cisco proprietary
- RFC 2281
- Most common
- Active/Standby model
- Version 1 และ Version 2
```

**2. VRRP (Virtual Router Redundancy Protocol)**

```
- Open standard (IEEE RFC 5798)
- Similar to HSRP
- Master/Backup model
- Vendor independent
```

**3. GLBP (Gateway Load Balancing Protocol)**

```
- Cisco proprietary
- Load balancing + redundancy
- Multiple active routers
- AVG (Active Virtual Gateway)
- AVF (Active Virtual Forwarder)
```

**เปรียบเทียบ:**

|Feature|HSRP|VRRP|GLBP|
|---|---|---|---|
|**Vendor**|Cisco|Open Standard|Cisco|
|**RFC**|2281|5798|-|
|**Active Routers**|1|1|Multiple|
|**Load Balancing**|No|No|Yes|
|**Default Priority**|100|100|100|
|**Preemption**|Off (default)|On (default)|On (default)|
|**Virtual MAC**|0000.0c07.acXX|0000.5e00.01XX|0007.b400.XXYY|
|**Multicast**|v1: 224.0.0.2<br>v2: 224.0.0.102|224.0.0.18|224.0.0.102|

---

## 2. HSRP (Hot Standby Router Protocol)

### โปรโตคอล Hot Standby ของ Cisco

### 2.1 HSRP Overview

**HSRP (Hot Standby Router Protocol):**

- Cisco proprietary
- RFC 2281
- Active/Standby model
- 1 router active, others standby
- Virtual IP และ virtual MAC

**HSRP Versions:**

```
HSRP Version 1:
- Original version
- Group numbers: 0-255
- Virtual MAC: 0000.0c07.acXX
- Multicast: 224.0.0.2
- Hello: 3 seconds
- Hold: 10 seconds

HSRP Version 2:
- Improved version
- Group numbers: 0-4095
- Virtual MAC: 0000.0c9f.fXXX
- Multicast: 224.0.0.102
- IPv6 support
- Millisecond timers
- Text authentication
```

### 2.2 HSRP Router Roles

**HSRP Roles:**

**1. Active Router**

```
- Router ที่ forward traffic
- Owns virtual IP address
- Responds to ARP requests for virtual IP
- Sends hello messages
- Highest priority (หรือ highest IP ถ้า priority เท่ากัน)
```

**2. Standby Router**

```
- Router สำรอง
- Monitors active router
- Ready to take over
- รับ hello messages จาก active
- Priority รองลงมา
```

**3. Other Routers**

```
- Routers อื่นๆ ใน HSRP group
- Listen to hello messages
- Ready to become standby
- Priority ต่ำกว่า standby
```

**Priority:**

```
Range: 0-255
Default: 100
Higher = Better

Example:
R1: Priority 150 → Active
R2: Priority 100 → Standby
R3: Priority 50  → Listening
```

### 2.3 HSRP States

**HSRP State Machine:**

```
1. Initial (เริ่มต้น)
   - HSRP ยังไม่ได้ start
   - Interface down หรือ HSRP ยังไม่ได้ configure

2. Learn (เรียนรู้)
   - Router รอรับ hello จาก active router
   - ยังไม่รู้ virtual IP
   - Waiting for information

3. Listen (ฟัง)
   - Router รู้ virtual IP แล้ว
   - ไม่ใช่ active หรือ standby
   - Monitor hello messages

4. Speak (พูด)
   - Router ส่ง hello messages
   - เข้าร่วมการเลือก active/standby
   - Participating in election

5. Standby (สำรอง)
   - Router เป็น standby
   - Monitor active router
   - Ready to become active

6. Active (ทำงาน)
   - Router เป็น active
   - Forward traffic
   - Send hello messages
   - Respond to ARP requests
```

**State Transitions:**

```
Initial → Learn → Listen → Speak → Standby/Active
                              ↓
                    Active ↔ Standby
                    (failover/recovery)
```

### 2.4 HSRP Operation

**HSRP Message Types:**

```
Hello Messages:
- ส่งโดย active router
- ทุก 3 วินาที (default)
- Multicast to HSRP group
- Keep-alive mechanism

Coup Message:
- ส่งเมื่อ router ต้องการเป็น active
- Priority สูงกว่า current active
- Preemption enabled

Resign Message:
- ส่งเมื่อ active router ต้องการลาออก
- Shutdown หรือ priority ลดลง
```

**HSRP Election Process:**

```
Step 1: Routers แลกเปลี่ยน hello messages
Step 2: เปรียบเทียบ priority
        - Highest priority = Active
        - Next highest = Standby
Step 3: ถ้า priority เท่ากัน
        - เปรียบเทียบ IP address
        - Highest IP = Active
```

**Example:**

```
R1: Priority 150, IP 192.168.1.1
R2: Priority 100, IP 192.168.1.2
R3: Priority 100, IP 192.168.1.3

Election:
1. R1 → Active (highest priority)
2. R3 → Standby (same priority as R2, but higher IP)
3. R2 → Listen
```

### 2.5 HSRP Preemption

**Preemption คืออะไร:**

- กลไกที่ router ที่มี priority สูงกว่าสามารถ take over active role ได้
- Default: Disabled
- ต้อง enable manual

**Without Preemption:**

```
Initial State:
R1 (Priority 150): Active
R2 (Priority 100): Standby

R1 fails:
R2 becomes Active

R1 recovers:
R1 becomes Standby (ไม่กลับมาเป็น Active)
= Active ยังคงเป็น R2

Problem: Router ที่มี priority ต่ำกว่ายังเป็น active
```

**With Preemption:**

```
Initial State:
R1 (Priority 150): Active
R2 (Priority 100): Standby

R1 fails:
R2 becomes Active

R1 recovers:
R1 preempts R2
R1 becomes Active again
R2 becomes Standby

Result: Router ที่มี priority สูงกว่ากลับมาเป็น active
```

**Preemption Delay:**

```
เพื่อป้องกัน "flapping" (สลับกันบ่อย)
ให้ router รอก่อนจะ preempt

Example:
preempt delay minimum 60
- รอ 60 วินาทีก่อนจะ preempt
```

### 2.6 HSRP Configuration

**Basic HSRP Configuration:**

```cisco
! R1 Configuration (Active)
R1(config)# interface gigabitethernet 0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# standby 1 ip 192.168.1.254
R1(config-if)# standby 1 priority 150
R1(config-if)# standby 1 preempt
R1(config-if)# no shutdown

! R2 Configuration (Standby)
R2(config)# interface gigabitethernet 0/0
R2(config-if)# ip address 192.168.1.2 255.255.255.0
R2(config-if)# standby 1 ip 192.168.1.254
R2(config-if)# standby 1 priority 100
R2(config-if)# no shutdown
```

**คำอธิบายคำสั่ง:**

```cisco
standby <group-number> ip <virtual-ip>
- สร้าง HSRP group
- กำหนด virtual IP address
- Group number: 0-255 (v1), 0-4095 (v2)

standby <group-number> priority <priority>
- กำหนด priority (0-255)
- Default: 100
- Higher = Better

standby <group-number> preempt [delay minimum <seconds>]
- Enable preemption
- Optional: delay before preempting

standby <group-number> timers <hello> <hold>
- กำหนด hello interval และ hold time
- Default: hello 3s, hold 10s
- Example: standby 1 timers 1 3

standby version {1 | 2}
- เลือก HSRP version
- Default: version 1
```

**Advanced Configuration:**

```cisco
! R1 Advanced Configuration
R1(config)# interface gigabitethernet 0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0

! HSRP Group 1
R1(config-if)# standby version 2
R1(config-if)# standby 1 ip 192.168.1.254
R1(config-if)# standby 1 priority 150
R1(config-if)# standby 1 preempt delay minimum 60
R1(config-if)# standby 1 timers 1 3
R1(config-if)# standby 1 authentication md5 key-string MySecret
R1(config-if)# standby 1 track gigabitethernet 0/1 50
! ถ้า Gi0/1 down → ลด priority 50
R1(config-if)# no shutdown
```

### 2.7 HSRP Interface Tracking

**Interface Tracking:**

- Monitor interface status
- ถ้า interface down → ลด priority
- ทำให้ router อื่นเป็น active

**Configuration:**

```cisco
R1(config-if)# standby 1 track gigabitethernet 0/1 decrement 50

If Gi0/1 goes down:
Priority 150 - 50 = 100
R1 priority becomes 100
R2 (priority 120) becomes active
```

**Example Scenario:**

```
Topology:
Internet ── Gi0/1 ─ [R1] ─ Gi0/0 ── LAN
                    150

ถ้า Gi0/1 (uplink) down:
- R1 ยังเป็น active ใน LAN
- แต่ไม่มี Internet access!
- ทำให้ LAN ไปไหนไม่ได้

Solution: Track Gi0/1
standby 1 track gigabitethernet 0/1 50

ถ้า Gi0/1 down:
- R1 priority: 150 - 50 = 100
- R2 (priority 120) becomes active
- LAN ใช้ R2 ที่มี Internet
```

### 2.8 Verify HSRP

**Verification Commands:**

**1. Show Standby:**

```cisco
R1# show standby

GigabitEthernet0/0 - Group 1
  State is Active
    2 state changes, last state change 00:05:23
  Virtual IP address is 192.168.1.254
  Active virtual MAC address is 0000.0c07.ac01
    Local virtual MAC address is 0000.0c07.ac01 (v1 default)
  Hello time 3 sec, hold time 10 sec
    Next hello sent in 1.264 secs
  Preemption enabled, delay min 60 secs
  Active router is local
  Standby router is 192.168.1.2, priority 100 (expires in 9.264 sec)
  Priority 150 (configured 150)
  Group name is "hsrp-Gi0/0-1" (default)
```

**2. Show Standby Brief:**

```cisco
R1# show standby brief

                     P indicates configured to preempt.
                     |
Interface   Grp  Pri P State    Active          Standby         Virtual IP
Gi0/0       1    150 P Active   local           192.168.1.2     192.168.1.254
```

**3. Show Standby on Standby Router:**

```cisco
R2# show standby

GigabitEthernet0/0 - Group 1
  State is Standby
    1 state change, last state change 00:05:30
  Virtual IP address is 192.168.1.254
  Active virtual MAC address is 0000.0c07.ac01
    Local virtual MAC address is 0000.0c07.ac01 (v1 default)
  Hello time 3 sec, hold time 10 sec
    Next hello sent in 2.512 secs
  Preemption disabled
  Active router is 192.168.1.1, priority 150 (expires in 8.512 sec)
  Standby router is local
  Priority 100 (default 100)
  Group name is "hsrp-Gi0/0-1" (default)
```

**4. Debug HSRP:**

```cisco
R1# debug standby events
R1# debug standby packets

! ดู HSRP state changes และ messages
! อย่าลืมปิด debug
R1# no debug all
```

### 2.9 HSRP for IPv6

**HSRPv2 รองรับ IPv6:**

```cisco
! Enable IPv6 routing
R1(config)# ipv6 unicast-routing

! Configure interface
R1(config)# interface gigabitethernet 0/0
R1(config-if)# ipv6 address 2001:db8:acad:1::1/64
R1(config-if)# ipv6 address fe80::1 link-local

! Configure HSRP for IPv6
R1(config-if)# standby version 2
R1(config-if)# standby 1 ipv6 2001:db8:acad:1::254/64
R1(config-if)# standby 1 priority 150
R1(config-if)# standby 1 preempt
R1(config-if)# no shutdown

! Verify
R1# show standby
```

**Note:**

- ต้องใช้ HSRP version 2 สำหรับ IPv6
- Virtual IPv6 address ต้องกำหนด prefix length

---

## 3. VRRP and GLBP

### โปรโตคอล FHRP อื่นๆ

### 3.1 VRRP Overview

**VRRP (Virtual Router Redundancy Protocol):**

- Open standard (RFC 5798)
- Similar to HSRP
- Vendor independent
- Master/Backup terminology

**VRRP Features:**

```
Protocol:
- IEEE RFC 5798
- IP protocol 112
- Multicast: 224.0.0.18

Roles:
- Master (เหมือน Active ใน HSRP)
- Backup (เหมือน Standby ใน HSRP)

Virtual MAC: 0000.5e00.01XX
- XX = VRRP group number (hex)

Priority:
- Range: 1-254
- Default: 100
- 255 = IP address owner (highest)

Timers:
- Advertisement: 1 second (default)
- Master Down Interval: 3 × Advertisement + Skew time

Preemption: Enabled by default
```

**VRRP vs HSRP:**

|Feature|HSRP|VRRP|
|---|---|---|
|Standard|Cisco proprietary|Open standard|
|Active Router|Active|Master|
|Standby Router|Standby|Backup|
|Priority Default|100|100|
|Priority Range|0-255|1-254|
|Preemption|Disabled (default)|Enabled (default)|
|Virtual MAC|0000.0c07.acXX|0000.5e00.01XX|
|Multicast|224.0.0.2 (v1)|224.0.0.18|
|Hello Time|3 seconds|1 second|
|Load Balancing|No|No|

**VRRP Configuration:**

```cisco
! R1 Configuration (Master)
R1(config)# interface gigabitethernet 0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# vrrp 1 ip 192.168.1.254
R1(config-if)# vrrp 1 priority 150
R1(config-if)# vrrp 1 preempt
! preempt enabled by default
R1(config-if)# no shutdown

! R2 Configuration (Backup)
R2(config)# interface gigabitethernet 0/0
R2(config-if)# ip address 192.168.1.2 255.255.255.0
R2(config-if)# vrrp 1 ip 192.168.1.254
R2(config-if)# vrrp 1 priority 100
R2(config-if)# no shutdown
```

**Verify VRRP:**

```cisco
R1# show vrrp

GigabitEthernet0/0 - Group 1
  State is Master
  Virtual IP address is 192.168.1.254
  Virtual MAC address is 0000.5e00.0101
  Advertisement interval is 1.000 sec
  Preemption enabled
  Priority is 150
  Master Router is 192.168.1.1 (local), priority is 150
  Master Advertisement interval is 1.000 sec
  Master Down interval is 3.609 sec
```

### 3.2 GLBP Overview

**GLBP (Gateway Load Balancing Protocol):**

- Cisco proprietary
- Redundancy + Load balancing
- Multiple routers active พร้อมกัน
- AVG และ AVF roles

**GLBP Concepts:**

**AVG (Active Virtual Gateway):**

```
- 1 router per GLBP group
- Assigns virtual MAC addresses
- Responds to ARP requests
- Highest priority = AVG
```

**AVF (Active Virtual Forwarder):**

```
- Up to 4 routers (default)
- แต่ละ router forward traffic
- แต่ละ router มี unique virtual MAC
- Load balancing!
```

**GLBP Operation:**

```
GLBP Group 1: Virtual IP 192.168.1.254

AVG: R1 (Priority 150)
AVF1: R1 → Virtual MAC: 0007.b400.0101
AVF2: R2 → Virtual MAC: 0007.b400.0102
AVF3: R3 → Virtual MAC: 0007.b400.0103

When PC1 ARPs for 192.168.1.254:
- R1 (AVG) responds with Virtual MAC 0101

When PC2 ARPs for 192.168.1.254:
- R1 (AVG) responds with Virtual MAC 0102

When PC3 ARPs for 192.168.1.254:
- R1 (AVG) responds with Virtual MAC 0103

Result:
Traffic distributed across all routers!
= Load Balancing
```

**GLBP Load Balancing Methods:**

**1. Round-Robin (Default):**

```
AVG แจก MAC addresses แบบสลับกัน
- PC1 → MAC1 (R1)
- PC2 → MAC2 (R2)
- PC3 → MAC3 (R3)
- PC4 → MAC1 (R1)
- ...
```

**2. Weighted:**

```
แจกตาม weight ของแต่ละ router
R1: Weight 200 → รับ traffic มากกว่า
R2: Weight 100 → รับ traffic น้อยกว่า
```

**3. Host-Dependent:**

```
Host เดียวกันได้ MAC เดียวกันเสมอ
เหมาะกับ applications ที่ต้องการ consistency
```

**GLBP Configuration:**

```cisco
! R1 Configuration (AVG)
R1(config)# interface gigabitethernet 0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# glbp 1 ip 192.168.1.254
R1(config-if)# glbp 1 priority 150
R1(config-if)# glbp 1 preempt
R1(config-if)# glbp 1 load-balancing round-robin
R1(config-if)# no shutdown

! R2 Configuration (AVF)
R2(config)# interface gigabitethernet 0/0
R2(config-if)# ip address 192.168.1.2 255.255.255.0
R2(config-if)# glbp 1 ip 192.168.1.254
R2(config-if)# glbp 1 priority 100
R2(config-if)# no shutdown

! R3 Configuration (AVF)
R3(config)# interface gigabitethernet 0/0
R3(config-if)# ip address 192.168.1.3 255.255.255.0
R3(config-if)# glbp 1 ip 192.168.1.254
R3(config-if)# glbp 1 priority 100
R3(config-if)# no shutdown
```

**Verify GLBP:**

```cisco
R1# show glbp

GigabitEthernet0/0 - Group 1
  State is Active
    2 state changes, last state change 00:10:24
  Virtual IP address is 192.168.1.254
  Hello time 3 sec, hold time 10 sec
    Next hello sent in 0.832 secs
  Redirect time 600 sec, forwarder timeout 14400 sec
  Preemption enabled, min delay 0 sec
  Active is local
  Standby is 192.168.1.2, priority 100 (expires in 9.216 sec)
  Priority 150 (configured)
  Weighting 100 (default 100), thresholds: lower 1, upper 100
  Load balancing: round-robin
  Group members:
    0001.c7b6.8f00 (192.168.1.1) local
    0001.c7b6.8f01 (192.168.1.2)
    0001.c7b6.8f02 (192.168.1.3)
  There are 3 forwarders (1 active)
  Forwarder 1
    State is Active
      1 state change, last state change 00:10:19
    MAC address is 0007.b400.0101 (default)
    Owner ID is 0001.c7b6.8f00
    Redirection enabled
    Preemption enabled, min delay 30 sec
    Active is local, weighting 100
  Forwarder 2
    State is Listen
    MAC address is 0007.b400.0102 (learnt)
    Owner ID is 0001.c7b6.8f01
    ...
```

### 3.3 FHRP Comparison

**เมื่อไหร่ควรใช้อะไร:**

**HSRP:**

```
✅ Cisco-only environment
✅ ไม่ต้องการ load balancing
✅ Simple active/standby
✅ Most common และ well-tested
✅ Good documentation

Use Case:
- Small-medium networks
- Cisco switches/routers
- Basic redundancy needs
```

**VRRP:**

```
✅ Multi-vendor environment
✅ Open standard required
✅ Similar to HSRP
✅ IEEE compliance

Use Case:
- Mixed vendor environment
- Standard compliance needed
- Migration from HSRP easy
```

**GLBP:**

```
✅ Load balancing + redundancy
✅ Multiple routers active
✅ Better bandwidth utilization
✅ Cisco environment

Use Case:
- High-traffic networks
- Need load balancing
- Multiple equal links
- Cisco equipment
```

**Summary Table:**

|Feature|HSRP|VRRP|GLBP|
|---|---|---|---|
|**Vendor**|Cisco|Open Standard|Cisco|
|**Active Routers**|1|1|Multiple (up to 4)|
|**Load Balancing**|❌|❌|✅|
|**Complexity**|Low|Low|Medium|
|**Common Use**|Very High|Medium|Low-Medium|
|**Best For**|General redundancy|Multi-vendor|Load balancing needed|

---

## สรุป

### สิ่งที่ได้เรียนรู้ใน Module 9:

✅ **First Hop Redundancy:**

- ความจำเป็นของ gateway redundancy
- Single point of failure problem
- FHRP solution

✅ **HSRP:**

- Active/Standby model
- Priority และ preemption
- Interface tracking
- HSRP states และ operation
- Configuration และ verification
- HSRPv2 for IPv6

✅ **VRRP:**

- Open standard alternative
- Master/Backup terminology
- Similar to HSRP
- Preemption enabled by default

✅ **GLBP:**

- Load balancing + redundancy
- AVG และ AVF roles
- Multiple active routers
- Load balancing methods

✅ **Comparison:**

- HSRP vs VRRP vs GLBP
- Use cases
- Selection criteria

---

## คำสั่งสำคัญสรุป

### HSRP Configuration:

```cisco
! Basic HSRP
interface <interface>
 standby <group> ip <virtual-ip>
 standby <group> priority <priority>
 standby <group> preempt [delay minimum <seconds>]
 standby <group> timers <hello> <hold>
 standby <group> track <interface> [decrement <value>]
 standby <group> authentication md5 key-string <password>
 standby version {1 | 2}

! HSRP for IPv6
 standby <group> ipv6 <virtual-ipv6>/<prefix-length>
```

### VRRP Configuration:

```cisco
interface <interface>
 vrrp <group> ip <virtual-ip>
 vrrp <group> priority <priority>
 vrrp <group> preempt
 vrrp <group> timers advertise <interval>
 vrrp <group> track <interface> [decrement <value>]
```

### GLBP Configuration:

```cisco
interface <interface>
 glbp <group> ip <virtual-ip>
 glbp <group> priority <priority>
 glbp <group> preempt
 glbp <group> load-balancing {round-robin | weighted | host-dependent}
 glbp <group> weighting <weight>
```

### Verification:

```cisco
show standby [brief]
show vrrp [brief]
show glbp [brief]
debug standby events
debug standby packets
```

---

## Best Practices

### FHRP Design:

```
✅ Use HSRP for Cisco-only environments
✅ Use VRRP for multi-vendor
✅ Use GLBP for load balancing needs
✅ Always use preemption (HSRP)
✅ Configure interface tracking
✅ Use authentication (HSRP)
✅ Document priority scheme
```

### Priority Planning:

```
✅ Primary router: 150-200
✅ Secondary router: 100-149
✅ Tertiary router: 50-99
✅ Leave gaps (เพิ่ม/ลบ routers ได้ง่าย)
✅ Track critical interfaces
✅ Adjust decrement values appropriately
```

### Timers:

```
✅ Default timers (3s/10s) เหมาะสมส่วนใหญ่
✅ Aggressive timers (1s/3s) ถ้าต้องการ fast failover
✅ ระวัง CPU load กับ aggressive timers
✅ Ensure consistent timers across group
```

### Security:

```
✅ Use MD5 authentication
✅ Unique keys per HSRP group
✅ Change keys regularly
✅ Don't use default settings
```

---

## Common Issues

**Problem 1: Both Routers Active**

```
Symptom:
- Duplicate virtual IP/MAC
- Network instability

Cause:
- Virtual IP mismatch
- Split HSRP group

Solution:
ตรวจสอบ virtual IP ตรงกันทุก router
```

**Problem 2: Flapping**

```
Symptom:
- Active/Standby สลับกันบ่อย

Cause:
- Preemption ไม่มี delay
- Interface unstable

Solution:
standby 1 preempt delay minimum 60
```

**Problem 3: No Failover**

```
Symptom:
- Active router fails แต่ standby ไม่ take over

Cause:
- Timers misconfigured
- Communication blocked

Troubleshooting:
show standby
debug standby events
```

**Problem 4: Tracking Issues**

```
Symptom:
- Interface down แต่ priority ไม่ลด

Cause:
- Track ไม่ได้ configure
- Wrong interface tracked

Solution:
standby 1 track <interface> decrement <value>
show standby
```

---

## Lab Activities

### Lab 9.1: Configure HSRP

**วัตถุประสงค์:**

- Configure basic HSRP
- Set priorities
- Enable preemption
- Test failover

### Lab 9.2: HSRP Interface Tracking

**วัตถุประสงค์:**

- Configure interface tracking
- Test priority changes
- Verify automatic failover

### Lab 9.3: Compare HSRP and VRRP

**วัตถุประสงค์:**

- Configure HSRP
- Configure VRRP
- Compare operations
- Document differences

---

## Packet Tracer Activities

### PT 9.1: Basic HSRP

**Tasks:**

1. Configure HSRP group
2. Set priorities
3. Configure virtual IP
4. Verify active/standby
5. Test connectivity

### PT 9.2: HSRP Failover

**Tasks:**

1. Configure complete HSRP
2. Enable preemption
3. Shutdown active router
4. Verify failover
5. Bring router back up

### PT 9.3: GLBP Load Balancing

**Tasks:**

1. Configure GLBP
2. Verify AVG/AVF
3. Test load balancing
4. Compare with HSRP

---

## คำถามทบทวน

1. FHRP ย่อมาจากอะไร? ทำไมต้องมี?
2. HSRP มี router roles อะไรบ้าง?
3. Priority ใน HSRP ทำงานอย่างไร?
4. Preemption คืออะไร? เปิดหรือปิด by default?
5. Interface tracking ทำอะไร?
6. ต่างระหว่าง HSRP และ VRRP อย่างไร?
7. GLBP ต่างจาก HSRP อย่างไร?
8. AVG และ AVF คืออะไร?
9. HSRP ใช้ virtual MAC address แบบไหน?
10. เมื่อไหร่ควรใช้ GLBP แทน HSRP?

---

## เฉลยคำถาม

1. **FHRP:** First Hop Redundancy Protocol, ป้องกัน single point of failure ที่ default gateway
2. **HSRP Roles:** Active (forward traffic), Standby (monitor active), Listening (other routers)
3. **Priority:** 0-255, higher = better, default = 100, highest priority = Active
4. **Preemption:** router ที่มี priority สูงกว่าสามารถ take over active role, disabled by default
5. **Interface Tracking:** monitor interface status, ถ้า down → ลด priority → failover
6. **HSRP vs VRRP:** HSRP = Cisco proprietary, VRRP = open standard; preemption default ต่างกัน
7. **GLBP vs HSRP:** GLBP มี load balancing, multiple routers active พร้อมกัน; HSRP มี 1 active
8. **AVG/AVF:** AVG = Active Virtual Gateway (assigns MACs), AVF = Active Virtual Forwarder (forwards traffic)
9. **Virtual MAC:** HSRP v1 = 0000.0c07.acXX, v2 = 0000.0c9f.fXXX (XX = group number)
10. **Use GLBP:** เมื่อต้องการ load balancing + redundancy, multiple equal links, Cisco environment

---

**หมายเหตุ:** FHRP เป็นโปรโตคอลสำคัญในการสร้าง high availability networks การเข้าใจ HSRP, VRRP, และ GLBP จำเป็นสำหรับการออกแบบ resilient networks

---

**เอกสารจัดทำโดย:** Claude (Anthropic AI)  
**Module:** CCNA 2 Module 9 - FHRP Concepts  
**Version:** v7.02 SRWE  
**วันที่อัพเดท:** พฤศจิกายน 2025