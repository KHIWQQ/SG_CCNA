# CCNA Course 1 - Module 8: Network Layer

## ชั้นเครือข่าย

---

## 8.1 Network Layer Characteristics (ลักษณะของชั้นเครือข่าย)

### Network Layer (Layer 3) Overview

**คำจำกัดความ:**

- **ชั้นที่ 3** ของ OSI Model
- รับผิดชอบ**การส่งข้อมูลระหว่างเครือข่าย** (end-to-end delivery)
- ให้บริการ**ที่อยู่ logical** (IP addressing)

**PDU (Protocol Data Unit):** **Packet**

**หน้าที่หลัก:**

- **Addressing** - กำหนด IP address (Logical addressing)
- **Routing** - เลือกเส้นทางที่ดีที่สุด
- **Encapsulation** - ห่อหุ้มข้อมูลจาก Transport Layer
- **Decapsulation** - แกะข้อมูลส่งต่อไป Transport Layer
- **Packet forwarding** - ส่งต่อ packet ไปยังปลายทาง

---

### Network Layer Protocols

#### Primary Protocols (โปรโตคอลหลัก)

**1. IPv4 (Internet Protocol version 4)**

- ใช้มากที่สุดในปัจจุบัน
- 32-bit address
- รูปแบบ: `192.168.1.1`

**2. IPv6 (Internet Protocol version 6)**

- รุ่นใหม่กว่า IPv4
- 128-bit address
- รูปแบบ: `2001:0db8:85a3::8a2e:0370:7334`

**3. ICMP (Internet Control Message Protocol)**

- ใช้สำหรับ diagnostics และ error reporting
- `ping`, `traceroute`

**4. ICMPv6**

- ICMP สำหรับ IPv6

**5. NAT (Network Address Translation)**

- แปลง Private IP เป็น Public IP

---

### Network Layer Functions (หน้าที่)

#### 1. Addressing (การกำหนดที่อยู่)

**Logical Addressing:**

- ใช้ **IP address** (ไม่ใช่ MAC address)
- IP address = **Network portion** + **Host portion**
- สามารถเปลี่ยนแปลงได้

**ตัวอย่าง:**

```
IPv4: 192.168.1.10
      ^^^^^^^^^^ ^^
      Network    Host

IPv6: 2001:db8:acad:1::10
      ^^^^^^^^^^^^^^^ ^^
      Network         Host
```

**เปรียบเทียบกับ MAC address:**

```
Feature          MAC Address         IP Address
----------------------------------------------------------
Layer            Layer 2             Layer 3
Type             Physical            Logical
Format           48-bit (hex)        32-bit (IPv4) / 128-bit (IPv6)
Assigned by      Manufacturer        Network admin / DHCP
Changeable       No (ยกเว้น spoofing) Yes
Scope            Local (same network) Global (across networks)
Used for         Local delivery      End-to-end delivery
----------------------------------------------------------
```

#### 2. Encapsulation (การห่อหุ้ม)

**กระบวนการ:**

1. รับ **Segment** จาก Transport Layer (Layer 4)
2. เพิ่ม **IP Header**
3. สร้าง **Packet**
4. ส่งไปยัง Data Link Layer (Layer 2)

**IP Packet Structure:**

```
+------------+------------------+
| IP Header  | Data (Segment)   |
+------------+------------------+
    Layer 3       From Layer 4

IP Header contains:
- Source IP address
- Destination IP address
- Protocol (TCP/UDP/ICMP)
- TTL (Time to Live)
- etc.
```

#### 3. Routing (การกำหนดเส้นทาง)

**คำจำกัดความ:**

- เลือก**เส้นทางที่ดีที่สุด** (best path) ไปยังปลายทาง
- ใช้ **Routing table**

**Routing Process:**

```
1. Router รับ packet
2. ตรวจสอบ Destination IP
3. ค้นหาใน Routing table
4. เลือก best path
5. Forward packet ออกไป
```

**Routing Table:**

```
Network Destination  Netmask          Gateway          Interface
192.168.1.0          255.255.255.0    0.0.0.0          Fa0/0
10.0.0.0             255.0.0.0        192.168.1.254    Fa0/0
0.0.0.0              0.0.0.0          192.168.1.1      Fa0/0 (Default route)
```

**Routing Protocols:**

- **Static routing** - กำหนดด้วยตนเอง
- **Dynamic routing** - เรียนรู้อัตโนมัติ
    - RIP, EIGRP, OSPF, BGP

#### 4. De-encapsulation (การแกะหุ้ม)

**กระบวนการ:**

1. รับ **Frame** จาก Data Link Layer
2. ลบ Data Link header/trailer
3. ได้ **Packet**
4. ตรวจสอบ Destination IP
5. ถ้าถึงปลายทางแล้ว → ลบ IP header
6. ส่ง Segment ไปยัง Transport Layer

---

### Connectionless vs Connection-Oriented

#### IP is Connectionless (ไม่มีการเชื่อมต่อ)

**คุณสมบัติ:**

- **ไม่สร้าง connection** ก่อนส่งข้อมูล
- **ไม่รับประกัน** delivery
- **ไม่มี acknowledgment**
- **ไม่มี flow control**
- แต่ละ packet เป็นอิสระ (อาจไปคนละเส้นทาง)

**ข้อดี:**

- ✅ **Fast** - ไม่ต้องเสียเวลา establish connection
- ✅ **Simple** - overhead น้อย
- ✅ **Scalable** - รองรับ traffic เยอะได้

**ข้อเสีย:**

- ❌ **Unreliable** - packet อาจหาย, ซ้ำ, หรือเรียงผิด
- ❌ ต้องอาศัย Layer 4 (TCP) จัดการ reliability

**ตัวอย่าง:**

```
Sending 3 packets from A to B:

Packet 1 → Router 1 → Router 2 → B
Packet 2 → Router 1 → Router 3 → B  (different path!)
Packet 3 → Router 1 → Router 2 → B

Packets อาจถึง B ไม่เรียง: 1, 3, 2
หรือ Packet 2 อาจหาย
IP ไม่สน - TCP จะจัดการ
```

---

### Best Effort Delivery (ส่งแบบพยายามสุดความสามารถ)

**คำจำกัดความ:**

- IP จะ**พยายาม**ส่ง packet ให้ถึงปลายทาง
- แต่**ไม่รับประกัน**ว่าจะถึงแน่นอน

