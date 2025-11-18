# CCNA Course 3 - Module 1: Single-Area OSPFv2 Concepts

## OSPF (Open Shortest Path First)

---

## 1.1 OSPF Features and Characteristics (คุณสมบัติและลักษณะของ OSPF)

### OSPF Overview

**คำจำกัดความ:**

- **Link-State Routing Protocol**
- เป็น **Interior Gateway Protocol (IGP)**
- ใช้ **Dijkstra's Algorithm** (SPF Algorithm)
- พัฒนาโดย **IETF** (Internet Engineering Task Force)
- เป็น **Open Standard** (ไม่ใช่ proprietary)

**PDU:** OSPF Packet

**Administrative Distance:** 110

---

### OSPF Versions

#### OSPFv2 (RFC 2328)

**คำจำกัดความ:**

- รองรับ **IPv4**
- เวอร์ชันที่ใช้กันแพร่หลาย
- ใช้ multicast: **224.0.0.5** และ **224.0.0.6**

#### OSPFv3 (RFC 5340)

**คำจำกัดความ:**

- รองรับ **IPv6**
- รองรับ **IPv4** ด้วย (OSPFv3 for IPv4)
- ใช้ link-local addresses

**ความแตกต่างหลัก:**

```
Feature              OSPFv2              OSPFv3
------------------------------------------------------------------------
IP Version           IPv4                IPv6 (และ IPv4)
Multicast Address    224.0.0.5/6         FF02::5/6
Network Command      Required            Not required
Authentication       Plain text/MD5      IPsec
Router ID            Auto/Manual         Manual required
Address per Interface 1 primary         Multiple
------------------------------------------------------------------------
```

---

### OSPF vs Distance Vector Protocols

**OSPF (Link-State) vs RIP (Distance Vector):**

```
Feature              OSPF                RIP
------------------------------------------------------------------------
Type                 Link-State          Distance Vector
Algorithm            Dijkstra (SPF)      Bellman-Ford
Routing Updates      Incremental         Periodic (30s)
Convergence          Fast (seconds)      Slow (minutes)
Scalability          Large networks      Small networks
Metric               Cost (Bandwidth)    Hop count
Max Metric           Unlimited           15 hops
VLSM Support         Yes                 RIPv2: Yes
Summarization        Manual              Auto (classful boundary)
Loop Prevention      SPF calculation     Split horizon, etc.
Bandwidth Usage      Low (after initial) High (periodic updates)
CPU Usage            High (SPF calc)     Low
Memory Usage         High (LSDB)         Low
Administrative Dist. 110                 120
------------------------------------------------------------------------
```

---

### OSPF Components

**OSPF ใช้ 3 ส่วนหลัก:**

#### 1. Data Structures (โครงสร้างข้อมูล)

**Neighbor Table:**

```
- เก็บข้อมูล OSPF neighbors
- Command: show ip ospf neighbor
```

**Link-State Database (LSDB):**

```
- เก็บข้อมูล topology ทั้งหมด
- เหมือนกันทุก router ใน area เดียวกัน
- Command: show ip ospf database
```

**Routing Table:**

```
- เก็บ best paths
- สร้างจาก SPF calculation
- Command: show ip route
```

#### 2. Routing Protocol Messages (ข้อความโปรโตคอล)

**OSPF Packet Types:**

```
Type 1: Hello
Type 2: Database Description (DBD)
Type 3: Link-State Request (LSR)
Type 4: Link-State Update (LSU)
Type 5: Link-State Acknowledgment (LSAck)
```

#### 3. Algorithm (อัลกอริทึม)

**Dijkstra's SPF Algorithm:**

```
- คำนวณ shortest path tree
- Router เป็น root
- สร้าง routing table
```

---

### OSPF Characteristics

#### 1. Classless Protocol

**คำจำกัดความ:**

- รองรับ **VLSM** (Variable Length Subnet Mask)
- รองรับ **CIDR** (Classless Inter-Domain Routing)
- ส่ง subnet mask ใน routing updates

**ตัวอย่าง:**

```
Network: 192.168.1.0/24
VLSM:
  192.168.1.0/26   (64 hosts)
  192.168.1.64/27  (32 hosts)
  192.168.1.96/28  (16 hosts)
  192.168.1.112/30 (2 hosts - point-to-point)

OSPF รองรับทั้งหมด
```

