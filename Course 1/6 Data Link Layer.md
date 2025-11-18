# CCNA Course 1 - Module 6: Data Link Layer

## ชั้นลิงก์ข้อมูล

---

## 6.1 Purpose of the Data Link Layer (วัตถุประสงค์ของชั้นลิงก์ข้อมูล)

### Data Link Layer (Layer 2)

**คำจำกัดความ:**

- ชั้นที่ 2 ของ OSI Model
- เชื่อมต่อระหว่าง Network Layer (Layer 3) และ Physical Layer (Layer 1)
- รับผิดชอบการส่งข้อมูลระหว่างอุปกรณ์ในเครือข่ายเดียวกัน

**PDU (Protocol Data Unit):** **Frame**

---

### หน้าที่หลักของ Data Link Layer:

#### 1. Framing (การสร้างเฟรม)

**คำจำกัดความ:**

- ห่อหุ้มข้อมูลจาก Network Layer เป็น **Frame**
- เพิ่ม **Header** และ **Trailer**

**ส่วนประกอบของ Frame:**

```
[Header][Data (from Layer 3)][Trailer]

Header: Source MAC, Destination MAC, Type/Length
Data: IP packet
Trailer: FCS (Frame Check Sequence)
```

**วัตถุประสงค์:**

- กำหนด boundary ของข้อมูล (เริ่มต้น-สิ้นสุด)
- ระบุที่อยู่ (addressing)
- ตรวจสอบข้อผิดพลาด (error detection)

#### 2. Physical Addressing (การกำหนดที่อยู่ทางกายภาพ)

**คำจำกัดความ:**

- ใช้ **MAC address** (Media Access Control address)
- 48-bit physical address
- ฝังอยู่ใน NIC (Network Interface Card)

**Format:**

- 6 bytes = 12 hex digits
- เขียนเป็น: `XX:XX:XX:XX:XX:XX` หรือ `XXXX.XXXX.XXXX`

**ตัวอย่าง:**

```
00:1A:2B:3C:4D:5E
หรือ
001A.2B3C.4D5E  (Cisco format)
```

**ส่วนประกอบ:**

- **OUI (Organizationally Unique Identifier):** 24 bits แรก - รหัสผู้ผลิต
- **Vendor Assigned:** 24 bits หลัง - รหัสเฉพาะอุปกรณ์

#### 3. Media Access Control (การควบคุมการเข้าถึงสื่อ)

**คำจำกัดความ:**

- ควบคุมว่าอุปกรณ์ใดสามารถส่งข้อมูลได้เมื่อไหร่
- ป้องกัน collision (การชน)

**วิธีการ:**

- **CSMA/CD** (Carrier Sense Multiple Access with Collision Detection)
    - ใช้ใน Ethernet (Half-duplex)
- **CSMA/CA** (Carrier Sense Multiple Access with Collision Avoidance)
    - ใช้ใน Wi-Fi (802.11)

#### 4. Error Detection (การตรวจสอบข้อผิดพลาด)

**คำจำกัดความ:**

- ตรวจสอบว่าข้อมูลที่ได้รับถูกต้องหรือไม่
- ใช้ **FCS (Frame Check Sequence)**

**FCS:**

- 4 bytes ใน Trailer
- คำนวณด้วย **CRC (Cyclic Redundancy Check)**
- ถ้าคำนวณไม่ตรง → ทิ้ง frame

**หมายเหตุ:**

- Data Link Layer ทำ **Error Detection** เท่านั้น
- **ไม่ทำ Error Correction**
- Error Correction ทำที่ Transport Layer (TCP)

#### 5. Flow Control (การควบคุมการไหล)

**คำจำกัดความ:**

- ควบคุมอัตราการส่งข้อมูล
- ป้องกันผู้รับรับข้อมูลไม่ทัน
- ใช้ในบาง protocols (เช่น PPP)

---

### Data Link Sub-layers (ชั้นย่อย)

Data Link Layer แบ่งเป็น 2 sub-layers:

#### LLC (Logical Link Control) - IEEE 802.2

**หน้าที่:**

- **Interface กับ Network Layer** (Layer 3)
- รองรับหลาย Network Layer protocols พร้อมกัน
    - IPv4, IPv6, IPX, AppleTalk
- **Multiplexing** - รวมข้อมูลจากหลาย protocols
- Flow control และ Error control

**ลักษณะ:**

- ไม่ขึ้นกับ Physical media
- เหมือนกันทุก LAN technology
- กำหนดโดย IEEE 802.2

**Services:**

