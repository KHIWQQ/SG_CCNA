# CCNA 2: Module 2 - Switching Concepts

## แนวคิดและหลักการทำงานของ Switch

---

## สารบัญ

1. [วัตถุประสงค์ของ Module](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#%E0%B8%A7%E0%B8%B1%E0%B8%95%E0%B8%96%E0%B8%B8%E0%B8%9B%E0%B8%A3%E0%B8%B0%E0%B8%AA%E0%B8%87%E0%B8%84%E0%B9%8C%E0%B8%82%E0%B8%AD%E0%B8%87-module)
2. [Frame Forwarding](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#1-frame-forwarding)
3. [Switching Domains](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#2-switching-domains)
4. [สรุป](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#%E0%B8%AA%E0%B8%A3%E0%B8%B8%E0%B8%9B)

---

## วัตถุประสงค์ของ Module

เมื่อจบ Module นี้ คุณจะสามารถ:

- ✅ อธิบายการทำงานของ Ethernet switches
- ✅ เข้าใจวิธีการส่งต่อ frames ของ switch
- ✅ อธิบาย MAC address table และการเรียนรู้
- ✅ แยกแยะ collision domains และ broadcast domains
- ✅ เข้าใจ switch forwarding methods
- ✅ เปรียบเทียบ cut-through และ store-and-forward switching
- ✅ อธิบาย memory buffering บน switches

---

## 1. Frame Forwarding

### การส่งต่อเฟรมของ Switch

### 1.1 Switch การทำงานของ Ethernet LAN

**Ethernet Switch คืออะไร:**

- อุปกรณ์ Layer 2 (Data Link Layer)
- ใช้ MAC addresses ในการตัดสินใจส่งต่อ frames
- สร้างและจัดการ MAC address table
- เพิ่มประสิทธิภาพของ network โดยแบ่ง collision domains

**หน้าที่หลักของ Switch:**

1. **Learning** - เรียนรู้ MAC addresses จาก source addresses
2. **Forwarding** - ส่งต่อ frames ไปยัง destination ที่ถูกต้อง
3. **Flooding** - ส่ง frames ออกทุกพอร์ต (ยกเว้นพอร์ตที่รับเข้ามา)
4. **Filtering** - ไม่ส่งต่อ frames ที่ไม่จำเป็น
5. **Aging** - ลบ MAC addresses ที่ไม่ได้ใช้งาน

### 1.2 Switch Learning และ Forwarding

**กระบวนการ Learning:**

```
ขั้นตอนที่ 1: Switch รับ Frame
- ตรวจสอบ Source MAC address
- บันทึก Source MAC และ Port ที่รับเข้ามาใน MAC address table

ขั้นตอนที่ 2: Switch ส่งต่อ Frame
- ตรวจสอบ Destination MAC address
- ค้นหาใน MAC address table
- ตัดสินใจส่งต่อ:
  * ถ้ารู้จัก → Forward ไปพอร์ตที่ถูกต้อง (Unicast)
  * ถ้าไม่รู้จัก → Flood ออกทุกพอร์ต (ยกเว้นพอร์ตที่รับเข้า)
  * ถ้าเป็น Broadcast → Flood ออกทุกพอร์ต
```

**ตัวอย่างการทำงาน:**

```
สถานการณ์: PC-A (MAC: 00-01) ส่ง frame ไป PC-B (MAC: 00-02)

Initial State:
- MAC Address Table: ว่างเปล่า

Step 1: PC-A ส่ง frame เข้า Switch port Fa0/1
- Switch เรียนรู้: 00-01 อยู่ที่ port Fa0/1
- MAC Table: 00-01 → Fa0/1

Step 2: Switch ตรวจสอบ Destination MAC (00-02)
- ไม่พบใน MAC Table
- Flood frame ออกทุกพอร์ต (ยกเว้น Fa0/1)

Step 3: PC-B ได้รับ frame และตอบกลับ
- Switch รับ frame จาก PC-B ที่ port Fa0/2
- Switch เรียนรู้: 00-02 อยู่ที่ port Fa0/2
- MAC Table: 00-01 → Fa0/1, 00-02 → Fa0/2

Step 4: การสื่อสารครั้งต่อไป
- Switch รู้ทั้ง 00-01 และ 00-02 แล้ว
- Forward โดยตรงไปพอร์ตที่ถูกต้อง (ไม่ flood)
```

### 1.3 MAC Address Table

**โครงสร้าง MAC Address Table:**

```
MAC Address         Port        VLAN
-----------------------------------------
0000.1111.aaaa      Fa0/1       1
0000.2222.bbbb      Fa0/2       1
0000.3333.cccc      Fa0/5       1
0000.4444.dddd      Fa0/10      10
```

**คำสั่งดู MAC Address Table:**

```cisco
! แสดง MAC address table ทั้งหมด
Switch# show mac address-table

! แสดงเฉพาะ dynamic entries
Switch# show mac address-table dynamic

! แสดงเฉพาะ VLAN
Switch# show mac address-table vlan 1

! แสดงเฉพาะ interface
Switch# show mac address-table interface fastethernet 0/1

! แสดงเฉพาะ MAC address
Switch# show mac address-table address 0000.1111.aaaa
```

**ประเภทของ MAC Address Entries:**

1. **Dynamic:**
    
    - เรียนรู้อัตโนมัติจาก source MAC
    - มี aging timer (default 300 วินาที / 5 นาที)
    - ถูกลบเมื่อไม่ได้ใช้งาน
2. **Static:**
    
    - กำหนดโดย administrator
    - ไม่ถูกลบโดย aging timer
    - ใช้สำหรับ security หรือ critical devices
3. **Secure:**
    
    - กำหนดโดย Port Security
    - จำกัดจำนวน MAC addresses ต่อพอร์ต

**การจัดการ MAC Address Table:**

```cisco
! ลบ MAC address table ทั้งหมด
Switch# clear mac address-table dynamic

! ลบเฉพาะ interface
Switch# clear mac address-table dynamic interface fa0/1

! ลบเฉพาะ VLAN
Switch# clear mac address-table dynamic vlan 1

! กำหนด aging time (60-1000000 วินาที)
Switch(config)# mac address-table aging-time 200

! เพิ่ม static MAC address
Switch(config)# mac address-table static 0000.1111.aaaa vlan 1 interface fa0/1
```

### 1.4 Switch Forwarding Methods

**1. Store-and-Forward Switching:**

**วิธีการทำงาน:**

- รับ frame ทั้งหมดเข้า buffer
- ตรวจสอบ FCS (Frame Check Sequence)
- ถ้า frame ถูกต้อง → ส่งต่อ
- ถ้า frame ผิดพลาด → drop

**ข้อดี:**

- ✅ Error checking ที่สมบูรณ์
- ✅ ป้องกัน corrupted frames
- ✅ เหมาะกับ networks ที่ต้องการความถูกต้องสูง

**ข้อเสีย:**

- ❌ Latency สูงกว่า (ต้องรอรับ frame ทั้งหมด)
- ❌ ช้ากว่า cut-through

**ใช้เมื่อ:**

- Network มีความเร็วต่างกันหลายแบบ (10/100/1000 Mbps)
- ต้องการ quality of service (QoS)
- ต้องการความถูกต้องของข้อมูล

**2. Cut-Through Switching:**

**วิธีการทำงาน:**

- อ่านเฉพาะ destination MAC address (6 bytes แรก)
- เริ่มส่งต่อทันทีโดยไม่รอ frame ทั้งหมด
- ไม่ตรวจสอบ FCS

**ข้อดี:**

- ✅ Latency ต่ำมาก (fast switching)
- ✅ เหมาะกับ real-time applications

**ข้อเสีย:**

- ❌ ไม่มี error checking
- ❌ ส่งต่อ corrupted frames ได้
- ❌ ใช้ไม่ได้ถ้าความเร็วต่างกัน

**ประเภทของ Cut-Through:**

a) **Fast-Forward Switching:**

- อ่านแค่ destination MAC (6 bytes)
- Latency ต่ำที่สุด
- ไม่มี error checking เลย

b) **Fragment-Free Switching:**

- อ่าน 64 bytes แรก
- ตรวจสอบ collision fragments
- Latency ต่ำ แต่มีการตรวจสอบบ้าง
- ดีกว่า fast-forward ในเรื่อง error detection

### 1.5 เปรียบเทียบ Switching Methods

|คุณสมบัติ|Store-and-Forward|Cut-Through (Fast)|Fragment-Free|
|---|---|---|---|
|**Latency**|สูง|ต่ำมาก|ต่ำ|
|**Error Checking**|เต็มรูปแบบ (FCS)|ไม่มี|ตรวจสอบ 64 bytes|
|**Reliability**|สูงสุด|ต่ำ|ปานกลาง|
|**Speed**|ช้าที่สุด|เร็วที่สุด|เร็ว|
|**Buffering**|ต้องการ buffer เต็ม|buffer น้อย|buffer 64 bytes|
|**Error Forwarding**|ไม่ส่งต่อ errors|ส่งต่อ errors|ส่งต่อ errors บางส่วน|

**ตัวเลือกของ Cisco:**

- Catalyst switches ส่วนใหญ่ใช้ **Store-and-Forward** เป็น default
- บาง models รองรับ cut-through (เช่น Catalyst 3750, 6500)
- Administrator สามารถเปลี่ยนได้ตาม requirement

### 1.6 Memory Buffering

**Memory Buffer คืออะไร:**

- พื้นที่ใน RAM สำหรับเก็บ frames ชั่วคราว
- ใช้เมื่อ destination port ยุ่งอยู่
- ป้องกันการสูญหายของ frames

**ประเภทของ Memory Buffering:**

**1. Port-Based Memory Buffering:**

```
[Input Port] → [Dedicated Buffer] → [Switching Fabric] → [Output Port]

แต่ละพอร์ตมี buffer เป็นของตัวเอง
```

**ข้อดี:**

- Buffer แยกกันชัดเจน
- ไม่แย่งกัน

**ข้อเสีย:**

- อาจเกิด buffer overflow บางพอร์ต
- ในขณะที่พอร์ตอื่นยังว่าง
- ใช้ memory ไม่คุ้ม

**2. Shared Memory Buffering:**

```
[All Ports] → [Shared Memory Pool] → [Switching Fabric] → [All Ports]

ทุกพอร์ตใช้ buffer pool ร่วมกัน
```

**ข้อดี:**

- ✅ ใช้ memory มีประสิทธิภาพ
- ✅ Dynamic allocation
- ✅ ลด buffer overflow
- ✅ รองรับ asymmetric switching

**ข้อเสียต:**

- ต้องมี logic ในการจัดการที่ซับซ้อน

**การทำงานของ Shared Memory:**

1. Frames เข้ามาจากหลายพอร์ต
2. เก็บไว้ใน shared memory pool
3. Link ไปยัง destination port ด้วย pointer
4. ส่งออกตาม priority และ queue

**Cisco Modern Switches:**

- ใช้ **Shared Memory Buffering** เป็นหลัก
- มีประสิทธิภาพและ flexible มากกว่า

### 1.7 Duplex และ Speed Settings

**Duplex Settings:**

**Half Duplex:**

- ส่งหรือรับทีละทิศทาง
- ใช้ CSMA/CD
- มี collision
- Legacy technology

**Full Duplex:**

- ส่งและรับพร้อมกันได้
- ไม่มี collision
- แบนด์วิธเพิ่มเป็น 2 เท่า
- **Modern standard**

**Auto-Negotiation:**

- Switch และ device เจรจากันอัตโนมัติ
- เลือก speed และ duplex ที่ดีที่สุด
- ถ้าไม่สำเร็จ → fallback ไป slower speed

**Duplex Mismatch:**

```
Problem: 
PC (Full Duplex) ←→ Switch (Half Duplex)

Results:
- Collisions บนฝั่ง half duplex
- Packet loss
- Poor performance
- Late collisions

Solution:
- ตรวจสอบ: show interface fa0/1
- แก้ไข: ตั้งค่าให้ตรงกันทั้งสองฝั่ง
```

**Best Practices:**

```cisco
! สำหรับ Server และ Critical Devices
Switch(config)# interface fa0/1
Switch(config-if)# speed 1000
Switch(config-if)# duplex full

! สำหรับ Access Ports ทั่วไป
Switch(config)# interface range fa0/2 - 24
Switch(config-if-range)# speed auto
Switch(config-if-range)# duplex auto
```

---

## 2. Switching Domains

### โดเมนของการสวิตช์

### 2.1 Collision Domains

**Collision Domain คืออะไร:**

- พื้นที่เครือข่ายที่ collision สามารถเกิดขึ้นได้
- ทุกอุปกรณ์ใน collision domain เดียวกันแข่งขันใช้ bandwidth
- ยิ่ง collision domain เล็ก → ประสิทธิภาพยิ่งดี

**Legacy Ethernet (Hub):**

```
Hub Network = 1 Collision Domain

[PC1] ─┐
[PC2] ─┼─ [HUB] ─ [Router]
[PC3] ─┘

ทุกอุปกรณ์อยู่ใน collision domain เดียวกัน
- ส่งข้อมูลได้ทีละตัว
- เกิด collision บ่อย
- Performance ต่ำ
```

**Modern Ethernet (Switch):**

```
Switch Network = แต่ละ Port เป็น Collision Domain แยกกัน

[PC1] ──┐
[PC2] ──┼── [Switch] ── [Router]
[PC3] ──┘

แต่ละ connection เป็น collision domain แยก
- ไม่มี collision ระหว่างพอร์ต
- Full duplex ไม่มี collision เลย
- Performance สูง
```

**การนับ Collision Domains:**

```
Topology 1: Hub
         [Hub]
        /  |  \
     [PC] [PC] [PC]

Collision Domains = 1

Topology 2: Switch
         [Switch]
        /   |   \
     [PC] [PC] [PC]

Collision Domains = 3
(แต่ละพอร์ต = 1 collision domain)

Topology 3: Mixed
[PC]─[Switch]─[Hub]─[PC]
                 └─[PC]

Collision Domains = 2
- Switch port to Hub = 1
- Hub side = 1 (ทั้ง hub และ PCs)
```

**วิธีแยก Collision Domains:**

- Switch แต่ละพอร์ต
- Bridge
- Router

### 2.2 Broadcast Domains

**Broadcast Domain คืออะไร:**

- พื้นที่เครือข่ายที่ broadcast frame สามารถไปถึงได้
- อุปกรณ์ทั้งหมดใน broadcast domain เดียวกันได้รับ broadcast
- ยิ่ง broadcast domain ใหญ่ → broadcast traffic ยิ่งมาก

**Broadcast Frame Types:**

1. **Layer 2 Broadcast:**
    
    - Destination MAC: FF:FF:FF:FF:FF:FF
    - ส่งถึงทุกอุปกรณ์ใน LAN เดียวกัน
2. **Layer 3 Broadcast:**
    
    - Destination IP: 255.255.255.255
    - หรือ network broadcast (เช่น 192.168.1.255)

**ตัวอย่าง Broadcast Traffic:**

- ARP requests
- DHCP discover messages
- NetBIOS name queries
- Routing protocol updates (บางโปรโตคอล)

**Hub Network:**

```
[Hub]
 ├── PC1
 ├── PC2
 └── PC3

= 1 Broadcast Domain
- Broadcast จาก PC1 ไปถึง PC2, PC3
```

**Switch Network (Single VLAN):**

```
[Switch]
 ├── PC1  (VLAN 1)
 ├── PC2  (VLAN 1)
 └── PC3  (VLAN 1)

= 1 Broadcast Domain
- Broadcast ไปถึงทุก PC ใน VLAN เดียวกัน
```

**Switch Network (Multiple VLANs):**

```
[Switch]
 ├── PC1  (VLAN 10)
 ├── PC2  (VLAN 10)
 ├── PC3  (VLAN 20)
 └── PC4  (VLAN 20)

= 2 Broadcast Domains
- VLAN 10: PC1, PC2
- VLAN 20: PC3, PC4
- Broadcast ใน VLAN 10 ไม่ไปถึง VLAN 20
```

**Router:**

```
[Router]
 ├── Network A (192.168.1.0/24)
 └── Network B (192.168.2.0/24)

= 2 Broadcast Domains
- Router แยก broadcast domains
- Broadcast จาก Network A ไม่ไปถึง Network B
```

**การนับ Broadcast Domains:**

```
Topology 1: Single Switch
[PC]─┐
[PC]─┼─[Switch]
[PC]─┘

Broadcast Domains = 1

Topology 2: Switch with VLANs
[PC VLAN10]─┐
[PC VLAN10]─┼─[Switch]
[PC VLAN20]─┘

Broadcast Domains = 2

Topology 3: Multiple Switches
[Switch1]────[Switch2]
   │            │
  [PC]         [PC]

Broadcast Domains = 1
(ถ้าไม่มี VLAN หรือ trunk ใช้ VLAN เดียวกัน)

Topology 4: Router
[Switch1]────[Router]────[Switch2]
   │                        │
  [PC]                     [PC]

Broadcast Domains = 2
(Router แยก broadcast domains)
```

**วิธีแยก Broadcast Domains:**

- **Router** - แยกระหว่าง networks
- **Layer 3 Switch** - แยกระหว่าง VLANs (inter-VLAN routing)
- **VLANs** - แยกภายในสวิตช์เดียว

### 2.3 Alleviating Network Congestion

**ปัญหา Network Congestion:**

**สาเหตุ:**

1. Broadcast traffic มากเกินไป
2. Collision domains ใหญ่เกินไป
3. ใช้ half-duplex
4. Bottlenecks ที่ uplinks
5. Bandwidth ไม่เพียงพอ

**วิธีแก้ไข:**

**1. Segmentation with Switches:**

```
Before (Hub):
     [Hub] ← 1 Collision Domain, 1 Broadcast Domain
    /  |  \
 [PC][PC][PC]

After (Switch):
    [Switch] ← 3 Collision Domains, 1 Broadcast Domain
    /  |  \
 [PC][PC][PC]
```

**2. Implement VLANs:**

```
[Switch]
 ├── PC1 (VLAN 10) ← Sales
 ├── PC2 (VLAN 10) ← Sales
 ├── PC3 (VLAN 20) ← Engineering
 └── PC4 (VLAN 20) ← Engineering

= แยก broadcast domains = ลด broadcast traffic
```

**3. Full Duplex:**

```
Half Duplex → Full Duplex
- เพิ่มแบนด์วิธ 2 เท่า
- ไม่มี collision
- Better performance
```

**4. Fast Port to Servers:**

```
[Switch]
 ├── User PCs (100 Mbps)
 └── Server (1 Gbps) ← Fast connection

= Asymmetric switching
```

**5. Multiple Switches:**

```
[Core Switch]
    /    \
[SW1]    [SW2]
  |        |
Users    Users

= แยก broadcast domains ด้วย VLANs
= ลด load บนแต่ละ switch
```

### 2.4 Switch Performance

**ปัจจัยที่ส่งผลต่อประสิทธิภาพ:**

**1. Port Density:**

- จำนวนพอร์ตที่มี
- Switch 24-port vs 48-port
- Modular switches (เพิ่มพอร์ตได้)

**2. Forwarding Rates:**

- ความเร็วในการส่งต่อ frames
- วัดเป็น fps (frames per second)
- หรือ pps (packets per second)

**3. Link Aggregation:**

- รวมหลายลิงก์เป็นลิงก์เดียว
- เพิ่ม bandwidth
- เพิ่ม redundancy

**4. Power over Ethernet (PoE):**

- จ่ายไฟผ่านสาย Ethernet
- สำหรับ IP phones, Access Points, Cameras
- PoE (15.4W), PoE+ (30W), PoE++ (60-100W)

**5. Multilayer Switching:**

- Layer 3 switching (routing)
- Wire-speed routing
- Better than routing ผ่าน router

**ตัวอย่างสเปค:**

```
Cisco Catalyst 2960-X Series:
- 24 หรือ 48 ports
- 10/100/1000 Mbps
- Forwarding Rate: 148 Mpps
- Switching Bandwidth: 200 Gbps
- PoE+ support
- Layer 2 switching
```

### 2.5 Frame Forwarding Example

**สถานการณ์จริง:**

```
Network Topology:
PC-A (00-AA) ── Fa0/1
                        [Switch]
PC-B (00-BB) ── Fa0/2

Initial: MAC Address Table ว่าง
```

**Scenario 1: PC-A ping PC-B (ครั้งแรก)**

```
Step 1: PC-A ต้องการ MAC ของ PC-B
Action: ส่ง ARP Request (Broadcast)
- Source MAC: 00-AA
- Destination MAC: FF:FF:FF:FF:FF:FF

Step 2: Switch รับ ARP Request จาก Fa0/1
Action: 
- Learning: บันทึก 00-AA → Fa0/1
- Forwarding: Broadcast → ส่งออกทุกพอร์ต (ยกเว้น Fa0/1)
- MAC Table: [00-AA → Fa0/1]

Step 3: PC-B รับ ARP Request
Action: ตอบกลับ ARP Reply (Unicast)
- Source MAC: 00-BB
- Destination MAC: 00-AA

Step 4: Switch รับ ARP Reply จาก Fa0/2
Action:
- Learning: บันทึก 00-BB → Fa0/2
- Forwarding: รู้ว่า 00-AA อยู่ที่ Fa0/1 → ส่งตรงไป Fa0/1
- MAC Table: [00-AA → Fa0/1, 00-BB → Fa0/2]

Step 5: PC-A รับ ARP Reply
Action: ได้ MAC ของ PC-B แล้ว → เริ่ม ping

Step 6: PC-A ส่ง ICMP Echo Request
Action: ส่ง Unicast frame
- Source MAC: 00-AA
- Destination MAC: 00-BB

Step 7: Switch รับจาก Fa0/1
Action:
- รู้ว่า 00-BB อยู่ที่ Fa0/2
- Forward ตรงไป Fa0/2 (ไม่ flood)

Step 8: PC-B ตอบกลับ ICMP Echo Reply
Action: ส่งกลับไป PC-A
- Switch รู้เส้นทางทั้งหมดแล้ว
- Forward ตรงๆ ไม่ต้อง flood
```

**Scenario 2: การติดต่อครั้งต่อไป**

```
ทุกครั้งที่ PC-A และ PC-B สื่อสารกัน:
- Switch รู้ MAC addresses ทั้งคู่แล้ว
- Forward โดยตรง (Unicast)
- ไม่มี flooding
- ประสิทธิภาพสูงสุด
```

---

## สรุป

### สิ่งที่ได้เรียนรู้ใน Module 2:

✅ **Switch Operation:**

- Learning, Forwarding, Flooding, Filtering
- MAC Address Table และการจัดการ
- Dynamic, Static, และ Secure MAC addresses

✅ **Frame Forwarding Methods:**

- Store-and-Forward (error checking เต็มรูปแบบ)
- Cut-Through (latency ต่ำ)
    - Fast-Forward
    - Fragment-Free
- Memory Buffering (Port-based vs Shared)

✅ **Switching Domains:**

- Collision Domains (แยกโดย switches)
- Broadcast Domains (แยกโดย routers และ VLANs)
- การนับและแยก domains

✅ **Network Performance:**

- Duplex และ Speed settings
- Auto-negotiation
- Duplex mismatch problems
- Network congestion solutions

✅ **Switch Features:**

- Port density
- Forwarding rates
- PoE support
- Multilayer switching

---

## แนวคิดสำคัญที่ต้องจำ

### 1. MAC Address Learning Process:

```
1. รับ frame
2. ดู Source MAC
3. บันทึก Source MAC + Port
4. ดู Destination MAC
5. ตัดสินใจ: Forward / Flood / Filter
```

### 2. การเปรียบเทียบ Forwarding Methods:

|Method|Latency|Error Check|Best For|
|---|---|---|---|
|Store-and-Forward|สูง|เต็มรูปแบบ|Mixed speeds, QoS|
|Cut-Through (Fast)|ต่ำที่สุด|ไม่มี|Same speeds, low latency|
|Fragment-Free|ต่ำ|64 bytes|Compromise|

### 3. การแยก Domains:

**Collision Domains:**

- Hub = 1 domain ทั้งหมด
- Switch = แต่ละพอร์ต = 1 domain
- Full duplex = ไม่มี collision

**Broadcast Domains:**

- Switch (no VLANs) = 1 domain
- VLANs = แยก domains
- Router = แยก domains

---

## คำสั่งสำคัญที่ต้องจำ

### MAC Address Table:

```cisco
show mac address-table
show mac address-table dynamic
show mac address-table interface fa0/1
show mac address-table address 0000.1111.aaaa
clear mac address-table dynamic
mac address-table aging-time <seconds>
mac address-table static <mac> vlan <id> interface <interface>
```

### Interface Settings:

```cisco
interface <interface>
 speed {10|100|1000|auto}
 duplex {half|full|auto}
 mdix auto
```

### Verification:

```cisco
show interface <interface>
show interface status
show interface <interface> | include duplex
show interface <interface> | include error
show controllers ethernet-controller fa0/1
```

---

## Lab Activities

### Lab 2.1: Observe MAC Address Learning

**วัตถุประสงค์:**

- สังเกตการเรียนรู้ MAC addresses
- ดู MAC address table updates
- ทดสอบ flooding behavior

**ขั้นตอน:**

1. Clear MAC address table
2. Ping ระหว่าง devices
3. สังเกต MAC table learning
4. Verify entries

### Lab 2.2: Configure Duplex and Speed

**วัตถุประสงค์:**

- Configure duplex และ speed settings
- สังเกต duplex mismatch
- แก้ไขปัญหา performance

---

## Packet Tracer Activities

### PT 2.1: Examine Frame Forwarding

**สิ่งที่ต้องทำ:**

1. สังเกต MAC learning process
2. ดู flooding behavior
3. เข้าใจ unicast forwarding
4. Verify MAC address table

### PT 2.2: Troubleshoot Duplex Mismatch

**สิ่งที่ต้องทำ:**

1. ระบุ duplex mismatch
2. ใช้ verification commands
3. แก้ไขปัญหา
4. Verify connectivity

---

## คำถามทบทวน

1. Switch เรียนรู้ MAC addresses อย่างไร?
2. Store-and-Forward และ Cut-Through ต่างกันอย่างไร?
3. Fragment-Free switching คืออะไร?
4. Collision Domain และ Broadcast Domain ต่างกันอย่างไร?
5. Switch ใช้ Shared Memory Buffering ดีอย่างไร?
6. Duplex mismatch ทำให้เกิดปัญหาอะไร?
7. VLAN ช่วยแยก broadcast domains ได้อย่างไร?
8. Auto-MDIX คืออะไร?
9. จะตรวจสอบ MAC address table ได้อย่างไร?
10. Full duplex ดีกว่า half duplex อย่างไร?

---

## เฉลยคำถาม

1. **MAC Learning:** รับ frame → ดู Source MAC → บันทึก MAC + Port ใน MAC table
2. **Store vs Cut-Through:** Store ตรวจสอบ error เต็มรูปแบบแต่ช้า, Cut-Through เร็วแต่ไม่ตรวจสอบ
3. **Fragment-Free:** อ่าน 64 bytes แรกเพื่อตรวจสอบ collision fragments, compromise ระหว่าง store และ cut-through
4. **Domains:** Collision = พื้นที่ที่เกิด collision ได้, Broadcast = พื้นที่ที่ broadcast ไปถึงได้
5. **Shared Memory:** ทุกพอร์ตใช้ memory pool ร่วมกัน, dynamic allocation, ใช้ memory มีประสิทธิภาพ
6. **Duplex Mismatch:** collisions, packet loss, poor performance, late collisions
7. **VLANs:** แต่ละ VLAN = แยก broadcast domain, broadcast ไม่ข้าม VLANs
8. **Auto-MDIX:** ตรวจจับและปรับใช้สาย straight/crossover อัตโนมัติ
9. **MAC Table:** `show mac address-table` หรือ `show mac address-table dynamic`
10. **Full Duplex:** ส่ง-รับพร้อมกัน, แบนด์วิธ 2 เท่า, ไม่มี collision, ประสิทธิภาพสูงกว่า

---

## สรุปสูตรสำคัญ

### การนับ Collision Domains:

```
Hub = 1 collision domain ทั้งหมด
Switch = จำนวนพอร์ตที่มีอุปกรณ์เชื่อมต่อ
Full Duplex = ไม่มี collision (แต่ยังนับเป็น collision domain)
```

### การนับ Broadcast Domains:

```
Switch (no VLANs) = 1 broadcast domain
Switch with VLANs = จำนวน VLANs
Router interfaces = จำนวน interfaces
```

### Forwarding Decision:

```
1. ดู Destination MAC
2. ค้นหาใน MAC table
3. ถ้าเจอ → Forward ไปพอร์ตนั้น (Unicast)
4. ถ้าไม่เจอ → Flood ทุกพอร์ต (ยกเว้นพอร์ตต้นทาง)
5. ถ้าเป็น Broadcast/Multicast → Flood ทุกพอร์ต
```

---

## Tips สำหรับการสอบ

1. **จำให้ขึ้นใจ:** Store-and-Forward ตรวจสอบ FCS, Cut-Through ไม่ตรวจสอบ
2. **นับ Domains:** Switch แยก collision แต่ไม่แยก broadcast (ถ้าไม่มี VLANs)
3. **Duplex Mismatch:** สาเหตุหลักของ performance issues
4. **MAC Learning:** เรียนรู้จาก Source MAC เสมอ, ตัดสินใจจาก Destination MAC
5. **Flooding:** เกิดเมื่อไม่รู้จัก Destination MAC หรือเป็น Broadcast
6. **Full Duplex:** ไม่มี collision, ประสิทธิภาพดีที่สุด
7. **Fragment-Free:** อ่าน 64 bytes (ขนาดต่ำสุดของ valid frame)
8. **Shared Memory:** ใช้ในสวิตช์สมัยใหม่, flexible กว่า port-based

---

**หมายเหตุ:** Module 2 เป็นพื้นฐานสำคัญในการเข้าใจการทำงานของ switches ความเข้าใจในหัวข้อนี้จะช่วยใน Module ถัดไป เช่น VLANs, STP, และ EtherChannel

---

**เอกสารจัดทำโดย:** Claude (Anthropic AI)  
**Module:** CCNA 2 Module 2 - Switching Concepts  
**Version:** v7.02 SRWE  
**วันที่อัพเดท:** พฤศจิกายน 2025