#### 2. Efficient Updates

**Triggered Updates:**

```
- ส่ง update เมื่อมีการเปลี่ยนแปลง
- ไม่ส่ง periodic updates (ไม่เหมือน RIP)
- ประหยัด bandwidth
```

**ตัวอย่าง:**

```
RIP: ส่ง routing table ทุก 30 วินาที (เสมอ)
OSPF: ส่งเมื่อ topology เปลี่ยน + Hello packets (10s/40s)
```

#### 3. Fast Convergence

**คำจำกัดความ:**

- Convergence = เวลาที่ routers ทั้งหมด sync routing info
- OSPF converge เร็ว (วินาที)
- ใช้ SPF algorithm คำนวณ backup paths

**ตัวอย่าง:**

```
Link down:
  1. Router detect (1 second - Dead interval or immediate)
  2. ส่ง LSU ไปทุก router (flood)
  3. ทุก router รัน SPF recalculation
  4. Update routing table
  Total: 1-5 seconds

RIP: อาจใช้เวลา 30-180 วินาที
```

#### 4. Hierarchical Design

**คำจำกัดความ:**

- แบ่ง network เป็น **Areas**
- **Area 0** = Backbone area (จำเป็น)
- Areas อื่นเชื่อมกับ Area 0

**ข้อดี:**

```
✅ ลด SPF calculations
✅ ลด LSDB size
✅ ลด routing updates
✅ Scalability (รองรับ network ขนาดใหญ่)
```

**ตัวอย่าง:**

```
Single-Area OSPF:
  [Area 0]
  R1 ---- R2 ---- R3

Multi-Area OSPF:
  [Area 1] --- [Area 0] --- [Area 2]
               (Backbone)
```

#### 5. Load Balancing

**คำจำกัดความ:**

- รองรับ **Equal-Cost Load Balancing**
- ส่ง traffic ผ่านหลาย paths ที่มี cost เท่ากัน
- Default: 4 paths (max: 32 paths)

**ตัวอย่าง:**

```
R1 ไป Network X:
  Path 1: R1 → R2 → R3 (Cost 20)
  Path 2: R1 → R4 → R3 (Cost 20)

OSPF ใช้ทั้ง 2 paths (load balance 50-50)
```

---

### OSPF Metric: Cost

**คำจำกัดความ:**

- OSPF ใช้ **Cost** เป็น metric
- Cost = **Reference Bandwidth / Interface Bandwidth**
- Cost ต่ำกว่า = ดีกว่า

**สูตร:**

```
Cost = Reference Bandwidth / Interface Bandwidth

Default Reference Bandwidth = 100 Mbps (100,000,000 bps)
```

**ตัวอย่างการคำนวณ:**

```
Interface         Bandwidth       Cost Calculation           Cost
------------------------------------------------------------------------
Serial            64 Kbps         100,000 / 64 =             1562
T1                1.544 Mbps      100,000 / 1544 =           64
Ethernet          10 Mbps         100,000 / 10,000 =         10
Fast Ethernet     100 Mbps        100,000 / 100,000 =        1
Gigabit Ethernet  1 Gbps          100,000 / 1,000,000 =      1 (ปัดเป็น 1)
10 Gig Ethernet   10 Gbps         100,000 / 10,000,000 =     1 (ปัดเป็น 1)
------------------------------------------------------------------------
```

**ปัญหา:**

```
Fast Ethernet (100 Mbps) → Cost = 1
Gigabit Ethernet (1 Gbps) → Cost = 1
10 Gig Ethernet (10 Gbps) → Cost = 1

ทั้งหมดเหมือนกัน! (ไม่แม่นยำ)
```

**วิธีแก้:**

**เปลี่ยน Reference Bandwidth:**

```
Router(config-router)# auto-cost reference-bandwidth <value-in-Mbps>

ตัวอย่าง:
auto-cost reference-bandwidth 10000

Reference Bandwidth = 10,000 Mbps (10 Gbps)

Interface         Cost (New)
--------------------------------
Fast Ethernet     10,000 / 100 = 100
Gigabit Ethernet  10,000 / 1000 = 10
10 Gig Ethernet   10,000 / 10,000 = 1
```