- **Type 1:** Unacknowledged connectionless (ใช้บ่อย)
- **Type 2:** Connection-oriented (มี ACK)
- **Type 3:** Acknowledged connectionless

#### MAC (Media Access Control)

**หน้าที่:**

- **ควบคุมการเข้าถึงสื่อ** (Media access)
- **Frame การสร้าง frame**
- **Physical addressing** (MAC address)
- **Error detection** (FCS)

**ลักษณะ:**

- **ขึ้นกับ Physical media**
- แตกต่างกันไปตาม technology:
    - Ethernet (802.3)
    - Wi-Fi (802.11)
    - Token Ring (802.5)
    - FDDI

**MAC Methods:**

- **Contention-based:** CSMA/CD (Ethernet), CSMA/CA (Wi-Fi)
- **Controlled:** Token passing (Token Ring, FDDI)

---

### IEEE Standards (มาตรฐาน IEEE)

**IEEE 802 LAN/MAN Standards Committee:**

```
IEEE 802.1  - Bridging and Management
IEEE 802.2  - LLC (Logical Link Control)
IEEE 802.3  - Ethernet (CSMA/CD)
IEEE 802.4  - Token Bus (Obsolete)
IEEE 802.5  - Token Ring (Obsolete)
IEEE 802.6  - MAN (Metropolitan Area Network)
IEEE 802.11 - Wireless LAN (Wi-Fi)
IEEE 802.15 - Wireless PAN (Bluetooth, Zigbee)
IEEE 802.16 - Broadband Wireless (WiMAX)
```

**สำคัญที่สุด:**

- **802.2** - LLC
- **802.3** - Ethernet ⭐
- **802.11** - Wi-Fi ⭐

---

## 6.2 Topologies (โครงสร้างเครือข่าย)

### Physical vs Logical Topology

#### Physical Topology (โครงสร้างทางกายภาพ)

**คำจำกัดความ:**

- แสดงตำแหน่ง**จริง**ของอุปกรณ์และสาย
- การเชื่อมต่อทางกายภาพ

**ข้อมูลที่แสดง:**

- ตำแหน่งอุปกรณ์
- ความยาวสาย
- ประเภทสาย
- ตำแหน่งในอาคาร

**ใช้สำหรับ:**

- การติดตั้ง (Installation)
- การบำรุงรักษา (Maintenance)
- Troubleshooting ปัญหาทางกายภาพ

#### Logical Topology (โครงสร้างทางตรรกะ)

**คำจำกัดความ:**

- แสดงเส้นทาง**การไหล**ของข้อมูล
- การเชื่อมต่อทางตรรกะ

**ข้อมูลที่แสดง:**

- IP addresses
- VLANs
- Routing paths
- Protocols

**ใช้สำหรับ:**

- Network design
- Troubleshooting connectivity
- Documentation

---

### WAN Topologies (โครงสร้าง WAN)

#### 1. Point-to-Point Topology

**ลักษณะ:**

- เชื่อมต่อ **2 จุดโดยตรง**
- Dedicated connection
- Simple ที่สุด

**การใช้งาน:**

- **Leased line** - T1, E1
- **Serial connection** - Router to Router
- **PPP** - Point-to-Point Protocol
- **HDLC** - High-Level Data Link Control

**ข้อดี:**

- ✅ ง่าย
- ✅ Dedicated bandwidth
- ✅ Low latency
- ✅ ความปลอดภัยสูง

**ข้อเสีย:**

- ❌ ราคาแพง (ต่อ link)
- ❌ ไม่ scalable
- ❌ Single point of failure

**ตัวอย่าง:**

```
[Router A] ========== [Router B]
           Serial link
```

#### 2. Hub and Spoke Topology (Star WAN)

**ลักษณะ:**

- อุปกรณ์ทั้งหมดเชื่อมต่อกับ **จุดกลาง** (Hub)
- Centralized architecture

**ส่วนประกอบ:**

- **Hub site** - สำนักงานใหญ่ (Central)
- **Spoke sites** - สาขา (Branch offices)

**การใช้งาน:**

- **VPN** - Site-to-Site VPN
- **MPLS** - Multiprotocol Label Switching
- **Enterprise WAN**

**ข้อดี:**

- ✅ ง่ายต่อการจัดการ (Centralized)
- ✅ ประหยัดต้นทุน (น้อยกว่า full mesh)
- ✅ Scalable

**ข้อเสีย:**

- ❌ Hub = Single point of failure
- ❌ Spoke-to-Spoke traffic ต้องผ่าน Hub
- ❌ Bandwidth ที่ Hub สูง

