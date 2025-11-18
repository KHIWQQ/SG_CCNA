# CCNA 2: Module 5 - STP Concepts

## Spanning Tree Protocol

---

## สารบัญ

1. [วัตถุประสงค์ของ Module](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#%E0%B8%A7%E0%B8%B1%E0%B8%95%E0%B8%96%E0%B8%B8%E0%B8%9B%E0%B8%A3%E0%B8%B0%E0%B8%AA%E0%B8%87%E0%B8%84%E0%B9%8C%E0%B8%82%E0%B8%AD%E0%B8%87-module)
2. [Purpose of Spanning Tree](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#1-purpose-of-spanning-tree)
3. [STP Operations](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#2-stp-operations)
4. [Evolution of STP](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#3-evolution-of-stp)
5. [สรุป](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#%E0%B8%AA%E0%B8%A3%E0%B8%B8%E0%B8%9B)

---

## วัตถุประสงค์ของ Module

เมื่อจบ Module นี้ คุณจะสามารถ:

- ✅ อธิบายวัตถุประสงค์ของ Spanning Tree Protocol
- ✅ เข้าใจปัญหา Layer 2 loops
- ✅ อธิบายการทำงานของ STP
- ✅ เข้าใจ BPDU (Bridge Protocol Data Unit)
- ✅ อธิบาย BID (Bridge ID) และการเลือก root bridge
- ✅ เข้าใจ port roles และ port states
- ✅ อธิบายกระบวนการ STP convergence
- ✅ เปรียบเทียบ STP variants (STP, RSTP, PVST+, Rapid PVST+)
- ✅ Configure PortFast และ BPDU Guard

---

## 1. Purpose of Spanning Tree

### วัตถุประสงค์ของ Spanning Tree

### 1.1 Redundancy in Layer 2 Switched Networks

**ทำไมต้องมี Redundancy:**

```
Single Point of Failure:
[PC] ─── [Switch] ─── [Server]
              ↓
          [failed]
              ↓
         Network Down!

With Redundancy:
[PC] ─── [Switch1] ──┬── [Switch3] ─── [Server]
              │      │       │
              └──[Switch2]───┘

- ถ้า switch ตัวใดเสีย ยังมีเส้นทางอื่น
- High Availability
- Fault Tolerance
```

**ประโยชน์ของ Redundancy:**

- ✅ No single point of failure
- ✅ Increased availability
- ✅ Load balancing (บางกรณี)
- ✅ Business continuity

### 1.2 Layer 2 Loops Problem

**ปัญหาเมื่อมี Redundant Links:**

```
Topology with Redundant Links:
    [Switch A]
     /      \
    /        \
[Switch B]──[Switch C]

= มี 3 paths ระหว่าง switches
= ถ้าไม่มี STP → เกิด loops!
```

**Layer 2 Loop คืออะไร:**

- Frames วนซ้ำไปมาในเครือข่ายไม่มีที่สิ้นสุด
- Switches ไม่มี TTL (Time to Live) mechanism
- Frames ไม่มีวันหมดอายุ
- ทำให้เครือข่ายล่ม

**สาเหตุของ Loops:**

1. **No TTL in Ethernet frames** - ไม่มีกลไกหยุด loops
2. **Continual frame forwarding** - switches ส่งต่อ frames ตลอดเวลา
3. **Broadcasting** - broadcast/multicast frames แพร่กระจายทุกพอร์ต

### 1.3 Issues with Layer 2 Loops

**1. Broadcast Storm:**

```
[PC-A] ส่ง Broadcast Frame
    ↓
[Switch A] flood ออกทุกพอร์ต
    ↓
[Switch B] และ [Switch C] รับ frame
    ↓
แต่ละ switch flood กลับไปอีก switch
    ↓
Loop วนซ้ำไม่มีที่สิ้นสุด!
    ↓
Network Congestion (100% CPU, Bandwidth)
    ↓
Network Down!
```

**ผลกระทบ:**

- 💥 CPU utilization 100%
- 💥 Link saturation
- 💥 Network unavailable
- 💥 Users ไม่สามารถใช้งานได้

**2. MAC Address Table Instability:**

```
สถานการณ์:
PC-A (MAC: 00-AA) เชื่อมกับ Switch A

Without Loop:
Switch B เรียนรู้: 00-AA → Port 1 (ไป Switch A)

With Loop:
- Frame จาก PC-A มาที่ Switch B ทาง Port 1
- Switch B เรียนรู้: 00-AA → Port 1
- แต่ frame เดียวกันวน loop กลับมาทาง Port 2
- Switch B เรียนรู้ใหม่: 00-AA → Port 2
- วนซ้ำ...

ผลลัพธ์:
MAC Address Table เปลี่ยนแปลงตลอดเวลา
= ไม่รู้ว่า 00-AA อยู่พอร์ตไหน
= Flooding ทุกครั้ง
= Network chaos!
```

**3. Duplicate Frame Transmission:**

```
PC-A ส่ง Unicast Frame ไป PC-B
    ↓
Frame ไป PC-B ได้หลายทาง (เพราะมี redundant links)
    ↓
PC-B รับ Frame เดียวกันหลายครั้ง
    ↓
ปัญหา:
- Upper layer protocols สับสน
- Duplicate data
- Application errors
```

### 1.4 STP Solution

**Spanning Tree Protocol (STP):**

- IEEE 802.1D standard
- ป้องกัน Layer 2 loops
- สร้าง loop-free topology
- Maintain redundancy

**วิธีการทำงาน:**

```
Physical Topology (มี loops):
    [Switch A]
     /      \
    /        \
[Switch B]──[Switch C]

Logical Topology (STP ทำให้ไม่มี loop):
    [Switch A]
     /      
    /        
[Switch B]  [Switch C]
           (link blocked)

- STP block บางพอร์ตเพื่อป้องกัน loops
- เมื่อมี failure → unblock พอร์ตเพื่อ restore connectivity
```

**STP Benefits:**

- ✅ Prevents broadcast storms
- ✅ Prevents MAC address table instability
- ✅ Prevents duplicate frames
- ✅ Maintains network redundancy
- ✅ Automatic failover

---

## 2. STP Operations

### การทำงานของ STP

### 2.1 STP Algorithm

**STP ใช้ Spanning Tree Algorithm (STA):**

1. เลือก root bridge (1 ตัวต่อ network)
2. เลือก root ports บนแต่ละ non-root bridge
3. เลือก designated ports บนแต่ละ segment
4. Block พอร์ตที่เหลือเพื่อป้องกัน loops

**กระบวนการตัดสินใจ:**

```
Step 1: Elect Root Bridge
- Bridge ที่มี lowest Bridge ID

Step 2: Elect Root Port (บนแต่ละ non-root bridge)
- พอร์ตที่มี lowest cost ไป root bridge

Step 3: Elect Designated Port (บนแต่ละ segment)
- พอร์ตที่มี lowest cost ไป root bridge

Step 4: Block Remaining Ports
- พอร์ตที่ไม่ได้เป็น root/designated → alternate/backup
```

### 2.2 Bridge ID (BID)

**โครงสร้าง Bridge ID:**

```
Original 802.1D (8 bytes):
┌─────────────────┬──────────────────────────────────┐
│ Bridge Priority │      MAC Address                 │
│    (2 bytes)    │        (6 bytes)                 │
│     0-65535     │   Unique per switch             │
└─────────────────┴──────────────────────────────────┘

Modern 802.1D with Extended System ID (8 bytes):
┌──────────────┬─────────────┬──────────────────────┐
│Bridge Priority│ Extended   │    MAC Address       │
│  (4 bits)    │ System ID  │     (6 bytes)        │
│   0-61440    │  (12 bits) │                      │
│  (incr 4096) │  VLAN ID   │                      │
└──────────────┴─────────────┴──────────────────────┘

ตัวอย่าง:
Bridge Priority: 32768 (default)
Extended System ID: 10 (VLAN 10)
MAC Address: 0C:D9:96:D2:48:00

BID = 32768 + 10 + 0C:D9:96:D2:48:00
    = 32778.0CD9.96D2.4800
```

**Bridge Priority:**

- Range: 0-61440 (เดิม 0-65535)
- Increment: 4096
- Default: 32768
- ค่าที่ใช้ได้: 0, 4096, 8192, 12288, 16384, 20480, 24576, 28672, 32768, 36864, 40960, 45056, 49152, 53248, 57344, 61440

**Extended System ID:**

- 12 bits = VLAN ID (1-4094)
- เพิ่มเข้าไปใน BID
- ทำให้แต่ละ VLAN มี BID ต่างกัน

**การเปรียบเทียบ BID:**

```
Lower BID = Better (จะเป็น root bridge)

Switch A: 32768.0011.1111.1111
Switch B: 32768.0022.2222.2222
Switch C: 24576.0033.3333.3333

Winner: Switch C (lowest priority)
```

### 2.3 Root Bridge Election

**กระบวนการเลือก Root Bridge:**

```
Step 1: ทุก switch คิดว่าตัวเองเป็น root
- ส่ง BPDU ด้วย BID ของตัวเอง

Step 2: Switches เปรียบเทียบ BIDs
- รับ BPDUs จาก neighbors
- เปรียบเทียบกับ BID ของตัวเอง

Step 3: เลือก root bridge
- Switch ที่มี lowest BID ชนะ
- Switches อื่นยอมรับ

Step 4: Update BPDUs
- Root bridge ส่ง BPDUs ทุก 2 วินาที
- Non-root bridges relay BPDUs
```

**ตัวอย่างการเลือก Root:**

```
Network:
Switch A: 32768.0AAA.AAAA.AAAA
Switch B: 32768.0BBB.BBBB.BBBB
Switch C: 32768.0CCC.CCCC.CCCC

Comparison:
0AAA < 0BBB < 0CCC

Result:
Root Bridge: Switch A (lowest MAC address)
```

**การควบคุม Root Bridge:**

```cisco
! ต้องการให้ Switch A เป็น root
SwitchA(config)# spanning-tree vlan 1 priority 24576

! หรือใช้ root primary (จะตั้งเป็น 24576 หรือต่ำกว่า current root)
SwitchA(config)# spanning-tree vlan 1 root primary

! ต้องการให้เป็น secondary root (backup)
SwitchB(config)# spanning-tree vlan 1 root secondary
```

### 2.4 Path Cost

**STP Path Cost:**

- ใช้คำนวณ best path ไป root bridge
- ยิ่ง bandwidth สูง cost ยิ่งต่ำ
- IEEE standard (revised in 2004)

**Cost Table:**

```
Link Speed          Original Cost    Revised Cost
─────────────────────────────────────────────────
10 Mbps                  100              2,000,000
100 Mbps                  19                200,000
1 Gbps                     4                 20,000
10 Gbps                    2                  2,000
100 Gbps                   -                    200
1 Tbps                     -                     20
```

**Cisco switches ใช้:**

- Short path cost method (default) - revised
- Long path cost method - original

**การคำนวณ Path Cost:**

```
Topology:
         Root
          │
    ┌─────┴─────┐
  1 Gbps      1 Gbps
    │            │
 Switch A     Switch B
    │            │
  100 Mbps     1 Gbps
    │            │
 Switch C     Switch C

Path Cost จาก Switch C ไป Root:
- ผ่าน Switch A: 20,000 + 200,000 = 220,000
- ผ่าน Switch B: 20,000 + 20,000 = 40,000

Best Path: ผ่าน Switch B (lower cost)
```

**ตั้งค่า Cost Manual:**

```cisco
Switch(config)# interface gigabitethernet 0/1
Switch(config-if)# spanning-tree cost 15000
```

### 2.5 BPDU (Bridge Protocol Data Unit)

**BPDU คืออะไร:**

- Messages ที่ switches แลกเปลี่ยนกัน
- ใช้สร้างและ maintain loop-free topology
- ส่งออก Layer 2 multicast (01:80:C2:00:00:00)

**ประเภท BPDU:**

**1. Configuration BPDU:**

- ส่งจาก root bridge ทุก 2 วินาที (hello time)
- ประกอบด้วย:
    - Root Bridge ID
    - Sender Bridge ID
    - Sender Port ID
    - Path Cost to Root
    - Timer values (hello, max age, forward delay)

**2. Topology Change Notification (TCN) BPDU:**

- ส่งเมื่อมีการเปลี่ยนแปลง topology
- บอกให้ switches update MAC address tables

**BPDU Fields:**

```
┌──────────────────────────────┐
│ Protocol Identifier          │
│ Version                      │
│ Message Type                 │
│ Flags                        │
│ Root Bridge ID              │
│ Root Path Cost              │
│ Sender Bridge ID            │
│ Port ID                     │
│ Message Age                 │
│ Max Age                     │
│ Hello Time                  │
│ Forward Delay               │
└──────────────────────────────┘
```

**BPDU Timers:**

```
Hello Time: 2 seconds (default)
- ความถี่ในการส่ง Configuration BPDUs

Max Age: 20 seconds (default)
- เวลารอก่อนจะถือว่า BPDU หมดอายุ
- ถ้าไม่ได้รับ BPDU ภายใน 20 วินาที → ถือว่าลิงก์มีปัญหา

Forward Delay: 15 seconds (default)
- เวลาที่พอร์ตอยู่ใน listening และ learning states
- รวม 30 วินาที (15 + 15) ก่อนจะ forward frames
```

### 2.6 Port Roles

**STP Port Roles:**

**1. Root Port:**

```
- พอร์ตที่มี best path ไป root bridge
- ทุก non-root bridge มี 1 root port
- Root bridge ไม่มี root port
- Status: Forwarding
- เลือกโดย: Lowest path cost ไป root
```

**2. Designated Port:**

```
- พอร์ตที่ส่ง best BPDU บน segment นั้น
- ทุก segment มี 1 designated port
- ทุกพอร์ตบน root bridge เป็น designated ports
- Status: Forwarding
- เลือกโดย: Lowest path cost ไป root (ของ switch)
```

**3. Alternate Port:**

```
- พอร์ตสำรอง (backup path ไป root)
- ถูก block เพื่อป้องกัน loop
- Status: Blocking/Discarding
- เลือกโดย: Higher path cost ไป root
- จะเป็น root port ถ้า current root port fail
```

**4. Backup Port:**

```
- พอร์ตสำรอง (redundant designated port)
- เกิดเมื่อ switch มี 2 พอร์ตเชื่อมกับ segment เดียวกัน
- Status: Blocking/Discarding
- ไม่ค่อยพบในเครือข่ายทั่วไป
```

**ตัวอย่าง Port Roles:**

```
Topology:
                [Root Bridge]
                 Gi0/1  Gi0/2
                   │      │
                   │      │
             ┌─────┴──────┴─────┐
             │                  │
           Gi0/1              Gi0/1
        [Switch A]          [Switch B]
           Gi0/2──────────────Gi0/2

Port Roles:
Root Bridge:
- Gi0/1, Gi0/2: Designated Ports (DP)

Switch A:
- Gi0/1: Root Port (RP) - best path ไป root
- Gi0/2: Designated Port (DP) - forwarding on this segment

Switch B:
- Gi0/1: Root Port (RP) - best path ไป root
- Gi0/2: Alternate Port (AP) - blocked to prevent loop
```

### 2.7 Port States

**STP Port States (802.1D Original):**

**1. Blocking:**

```
- ไม่ forward data frames
- รับ BPDUs เท่านั้น
- Alternate/Backup ports อยู่ใน state นี้
- ป้องกัน loops
- Duration: 20 seconds (max age)
```

**2. Listening:**

```
- ไม่ forward data frames
- ส่งและรับ BPDUs
- เตรียมเข้าสู่ forwarding state
- Duration: 15 seconds (forward delay)
```

**3. Learning:**

```
- ไม่ forward data frames (ยัง)
- ส่งและรับ BPDUs
- เรียนรู้ MAC addresses
- Build MAC address table
- Duration: 15 seconds (forward delay)
```

**4. Forwarding:**

```
- Forward data frames
- ส่งและรับ BPDUs
- เรียนรู้ MAC addresses
- Port ทำงานปกติ
- Root ports และ Designated ports
```

**5. Disabled:**

```
- Port ถูก shutdown โดย administrator
- ไม่ participate ใน STP
- ไม่ forward frames
- ไม่รับ BPDUs
```

**Port State Transitions:**

```
Port ขึ้นใหม่:
Blocking (20s) → Listening (15s) → Learning (15s) → Forwarding
                                                      
Total time: 50 seconds (convergence time)

Port down:
Forwarding → Blocking (immediate)
```

**ตาราง Port States:**

```
State       Forward    Learn MAC    Receive      Send
            Frames     Addresses    BPDUs        BPDUs
──────────────────────────────────────────────────────────
Blocking      No          No          Yes         No
Listening     No          No          Yes         Yes
Learning      No          Yes         Yes         Yes
Forwarding    Yes         Yes         Yes         Yes
Disabled      No          No          No          No
```

### 2.8 STP Convergence

**Convergence คืออะไร:**

- กระบวนการที่ switches เห็นด้วยกับ loop-free topology
- เกิดเมื่อ:
    - Network เริ่มทำงานครั้งแรก
    - มีการเปลี่ยนแปลง topology (link fail, switch fail)

**Steps to Convergence:**

```
Step 1: Elect Root Bridge (ทันที)
- Switches แลกเปลี่ยน BPDUs
- เลือก root bridge

Step 2: Elect Root Ports (ทันที)
- แต่ละ non-root bridge เลือก root port
- Based on lowest path cost

Step 3: Elect Designated Ports (ทันที)
- แต่ละ segment เลือก designated port
- Based on lowest path cost to root

Step 4: Block Other Ports (ทันที)
- Ports ที่ไม่ได้เป็น root/designated → block

Step 5: Port State Transitions (50 วินาที)
- Blocking → Listening (15s) → Learning (15s) → Forwarding
- จาก listening ใช้เวลา 30 วินาที
```

**Convergence Time:**

```
Original STP (802.1D):
- Initial convergence: ~50 seconds
- Topology change: ~30-50 seconds

Factors:
- Hello time: 2 seconds
- Max age: 20 seconds
- Forward delay: 15 seconds (×2)

Total: 2 + 20 + 15 + 15 = 52 seconds (worst case)
```

**ตัวอย่าง Convergence:**

```
Scenario: Link failure

Before:
[Root] ─── [Switch A] ─── [Switch B]
             │              │
             └──(blocked)───┘

Link ระหว่าง Root และ Switch A ขาด:

Step 1: Switch A หยุดรับ BPDUs จาก root
Wait: 20 seconds (max age)

Step 2: Switch A unblock alternate port
Transition: Blocking → Listening (15s)

Step 3: Listening → Learning (15s)

Step 4: Learning → Forwarding

New Topology:
[Root] ────────────── [Switch B]
                        │
                    [Switch A]

Total Time: 20 + 15 + 15 = 50 seconds
```

### 2.9 STP Decision Sequence

**เมื่อ switch ต้องตัดสินใจ:**

```
1. Lowest Root Bridge ID
   - เปรียบเทียบ root BID ใน BPDUs
   - เลือก path ที่มา root BID ต่ำสุด

2. Lowest Path Cost to Root Bridge
   - ถ้า root BID เท่ากัน
   - เลือก path ที่มี cost ต่ำสุด

3. Lowest Sender Bridge ID
   - ถ้า path cost เท่ากัน
   - เลือก path จาก switch ที่มี BID ต่ำสุด

4. Lowest Sender Port ID
   - ถ้า sender BID เท่ากัน
   - เลือก path จาก port ที่มี port ID ต่ำสุด
```

**Port ID:**

```
Port ID = Port Priority (4 bits) + Port Number (12 bits)

ตัวอย่าง:
Fa0/1: 128.1 (priority 128, port 1)
Fa0/2: 128.2 (priority 128, port 2)

Fa0/1 < Fa0/2 (lower port number)

ตั้งค่า priority:
Switch(config-if)# spanning-tree port-priority 64
```

---

## 3. Evolution of STP

### วิวัฒนาการของ STP

### 3.1 STP Variants

**1. STP (802.1D):**

```
- Original Spanning Tree Protocol
- IEEE standard (1990)
- Single instance สำหรับ network ทั้งหมด
- Convergence: ~50 seconds
- Support: All vendors

ข้อเสีย:
- Slow convergence
- ไม่รองรับ per-VLAN spanning tree
- ไม่มี load balancing
```

**2. PVST+ (Per-VLAN Spanning Tree Plus):**

```
- Cisco proprietary
- แยก STP instance ต่อ VLAN
- แต่ละ VLAN มี root bridge แยกกัน
- Convergence: ~50 seconds
- Support: Cisco switches only

ข้อดี:
+ แยก topology ต่อ VLAN
+ Load balancing (different root per VLAN)

ข้อเสีย:
- Slow convergence เหมือน 802.1D
- ใช้ CPU/Memory มากขึ้น (multiple instances)
```

**3. RSTP (802.1w):**

```
- Rapid Spanning Tree Protocol
- IEEE standard (2001)
- Improvement ของ 802.1D
- Fast convergence: 1-2 seconds
- Backward compatible with 802.1D

ข้อดี:
+ Fast convergence
+ IEEE standard
+ Better port roles และ states

ข้อเสีย:
- Single instance (ไม่มี per-VLAN)
```

**4. Rapid PVST+:**

```
- Cisco proprietary
- RSTP + PVST+ combined
- แยก STP instance ต่อ VLAN
- Fast convergence: 1-2 seconds
- Support: Cisco switches only
- Default บน modern Cisco switches

ข้อดี:
+ Fast convergence
+ Per-VLAN spanning tree
+ Load balancing
+ Best of both worlds

ข้อเสีย:
- Cisco proprietary
- ใช้ resources มากขึ้น
```

**5. MSTP (802.1s):**

```
- Multiple Spanning Tree Protocol
- IEEE standard (2003)
- Map หลาย VLANs ไป spanning tree instances
- เหมาะกับ large networks
- Convergence: Fast (เหมือน RSTP)

ข้อดี:
+ IEEE standard
+ Efficient (fewer instances than PVST+)
+ Scalable

ข้อเสีย:
- Complex configuration
- Difficult to troubleshoot
```

### 3.2 เปรียบเทียบ STP Variants

**ตารางเปรียบเทียบ:**

|Feature|STP (802.1D)|PVST+|RSTP (802.1w)|Rapid PVST+|MSTP (802.1s)|
|---|---|---|---|---|---|
|**Standard**|IEEE|Cisco|IEEE|Cisco|IEEE|
|**Convergence**|50s|50s|1-2s|1-2s|1-2s|
|**Per-VLAN**|No|Yes|No|Yes|Groups|
|**Instances**|1|1 per VLAN|1|1 per VLAN|1 per group|
|**Load Balancing**|No|Yes|No|Yes|Yes|
|**CPU Usage**|Low|High|Low|High|Medium|
|**Default**|Old|Old|-|Modern Cisco|Advanced|

### 3.3 RSTP Concepts

**RSTP Port States (3 states only):**

```
802.1D (STP)          802.1w (RSTP)
────────────          ─────────────
Disabled       →      Discarding
Blocking       →      Discarding
Listening      →      Discarding
Learning       →      Learning
Forwarding     →      Forwarding
```

**RSTP Port Roles:**

```
Same as STP:
- Root Port
- Designated Port

Additional/Changed:
- Alternate Port (backup path to root)
- Backup Port (redundant designated port)
- Disabled Port
```

**RSTP Port Types:**

**1. Edge Port:**

```
- เชื่อมต่อกับ end devices (PCs, servers)
- Transition to forwarding ทันที (ไม่ต้องรอ)
- เทียบเท่า PortFast
- ถ้าได้รับ BPDU จะกลับเป็น normal port

Configuration:
Switch(config-if)# spanning-tree portfast
```

**2. Point-to-Point Port:**

```
- เชื่อมต่อระหว่าง 2 switches (full duplex)
- Rapid transition ได้
- ส่วนใหญ่เป็น point-to-point
```

**3. Shared Port:**

```
- เชื่อมต่อกับ hub (half duplex)
- ไม่สามารถ rapid transition
- Rare ในเครือข่ายสมัยใหม่
```

**RSTP Fast Convergence:**

```
Mechanism:
1. Direct links (point-to-point)
2. Proposal/Agreement handshake
3. Synchronization
4. Edge ports (PortFast)

Result:
Convergence ใน 1-2 วินาที (แทน 50 วินาที)
```

### 3.4 PVST+ Configuration

**Configure PVST+:**

```cisco
! PVST+ เป็น default บน Cisco switches รุ่นเก่า
! ไม่ต้อง enable เพิ่ม

! กำหนด root bridge
Switch(config)# spanning-tree vlan 1 root primary

! หรือตั้ง priority manual
Switch(config)# spanning-tree vlan 1 priority 24576

! กำหนด secondary root
Switch(config)# spanning-tree vlan 1 root secondary

! ตั้งค่าแยกต่าง VLAN (load balancing)
SwitchA(config)# spanning-tree vlan 10 root primary
SwitchA(config)# spanning-tree vlan 20 root secondary

SwitchB(config)# spanning-tree vlan 10 root secondary
SwitchB(config)# spanning-tree vlan 20 root primary
```

**Verify PVST+:**

```cisco
Switch# show spanning-tree

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    24577
             Address     0cd9.96d2.4800
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    24577  (priority 24576 sys-id-ext 1)
             Address     0cd9.96d2.4800
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
```

### 3.5 Rapid PVST+ Configuration

**Configure Rapid PVST+:**

```cisco
! Enable Rapid PVST+ (default บน switches รุ่นใหม่)
Switch(config)# spanning-tree mode rapid-pvst

! กำหนด root bridge
Switch(config)# spanning-tree vlan 1 root primary

! Configure edge ports (PortFast)
Switch(config)# interface range fa0/1 - 24
Switch(config-if-range)# spanning-tree portfast

! หรือ enable globally สำหรับ access ports ทั้งหมด
Switch(config)# spanning-tree portfast default

! Configure BPDU Guard (ป้องกัน loops จาก end devices)
Switch(config)# interface range fa0/1 - 24
Switch(config-if-range)# spanning-tree bpduguard enable

! หรือ enable globally
Switch(config)# spanning-tree portfast bpduguard default
```

**Verify Rapid PVST+:**

```cisco
Switch# show spanning-tree summary

Switch is in rapid-pvst mode
Root bridge for: VLAN0010, VLAN0020
Extended system ID           is enabled
Portfast Default             is disabled
PortFast BPDU Guard Default  is disabled
Portfast BPDU Filter Default is disabled
Loopguard Default            is disabled
EtherChannel misconfig guard is enabled
UplinkFast                   is disabled
BackboneFast                 is disabled
```

### 3.6 PortFast and BPDU Guard

**PortFast:**

```
Purpose:
- พอร์ตที่เชื่อมกับ end devices transition to forwarding ทันที
- ไม่ต้องผ่าน listening และ learning states
- ลดเวลา DHCP timeout

⚠️ Warning:
ใช้กับ access ports เท่านั้น!
ถ้าใช้กับ trunk หรือ ports ที่เชื่อมต่อ switches → loops!
```

**Configuration:**

```cisco
! Per-interface
Switch(config)# interface fastethernet 0/1
Switch(config-if)# switchport mode access
Switch(config-if)# spanning-tree portfast

! Interface range
Switch(config)# interface range fa0/1 - 24
Switch(config-if-range)# spanning-tree portfast

! Global (applies to all access ports)
Switch(config)# spanning-tree portfast default
```

**BPDU Guard:**

```
Purpose:
- ป้องกัน loops ถ้ามี switch เชื่อมเข้ามาที่พอร์ต PortFast
- ถ้าได้รับ BPDU → shutdown port ทันที (err-disabled)
- Security feature

Scenario:
User เสียบ switch เข้า access port
→ BPDU Guard detect BPDU
→ Port shutdown (err-disabled)
→ Prevent loop
```

**Configuration:**

```cisco
! Per-interface
Switch(config)# interface fastethernet 0/1
Switch(config-if)# spanning-tree portfast
Switch(config-if)# spanning-tree bpduguard enable

! Global (applies to all PortFast ports)
Switch(config)# spanning-tree portfast bpduguard default

! Recovery from err-disabled
Switch(config)# errdisable recovery cause bpduguard
Switch(config)# errdisable recovery interval 300
```

**Verify:**

```cisco
Switch# show spanning-tree interface fa0/1 detail

Port 1 (FastEthernet0/1) of VLAN0001 is forwarding
   Port path cost 19, Port priority 128, Port Identifier 128.1
   Designated root has priority 32769, address 0cd9.96d2.4800
   Number of transitions to forwarding state: 1
   The port is in the portfast mode
   BPDU: sent 3, received 0

Switch# show spanning-tree summary

Root bridge for: none
Extended system ID           is enabled
Portfast Default             is enabled
PortFast BPDU Guard Default  is enabled
```

**Recovery from err-disabled:**

```cisco
! ตรวจสอบ err-disabled ports
Switch# show interfaces status err-disabled

Port      Name               Status       Reason
Fa0/5                        err-disabled bpduguard

! Manual recovery
Switch# configure terminal
Switch(config)# interface fa0/5
Switch(config-if)# shutdown
Switch(config-if)# no shutdown

! หรือรอ auto recovery (ถ้า configure ไว้)
```

---

## สรุป

### สิ่งที่ได้เรียนรู้ใน Module 5:

✅ **Layer 2 Loops Problems:**

- Broadcast storms
- MAC address table instability
- Duplicate frames
- Network failures

✅ **STP Purpose:**

- Prevent Layer 2 loops
- Maintain redundancy
- Automatic failover
- Create loop-free topology

✅ **STP Operations:**

- Root bridge election
- Port role selection (Root, Designated, Alternate)
- Port states (Blocking, Listening, Learning, Forwarding)
- BPDU exchanges
- Convergence process

✅ **STP Components:**

- Bridge ID (Priority + Extended System ID + MAC)
- Path Cost
- BPDU messages
- Timers (Hello, Max Age, Forward Delay)

✅ **STP Variants:**

- STP (802.1D) - Original, slow
- PVST+ - Cisco, per-VLAN, slow
- RSTP (802.1w) - Fast convergence
- Rapid PVST+ - Cisco, per-VLAN, fast (default modern)
- MSTP (802.1s) - Grouped instances

✅ **Enhancements:**

- PortFast - Fast transition for access ports
- BPDU Guard - Security protection

---

## คำสั่งสำคัญสรุป

### Basic STP:

```cisco
! ดู spanning tree
show spanning-tree
show spanning-tree vlan <vlan-id>
show spanning-tree summary
show spanning-tree interface <interface>
show spanning-tree interface <interface> detail

! กำหนด root bridge
spanning-tree vlan <vlan-id> root primary
spanning-tree vlan <vlan-id> root secondary
spanning-tree vlan <vlan-id> priority <priority>

! ตั้งค่า cost และ priority
spanning-tree cost <cost>
spanning-tree port-priority <priority>
```

### Mode Configuration:

```cisco
! เปลี่ยน STP mode
spanning-tree mode pvst
spanning-tree mode rapid-pvst
spanning-tree mode mst

! ดู mode
show spanning-tree summary
```

### PortFast และ BPDU Guard:

```cisco
! PortFast
spanning-tree portfast                    (per-interface)
spanning-tree portfast default            (global)

! BPDU Guard
spanning-tree bpduguard enable            (per-interface)
spanning-tree portfast bpduguard default  (global)

! Recovery
errdisable recovery cause bpduguard
errdisable recovery interval <seconds>
show interfaces status err-disabled
```

---

## Best Practices

### STP Design:

```
✅ Plan root bridge location (core/distribution layer)
✅ Use Rapid PVST+ (fast convergence)
✅ Configure root primary และ secondary
✅ Different roots per VLAN (load balancing)
✅ Lower priority on better/faster switches
✅ Document STP topology
```

### Port Configuration:

```
✅ PortFast บน access ports เท่านั้น
✅ BPDU Guard กับ PortFast เสมอ
✅ ไม่ใช้ PortFast บน trunk links
✅ Verify configuration หลัง changes
✅ Monitor for err-disabled ports
```

### Operational:

```
✅ Avoid manual intervention
✅ Let STP converge naturally
✅ Monitor convergence times
✅ Check for unexpected topology changes
✅ Review logs regularly
```

---

## Common Issues

**Problem 1: Slow Convergence**

```
Cause: Using STP/PVST+ instead of Rapid PVST+
Solution: 
Switch(config)# spanning-tree mode rapid-pvst
```

**Problem 2: Suboptimal Root Bridge**

```
Cause: ไม่ได้กำหนด root bridge, เลือกตาม MAC
Solution:
Switch(config)# spanning-tree vlan <vlan> root primary
```

**Problem 3: Loops from End Devices**

```
Cause: Users เสียบ switches ที่ access ports
Solution:
Switch(config-if)# spanning-tree portfast
Switch(config-if)# spanning-tree bpduguard enable
```

**Problem 4: Port Stuck in Blocking**

```
Cause: Inferior BPDUs, misconfiguration
Troubleshooting:
show spanning-tree interface <interface> detail
show spanning-tree inconsistentports
```

---

## Lab Activities

### Lab 5.1: Observe STP Convergence

**วัตถุประสงค์:**

- สังเกต root bridge election
- ดู port roles และ states
- สังเกต convergence process
- Test failover

### Lab 5.2: Configure Rapid PVST+

**วัตถุประสงค์:**

- Enable Rapid PVST+
- Configure root bridges
- Verify operation
- Compare convergence times

### Lab 5.3: Configure PortFast and BPDU Guard

**วัตถุประสงค์:**

- Configure PortFast on access ports
- Enable BPDU Guard
- Test BPDU Guard operation
- Recover from err-disabled

---

## Packet Tracer Activities

### PT 5.1: STP Topology

**Tasks:**

1. สร้าง redundant topology
2. สังเกต default STP operation
3. ระบุ root bridge
4. ระบุ port roles
5. Verify loop prevention

### PT 5.2: Configure Root Bridge

**Tasks:**

1. กำหนด primary root
2. กำหนด secondary root
3. Verify root bridge election
4. Test different priorities

### PT 5.3: PortFast and BPDU Guard

**Tasks:**

1. Configure PortFast on access ports
2. Enable BPDU Guard
3. Test with rogue switch
4. Observe err-disabled state

---

## คำถามทบทวน

1. Layer 2 loops ทำให้เกิดปัญหาอะไรบ้าง?
2. STP แก้ปัญหา loops อย่างไร?
3. Bridge ID ประกอบด้วยอะไรบ้าง?
4. Root bridge ถูกเลือกอย่างไร?
5. Port roles ใน STP มีอะไรบ้าง?
6. Port states ใน STP มีอะไรบ้าง?
7. BPDU คืออะไร?
8. Convergence time ของ STP คือเท่าไร?
9. Rapid PVST+ ต่างจาก PVST+ อย่างไร?
10. PortFast ใช้ทำอะไร? ต้องระวังอะไร?

---

## เฉลยคำถาม

1. **Layer 2 Loops:** Broadcast storms, MAC table instability, duplicate frames, network down
2. **STP Solution:** Block redundant paths, create loop-free topology, unblock when needed
3. **Bridge ID:** Bridge Priority (4 bits) + Extended System ID (12 bits) + MAC Address (48 bits)
4. **Root Election:** Lowest Bridge ID wins (compare priority first, then MAC)
5. **Port Roles:** Root Port, Designated Port, Alternate Port, Backup Port
6. **Port States:** Blocking, Listening, Learning, Forwarding, Disabled
7. **BPDU:** Messages แลกเปลี่ยนระหว่าง switches เพื่อสร้าง loop-free topology
8. **Convergence:** ~50 seconds (802.1D), ~1-2 seconds (RSTP/Rapid PVST+)
9. **Rapid PVST+:** Fast convergence (1-2s แทน 50s), backward compatible
10. **PortFast:** Transition to forwarding ทันที สำหรับ access ports, ห้ามใช้กับ trunks, ใช้กับ BPDU Guard

---

**หมายเหตุ:** STP เป็นโปรโตคอลสำคัญในการป้องกัน Layer 2 loops ความเข้าใจใน STP จำเป็นสำหรับการออกแบบและ troubleshoot switched networks

---

**เอกสารจัดทำโดย:** Claude (Anthropic AI)  
**Module:** CCNA 2 Module 5 - STP Concepts  
**Version:** v7.02 SRWE  
**วันที่อัพเดท:** พฤศจิกายน 2025