**คำนวณ Total Cost:**

```
Cost ของ path = ผลรวม cost ของ outgoing interfaces

ตัวอย่าง:
R1 (Gi0/0) ---- R2 (Gi0/0) ---- R3 (Gi0/1) ---- Network X
Cost=1         Cost=1         Cost=1

Total Cost (R1 → Network X) = 1 + 1 + 1 = 3
```

**ตั้งค่า Cost ด้วยตนเอง:**

```
Router(config-if)# ip ospf cost <cost-value>

ตัวอย่าง:
R1(config)# interface gi0/0
R1(config-if)# ip ospf cost 50

→ Override automatic cost calculation
```

---

### OSPF Administrative Distance

**คำจำกัดความ:**

- **Administrative Distance (AD)** = ความน่าเชื่อถือของ routing protocol
- AD ต่ำกว่า = น่าเชื่อถือกว่า

**OSPF AD:**

```
OSPF = 110

Comparison:
Connected    0
Static       1
EIGRP        90
OSPF         110
RIP          120
External     170
Unknown      255
```

**ตัวอย่าง:**

```
R1 เรียนรู้เส้นทางไป 10.1.1.0/24:
  - จาก OSPF: AD 110
  - จาก RIP: AD 120

R1 เลือก OSPF (AD ต่ำกว่า)
```

---

## 1.2 OSPF Packets (แพ็กเก็ต OSPF)

### OSPF Packet Types

**5 ประเภท:**

#### Type 1: Hello Packet

**วัตถุประสงค์:**

- **Discover** OSPF neighbors
- **Establish** neighbor relationships
- **Maintain** neighbor relationships (keepalive)
- **Elect** DR/BDR (Designated Router/Backup DR)

**ส่งทุก:**

```
Point-to-point: 10 seconds
Broadcast/NBMA: 10 seconds
```

**ฟิลด์สำคัญ:**

```
- Router ID
- Hello Interval
- Dead Interval
- Area ID
- Network Mask
- Priority
- DR/BDR
- Neighbor List
- Authentication
```

#### Type 2: Database Description (DBD) Packet

**วัตถุประสงค์:**

- แลกเปลี่ยน **summary** ของ LSDB
- ไม่มี detail (เพียงแค่หัวข้อ)

**เมื่อไหร่ใช้:**

```
ระหว่าง EXSTART และ EXCHANGE states
```

**เนื้อหา:**

```
- LSA headers (Type, Link-State ID, Advertising Router, Sequence #)
- ไม่มี full LSA details
```

#### Type 3: Link-State Request (LSR) Packet

**วัตถุประสงค์:**

- **ขอ** LSA details จาก neighbor
- ใช้เมื่อ router ต้องการข้อมูล LSA เฉพาะ

**เมื่อไหร่ใช้:**

```
หลังจากรับ DBD แล้วพบว่าขาด LSA หรือ LSA เก่ากว่า
```

**เนื้อหา:**

```
- List ของ LSAs ที่ต้องการ (ระบุ Type, Link-State ID, Advertising Router)
```

#### Type 4: Link-State Update (LSU) Packet

**วัตถุประสงค์:**

- **ส่ง** LSA details
- ใช้ในการ flooding LSAs
- เป็น response ของ LSR

**เนื้อหา:**

```
- Full LSA details (1 หรือหลาย LSAs)
```

**เมื่อไหร่ใช้:**

```
- เมื่อมีการเปลี่ยนแปลง topology
- Response ของ LSR
- Periodic refresh (ทุก 30 นาที)
```

#### Type 5: Link-State Acknowledgment (LSAck) Packet

**วัตถุประสงค์:**

- **ยืนยัน** การรับ LSU
- ทำให้ OSPF reliable

**เนื้อหา:**

```
- LSA headers ที่ได้รับแล้ว
```

---

### OSPF Packet Format

**OSPF Packet Header (24 bytes):**

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|   Version #   |     Type      |         Packet Length         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                          Router ID                            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                           Area ID                             |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           Checksum            |             AuType            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                       Authentication                          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                       Authentication                          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

**ฟิลด์:**

**Version:**

```
- OSPFv2 = 2
- OSPFv3 = 3
```