**ตัวอย่าง:**

```
        [Spoke 1]
             |
             |
[Spoke 2]--[Hub]--[Spoke 3]
             |
             |
        [Spoke 4]
```

#### 3. Mesh Topology

**ลักษณะ:**

- มีเส้นทางหลายเส้น (Multiple paths)
- **Redundancy** สูง

**ประเภท:**

##### Full Mesh (เต็มรูปแบบ)

- **ทุกอุปกรณ์เชื่อมต่อกับทุกอุปกรณ์**
- Maximum redundancy

**สูตรจำนวน links:**

```
n(n-1)/2

n = จำนวนอุปกรณ์

ตัวอย่าง:
4 อุปกรณ์ = 4(4-1)/2 = 6 links
5 อุปกรณ์ = 5(5-1)/2 = 10 links
10 อุปกรณ์ = 10(10-1)/2 = 45 links
```

**ข้อดี:**

- ✅ **Redundancy สูงสุด**
- ✅ **Fault tolerance** - อุปกรณ์เสียไม่กระทบ
- ✅ High availability
- ✅ Load balancing

**ข้อเสีย:**

- ❌ **ราคาแพงมาก** (จำนวน links เยอะ)
- ❌ **ซับซ้อนมาก**
- ❌ ยากต่อการจัดการ
- ❌ Configuration ยาก

##### Partial Mesh (บางส่วน)

- **บางอุปกรณ์เชื่อมต่อกับหลายอุปกรณ์**
- ไม่ใช่ทุกต่อทุก

**ข้อดี:**

- ✅ Redundancy ดี (แต่น้อยกว่า full mesh)
- ✅ ราคาถูกกว่า full mesh
- ✅ Flexible

**ข้อเสีย:**

- ❌ ซับซ้อน
- ❌ ต้องวางแผน topology ให้ดี

**การใช้งาน Mesh:**

- **Internet backbone** (Partial mesh)
- **ISP networks**
- **Enterprise core**
- **Critical systems**

**ตัวอย่าง Full Mesh (4 nodes):**

```
    [A]————————[B]
     |  \    /  |
     |   \  /   |
     |    \/    |
     |    /\    |
     |   /  \   |
     |  /    \  |
    [D]————————[C]

Total links = 6
```

---

### LAN Topologies (โครงสร้าง LAN)

#### 1. Star Topology (ใช้มากที่สุด)

**ลักษณะ:**

- อุปกรณ์ทั้งหมดเชื่อมต่อกับ **อุปกรณ์กลาง**
- อุปกรณ์กลาง: Switch, Hub (ล้าสมัย)

**Physical Layout:**

```
      [PC1]
        |
[PC2]—[Switch]—[PC3]
        |
      [PC4]
```

**ข้อดี:**

- ✅ **ติดตั้งง่าย**
- ✅ **จัดการง่าย**
- ✅ **แก้ปัญหาง่าย** - ถ้าสายเส้นหนึ่งเสีย อุปกรณ์เครื่องนั้นเท่านั้นที่เสีย
- ✅ **Scalable** - เพิ่มอุปกรณ์ได้ง่าย
- ✅ Centralized management

**ข้อเสีย:**

- ❌ **Switch เสีย = เครือข่ายทั้งหมดเสีย** (Single point of failure)
- ❌ ใช้สายเคเบิลมาก
- ❌ ขึ้นกับอุปกรณ์กลาง

**Logical Operation:**

- Switch: แต่ละ device สื่อสารโดยตรง (Logically)
- Hub: Shared medium (Broadcast to all)

**การใช้งาน:**

- **LAN มาตรฐาน**
- ใช้ทั่วไปที่สุดในปัจจุบัน
- Office networks
- Home networks

#### 2. Extended Star Topology

**ลักษณะ:**

- **Star หลายตัวเชื่อมต่อกัน**
- Switch เชื่อมต่อกับ Switch อื่น
- Hierarchical design

**Physical Layout:**

```
                [Core Switch]
                 /         \
          [Switch 1]     [Switch 2]
           /  |  \         /  |  \
         PC  PC  PC      PC  PC  PC
```

**ข้อดี:**

- ✅ **Scalable** มาก
- ✅ รองรับอุปกรณ์จำนวนมาก
- ✅ แบ่งกลุ่มได้ (Departments, Floors)
- ✅ ง่ายต่อการ troubleshoot

**ข้อเสีย:**

- ❌ Core switch เสีย = กระทบมาก
- ❌ ต้องวางแผน hierarchy