**ลักษณะ:**

- No acknowledgment
- No retransmission (ที่ Layer 3)
- No error correction (ที่ Layer 3)
- No flow control (ที่ Layer 3)

**Reliability อยู่ที่ไหน?**

- **Transport Layer (Layer 4)**
- **TCP** = reliable (มี ACK, retransmit)
- **UDP** = unreliable (เหมือน IP - best effort)

**Analogy:**

```
IP = บริษัทขนส่ง
- พยายามส่งของให้ถึง
- แต่ไม่รับประกัน (อาจหาย, เสีย)

TCP = บริษัทขนส่ง + ประกัน
- รับประกันของถึง
- ถ้าหาย → ส่งใหม่
```

---

### Media Independence (ไม่ขึ้นกับสื่อ)

**คำจำกัดความ:**

- IP ทำงานได้กับ**ทุกประเภทของ media**
- ไม่สนใจว่าเป็น Ethernet, Wi-Fi, Fiber, Serial, etc.

**ทำไมเป็นไปได้?**

- IP อยู่ที่ Layer 3
- Data Link Layer (Layer 2) จัดการ media
- Layer 3 ไม่ต้องรู้รายละเอียด Layer 1-2

**ตัวอย่าง:**

```
[PC] --Ethernet--> [Router] --Fiber--> [Router] --Serial--> [PC]
      (802.3)                (Fiber)              (PPP)

IP packet เดียวกัน
แต่ถูก encapsulate ใน Data Link frames ต่างกัน
```

**Maximum Transmission Unit (MTU):**

- ขนาดสูงสุดของ packet ที่ media รองรับ
- แต่ละ media มี MTU ต่างกัน
- ถ้า packet ใหญ่กว่า MTU → **Fragmentation**

**MTU ของ media ต่างๆ:**

```
Media              MTU
-------------------------------
Ethernet           1500 bytes
Wi-Fi (802.11)     1500 bytes (usually)
PPP                1500 bytes
Token Ring         4464 bytes
FDDI               4352 bytes
Serial (WAN)       varies
```

---

## 8.2 IPv4 Packet Header (ส่วนหัวแพ็กเก็ต IPv4)

### IPv4 Packet Structure

**โครงสร้าง:**

```
+-----------------+-------------------+
| IPv4 Header     | Data (Payload)    |
| 20-60 bytes     | varies            |
+-----------------+-------------------+
```

---

### IPv4 Header Fields (ฟิลด์ใน Header)

**Header รวม: 20-60 bytes (usually 20 bytes)**

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version|  IHL  |Type of Service|          Total Length         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Identification        |Flags|      Fragment Offset    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Time to Live |    Protocol   |         Header Checksum       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                       Source IP Address                       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Destination IP Address                     |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options (if IHL > 5)                       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

---

#### 1. Version (4 bits)

**คำจำกัดความ:**

- บอก**เวอร์ชัน**ของ IP protocol

**ค่า:**

- **IPv4** = `0100` (4 in binary)
- **IPv6** = `0110` (6 in binary)

**การใช้งาน:**

- Router ใช้ตรวจสอบว่าเป็น IPv4 หรือ IPv6
- เพื่อ process packet อย่างถูกต้อง

---

#### 2. Internet Header Length (IHL) - 4 bits

**คำจำกัดความ:**

- ความยาวของ **IPv4 header**
- หน่วยเป็น **32-bit words** (4 bytes)

**ค่า:**

- **Minimum:** 5 (5 × 4 = 20 bytes) - no options
- **Maximum:** 15 (15 × 4 = 60 bytes) - with options

**สูตร:**

```
Header length (bytes) = IHL × 4

ตัวอย่าง:
IHL = 5 → Header = 5 × 4 = 20 bytes (standard)
IHL = 6 → Header = 6 × 4 = 24 bytes (with 4 bytes options)
```

---

#### 3. Type of Service (ToS) / Differentiated Services (DS) - 8 bits

**คำจำกัดความ:**

- กำหนด**ความสำคัญ**ของ packet (priority)
- ใช้สำหรับ **Quality of Service (QoS)**

**โครงสร้าง (RFC 2474):**

```
+-------+---+
| DSCP  |ECN|
+-------+---+
  6 bits  2 bits

DSCP = Differentiated Services Code Point
ECN  = Explicit Congestion Notification
```

**DSCP Values (ตัวอย่าง):**

```
Value   Name            Usage
-------------------------------------------
0       Best Effort     Default (normal traffic)
8       CS1             Low priority
16      CS2             
24      CS3             
32      CS4             
40      CS5             Voice/Video
46      EF (Expedited)  Voice (highest priority)
48      CS6             
56      CS7             
```

**การใช้งาน:**

- Voice over IP (VoIP) → High priority (EF)
- Video streaming → Medium-high priority
- Email, Web → Best effort (default)

---

#### 4. Total Length (16 bits)

**คำจำกัดความ:**

- ความยาว**ทั้งหมด**ของ IP packet
- รวม **Header + Data**
- หน่วยเป็น **bytes**

**ค่า:**

- **Minimum:** 20 bytes (header only, no data)
- **Maximum:** 65,535 bytes (2^16 - 1)

**สูตร:**

```
Total Length = Header Length + Data Length

ตัวอย่าง:
Header = 20 bytes
Data = 1480 bytes
Total Length = 20 + 1480 = 1500 bytes
```

**หมายเหตุ:**

- ในทางปฏิบัติ, Total Length มักไม่เกิน **MTU** (1500 bytes)
- ถ้ามากกว่า MTU → ต้อง **Fragmentation**

---

#### 5. Identification (16 bits)

**คำจำกัดความ:**

- รหัสเฉพาะของ packet
- ใช้สำหรับ**รวม fragments**

**การใช้งาน:**

- ถ้า packet ถูก fragment (แบ่งเป็นชิ้นเล็ก)
- ทุก fragment ของ packet เดียวกันจะมี **Identification เดียวกัน**
- ปลายทางใช้ Identification รวม fragments กลับเป็น packet เดิม

**ตัวอย่าง:**

```
Original packet: ID = 12345

After fragmentation:
Fragment 1: ID = 12345, Offset = 0
Fragment 2: ID = 12345, Offset = 1480
Fragment 3: ID = 12345, Offset = 2960

Receiver รู้ว่าทั้ง 3 fragments เป็น packet เดียวกัน (ID = 12345)
```