**Type:**

```
1 = Hello
2 = Database Description
3 = Link-State Request
4 = Link-State Update
5 = Link-State Acknowledgment
```

**Packet Length:**

```
- ความยาวของ packet ทั้งหมด (header + data)
```

**Router ID:**

```
- ระบุ router ที่ส่ง packet
- 32-bit (รูปแบบ IPv4 address)
```

**Area ID:**

```
- Area ที่ packet นี้อยู่
- 0.0.0.0 = Area 0 (Backbone)
```

**Checksum:**

```
- ตรวจสอบความถูกต้องของ packet
```

**AuType:**

```
0 = Null (no authentication)
1 = Simple password
2 = MD5
```

**Authentication:**

```
- 64-bit authentication data
```

---

### Hello Packet Format

**Hello Packet (เพิ่มจาก OSPF header):**

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Network Mask                           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Hello Interval        |    Options    |   Priority    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                     Dead Interval                             |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                  Designated Router (DR)                       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|               Backup Designated Router (BDR)                  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                         Neighbor 1                            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                         Neighbor 2                            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                            ...                                |
```

**ฟิลด์:**

**Network Mask:**

```
- Subnet mask ของ interface ที่ส่ง Hello
```

**Hello Interval:**

```
- ความถี่ในการส่ง Hello (seconds)
- Default: 10s (broadcast), 30s (NBMA)
- ต้องตรงกันจึงจะเป็น neighbor
```

**Options:**

```
- Optional capabilities
```

**Priority:**

```
- ใช้ใน DR/BDR election
- 0-255 (0 = ไม่สามารถเป็น DR/BDR)
- Default: 1
```

**Dead Interval:**

```
- เวลาที่รอ Hello จาก neighbor ก่อนประกาศว่า neighbor down
- Default: 4 × Hello Interval (40s broadcast, 120s NBMA)
- ต้องตรงกันจึงจะเป็น neighbor
```

**Designated Router (DR):**

```
- Router ID ของ DR (0.0.0.0 ถ้ายังไม่มี)
```

**Backup Designated Router (BDR):**

```
- Router ID ของ BDR (0.0.0.0 ถ้ายังไม่มี)
```

**Neighbor List:**

```
- Router IDs ของ neighbors ที่เห็น
```

---

### OSPF Multicast Addresses

**OSPFv2 (IPv4):**

#### 224.0.0.5 - AllSPFRouters

**ใช้โดย:**

```
- Hello packets
- LSU packets (บน point-to-point)
- ส่งไปยัง routers ทั้งหมดใน segment
```

#### 224.0.0.6 - AllDRouters

**ใช้โดย:**

```
- LSU packets บน broadcast networks
- ส่งไปยัง DR และ BDR เท่านั้น
```

**ตัวอย่าง:**

```
Broadcast Network:
  DROther → 224.0.0.6 → DR/BDR (ส่ง LSU)
  DR → 224.0.0.5 → All Routers (flood LSU)