**การใช้งาน:**

- **Enterprise networks**
- Multi-floor buildings
- Campus networks

#### 3. Bus Topology (ล้าสมัย)

**ลักษณะ:**

- อุปกรณ์ทั้งหมดต่อกับ**สายเส้นเดียว** (Backbone)
- ใช้ Coaxial cable
- Terminators ที่ปลายทั้งสองข้าง

**Physical Layout:**

```
[Term]—[PC1]—[PC2]—[PC3]—[PC4]—[Term]
         Backbone cable
```

**ข้อดี:**

- ✅ ใช้สายน้อย
- ✅ ติดตั้งง่าย (สมัยก่อน)
- ✅ ราคาถูก

**ข้อเสีย:**

- ❌ **สายหลักเสีย = เครือข่ายทั้งหมดเสีย**
- ❌ **Collision มาก** (Shared medium)
- ❌ ยากต่อการ troubleshoot
- ❌ ประสิทธิภาพลดลงเมื่อมีอุปกรณ์มาก
- ❌ จำกัดจำนวนอุปกรณ์

**Logical Operation:**

- **Broadcast domain** - ทุกอุปกรณ์รับข้อมูลทั้งหมด
- **Half-duplex**
- **CSMA/CD** ป้องกัน collision

**สถานะ:**

- **ล้าสมัยมาก** - ไม่ใช้ใน LAN แล้ว
- ถูกแทนที่ด้วย Star topology

**ตัวอย่างเทคโนโลยี:**

- 10BASE2 (Thinnet)
- 10BASE5 (Thicknet)

#### 4. Ring Topology (ล้าสมัย)

**ลักษณะ:**

- อุปกรณ์เชื่อมต่อเป็น**วงกลม**
- ข้อมูลไหลทางเดียว (หรือสองทาง)
- ใช้ **Token passing**

**Physical Layout:**

```
    [PC1]———[PC2]
      |         |
    [PC4]———[PC3]
```

**Token Passing:**

- มี **Token** วนรอบ ring
- อุปกรณ์ที่มี token เท่านั้นที่ส่งได้
- ป้องกัน collision

**ข้อดี:**

- ✅ **ไม่มี collision** (Token passing)
- ✅ **Performance คงที่**
- ✅ Fair access (ทุกคนได้ใช้เท่าๆ กัน)

**ข้อเสีย:**

- ❌ **อุปกรณ์หนึ่งเสีย = เครือข่ายทั้งหมดเสีย** (Single ring)
- ❌ เพิ่มอุปกรณ์ยาก (ต้อง break ring)
- ❌ ช้าเมื่อมีอุปกรณ์มาก

**Dual Ring:**

- มี 2 rings (หมุนตรงข้ามกัน)
- ถ้า ring หนึ่งเสีย ใช้อีก ring
- Fault tolerance ดีกว่า

**สถานะ:**

- **ล้าสมัย** - ไม่ใช้ใน LAN แล้ว
- ถูกแทนที่ด้วย Ethernet

**ตัวอย่างเทคโนโลยี:**

- **Token Ring** (IEEE 802.5)
- **FDDI** (Fiber Distributed Data Interface)

---

### Half-Duplex vs Full-Duplex

#### Half-Duplex (ส่งครั้งละทาง)

**คำจำกัดความ:**

- ส่ง**หรือ**รับทีละอย่าง
- ไม่สามารถส่งและรับพร้อมกันได้

**ลักษณะ:**

- Shared medium
- **CSMA/CD** ป้องกัน collision
- Collision domain

**การใช้งาน:**

- **Hub** (ล้าสมัย)
- **Wi-Fi** (802.11)
- **Walkie-talkie**

**Bandwidth:**

```
Example: 100 Mbps Half-duplex
Actual throughput ≈ 50-60% (เพราะ collision และ waiting)
```

**ข้อเสีย:**

- ❌ ช้ากว่า Full-duplex
- ❌ Collision
- ❌ CSMA/CD overhead

#### Full-Duplex (ส่งพร้อมกัน)

**คำจำกัดความ:**

- ส่ง**และ**รับพร้อมกัน
- Point-to-point connection

**ลักษณะ:**

- Dedicated connection
- **ไม่มี collision**
- ไม่ต้องใช้ CSMA/CD

**การใช้งาน:**

- **Switch** (Switched Ethernet)
- Modern LAN
- **Default** ในปัจจุบัน

**Bandwidth:**

```
Example: 100 Mbps Full-duplex
= 100 Mbps send + 100 Mbps receive simultaneously
Effective = 200 Mbps (aggregate)
```