---

#### 6. Flags (3 bits)

**คำจำกัดความ:**

- ควบคุม**การแบ่ง packet** (fragmentation)

**โครงสร้าง:**

```
Bit 0: Reserved (ไม่ใช้) - ต้องเป็น 0
Bit 1: Don't Fragment (DF)
Bit 2: More Fragments (MF)
```

**DF (Don't Fragment) bit:**

- **0** = อนุญาตให้ fragment ได้
- **1** = **ห้าม fragment** (ถ้า packet ใหญ่เกิน MTU → drop packet)

**MF (More Fragments) bit:**

- **0** = นี่คือ fragment **สุดท้าย** (หรือไม่มี fragmentation)
- **1** = **ยังมี fragments** ตามมาอีก

**ตัวอย่าง:**

```
Original packet (no fragmentation):
DF = 0, MF = 0

Fragmented packet:
Fragment 1: DF = 0, MF = 1  (มี fragments ตามมาอีก)
Fragment 2: DF = 0, MF = 1  (มี fragments ตามมาอีก)
Fragment 3: DF = 0, MF = 0  (fragment สุดท้าย)

Don't Fragment set:
DF = 1, MF = 0  (ห้าม fragment)
```

**การใช้งาน DF flag:**

- **Path MTU Discovery** - หา MTU ต่ำสุดในเส้นทาง
- ส่ง packet ขนาดใหญ่ด้วย DF=1
- ถ้า Router ไหน fragment ไม่ได้ → ส่ง ICMP error กลับ
- ผู้ส่งปรับลด packet size

---

#### 7. Fragment Offset (13 bits)

**คำจำกัดความ:**

- ตำแหน่งของ fragment นี้ใน**ข้อมูลเดิม**
- หน่วยเป็น **8-byte blocks**

**สูตร:**

```
Actual offset (bytes) = Fragment Offset × 8

ตัวอย่าง:
Fragment Offset = 0    → byte 0-1479
Fragment Offset = 185  → byte 1480-2959 (185 × 8 = 1480)
Fragment Offset = 370  → byte 2960-...  (370 × 8 = 2960)
```

**การใช้งาน:**

- ปลายทางใช้ Fragment Offset **เรียง fragments**
- รวมกลับเป็น packet เดิม

**ตัวอย่าง Fragmentation:**

```
Original packet: 4000 bytes data
MTU: 1500 bytes

Fragment 1:
  Header: 20 bytes
  Data: 1480 bytes (offset 0)
  Fragment Offset = 0
  MF = 1

Fragment 2:
  Header: 20 bytes
  Data: 1480 bytes (offset 1480)
  Fragment Offset = 185 (1480 ÷ 8)
  MF = 1

Fragment 3:
  Header: 20 bytes
  Data: 1040 bytes (offset 2960)
  Fragment Offset = 370 (2960 ÷ 8)
  MF = 0 (last fragment)
```

---

#### 8. Time to Live (TTL) - 8 bits

**คำจำกัดความ:**

- **อายุ**ของ packet
- จำนวน **hops** (routers) สูงสุดที่ packet สามารถผ่านได้

**การทำงาน:**

1. ผู้ส่งตั้ง TTL (เช่น 64, 128, 255)
2. ทุกครั้งที่ผ่าน Router → TTL **-1**
3. ถ้า TTL = 0 → Router **drop packet** และส่ง **ICMP Time Exceeded** กลับ

**วัตถุประสงค์:**

- ป้องกัน **routing loop** (packet วนไปมาไม่รู้จบ)
- Packet จะไม่วนอยู่ในเครือข่ายตลอดไป

**ค่าทั่วไป:**

```
OS              Default TTL
--------------------------------
Linux/Unix      64
Windows         128
Cisco IOS       255
```

**ตัวอย่าง:**

```
PC1 (TTL=64) → R1 (TTL=63) → R2 (TTL=62) → R3 (TTL=61) → PC2

ถ้ามี routing loop:
PC1 (TTL=64) → R1 (TTL=63) → R2 (TTL=62) → R3 (TTL=61) → R2 (TTL=60) → R3 (TTL=59) → ... → TTL=0 → DROP
```

**การใช้งาน:**

- **traceroute** ใช้ TTL เพื่อหา path
    - ส่ง packet TTL=1 → Router แรก reply
    - ส่ง packet TTL=2 → Router ที่สอง reply
    - ...

---

#### 9. Protocol (8 bits)

**คำจำกัดความ:**

- บอก**โปรโตคอล**ของ Layer 4 (Transport Layer)
- Data ใน packet เป็นอะไร

**ค่าที่ใช้บ่อย:**

```
Value   Protocol
-------------------------
1       ICMP
2       IGMP
6       TCP
17      UDP
41      IPv6 (IPv6 over IPv4 - tunnel)
47      GRE
50      ESP (IPsec)
51      AH (IPsec)
89      OSPF
```

**การใช้งาน:**

- ปลายทางใช้ Protocol field เพื่อส่ง data ไปยัง Layer 4 ที่ถูกต้อง
- Protocol = 6 → ส่งไป TCP
- Protocol = 17 → ส่งไป UDP
- Protocol = 1 → ส่งไป ICMP

---

#### 10. Header Checksum (16 bits)

**คำจำกัดความ:**

- ตรวจสอบความถูกต้องของ **header**
- **ไม่ตรวจสอบ data** (เฉพาะ header เท่านั้น)

**การทำงาน:**

1. **ผู้ส่ง:**
    
    - คำนวณ checksum จาก header
    - ใส่ใน Header Checksum field
2. **Router (ทุก hop):**
    
    - คำนวณ checksum ใหม่
    - เปรียบเทียบกับ Header Checksum
    - ถ้าไม่ตรง → **drop packet**
    - ถ้าตรง → update TTL (และ checksum ใหม่)
3. **ปลายทาง:**
    
    - ตรวจสอบ checksum ครั้งสุดท้าย

**หมายเหตุ:**

- ทุก Router ต้องคำนวณ checksum ใหม่ (เพราะ TTL เปลี่ยน)
- IPv6 **ไม่มี** header checksum (ลด overhead)

---

#### 11. Source IP Address (32 bits)

**คำจำกัดความ:**

- IP address ของ**ผู้ส่ง**

**รูปแบบ:**

```
32 bits = 4 bytes = 4 octets

Binary:  11000000.10101000.00000001.00001010
Decimal: 192     .168     .1       .10
```

**การใช้งาน:**

- ปลายทางใช้ Source IP เพื่อ**ตอบกลับ**
- Router ใช้ในการ logging, filtering, etc.

---

#### 12. Destination IP Address (32 bits)

**คำจำกัดความ:**

- IP address ของ**ปลายทาง**

**การใช้งาน:**

- Router ใช้ Destination IP เพื่อ**หาเส้นทาง** (routing)
- ค้นหาใน routing table
- Forward packet ไปยัง next hop

---

#### 13. Options (0-40 bytes) - ไม่ค่อยใช้

**คำจำกัดความ:**

- ฟิลด์เสริม (optional)
- ใช้สำหรับ testing, debugging, security

**ตัวอย่าง Options:**

- **Source Route** - กำหนดเส้นทางที่ packet ต้องผ่าน
- **Record Route** - บันทึก IP ของ router ที่ผ่าน
- **Timestamp** - บันทึกเวลาที่ผ่านแต่ละ router

**หมายเหตุ:**

- Options **ไม่ค่อยใช้** ในปัจจุบัน
- มี overhead สูง
- บาง Router ปิดการรองรับ Options (security)

---

### IPv4 Header Example

**ตัวอย่าง Header (hex):**

```
45 00 00 3c 1c 46 40 00 40 06 b1 e6 c0 a8 01 0a c0 a8 01 01

Breakdown:
45       = Version 4, IHL 5 (20 bytes header)
00       = ToS/DSCP 0 (best effort)
00 3c    = Total Length 60 bytes
1c 46    = Identification 7238
40 00    = Flags DF=1, Fragment Offset 0
40       = TTL 64
06       = Protocol TCP (6)
b1 e6    = Header Checksum
c0 a8 01 0a = Source IP 192.168.1.10
c0 a8 01 01 = Destination IP 192.168.1.1
```

---

## 8.3 IPv6 Packet Header (ส่วนหัวแพ็กเก็ต IPv6)

### IPv6 Overview

**คำจำกัดความ:**

- รุ่นใหม่ของ IP protocol
- แก้ปัญหา IPv4 address exhaustion

**คุณสมบัติ:**

- **128-bit address** (vs 32-bit ใน IPv4)
- **Simplified header** (40 bytes คงที่)
- **No broadcast** (ใช้ multicast แทน)
- **Built-in security** (IPsec)
- **Better QoS** support
- **Auto-configuration**

**Address Space:**

```
IPv4: 2^32 = 4.3 billion addresses
IPv6: 2^128 = 340 undecillion addresses
      (340,282,366,920,938,463,463,374,607,431,768,211,456)
```

---

### IPv6 Packet Structure

```
+-----------------+----------------------+
| IPv6 Header     | Extension Headers    | Data (Payload)
| 40 bytes (fixed)| (optional)           |
+-----------------+----------------------+
```

---

### IPv6 Header Fields

**Header: 40 bytes (ทุก packet)**

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version| Traffic Class |           Flow Label                  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Payload Length        |  Next Header  |   Hop Limit   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
+                                                               +
|                                                               |
+                         Source Address                        +
|                                                               |
+                                                               +
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
+                                                               +
|                                                               |
+                      Destination Address                      +
|                                                               |
+                                                               +
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

---

#### 1. Version (4 bits)

**ค่า:**

- **6** = `0110` (IPv6)

---

#### 2. Traffic Class (8 bits)

**คำจำกัดความ:**

- เหมือน **ToS/DSCP** ใน IPv4
- ใช้สำหรับ QoS

**โครงสร้าง:**

```
+-------+---+
| DSCP  |ECN|
+-------+---+
  6 bits  2 bits
```

---

#### 3. Flow Label (20 bits)

**คำจำกัดความ:**

- กำหนด **flow** ของ traffic
- Packets ใน flow เดียวกันได้รับการปฏิบัติเหมือนกัน

**การใช้งาน:**

- **Real-time traffic** (Voice, Video)
- Router สามารถจดจำ flow และให้ QoS เหมือนกัน
- ไม่ต้อง inspect ลึกทุก packet

**ตัวอย่าง:**

```
Video streaming:
All packets from same video session = same Flow Label
Router รู้ว่าเป็น flow เดียวกัน → apply QoS policy เดียวกัน
```

---

#### 4. Payload Length (16 bits)

**คำจำกัดความ:**

- ความยาวของ **payload** (ไม่รวม header)
- หน่วยเป็น bytes

**ค่า:**

- **Maximum:** 65,535 bytes

**เปรียบเทียบ IPv4:**

```
IPv4: Total Length = Header + Data
IPv6: Payload Length = Data only (ไม่รวม header 40 bytes)
```

**Jumbo Payload:**

- ถ้า Payload > 65,535 bytes
- ใช้ **Hop-by-Hop Options** extension header
- รองรับ payload ถึง 4 GB

---

#### 5. Next Header (8 bits)

**คำจำกัดความ:**

- บอกประเภทของ **header ถัดไป**
- อาจเป็น Transport Layer protocol หรือ Extension Header

**ค่าที่ใช้บ่อย:**

```
Value   Protocol
-------------------------
6       TCP
17      UDP
58      ICMPv6
0       Hop-by-Hop Options
43      Routing
44      Fragment
50      ESP (IPsec)
51      AH (IPsec)
59      No Next Header
```

**ตัวอย่าง:**

```
IPv6 Header (Next Header = 6) → TCP Segment

IPv6 Header (Next Header = 43) → Routing Header (Next Header = 6) → TCP
```

---

#### 6. Hop Limit (8 bits)

**คำจำกัดความ:**

- เหมือน **TTL** ใน IPv4
- จำนวน hops สูงสุด

**การทำงาน:**

- เริ่มต้นที่ค่าหนึ่ง (64, 128, 255)
- ทุก Router → Hop Limit **-1**
- Hop Limit = 0 → Drop packet, ส่ง ICMPv6 Time Exceeded

---

#### 7. Source Address (128 bits)

**คำจำกัดความ:**

- IPv6 address ของผู้ส่ง
- **128 bits = 16 bytes**

**รูปแบบ:**

```
2001:0db8:85a3:0000:0000:8a2e:0370:7334

หรือย่อได้:
2001:db8:85a3::8a2e:370:7334
```

---

#### 8. Destination Address (128 bits)

**คำจำกัดความ:**

- IPv6 address ของปลายทาง
- **128 bits = 16 bytes**

---

### IPv6 Extension Headers

**คำจำกัดความ:**

- Headers เสริม (optional)
- ใส่ระหว่าง IPv6 header กับ payload
- แต่ละ extension header มี **Next Header** field บอกว่ามีอะไรต่อ

**ลำดับที่แนะนำ:**

```
IPv6 Header
  → Hop-by-Hop Options
  → Destination Options
  → Routing Header
  → Fragment Header
  → Authentication Header (AH)
  → Encapsulating Security Payload (ESP)
  → Destination Options
  → Upper Layer (TCP/UDP/ICMPv6)
```

**ตัวอย่าง Extension Headers:**

**1. Hop-by-Hop Options (0)**

- Router ทุกตัวต้อง process
- เช่น Jumbo Payload

**2. Routing Header (43)**

- กำหนดเส้นทางที่ packet ต้องผ่าน (Source Routing)

**3. Fragment Header (44)**

- ใช้สำหรับ fragmentation
- **IPv6 routers ไม่ fragment** - เฉพาะ source ที่ fragment

**4. Destination Options (60)**

- ปลายทางอ่าน

**5. Authentication Header (51) และ ESP (50)**

- IPsec - เข้ารหัสและ authentication

---

### IPv6 vs IPv4 Header Comparison

```
Feature              IPv4                  IPv6
------------------------------------------------------------------------
Header Size          20-60 bytes           40 bytes (fixed)
Address Size         32 bits               128 bits
Checksum             Yes                   No (ลบออก)
Options              In header             Extension headers
Fragmentation        Routers can fragment  Only source fragments
TTL/Hop Limit        TTL                   Hop Limit
Broadcast            Yes                   No (use multicast)
Header fields        12-13 fields          8 fields (simplified)
QoS                  ToS                   Traffic Class + Flow Label
------------------------------------------------------------------------
```

**ข้อดีของ IPv6 Header:**

- ✅ **Simpler** - น้อยกว่า, ขนาดคงที่
- ✅ **Faster processing** - Router ไม่ต้องคำนวณ checksum
- ✅ **Extensible** - Extension headers
- ✅ **No fragmentation** by routers - ลด overhead

---

## 8.4 How a Host Routes (การกำหนดเส้นทางของโฮสต์)

### Host Routing Decision

**เมื่อ Host ต้องการส่ง packet:**

#### Decision Process (กระบวนการตัดสินใจ)

**คำถาม:** ปลายทางอยู่ใน**เครือข่ายเดียวกัน**หรือไม่?

**1. ปลายทางอยู่ใน Local Network (Same Network):**

```
Source: 192.168.1.10/24
Dest:   192.168.1.20

Network ของ Source: 192.168.1.0/24
Network ของ Dest:   192.168.1.0/24 (เดียวกัน!)

→ ส่งตรงไปยัง Destination (Direct delivery)
```

**กระบวนการ:**

1. ใช้ **ARP** หา MAC address ของ destination
2. Encapsulate packet ใน frame
3. ส่งไปยัง destination โดยตรง (Layer 2)

**2. ปลายทางอยู่ใน Remote Network (Different Network):**

```
Source: 192.168.1.10/24
Dest:   10.0.0.5

Network ของ Source: 192.168.1.0/24
Network ของ Dest:   10.0.0.0/8 (ต่างกัน!)

→ ส่งไปยัง Default Gateway (Router)
```

**กระบวนการ:**

1. ใช้ **ARP** หา MAC address ของ **Default Gateway** (Router)
2. Encapsulate packet (Destination IP ยังเป็น 10.0.0.5)
3. ส่ง frame ไปยัง Default Gateway (MAC ของ Router)
4. Router จะ forward ต่อไป

---

### Default Gateway

**คำจำกัดความ:**

- **Router** ที่ host ใช้ส่ง packet ไปยัง remote networks
- เรียกอีกชื่อว่า **Default Route**

**การตั้งค่า:**

```
Windows:
Default Gateway: 192.168.1.1

Linux:
Default Gateway: 192.168.1.1
หรือ
Default Route: 0.0.0.0/0 via 192.168.1.1

Cisco:
ip default-gateway 192.168.1.1
หรือ (Router)
ip route 0.0.0.0 0.0.0.0 192.168.1.1
```

**ตัวอย่าง:**

```
Topology:
[PC1: 192.168.1.10/24] ---[Switch]--- [Router: 192.168.1.1] ---[Internet]
                                       Default Gateway

PC1 ต้องการส่งไปยัง 8.8.8.8:
1. PC1 เช็ค: 8.8.8.8 อยู่ใน 192.168.1.0/24 หรือไม่? → ไม่ใช่!
2. PC1 ส่งไปยัง Default Gateway (192.168.1.1)
3. Router forward ต่อไปยัง Internet
```

---

### Host Routing Table

**คำจำกัดความ:**

- ตารางที่เก็บข้อมูล**เส้นทาง**
- Host ใช้ตัดสินใจว่าจะส่ง packet ไปไหน

**ดู Routing Table:**

**Windows:**

```cmd
route print
หรือ
netstat -r
```

**Linux/macOS:**

```bash
route -n
หรือ
ip route show
หรือ
netstat -rn
```

**ตัวอย่าง Output (Windows):**

```
Network Destination    Netmask          Gateway       Interface
0.0.0.0                0.0.0.0          192.168.1.1   192.168.1.10
127.0.0.0              255.0.0.0        On-link       127.0.0.1
192.168.1.0            255.255.255.0    On-link       192.168.1.10
192.168.1.10           255.255.255.255  On-link       192.168.1.10
192.168.1.255          255.255.255.255  On-link       192.168.1.10
```

**อธิบาย:**

```
0.0.0.0/0 via 192.168.1.1
  → Default route (ทุก destination ที่ไม่ match routes อื่น → ส่งไปยัง 192.168.1.1)

192.168.1.0/24 via On-link
  → Local network (ส่งตรงไปได้, ไม่ต้องผ่าน gateway)

127.0.0.0/8 via On-link
  → Loopback (localhost)
```

---

### Routing Example

**Scenario 1: Local Communication**

```
PC1: 192.168.1.10/24
PC2: 192.168.1.20/24

PC1 ping PC2:

1. PC1 checks: Is 192.168.1.20 in same network?
   My network: 192.168.1.0/24 (192.168.1.10 AND 255.255.255.0)
   Dest network: 192.168.1.0/24 (192.168.1.20 AND 255.255.255.255.0)
   → Same network! ✅

2. PC1 uses ARP to find MAC of 192.168.1.20
   ARP Request: Who has 192.168.1.20? Tell 192.168.1.10

3. PC2 replies: 192.168.1.20 is at AA:BB:CC:DD:EE:FF

4. PC1 sends ICMP Echo Request:
   Frame:
     Dest MAC: AA:BB:CC:DD:EE:FF (PC2)
     Src MAC:  11:22:33:44:55:66 (PC1)
   Packet:
     Dest IP: 192.168.1.20
     Src IP:  192.168.1.10

5. PC2 receives and replies
```

**Scenario 2: Remote Communication**

```
PC1: 192.168.1.10/24, Gateway: 192.168.1.1
Server: 8.8.8.8

PC1 ping 8.8.8.8:

1. PC1 checks: Is 8.8.8.8 in same network?
   My network: 192.168.1.0/24
   Dest network: 8.0.0.0/8
   → Different network! ❌

2. PC1 looks up routing table:
   0.0.0.0/0 via 192.168.1.1 → Use default gateway

3. PC1 uses ARP to find MAC of 192.168.1.1 (Router)
   ARP Request: Who has 192.168.1.1?

4. Router replies: 192.168.1.1 is at AA:AA:AA:AA:AA:AA

5. PC1 sends ICMP Echo Request:
   Frame:
     Dest MAC: AA:AA:AA:AA:AA:AA (Router) ← MAC of Gateway
     Src MAC:  11:22:33:44:55:66 (PC1)
   Packet:
     Dest IP: 8.8.8.8 ← Still destination server
     Src IP:  192.168.1.10

6. Router receives frame:
   - Removes Data Link header (MAC addresses)
   - Reads Destination IP: 8.8.8.8
   - Looks up routing table
   - Forwards to next hop

7. Process repeats at each router until reaches 8.8.8.8
```

---

### Key Differences: Local vs Remote

```
Aspect              Local Delivery          Remote Delivery
------------------------------------------------------------------------
Destination IP      Same network            Different network
MAC Address (Dest)  Destination's MAC       Gateway's MAC
ARP target          Destination IP          Gateway IP
Routers involved    0                       1+
Hops                1 (direct)              Multiple
------------------------------------------------------------------------
```

---

## 8.5 Router Routing Tables (ตารางการกำหนดเส้นทางของเราเตอร์)

### Router Routing Table Overview

**คำจำกัดความ:**

- ตารางที่ Router ใช้**ตัดสินใจ**ว่าจะ forward packet ไปทางไหน
- เก็บข้อมูล**เครือข่าย**และ**วิธีการไป**

**ข้อมูลใน Routing Table:**

- **Network destination** (Network address)
- **Subnet mask** (Prefix length)
- **Next-hop** (Gateway) - Router ตัวถัดไป
- **Exit interface** - Interface ที่ส่งออก
- **Metric** - ระยะทาง/ต้นทุน
- **Administrative Distance** - ความน่าเชื่อถือของ routing source
- **Route source** - Static, RIP, OSPF, EIGRP, BGP, etc.

---

### Viewing Routing Table (Cisco)

**คำสั่ง:**

```
Router# show ip route
```

**ตัวอย่าง Output:**

```
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       + - replicated route, % - next hop override

Gateway of last resort is 192.168.1.1 to network 0.0.0.0

S*   0.0.0.0/0 [1/0] via 192.168.1.1
     10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C       10.0.0.0/24 is directly connected, GigabitEthernet0/0
L       10.0.0.1/32 is directly connected, GigabitEthernet0/0
     192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.1.0/24 is directly connected, GigabitEthernet0/1
L       192.168.1.1/32 is directly connected, GigabitEthernet0/1
D    172.16.0.0/16 [90/2816] via 192.168.1.2, 00:05:23, GigabitEthernet0/1
```

---

### Route Entry Components

**รูปแบบ:**

```
S*   0.0.0.0/0 [1/0] via 192.168.1.1, GigabitEthernet0/1
^    ^         ^     ^                 ^
|    |         |     |                 |
|    |         |     |                 Exit Interface
|    |         |     Next-hop IP
|    |         [AD/Metric]
|    Network/Prefix Length
Route Source Code
```

**อธิบายแต่ละส่วน:**

#### 1. Route Source Code

**ย่อมาจาก:**

- **S** = Static route
- **C** = Connected route (directly connected network)
- **L** = Local route (IP address of interface)
- **D** = EIGRP
- **O** = OSPF
- **R** = RIP
- **B** = BGP
- **S*** = Default static route

#### 2. Network Destination / Prefix Length

**ตัวอย่าง:**

```
10.0.0.0/24
172.16.0.0/16
0.0.0.0/0 (default route - match ทุก destination)
```

#### 3. Administrative Distance (AD)

**คำจำกัดความ:**

- ความน่าเชื่อถือของ **routing source**
- ค่า**ต่ำ** = น่าเชื่อถือ**มากกว่า**

**ค่า AD มาตรฐาน:**

```
Route Source              AD Value
----------------------------------------
Connected interface       0
Static route              1
EIGRP summary route       5
External BGP (eBGP)       20
Internal EIGRP            90
IGRP                      100
OSPF                      110
IS-IS                     115
RIP                       120
External EIGRP            170
Internal BGP (iBGP)       200
Unknown/Unreachable       255
```

**ตัวอย่าง:**

```
D    172.16.0.0/16 [90/2816]
                    ^^
                    AD = 90 (EIGRP)

S    192.168.10.0/24 [1/0]
                      ^
                      AD = 1 (Static)
```

**การใช้ AD:**

- ถ้ามีหลาย routes ไปยัง destination เดียวกัน
- Router เลือก route ที่มี **AD ต่ำกว่า**

**ตัวอย่าง:**

```
10.0.0.0/24 ได้รับจาก 2 sources:
- Static route: AD = 1
- OSPF: AD = 110

Router เลือก Static route (AD ต่ำกว่า)
```

#### 4. Metric

**คำจำกัดความ:**

- **ต้นทุน**หรือ**ระยะทาง**ไปยัง destination
- ใช้เปรียบเทียบ routes จาก**routing protocol เดียวกัน**

**Metric แต่ละ Protocol:**

```
Protocol    Metric
--------------------------------------
RIP         Hop count (จำนวน routers)
OSPF        Cost (based on bandwidth)
EIGRP       Composite (bandwidth, delay, load, reliability)
BGP         Path attributes (AS-PATH length, etc.)
Static      Default = 0 (or configured)
```

**ตัวอย่าง:**

```
D    172.16.0.0/16 [90/2816]
                       ^^^^
                       Metric = 2816 (EIGRP metric)

O    10.10.0.0/16 [110/65]
                      ^^
                      Metric = 65 (OSPF cost)
```

**การใช้ Metric:**

- ถ้ามีหลาย routes ไปยัง destination เดียวกัน
- **จาก routing protocol เดียวกัน** (AD เท่ากัน)
- Router เลือก route ที่มี **Metric ต่ำกว่า**

#### 5. Next-Hop

**คำจำกัดความ:**

- **IP address ของ Router ตัวถัดไป**
- ส่ง packet ไปยัง next-hop นี้

**ตัวอย่าง:**

```
S    10.0.0.0/8 via 192.168.1.254
                ^^^^^^^^^^^^^^^^
                Next-hop IP

D    172.16.0.0/16 via 10.0.0.2
```

**หมายเหตุ:**

- **Connected routes** ไม่มี next-hop (directly connected)
- **Local routes** ไม่มี next-hop

#### 6. Exit Interface

**คำจำกัดความ:**

- **Interface** ที่ใช้ส่ง packet ออก

**ตัวอย่าง:**

```
C    10.0.0.0/24 is directly connected, GigabitEthernet0/0
S    192.168.10.0/24 via 192.168.1.254, GigabitEthernet0/1
D    172.16.0.0/16 [90/2816] via 10.0.0.2, 00:05:23, GigabitEthernet0/0
```

---

### Types of Routes

#### 1. Connected Routes (C)

**คำจำกัดความ:**

- เครือข่ายที่**เชื่อมต่อโดยตรง**กับ interface ของ Router
- เกิดขึ้น**อัตโนมัติ**เมื่อ interface มี IP และ up/up

**ตัวอย่าง:**

```
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip address 10.0.0.1 255.255.255.0
Router(config-if)# no shutdown

Routing table จะมี:
C    10.0.0.0/24 is directly connected, GigabitEthernet0/0
```

**ลักษณะ:**

- **AD = 0** (น่าเชื่อถือที่สุด)
- ไม่มี next-hop (directly connected)
- Auto-added และ auto-removed

#### 2. Local Routes (L)

**คำจำกัดความ:**

- **IP address ของ interface นั้นเอง** (/32)
- ระบุว่า IP นี้เป็นของ Router

**ตัวอย่าง:**

```
Interface Gi0/0: 10.0.0.1/24

Routing table:
C    10.0.0.0/24 is directly connected, GigabitEthernet0/0
L    10.0.0.1/32 is directly connected, GigabitEthernet0/0
     ^^^^^^^^^^
     /32 = host route (specific IP)
```

**วัตถุประสงค์:**

- Router รู้ว่า IP นี้เป็นของตัวเอง
- ไม่ forward packet ที่ destination = IP ของตัวเอง

#### 3. Static Routes (S)

**คำจำกัดความ:**

- Routes ที่**กำหนดด้วยตนเอง**
- Administrator ต้องบอก Router ว่าจะไปเครือข่ายไหนยังไง

**Syntax:**

```
Router(config)# ip route <network> <mask> <next-hop-ip | exit-interface>
```

**ตัวอย่าง:**

```
Router(config)# ip route 192.168.10.0 255.255.255.0 192.168.1.254

Routing table:
S    192.168.10.0/24 [1/0] via 192.168.1.254
```

**ข้อดี:**

- ✅ **Simple** - เข้าใจง่าย
- ✅ **Predictable** - ทราบเส้นทางแน่นอน
- ✅ **Secure** - ไม่มี routing updates (ไม่มีคนดัก)
- ✅ **Low resource** - ไม่กิน CPU, bandwidth

**ข้อเสีย:**

- ❌ **No automatic failover** - ถ้า link เสีย ต้องเปลี่ยนเอง
- ❌ **Scalability** - เครือข่ายใหญ่ยาก
- ❌ **Administrative overhead** - ต้องกำหนดทุกอย่าง

**เมื่อไหร่ใช้ Static Routes:**

- เครือข่ายเล็ก
- Stub networks (เครือข่ายที่มีทางเดียว)
- Default routes
- Backup routes

#### 4. Dynamic Routes (D, O, R, B, etc.)

**คำจำกัดความ:**

- Routes ที่เรียนรู้จาก **routing protocols**
- Router แลกเปลี่ยนข้อมูล routing กับ Router อื่น

**Routing Protocols:**

- **RIP** (Routing Information Protocol) - เก่า, hop count, max 15 hops
- **EIGRP** (Enhanced Interior Gateway Routing Protocol) - Cisco proprietary
- **OSPF** (Open Shortest Path First) - Standard, link-state
- **BGP** (Border Gateway Protocol) - Internet, AS-to-AS

**ตัวอย่าง:**

```
D    172.16.0.0/16 [90/2816] via 10.0.0.2, 00:05:23, GigabitEthernet0/0
O    192.168.20.0/24 [110/65] via 192.168.1.2, 00:10:15, GigabitEthernet0/1
```

**ข้อดี:**

- ✅ **Automatic** - เรียนรู้และปรับเส้นทางอัตโนมัติ
- ✅ **Scalable** - เหมาะกับเครือข่ายใหญ่
- ✅ **Failover** - เปลี่ยนเส้นทางอัตโนมัติเมื่อ link เสีย
- ✅ **Load balancing** - แบ่ง traffic หลายเส้นทาง

**ข้อเสีย:**

- ❌ **Complex** - ยากกว่า static
- ❌ **Resource intensive** - ใช้ CPU, RAM, bandwidth
- ❌ **Convergence time** - ต้องใช้เวลาปรับตัว

#### 5. Default Route (S* or D* or O*)

**คำจำกัดความ:**

- Route สำหรับ**ทุก destination ที่ไม่ match routes อื่น**
- เรียกว่า **Gateway of Last Resort**
- Network = **0.0.0.0/0** (match ทุกอย่าง)

**Static Default Route:**

```
Router(config)# ip route 0.0.0.0 0.0.0.0 192.168.1.1

Routing table:
S*   0.0.0.0/0 [1/0] via 192.168.1.1
     ^
     * = Candidate default route
```

**การใช้งาน:**

- **Stub networks** - เครือข่ายที่มีทางเดียวออกไป
- **Internet connection** - ส่งทุกอย่างที่ไม่รู้จักไป Internet

**ตัวอย่าง:**

```
[Branch Office] --[Router]-- [ISP] --[Internet]

Branch Router:
ip route 0.0.0.0 0.0.0.0 <ISP IP>

Meaning: ทุก destination ที่ไม่รู้จัก → ส่งไป ISP
```

---

### Route Selection Process

**เมื่อ Router รับ packet:**

**1. อ่าน Destination IP**

```
Packet: Dest IP = 172.16.10.5
```

**2. ค้นหาใน Routing Table**

- หา route ที่ **match** กับ Destination IP

**3. Longest Prefix Match**

- ถ้ามีหลาย routes match
- เลือก route ที่มี **prefix ยาวที่สุด** (specific มากที่สุด)

**ตัวอย่าง:**

```
Destination: 172.16.10.5

Routing table:
172.16.0.0/16   via 10.0.0.1
172.16.10.0/24  via 10.0.0.2  ← More specific!
0.0.0.0/0       via 192.168.1.1

Router เลือก: 172.16.10.0/24 (longest prefix /24)
```

**4. Administrative Distance (ถ้า prefix เท่ากัน)**

- เลือก route ที่มี **AD ต่ำกว่า**

**ตัวอย่าง:**

```
Destination: 10.0.0.0/8

Routes:
S    10.0.0.0/8 [1/0] via 192.168.1.1    ← Lower AD
O    10.0.0.0/8 [110/65] via 192.168.1.2

Router เลือก: Static route (AD = 1)
```

**5. Metric (ถ้า AD เท่ากัน)**

- เลือก route ที่มี **Metric ต่ำกว่า**

**ตัวอย่าง:**

```
Destination: 192.168.20.0/24

Routes (both OSPF):
O    192.168.20.0/24 [110/20] via 10.0.0.1  ← Lower metric
O    192.168.20.0/24 [110/50] via 10.0.0.2

Router เลือก: via 10.0.0.1 (Metric = 20)
```

**6. Load Balancing (ถ้า Metric เท่ากัน)**

- ใช้**ทั้งสอง routes** พร้อมกัน (Equal-Cost Multi-Path)

**ตัวอย่าง:**

```
O    192.168.30.0/24 [110/20] via 10.0.0.1
O    192.168.30.0/24 [110/20] via 10.0.0.2

Router ใช้ทั้งสองเส้นทาง (แบ่ง traffic)
```

---

### Routing Table Example

**Topology:**

```
[LAN1: 10.0.0.0/24] ---Gi0/0--- [R1] ---Gi0/1--- [LAN2: 192.168.1.0/24]
                                  |
                                Gi0/2
                                  |
                               [Internet]
```

**R1 Configuration:**

```
interface GigabitEthernet0/0
 ip address 10.0.0.1 255.255.255.0
 no shutdown

interface GigabitEthernet0/1
 ip address 192.168.1.1 255.255.255.0
 no shutdown

interface GigabitEthernet0/2
 ip address 203.0.113.10 255.255.255.252
 no shutdown

ip route 0.0.0.0 0.0.0.0 203.0.113.9
```

**Routing Table:**

```
Router# show ip route

Gateway of last resort is 203.0.113.9 to network 0.0.0.0

S*   0.0.0.0/0 [1/0] via 203.0.113.9
     10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C       10.0.0.0/24 is directly connected, GigabitEthernet0/0
L       10.0.0.1/32 is directly connected, GigabitEthernet0/0
     192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.1.0/24 is directly connected, GigabitEthernet0/1
L       192.168.1.1/32 is directly connected, GigabitEthernet0/1
     203.0.113.0/24 is variably subnetted, 2 subnets, 2 masks
C       203.0.113.8/30 is directly connected, GigabitEthernet0/2
L       203.0.113.10/32 is directly connected, GigabitEthernet0/2
```

**Routing Decisions:**

```
PC1 (10.0.0.10) → PC2 (192.168.1.10):
  Dest: 192.168.1.10
  Match: C 192.168.1.0/24 → Exit Gi0/1 (directly connected)

PC1 (10.0.0.10) → 8.8.8.8:
  Dest: 8.8.8.8
  No specific match
  Match: S* 0.0.0.0/0 → via 203.0.113.9, Exit Gi0/2
```

---

## Summary (สรุป)

Module 8 นี้เราได้เรียนรู้:

1. ✅ **Network Layer Functions** - Addressing, Routing, Encapsulation, Best Effort
2. ✅ **IPv4 Packet Header** - Version, TTL, Protocol, Source/Dest IP, Checksum, etc.
3. ✅ **IPv6 Packet Header** - 128-bit address, Simplified header, Extension headers
4. ✅ **Host Routing** - Local vs Remote delivery, Default Gateway
5. ✅ **Router Routing Tables** - Connected, Local, Static, Dynamic routes
6. ✅ **Route Selection** - Longest Prefix Match, Administrative Distance, Metric

**สิ่งสำคัญที่ต้องจำ:**

- Network Layer = Layer 3, PDU = Packet
- IP = Connectionless, Best Effort, Media Independent
- IPv4 = 32-bit, IPv6 = 128-bit
- TTL (IPv4) / Hop Limit (IPv6) = ป้องกัน routing loop
- Host: Local delivery = ส่งตรง, Remote delivery = ส่งผ่าน Default Gateway
- Router: ใช้ Routing Table เลือก best path
- Route Selection: Longest Prefix → Lowest AD → Lowest Metric
- Connected (C) > Static (S) > Dynamic (D/O/R) based on AD

**Next Module:** Module 9 - Address Resolution

---

**[ไฟล์ Module 8 สมบูรณ์แล้ว!]**