```

**OSPFv3 (IPv6):**

```
FF02::5 - AllSPFRouters
FF02::6 - AllDRouters
```

---

## 1.3 OSPF Operation (การทำงานของ OSPF)

### OSPF Operational States

**OSPF มี 7 states ในการสร้าง neighbor relationship:**

#### State 1: Down State

**คำจำกัดความ:**

- ยังไม่ได้รับ Hello จาก neighbor
- ไม่มีข้อมูล neighbor

**การทำงาน:**

```
Router เริ่มส่ง Hello packets
```

#### State 2: Init State

**คำจำกัดความ:**

- รับ Hello จาก neighbor แล้ว
- แต่ neighbor ยังไม่เห็น Router ID ของเราใน Hello packet

**ตัวอย่าง:**

```
R1 ส่ง Hello → R2
R2 รับ Hello จาก R1
R2 เข้า Init state (ยังไม่เห็น R2 ID ใน R1's Hello)
```

#### State 3: Two-Way State

**คำจำกัดความ:**

- Bidirectional communication established
- เห็น Router ID ของกันและกันใน Hello packets
- **DR/BDR election** เกิดขึ้นใน state นี้

**ตัวอย่าง:**

```
R1 ส่ง Hello (มี R2 ID ใน Neighbor List)
R2 รับ Hello จาก R1 และเห็น R2 ID
→ R2 เข้า Two-Way state
```

**การตัดสินใจ:**

```
Point-to-point: ไป EXSTART state
Broadcast (DROther): อยู่ที่ Two-Way (ไม่ exchange LSDB กับ DROther)
Broadcast (DR/BDR): ไป EXSTART state
```

#### State 4: ExStart State

**คำจำกัดความ:**

- เตรียม exchange LSDBs
- กำหนด **Master/Slave relationship**
- Router ID สูงกว่า = Master

**การทำงาน:**

```
R1 (Router ID: 1.1.1.1)
R2 (Router ID: 2.2.2.2)

R2 มี Router ID สูงกว่า → R2 = Master, R1 = Slave

Master:
  - กำหนด initial sequence number
  - เริ่ม DBD exchange
Slave:
  - รอ Master เริ่ม
  - Respond ตาม Master
```

#### State 5: Exchange State

**คำจำกัดความ:**

- แลกเปลี่ยน **Database Description (DBD)** packets
- DBD มี LSA headers (summary)

**การทำงาน:**

```
Master ส่ง DBD → Slave
Slave ส่ง DBD → Master

แต่ละฝั่งเปรียบเทียบ LSAs:
  - LSA ใหม่กว่า → เก็บ
  - LSA เก่ากว่าหรือไม่มี → ทำ list เพื่อขอ
```

#### State 6: Loading State

**คำจำกัดความ:**

- ขอ LSA details ที่ขาดหรือเก่ากว่า
- ใช้ **LSR** (Link-State Request)
- รับ **LSU** (Link-State Update)
- ส่ง **LSAck** (Link-State Acknowledgment)

**การทำงาน:**

```
1. Router ส่ง LSR (ขอ LSAs เฉพาะที่ต้องการ)
2. Neighbor ส่ง LSU (ส่ง full LSA details)
3. Router ส่ง LSAck (ยืนยันรับ)
4. Repeat จนได้ LSAs ครบ
```

#### State 7: Full State

**คำจำกัดความ:**

- LSDBs sync กันแล้ว (เหมือนกัน)
- Neighbors ใน **FULL** state
- พร้อมรัน SPF algorithm

**การทำงาน:**

```
Router รัน Dijkstra's SPF algorithm
คำนวณ best paths
สร้าง routing table
```

---

### OSPF Neighbor States Summary

```
State           Description                           Next State
------------------------------------------------------------------------
Down            No Hello received                     Init
Init            Hello received, 1-way                 Two-Way
Two-Way         Hello received, 2-way, DR/BDR elect   ExStart
ExStart         Master/Slave determined               Exchange
Exchange        DBD packets exchanged                 Loading
Loading         LSR/LSU/LSAck exchanged               Full
Full            LSDBs synchronized                    Stable
------------------------------------------------------------------------
```

**ตัวอย่าง Timeline:**

```
Time    R1 State        R2 State        Event
------------------------------------------------------------------------
0s      Down            Down            -
1s      Init            Down            R1 รับ Hello จาก R2
2s      Two-Way         Two-Way         R1 เห็น R1 ID ใน R2 Hello
3s      ExStart         ExStart         กำหนด Master/Slave
4s      Exchange        Exchange        แลกเปลี่ยน DBD
5s      Loading         Loading         ขอ/ส่ง LSAs
10s     Full            Full            LSDBs sync
------------------------------------------------------------------------
```

---

### DR/BDR Election (การเลือก Designated Router)

**คำจำกัดความ:**

- ใช้ใน **multi-access networks** (Broadcast, NBMA)
- **DR** (Designated Router) = Router หลัก
- **BDR** (Backup Designated Router) = Router สำรอง
- **DROther** = Routers อื่นๆ

**วัตถุประสงค์:**

```
✅ ลด adjacencies (ไม่ต้อง full mesh)
✅ ลด LSA flooding (ลดการส่งซ้ำ)
✅ ประหยัด bandwidth และ CPU
```

**ปัญหาที่แก้:**

```
ไม่มี DR (Full mesh):

5 routers → 5×(5-1)/2 = 10 adjacencies
LSA flood: 5×4 = 20 LSAs (แต่ละ router ส่งไปทุกคน)

มี DR:

5 routers → 4 adjacencies (DROthers ↔ DR/BDR)
LSA flood: 4+4 = 8 LSAs (DROthers → DR → All)
```

---

### DR/BDR Election Process

**เกณฑ์ (ตามลำดับ):**

#### 1. Priority

```
- OSPF Priority: 0-255
- Default: 1
- สูงกว่า = ดีกว่า
- Priority 0 = ไม่สามารถเป็น DR/BDR
```

**ตัวอย่าง:**

```
R1: Priority 200
R2: Priority 100
R3: Priority 1 (default)
R4: Priority 0 (ineligible)

R1 = DR (Priority สูงสุด)
R2 = BDR (Priority รองลงมา)
```

#### 2. Router ID

```
- ถ้า Priority เท่ากัน
- Router ID สูงกว่า = ดีกว่า
- Router ID = 32-bit (รูปแบบ IPv4)
```

**ตัวอย่าง:**

```
R1: Priority 1, Router ID 3.3.3.3
R2: Priority 1, Router ID 2.2.2.2
R3: Priority 1, Router ID 1.1.1.1

R1 = DR (Router ID สูงสุด)
R2 = BDR
```

---

### Router ID Selection

**OSPF ต้องมี Router ID เสมอ**

**การเลือก Router ID (ตามลำดับ):**

#### 1. Manual Configuration (แนะนำ)

```
Router(config)# router ospf <process-id>
Router(config-router)# router-id <router-id>

ตัวอย่าง:
R1(config-router)# router-id 1.1.1.1
```

#### 2. Highest Loopback IP (ถ้าไม่ configure manual)

```
Loopback interfaces:
  Lo0: 10.0.0.1
  Lo1: 192.168.1.1

Router ID = 192.168.1.1 (สูงสุด)
```

#### 3. Highest Active Physical IP (ถ้าไม่มี Loopback)

```
Gi0/0: 192.168.1.1 (up)
Gi0/1: 10.1.1.1 (up)
Gi0/2: 172.16.1.1 (down) ← ไม่นับ

Router ID = 192.168.1.1
```

**ตัวอย่าง:**

```
R1:
  Lo0: 1.1.1.1
  Gi0/0: 192.168.1.1
  
  → Router ID = 1.1.1.1 (Loopback มีความสำคัญสูงกว่า physical interface)
```

**เปลี่ยน Router ID:**

```
Router(config-router)# router-id <new-id>
Router# clear ip ospf process

หรือ reboot router
```

**หมายเหตุ:**

```
✅ ใช้ Loopback interface สำหรับ Router ID (แนะนำ)
   → Loopback ไม่ down (stable)
✅ Configure manual router-id (best practice)
✅ Router ID ไม่จำเป็นต้องเป็น IP ที่ใช้ใน network (แต่ควรอยู่ใน 32-bit format)
```

---

### DR/BDR Example

**Scenario:**

```
Broadcast Network (Ethernet):

R1: Priority 100, Router ID 1.1.1.1
R2: Priority 200, Router ID 2.2.2.2
R3: Priority 50, Router ID 3.3.3.3
R4: Priority 1, Router ID 4.4.4.4
```

**Election Result:**

```
DR = R2 (Priority สูงสุด: 200)
BDR = R1 (Priority รองลงมา: 100)
DROther = R3, R4
```

**Adjacencies:**

```
R1 ↔ R2 (DR)
R1 ↔ BDR (ถ้า R1 คือ DROther)
R3 ↔ R2 (DR)
R3 ↔ R1 (BDR)
R4 ↔ R2 (DR)
R4 ↔ R1 (BDR)

DROthers ไม่ full adjacency กัน (อยู่ที่ Two-Way)
```

**ถ้า DR down:**

```
R2 (DR) down
→ R1 (BDR) become DR
→ Election ใหม่สำหรับ BDR
→ R3 become BDR (Priority 50)

New:
DR = R1
BDR = R3
```

**คุณสมบัติพิเศษ:**

```
❌ DR/BDR election เป็น non-preemptive
   → ถ้า router ที่มี Priority/Router ID สูงกว่ามาทีหลัง
   → ไม่แย่ง DR/BDR
   → ต้อง reload หรือ clear ospf process

ตัวอย่าง:
  R1 = DR
  R2 = BDR
  R3 (Priority 255) มาทีหลัง
  → R3 เป็น DROther (ไม่แย่ง DR/BDR)
```

---

### OSPF Network Types

**OSPF รองรับ 5 network types:**

#### 1. Broadcast

**คำจำกัดความ:**

- Multi-access network
- รองรับ broadcast (Ethernet)
- มี DR/BDR election

**ค่า default:**

```
Hello Interval: 10 seconds
Dead Interval: 40 seconds
```

**ตัวอย่าง:**

```
Ethernet, Fast Ethernet, Gigabit Ethernet
```

#### 2. Non-Broadcast (NBMA)

**คำจำกัดความ:**

- Multi-access network
- ไม่รองรับ broadcast (Frame Relay, ATM)
- มี DR/BDR election
- ต้อง configure neighbors manually

**ค่า default:**

```
Hello Interval: 30 seconds
Dead Interval: 120 seconds
```

**ตัวอย่าง:**

```
Frame Relay, ATM, X.25
```

#### 3. Point-to-Point

**คำจำกัดความ:**

- เชื่อมต่อ router 2 ตัว
- ไม่มี DR/BDR election
- Automatically discover neighbors

**ค่า default:**

```
Hello Interval: 10 seconds
Dead Interval: 40 seconds
```

**ตัวอย่าง:**

```
Serial point-to-point, PPP, HDLC
```

#### 4. Point-to-Multipoint

**คำจำกัดความ:**

- Treat NBMA เป็น collection ของ point-to-point links
- ไม่มี DR/BDR election
- Automatically discover neighbors

**ค่า default:**

```
Hello Interval: 30 seconds
Dead Interval: 120 seconds
```

#### 5. Loopback

**คำจำกัดความ:**

- Loopback interface
- Advertised เป็น /32 host route

---

### Link-State Database (LSDB)

**คำจำกัดความ:**

- เก็บข้อมูล topology ทั้งหมด
- เก็บเป็น **Link-State Advertisements (LSAs)**
- ทุก router ใน area เดียวกันมี LSDB เหมือนกัน

**ตรวจสอบ LSDB:**

```
Router# show ip ospf database
```

---

### Link-State Advertisements (LSAs)

**LSA Types (Single-Area):**

#### Type 1: Router LSA

**คำจำกัดความ:**

- สร้างโดย**ทุก router**
- อธิบาย router's interfaces, costs, neighbors
- Flood ภายใน area เดียวกัน
- ไม่ cross ABR (Area Border Router)

**เนื้อหา:**

```
- Router ID
- Links (interfaces)
- Link costs
- Link types (point-to-point, transit, stub)
```

#### Type 2: Network LSA

**คำจำกัดความ:**

- สร้างโดย **DR** บน broadcast/NBMA networks
- อธิบาย routers ที่ต่อกับ segment นั้น
- Flood ภายใน area เดียวกัน

**เนื้อหา:**

```
- DR Router ID
- Attached routers (DROthers, BDR)
- Network mask
```

---

### SPF Algorithm (Dijkstra's Algorithm)

**คำจำกัดความ:**

- คำนวณ **shortest path tree**
- Router เป็น root
- คำนวณ best path ไปทุก destination

**ขั้นตอน:**

```
1. Router อ่าน LSDB
2. Router เป็น root ของ tree
3. คำนวณ cost ไปทุก destination
4. เลือก path ที่มี cost ต่ำสุด
5. เพิ่ม path นั้นใน routing table
```

**ตัวอย่าง:**

```
Topology:
R1 ---- (Cost 10) ---- R2 ---- (Cost 10) ---- R3
 |                                              |
 +------------ (Cost 50) ---------------------+

R1 ไป R3:
  Path 1: R1 → R2 → R3 (Cost 10+10=20)
  Path 2: R1 → R3 (Cost 50)

SPF เลือก Path 1 (Cost 20)
```

---

### OSPF LSA Flooding

**กระบวนการ:**

**1. Router detect topology change:**

```
Interface down, new neighbor, cost change
```

**2. Router สร้าง/update LSA:**

```
Increase sequence number
Update content
```

**3. Router flood LSA:**

```
ส่ง LSU (LSA inside) ไปทุก neighbors
```

**4. Neighbors รับ LSA:**

```
- ตรวจสอบ sequence number
- ถ้าใหม่กว่า: update LSDB, flood ต่อ
- ถ้าเก่ากว่า: ignore
- ส่ง LSAck กลับ
```

**5. ทุก router รัน SPF:**

```
Recalculate routing table
```

**ตัวอย่าง:**

```
R1 interface down:

1. R1 detect change
2. R1 สร้าง LSA ใหม่ (Seq #100 → #101)
3. R1 ส่ง LSU → R2, R3
4. R2 รับ:
   - Update LSDB
   - ส่ง LSAck → R1
   - Flood LSU → R4, R5 (ไม่ส่งกลับ R1)
5. R3 รับ:
   - Update LSDB
   - ส่ง LSAck → R1
   - Flood LSU → R6, R7
6. Process repeat จนทุก router รับ LSA
7. ทุก router รัน SPF
```

---

### OSPF Timers

#### Hello Interval

**คำจำกัดความ:**

- ความถี่ในการส่ง Hello packets
- ใช้ detect neighbors และ maintain relationship

**ค่า default:**

```
Broadcast/Point-to-point: 10 seconds
NBMA: 30 seconds
```

**Configuration:**

```
Router(config-if)# ip ospf hello-interval <seconds>

ตัวอย่าง:
R1(config-if)# ip ospf hello-interval 5
```

#### Dead Interval

**คำจำกัดความ:**

- เวลาที่รอ Hello ก่อนประกาศ neighbor down
- Default = 4 × Hello Interval

**ค่า default:**

```
Broadcast/Point-to-point: 40 seconds
NBMA: 120 seconds
```

**Configuration:**

```
Router(config-if)# ip ospf dead-interval <seconds>

ตัวอย่าง:
R1(config-if)# ip ospf dead-interval 20
```

**หมายเหตุ:**

```
✅ Hello และ Dead intervals ต้องตรงกันระหว่าง neighbors
   → ถ้าไม่ตรง = ไม่เป็น neighbors
```

---

## Summary (สรุป)

Module 1 นี้เราได้เรียนรู้:

1. ✅ **OSPF Overview** - Link-State protocol, Open standard
2. ✅ **OSPF Characteristics**:
    - Classless (VLSM/CIDR)
    - Fast convergence
    - Hierarchical (Areas)
    - Metric = Cost (Bandwidth-based)
    - AD = 110
3. ✅ **OSPF Packets**:
    - Hello (discover/maintain)
    - DBD (LSDB summary)
    - LSR (request LSAs)
    - LSU (send LSAs)
    - LSAck (acknowledge)
4. ✅ **OSPF States**: Down → Init → Two-Way → ExStart → Exchange → Loading → Full
5. ✅ **DR/BDR**:
    - ใช้ใน multi-access networks
    - Election: Priority → Router ID
    - ลด adjacencies และ flooding
6. ✅ **Router ID**: Manual > Loopback > Physical IP
7. ✅ **LSAs**:
    - Type 1: Router LSA
    - Type 2: Network LSA
8. ✅ **SPF Algorithm**: คำนวณ shortest path tree
9. ✅ **Timers**:
    - Hello: 10s (broadcast), 30s (NBMA)
    - Dead: 40s (broadcast), 120s (NBMA)

**สิ่งสำคัญที่ต้องจำ:**

- OSPF = Link-State protocol
- Metric = Cost (Reference BW / Interface BW)
- Default Reference BW = 100 Mbps
- Router ID: Manual config > Loopback > Physical IP
- DR/BDR: Priority (สูงกว่าดีกว่า) → Router ID (สูงกว่าดีกว่า)
- Neighbor requirements: Same area, subnet, hello/dead timers
- OSPF states: Down-Init-TwoWay-ExStart-Exchange-Loading-Full
- Multicast: 224.0.0.5 (AllSPFRouters), 224.0.0.6 (AllDRouters)
- LSA flooding → SPF calculation → Routing table update

**Next Module:** Module 2 - Single-Area OSPFv2 Configuration

---

**[ไฟล์ Module 1 สมบูรณ์แล้ว!]**