**ข้อดี:**

- ✅ **เร็วกว่า** - ประมาณ 2 เท่า
- ✅ **ไม่มี collision**
- ✅ Efficient

**Comparison:**

```
Feature           Half-Duplex      Full-Duplex
------------------------------------------------
Direction         One way at time  Both ways simultaneously
Collision         Yes              No
CSMA/CD           Required         Not required
Device            Hub              Switch
Speed             Slower           Faster (2x effective)
Status            Obsolete         Current standard
```

**Auto-negotiation:**

- Switch และ NIC เจรจากัน (negotiate)
- เลือก speed (10/100/1000 Mbps) และ duplex mode
- Default: Auto

---

## 6.3 Data Link Frame (เฟรมชั้นลิงก์ข้อมูล)

### Generic Frame Structure (โครงสร้างเฟรมทั่วไป)

**ส่วนประกอบหลัก:**

```
+----------+-------------+------+------------+---------+
| Frame    | Addressing  | Data | Error      | Frame   |
| Start    |             |      | Detection  | End     |
+----------+-------------+------+------------+---------+
| Header   |   Header    |Payload|  Trailer  |Trailer  |
```

**รายละเอียด:**

#### 1. Frame Start (จุดเริ่มต้น)

**หน้าที่:**

- บอกว่า frame เริ่มแล้ว
- Synchronization

**เรียกอีกชื่อว่า:**

- **Flag**
- **Preamble**
- **Start of Frame Delimiter (SFD)**

#### 2. Addressing (การกำหนดที่อยู่)

**ประกอบด้วย:**

- **Source Address** - MAC address ของผู้ส่ง
- **Destination Address** - MAC address ของผู้รับ

**ขนาด:**

- 6 bytes แต่ละ address
- รวม 12 bytes

#### 3. Type/Length

**หน้าที่:**

- บอกประเภทของ protocol ใน Data field
- หรือความยาวของ data

**ตัวอย่าง:**

- `0x0800` = IPv4
- `0x0806` = ARP
- `0x86DD` = IPv6
- `0x8100` = VLAN tag (802.1Q)

#### 4. Data (Payload)

**คำจำกัดความ:**

- ข้อมูลจริงจาก Layer 3 (Network Layer)
- IP packet, ARP request/reply, etc.

**ขนาด:**

- **Minimum:** 46 bytes (ถ้าน้อยกว่าต้อง pad)
- **Maximum:** 1500 bytes (MTU)

#### 5. Error Detection (FCS)

**Frame Check Sequence (FCS):**

- 4 bytes ใน Trailer
- คำนวณด้วย **CRC-32** (Cyclic Redundancy Check)
- ตรวจสอบความถูกต้องของ frame

**การทำงาน:**

1. **ผู้ส่ง:** คำนวณ FCS จากข้อมูลทั้งหมด
2. **ผู้รับ:** คำนวณ FCS ใหม่
3. **เปรียบเทียบ:** ถ้าเหมือนกัน = OK, ถ้าไม่เหมือน = Error (ทิ้ง frame)

#### 6. Frame End (จุดสิ้นสุด)

**หน้าที่:**

- บอกว่า frame จบแล้ว

---

### Frame Size (ขนาดเฟรม)

**Minimum Frame Size:**

```
Ethernet: 64 bytes (รวม header + FCS)

Header: 14 bytes (Dest MAC + Src MAC + Type)
Data:   46 bytes (minimum)
FCS:    4 bytes

Total:  64 bytes
```

**ทำไมต้องมี minimum?**

- เพื่อ **Collision Detection** (CSMA/CD)
- Frame ต้องยาวพอที่จะตรวจจับ collision
- ถ้า data น้อยกว่า 46 bytes → **Padding** (เติม 0)

**Maximum Frame Size:**

```
Standard Ethernet: 1518 bytes

Header: 14 bytes
Data:   1500 bytes (MTU - Maximum Transmission Unit)
FCS:    4 bytes

Total:  1518 bytes
```

**Jumbo Frame:**

- ขนาดใหญ่กว่า 1518 bytes
- สูงสุด: 9000-9216 bytes (ขึ้นกับอุปกรณ์)
- ใช้ใน: **Data Center**, **Storage networks**
- **ไม่ใช่มาตรฐาน** - ต้องรองรับทุกอุปกรณ์

**ข้อดีของ Jumbo Frame:**

- ✅ ลด overhead (fewer frames)
- ✅ เพิ่ม throughput
- ✅ ลด CPU usage

**ข้อเสีย:**

- ❌ ไม่ใช่มาตรฐาน
- ❌ อุปกรณ์ต้องรองรับทั้งหมด
- ❌ Error มีผลมากกว่า (retransmit ข้อมูลเยอะ)

---

### Layer 2 Protocols (โปรโตคอล Layer 2)

**Ethernet** (IEEE 802.3) - ใช้มากที่สุด **Wi-Fi** (IEEE 802.11) **PPP** (Point-to-Point Protocol) **HDLC** (High-Level Data Link Control) **Frame Relay** **ATM** (Asynchronous Transfer Mode)

แต่ละ protocol มี frame format ต่างกัน

---

## 6.4 MAC Address (Media Access Control Address)

### MAC Address Structure (โครงสร้าง MAC Address)

**คำจำกัดความ:**

- **Physical address** - ที่อยู่ทางกายภาพ
- **48-bit address** (6 bytes)
- เขียนเป็น **Hexadecimal**
- **Burned-In Address (BIA)** - ฝังอยู่ใน ROM ของ NIC
- ควรเป็น **Unique** ทั่วโลก

**Format:**

```
XX:XX:XX:XX:XX:XX    (Unix/Linux style)
XX-XX-XX-XX-XX-XX    (Windows style)
XXXX.XXXX.XXXX       (Cisco style)

ตัวอย่าง:
00:1A:2B:3C:4D:5E
00-1A-2B-3C-4D-5E
001A.2B3C.4D5E
```

---

### MAC Address Components (ส่วนประกอบ)

**โครงสร้าง:**

```
    OUI (24 bits)          Vendor Assigned (24 bits)
|----------------------|  |------------------------|
  00:1A:2B              :  3C:4D:5E
  Manufacturer ID          Serial Number/Unique
```

#### 1. OUI (Organizationally Unique Identifier)

**คำจำกัดความ:**

- **24 bits แรก** (3 bytes)
- กำหนดโดย **IEEE**
- ระบุ**ผู้ผลิต** (Manufacturer)

**ตัวอย่าง OUI:**

```
00:1A:2B  - Cisco Systems
00:50:56  - VMware, Inc.
00:0C:29  - VMware, Inc.
DC:A6:32  - Raspberry Pi Foundation
00:1B:63  - Apple, Inc.
00:E0:4C  - Realtek
B8:27:EB  - Raspberry Pi
```

**I/G Bit (Individual/Group):**

- Bit ตัวแรกของ byte แรก
- **0** = **Unicast** MAC (Individual)
- **1** = **Multicast** MAC (Group)

**U/L Bit (Universal/Local):**

- Bit ตัวที่ 2 ของ byte แรก
- **0** = **Globally Unique** (Universal) - กำหนดโดยผู้ผลิต
- **1** = **Locally Administered** - กำหนดโดย admin

**ตัวอย่าง:**

```
00:1A:2B:3C:4D:5E

Binary ของ byte แรก: 00000000
                       ^    ^
                      I/G  U/L
                       0    0

I/G = 0 → Unicast
U/L = 0 → Globally unique (จากผู้ผลิต)
```

#### 2. Vendor Assigned (Device Identifier)

**คำจำกัดความ:**

- **24 bits หลัง** (3 bytes)
- กำหนดโดย**ผู้ผลิต**
- ทำให้แต่ละอุปกรณ์มี MAC address **ไม่ซ้ำกัน**

**จำนวนที่เป็นไปได้:**

- 2^24 = 16,777,216 addresses ต่อ OUI หนึ่งตัว

---

### Types of MAC Addresses

#### 1. Unicast MAC Address (ยูนิคาสต์)

**คำจำกัดความ:**

- ส่งไปยัง**อุปกรณ์เครื่องเดียว**
- I/G bit = **0**

**ลักษณะ:**

- ใช้บ่อยที่สุด
- MAC address ปกติของ NIC

**ตัวอย่าง:**

```
00:1A:2B:3C:4D:5E  (Unicast)

Binary ของ byte แรก: 00000000
                       ^
                      I/G = 0 (Unicast)
```

**การทำงาน:**

- Switch forward ไปยัง port ที่เฉพาะเจาะจง
- เฉพาะอุปกรณ์นั้นรับ frame

#### 2. Multicast MAC Address (มัลติคาสต์)

**คำจำกัดความ:**

- ส่งไปยัง**กลุ่มอุปกรณ์**
- I/G bit = **1**

**Range:**

```
01:00:5E:00:00:00 - 01:00:5E:7F:FF:FF  (IPv4 Multicast)
33:33:XX:XX:XX:XX                      (IPv6 Multicast)
```

**ตัวอย่าง:**

```
01:00:5E:00:00:01  (All hosts - Multicast)

Binary ของ byte แรก: 00000001
                       ^
                      I/G = 1 (Multicast)
```

**IPv4 Multicast to MAC Mapping:**

```
IPv4 Multicast:  224.0.0.1
MAC Address:     01:00:5E:00:00:01

Formula: 01:00:5E:XX:XX:XX (copy last 23 bits of IP)
```

**การทำงาน:**

- อุปกรณ์ **subscribe** ไปยัง multicast group
- Switch forward ไปยังสมาชิกของกลุ่ม
- ลด broadcast traffic

**ตัวอย่าง Multicast Groups:**

```
224.0.0.1   → 01:00:5E:00:00:01  (All hosts)
224.0.0.2   → 01:00:5E:00:00:02  (All routers)
224.0.0.5   → 01:00:5E:00:00:05  (OSPF routers)
224.0.0.9   → 01:00:5E:00:00:09  (RIPv2 routers)
224.0.0.10  → 01:00:5E:00:00:0A  (EIGRP routers)
```

#### 3. Broadcast MAC Address (บรอดคาสต์)

**คำจำกัดความ:**

- ส่งไปยัง**ทุกอุปกรณ์**ในเครือข่ายเดียวกัน
- ทุก bit เป็น **1**

**Address:**

```
FF:FF:FF:FF:FF:FF  (Broadcast)

หรือ
FF-FF-FF-FF-FF-FF
FFFF.FFFF.FFFF
```

**การทำงาน:**

- Switch **flood** ไปยัง**ทุก port** (ยกเว้น port ที่รับเข้ามา)
- ทุกอุปกรณ์รับ frame
- Router **ไม่ forward** broadcast (แบ่ง broadcast domain)

**การใช้งาน:**

- **ARP Request** - หา MAC address จาก IP
- **DHCP Discovery** - หา DHCP server
- **NetBIOS name resolution**

**ตัวอย่าง:**

```
DHCP Discovery:
  Source MAC:      AA:BB:CC:DD:EE:FF (Client)
  Destination MAC: FF:FF:FF:FF:FF:FF (Broadcast)
  → ทุกคนได้รับ แต่เฉพาะ DHCP server ตอบกลับ
```

---

### MAC Address Assignment

#### 1. Static (Manual)

**คำจำกัดความ:**

- กำหนดโดย administrator
- ใช้ในบางกรณีพิเศษ

**วิธีการ:**

- เปลี่ยนใน software (driver, OS)
- **MAC spoofing** - ปลอม MAC address

**การใช้งาน:**

- Bypass MAC filtering
- Testing
- Virtual machines (VMs)
- Privacy

**ตัวอย่างคำสั่ง:**

```bash
# Linux
sudo ip link set eth0 address 00:11:22:33:44:55

# Windows (Registry edit)
# macOS (Network Preferences)
```

#### 2. Dynamic (Automatic)

**คำจำกัดความ:**

- ใช้ BIA (Burned-In Address) จาก NIC
- ผู้ผลิตกำหนดตอนผลิต
- เก็บใน ROM/EEPROM ของ NIC

**ลักษณะ:**

- **Unique** ทั่วโลก (ในทฤษฎี)
- ไม่เปลี่ยนแปลง (ยกเว้นเปลี่ยน NIC)

---

### Viewing MAC Addresses

**Windows:**

```cmd
ipconfig /all
```

Output:

```
Physical Address. . . . . . . . . : 00-1A-2B-3C-4D-5E
```

**Linux:**

```bash
ip link show
# หรือ
ifconfig
```

Output:

```
eth0: <BROADCAST,MULTICAST,UP> mtu 1500
      link/ether 00:1a:2b:3c:4d:5e
```

**macOS:**

```bash
ifconfig en0 | grep ether
```

**Cisco Router/Switch:**

```
Router# show interfaces
Router# show mac address-table
```

---

### MAC Address Table (Switch)

**คำจำกัดความ:**

- ตารางที่ Switch เก็บ MAC address และ port ที่เชื่อมต่อ
- เรียกอีกชื่อว่า **CAM (Content Addressable Memory) Table**

**ข้อมูลที่เก็บ:**

- MAC Address
- Port number
- VLAN
- Age (เวลา)

**ตัวอย่าง:**

```
MAC Address          Port    VLAN    Age (sec)
---------------------------------------------------
00:1A:2B:3C:4D:5E    Fa0/1   1       60
AA:BB:CC:DD:EE:FF    Fa0/2   1       45
11:22:33:44:55:66    Fa0/3   1       120
```

**การทำงาน:**

**1. Learning (เรียนรู้):**

- Switch รับ frame
- อ่าน **Source MAC address**
- บันทึก MAC + Port ลงตาราง

**2. Forwarding Decision:**

- อ่าน **Destination MAC address**
- ค้นหาในตาราง:
    - **Found (Known unicast):** Forward ออก port ที่ระบุ
    - **Not found (Unknown unicast):** **Flood** ออกทุก port (ยกเว้น port ที่รับเข้ามา)
    - **Broadcast/Multicast:** Flood ออกทุก port (ยกเว้น port ที่รับเข้ามา)

**3. Aging (หมดอายุ):**

- MAC address entries หมดอายุหลังไม่ใช้งาน
- Default: **300 seconds** (5 minutes)
- ป้องกันตารางเต็มด้วย MAC address เก่าๆ

**4. Updating:**

- ถ้าอุปกรณ์เดิม (MAC เดิม) ปรากฏที่ port อื่น
- Switch **อัพเดท** port ใหม่

**คำสั่ง Cisco:**

```
Switch# show mac address-table
Switch# show mac address-table dynamic
Switch# show mac address-table address 001a.2b3c.4d5e
Switch# show mac address-table interface fa0/1
Switch# clear mac address-table dynamic
```

---

### Switch Learning Process (กระบวนการเรียนรู้)

**Scenario: PC1 ส่งไปยัง PC2**

```
Topology:
[PC1]---Fa0/1---[Switch]---Fa0/2---[PC2]
                    |
                  Fa0/3
                    |
                  [PC3]

Step 0: MAC Address Table ว่างเปล่า
MAC Address    Port    Age
---------------------------
(empty)

Step 1: PC1 (MAC: AAAA) sends frame to PC2 (MAC: BBBB)
Frame: Src=AAAA, Dst=BBBB

Step 2: Switch receives on port Fa0/1
  → Learns: AAAA is on Fa0/1

MAC Address    Port    Age
---------------------------
AAAA           Fa0/1   0

Step 3: Switch looks for BBBB in table
  → Not found! (Unknown unicast)
  → **Flood** to all ports except Fa0/1 (Fa0/2, Fa0/3)

Step 4: PC2 receives frame and replies
Frame: Src=BBBB, Dst=AAAA

Step 5: Switch receives reply on port Fa0/2
  → Learns: BBBB is on Fa0/2

MAC Address    Port    Age
---------------------------
AAAA           Fa0/1   15
BBBB           Fa0/2   0

Step 6: Switch looks for AAAA (destination)
  → Found! On Fa0/1
  → Forward only to Fa0/1 (Efficient!)

Result: Switch now knows both MACs
        Future frames between PC1↔PC2 = Unicast (no flooding)
```

---

## Summary (สรุป)

Module 6 นี้เราได้เรียนรู้:

1. ✅ **Purpose of Data Link Layer** - Framing, Addressing, Media Access, Error Detection
2. ✅ **LLC และ MAC Sub-layers** - IEEE 802.2 และ 802.3
3. ✅ **Topologies** - Physical vs Logical, WAN (Point-to-Point, Hub-Spoke, Mesh), LAN (Star, Bus, Ring)
4. ✅ **Half-Duplex vs Full-Duplex**
5. ✅ **Data Link Frame** - Structure, Size, FCS
6. ✅ **MAC Address** - 48-bit, OUI + Vendor Assigned, Unicast/Multicast/Broadcast
7. ✅ **MAC Address Table** - Learning, Forwarding, Aging

**สิ่งสำคัญที่ต้องจำ:**

- Data Link Layer = Layer 2
- PDU = **Frame**
- MAC address = 48-bit physical address (6 bytes)
- OUI (24 bits) = ผู้ผลิต, Vendor Assigned (24 bits) = รหัสเฉพาะ
- Unicast (1-to-1), Multicast (1-to-group), Broadcast (1-to-all)
- Broadcast MAC = FF:FF:FF:FF:FF:FF
- Switch เรียนรู้ MAC address จาก **Source MAC**
- Switch forward ตาม **Destination MAC**
- Star topology = ใช้มากที่สุดในปัจจุบัน
- Full-duplex = Default, เร็วกว่า Half-duplex

**Next Module:** Module 7 - Ethernet Switching

**[ไฟล์ Module 6 สมบูรณ์แล้ว!]**