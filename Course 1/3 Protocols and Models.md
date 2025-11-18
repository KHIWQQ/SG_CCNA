# CCNA Course 1 - Module 3: Protocols and Models

## โปรโตคอลและโมเดล

---

## 3.1 Rules of Communication (กฎของการสื่อสาร)

### Protocol (โปรโตคอล)

**คำจำกัดความ:**

- ชุดของกฎและข้อตกลงที่กำหนดวิธีการสื่อสารข้อมูลในเครือข่าย
- เปรียบเสมือนภาษาและมารยาทในการสื่อสาร

### Protocol Requirements (ข้อกำหนดของโปรโตคอล)

#### 1. Message Encoding (การเข้ารหัสข้อความ)

- การแปลงข้อมูลเป็นรูปแบบที่ส่งได้ผ่านเครือข่าย
- ตัวอย่าง: Text → Binary, Voice → Digital

#### 2. Message Formatting and Encapsulation

- การจัดรูปแบบและห่อหุ้มข้อมูล
- เพิ่ม Header และ Trailer
- ระบุ Source และ Destination

#### 3. Message Size (ขนาดข้อความ)

- กำหนดขนาดที่เหมาะสม
- ใหญ่เกินไป → แบ่งเป็น Segments
- เล็กเกินไป → Overhead สูง

#### 4. Message Timing (เวลาของข้อความ)

**Flow Control:**

- ควบคุมอัตราการส่งข้อมูล
- ป้องกันผู้รับรับไม่ทัน

**Response Timeout:**

- เวลารอการตอบกลับ
- ถ้าหมดเวลา → ส่งใหม่

**Access Method:**

- วิธีการเข้าถึงสื่อ
- ใครส่งก่อน-หลัง

#### 5. Message Delivery Options

**Unicast (ยูนิคาสต์):**

- ส่งแบบ 1 ต่อ 1
- หนึ่งผู้ส่ง → หนึ่งผู้รับ
- ตัวอย่าง: Email, Web browsing

**Multicast (มัลติคาสต์):**

- ส่งแบบ 1 ต่อกลุ่ม
- หนึ่งผู้ส่ง → กลุ่มผู้รับ
- ตัวอย่าง: Video streaming, IPTV

**Broadcast (บรอดคาสต์):**

- ส่งแบบ 1 ต่อทุกคน
- หนึ่งผู้ส่ง → ทุกคนในเครือข่าย
- ตัวอย่าง: DHCP Discovery, ARP Request

---

## 3.2 Protocol Suites (ชุดโปรโตคอล)

### Protocol Suite

**คำจำกัดความ:**

- กลุ่มของโปรโตคอลที่ทำงานร่วมกัน
- แต่ละ protocol รับผิดชอบงานต่างกัน

### TCP/IP Protocol Suite

**ประวัติ:**

- พัฒนาโดย Department of Defense (DoD) สหรัฐอเมริกา
- ชุดโปรโตคอลมาตรฐานของอินเทอร์เน็ต
- Open standard

**Layers ของ TCP/IP:**

1. Application Layer
2. Transport Layer
3. Internet Layer
4. Network Access Layer

---

## 3.3 OSI Reference Model (โมเดลอ้างอิง OSI)

### OSI Model

**ย่อมาจาก:** Open Systems Interconnection  
**พัฒนาโดย:** ISO (International Organization for Standardization)  
**จำนวน Layers:** 7 ชั้น

### ทำไมต้องมี Layered Model?

**ข้อดี:**

1. **Modularity** - แบ่งงานเป็นส่วนๆ
2. **Interoperability** - อุปกรณ์ต่างผู้ผลิตทำงานร่วมกันได้
3. **Standardization** - มาตรฐานเดียวกัน
4. **Easier Troubleshooting** - แก้ปัญหาง่ายขึ้น
5. **Faster Development** - พัฒนาเร็วขึ้น

### 7 Layers of OSI Model

```
Layer 7 - Application      (ใกล้ User)
Layer 6 - Presentation     ↑
Layer 5 - Session          |
Layer 4 - Transport        |
Layer 3 - Network          |
Layer 2 - Data Link        |
Layer 1 - Physical         ↓ (ใกล้สื่อกลาง)
```

---

## Layer 7: Application Layer (ชั้นแอปพลิเคชัน)

### หน้าที่:

- ใกล้ผู้ใช้มากที่สุด
- ให้บริการเครือข่ายแก่ applications
- User interface

### โปรโตคอล:

- **HTTP/HTTPS** - Web browsing (Port 80/443)
- **FTP** - File transfer (Port 20/21)
- **SMTP** - Email sending (Port 25)
- **POP3** - Email receiving (Port 110)
- **IMAP** - Email receiving (Port 143)
- **DNS** - Domain Name System (Port 53)
- **DHCP** - Dynamic IP assignment (Port 67/68)
- **Telnet** - Remote access (Port 23)
- **SSH** - Secure remote access (Port 22)

### PDU: Data

---

## Layer 6: Presentation Layer (ชั้นนำเสนอ)

### หน้าที่:

- **Data Translation** - แปลงรูปแบบข้อมูล
- **Data Encryption** - เข้ารหัสข้อมูล
- **Data Compression** - บีบอัดข้อมูล

### ตัวอย่าง:

- **Image formats:** JPEG, GIF, PNG
- **Video formats:** MPEG, AVI, MP4
- **Text encoding:** ASCII, EBCDIC, Unicode
- **Encryption:** SSL/TLS, AES

### PDU: Data

---

## Layer 5: Session Layer (ชั้นเซสชัน)

### หน้าที่:

- **Session Management** - จัดการ session
- **Establish** - สร้างการเชื่อมต่อ
- **Maintain** - รักษาการเชื่อมต่อ
- **Terminate** - ปิดการเชื่อมต่อ
- **Synchronization** - ซิงโครไนซ์
- **Dialog Control** - ควบคุมการสื่อสาร (Half-duplex/Full-duplex)

### โปรโตคอล:

- **NetBIOS** - Network Basic Input/Output System
- **RPC** - Remote Procedure Call
- **PPTP** - Point-to-Point Tunneling Protocol

### PDU: Data

---

## Layer 4: Transport Layer (ชั้นการขนส่ง)

### หน้าที่:

- **End-to-End Communication** - การสื่อสารแบบ end-to-end
- **Segmentation** - แบ่งข้อมูลเป็น segments
- **Flow Control** - ควบคุมอัตราการส่ง
- **Error Recovery** - แก้ไขข้อผิดพลาด (TCP)
- **Connection Management** - จัดการการเชื่อมต่อ

### โปรโตคอล:

**TCP (Transmission Control Protocol):**

- Connection-oriented
- Reliable delivery
- Flow control
- Error recovery
- Ordered delivery
- ตัวอย่าง: HTTP, FTP, SMTP, SSH

**UDP (User Datagram Protocol):**

- Connectionless
- Unreliable (Best effort)
- No flow control
- Faster than TCP
- ตัวอย่าง: DNS, DHCP, VoIP, Video streaming

### Port Numbers:

- **Well-known:** 0-1023
- **Registered:** 1024-49151
- **Dynamic/Private:** 49152-65535

### PDU: **Segment** (TCP) or **Datagram** (UDP)

---

## Layer 3: Network Layer (ชั้นเครือข่าย)

### หน้าที่:

- **Logical Addressing** - IP address
- **Routing** - การหาเส้นทาง
- **Packet Forwarding** - ส่งต่อ packet
- **Path Selection** - เลือกเส้นทางที่ดีที่สุด

### โปรโตคอล:

- **IP (IPv4, IPv6)** - Internet Protocol
- **ICMP** - Internet Control Message Protocol
- **ARP** - Address Resolution Protocol (IPv4)
- **OSPF** - Open Shortest Path First
- **EIGRP** - Enhanced Interior Gateway Routing Protocol
- **BGP** - Border Gateway Protocol
- **RIP** - Routing Information Protocol

### อุปกรณ์:

- **Router** (หลัก)
- **Layer 3 Switch**

### PDU: **Packet**

---

## Layer 2: Data Link Layer (ชั้นลิงก์ข้อมูล)

### หน้าที่:

- **Physical Addressing** - MAC address
- **Frame** - ห่อหุ้มข้อมูลเป็น frame
- **Error Detection** - ตรวจสอบข้อผิดพลาด (FCS)
- **Media Access Control** - ควบคุมการเข้าถึงสื่อ

### Sub-layers:

**LLC (Logical Link Control) - IEEE 802.2:**

- ติดต่อกับ Network Layer
- Flow control
- Error control

**MAC (Media Access Control):**

- ควบคุมการเข้าถึงสื่อ
- MAC address management
- CSMA/CD (Ethernet)
- CSMA/CA (Wi-Fi)

### โปรโตคอล:

- **Ethernet** (IEEE 802.3)
- **Wi-Fi** (IEEE 802.11)
- **PPP** - Point-to-Point Protocol
- **HDLC** - High-Level Data Link Control
- **Frame Relay**

### อุปกรณ์:

- **Switch** (หลัก)
- **Bridge**
- **NIC (Network Interface Card)**
- **Wireless Access Point**

### PDU: **Frame**

---

## Layer 1: Physical Layer (ชั้นกายภาพ)

### หน้าที่:

- **Transmission of Bits** - ส่งสัญญาณ 0 และ 1
- **Physical Characteristics** - กำหนดลักษณะทางกายภาพ
- **Signal Encoding** - เข้ารหัสสัญญาณ
- **Data Rate** - อัตราการส่งข้อมูล

### ลักษณะที่กำหนด:

- **Mechanical:** Connectors, Pins, Cable types
- **Electrical:** Voltage levels, Signal timing
- **Functional:** Pin assignments
- **Procedural:** Sequence of events

### สื่อกลาง:

- **Copper Cable** - UTP, STP, Coaxial
- **Fiber Optic** - SMF, MMF
- **Wireless** - Radio waves

### โปรโตคอล/มาตรฐาน:

- **Ethernet:** 10BASE-T, 100BASE-TX, 1000BASE-T
- **Wi-Fi:** 802.11a/b/g/n/ac/ax
- **DSL, ISDN**

### อุปกรณ์:

- **Hub**
- **Repeater**
- **Cable**
- **Connector**
- **Modem**

### PDU: **Bits**

---

## Memory Tricks (เทคนิคจำ OSI Model)

### จากบนลงล่าง (Layer 7→1):

**ภาษาอังกฤษ:**

- **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing
- **A**way **P**izza **S**ausage **T**hrow **N**ot **D**o **P**lease

**ภาษาไทย:**

- **อ**ยาก **พ**บ **เ**พื่อน **ที**่ **น**่า **รั**ก **มา**ก

### จากล่างขึ้นบน (Layer 1→7):

**ภาษาอังกฤษ:**

- **P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way
- **P**eople **D**on't **N**eed **T**o **S**ee **P**aul **A**llen

---

## 3.4 TCP/IP Protocol Model

### TCP/IP Model (4 Layers)

```
TCP/IP Model          OSI Model
-----------------     -----------------
Application    ←→     Application (7)
                      Presentation (6)
                      Session (5)
-----------------     -----------------
Transport      ←→     Transport (4)
-----------------     -----------------
Internet       ←→     Network (3)
-----------------     -----------------
Network Access ←→     Data Link (2)
                      Physical (1)
```

### Layer 4: Application Layer

**รวม Layer 5, 6, 7 ของ OSI**

**โปรโตคอล:**

- HTTP, HTTPS, FTP, SMTP, POP3, IMAP
- DNS, DHCP, Telnet, SSH, SNMP, TFTP

**PDU:** Data

### Layer 3: Transport Layer

**เหมือน Layer 4 ของ OSI**

**โปรโตคอล:**

- TCP (Connection-oriented, Reliable)
- UDP (Connectionless, Unreliable)

**PDU:** Segment (TCP) or Datagram (UDP)

### Layer 2: Internet Layer

**เหมือน Layer 3 ของ OSI**

**โปรโตคอล:**

- IP (IPv4, IPv6)
- ICMP (Internet Control Message Protocol)
- ARP (Address Resolution Protocol)
- IGMP (Internet Group Management Protocol)

**PDU:** Packet

### Layer 1: Network Access Layer

**รวม Layer 1 และ 2 ของ OSI**

**โปรโตคอล:**

- Ethernet
- Wi-Fi (802.11)
- PPP (Point-to-Point Protocol)
- Token Ring

**PDU:** Frame/Bits

---

## 3.5 Data Encapsulation (การห่อหุ้มข้อมูล)

### Encapsulation Process (กระบวนการห่อหุ้ม)

**เมื่อส่งข้อมูล (Top-Down):**

```
Application Layer
    ↓
[Data]
    ↓
Transport Layer
    ↓
[TCP/UDP Header][Data] = Segment/Datagram
    ↓
Network Layer
    ↓
[IP Header][Segment] = Packet
    ↓
Data Link Layer
    ↓
[Frame Header][Packet][Frame Trailer] = Frame
    ↓
Physical Layer
    ↓
01010101... = Bits
```

### De-encapsulation Process (กระบวนการคลายการห่อหุ้ม)

**เมื่อรับข้อมูล (Bottom-Up):**

```
Physical Layer
    ↓
Bits → Frame
    ↓
Data Link Layer
    ↓
ถอด Frame Header/Trailer → Packet
    ↓
Network Layer
    ↓
ถอด IP Header → Segment
    ↓
Transport Layer
    ↓
ถอด TCP/UDP Header → Data
    ↓
Application Layer
    ↓
Data → User
```

### PDU (Protocol Data Unit) Summary

|Layer|OSI Model|TCP/IP Model|PDU|
|---|---|---|---|
|7|Application|Application|Data|
|6|Presentation|Application|Data|
|5|Session|Application|Data|
|4|Transport|Transport|Segment (TCP)<br>Datagram (UDP)|
|3|Network|Internet|Packet|
|2|Data Link|Network Access|Frame|
|1|Physical|Network Access|Bits|

### Encapsulation Example

**Scenario:** User เปิดเว็บไปที่ www.example.com

```
Layer 7 (Application):
- HTTP GET request
- Data: "GET /index.html HTTP/1.1"

Layer 4 (Transport):
- Add TCP header
- Source Port: 50000
- Destination Port: 80
- PDU: Segment

Layer 3 (Network):
- Add IP header
- Source IP: 192.168.1.10
- Destination IP: 93.184.216.34
- PDU: Packet

Layer 2 (Data Link):
- Add Ethernet header and trailer
- Source MAC: AA:BB:CC:DD:EE:FF
- Destination MAC: (Gateway's MAC)
- Add FCS (Frame Check Sequence)
- PDU: Frame

Layer 1 (Physical):
- Convert to electrical signals
- PDU: Bits (010101...)
```

---

## 3.6 Data Access (การเข้าถึงข้อมูล)

### Addressing at Different Layers

#### Network Layer - IP Address

**Logical Address:**

- 32-bit (IPv4) or 128-bit (IPv6)
- ระบุ network และ host
- เปลี่ยนแปลงได้ตามตำแหน่ง
- ใช้สำหรับ routing ระหว่างเครือข่าย
- ไม่เปลี่ยนแปลงตลอดการเดินทาง (end-to-end)

**Format:**

```
IPv4: 192.168.1.10
IPv6: 2001:db8::1
```

#### Data Link Layer - MAC Address

**Physical Address:**

- 48-bit (6 bytes)
- Burned-in Address (BIA)
- ควรเป็น unique ทั่วโลก
- ใช้สำหรับส่งข้อมูลในเครือข่ายเดียวกัน
- **เปลี่ยนแปลง** ทุกครั้งที่ผ่าน router (hop-by-hop)

**Format:**

```
XX:XX:XX:XX:XX:XX
XXXX.XXXX.XXXX
```

### Same Network Communication

**Scenario:** PC1 (192.168.1.10) → PC2 (192.168.1.20)

```
1. PC1 ตรวจสอบว่า PC2 อยู่ใน network เดียวกัน
2. PC1 ใช้ ARP หา MAC address ของ PC2
3. PC1 สร้าง frame:
   - Source IP: 192.168.1.10
   - Destination IP: 192.168.1.20
   - Source MAC: PC1's MAC
   - Destination MAC: PC2's MAC
4. Switch forward frame ไปยัง PC2
```

### Different Network Communication

**Scenario:** PC1 (192.168.1.10) → Server (8.8.8.8 - Internet)

```
1. PC1 ตรวจสอบว่า Server อยู่ต่าง network
2. PC1 ส่งไปยัง Default Gateway (Router)
3. PC1 ใช้ ARP หา MAC address ของ Gateway
4. PC1 สร้าง frame:
   - Source IP: 192.168.1.10 (ไม่เปลี่ยน)
   - Destination IP: 8.8.8.8 (ไม่เปลี่ยน)
   - Source MAC: PC1's MAC
   - Destination MAC: Gateway's MAC
5. Router รับ frame:
   - ถอด Layer 2 header
   - อ่าน Destination IP (8.8.8.8)
   - ค้นหา routing table
   - สร้าง frame ใหม่:
     * Source MAC: Router's outgoing interface MAC
     * Destination MAC: Next hop's MAC
   - Forward ไปยัง next hop
6. ทำซ้ำจนถึง destination
```

### Address Resolution

**ARP (Address Resolution Protocol) - IPv4:**

- แปลง IP address → MAC address
- ใช้ใน IPv4
- Broadcast request
- Unicast reply

**NDP (Neighbor Discovery Protocol) - IPv6:**

- แปลง IPv6 address → MAC address
- ใช้ใน IPv6
- Multicast request
- Unicast/Multicast reply

---

## Summary Table: OSI vs TCP/IP

|OSI Layer|TCP/IP Layer|Function|Protocols|PDU|Device|
|---|---|---|---|---|---|
|7 - Application|Application|User services|HTTP, FTP, DNS, SMTP|Data|-|
|6 - Presentation|Application|Data format|SSL/TLS, JPEG, GIF|Data|-|
|5 - Session|Application|Session mgmt|NetBIOS, RPC|Data|-|
|4 - Transport|Transport|End-to-end|TCP, UDP|Segment|-|
|3 - Network|Internet|Routing|IP, ICMP, ARP|Packet|Router|
|2 - Data Link|Network Access|Framing|Ethernet, Wi-Fi|Frame|Switch|
|1 - Physical|Network Access|Bits|Cable, Wireless|Bits|Hub|

---

## Key Terminology (คำศัพท์สำคัญ)

|English|Thai|Layer|
|---|---|---|
|Protocol|โปรโตคอล|All|
|Encapsulation|การห่อหุ้ม|All|
|De-encapsulation|การคลายการห่อหุ้ม|All|
|PDU|หน่วยข้อมูล|All|
|Segment|เซ็กเมนต์|4|
|Packet|แพ็กเก็ต|3|
|Frame|เฟรม|2|
|Bits|บิต|1|
|Header|ส่วนหัว|2-7|
|Trailer|ส่วนท้าย|2|
|Payload|ข้อมูลจริง|All|

---

## Practice Questions (คำถามทบทวน)

1. **OSI Model มีกี่ layers?**
    
    - ตอบ: 7 layers
2. **TCP/IP Model มีกี่ layers?**
    
    - ตอบ: 4 layers
3. **Layer ไหนใช้ IP address?**
    
    - ตอบ: Layer 3 (Network Layer)
4. **Layer ไหนใช้ MAC address?**
    
    - ตอบ: Layer 2 (Data Link Layer)
5. **Router ทำงานที่ Layer ใด?**
    
    - ตอบ: Layer 3 (Network Layer)
6. **Switch ทำงานที่ Layer ใด?**
    
    - ตอบ: Layer 2 (Data Link Layer)
7. **PDU ของ Transport Layer คืออะไร?**
    
    - ตอบ: Segment (TCP) หรือ Datagram (UDP)
8. **TCP และ UDP อยู่ที่ Layer ใด?**
    
    - ตอบ: Layer 4 (Transport Layer)
9. **HTTP ทำงานที่ Layer ใด?**
    
    - ตอบ: Layer 7 (Application Layer)
10. **การห่อหุ้มข้อมูลเรียกว่าอะไร?**
    
    - ตอบ: Encapsulation

---

## Summary (สรุป)

Module 3 นี้เราได้เรียนรู้:

1. ✅ **กฎของการสื่อสาร** - Protocol requirements
2. ✅ **OSI Model** - 7 layers พร้อมหน้าที่ของแต่ละ layer
3. ✅ **TCP/IP Model** - 4 layers
4. ✅ **Data Encapsulation** - การห่อหุ้มข้อมูล
5. ✅ **PDU** - Protocol Data Units ของแต่ละ layer
6. ✅ **Addressing** - IP address และ MAC address
7. ✅ **ความแตกต่างระหว่าง OSI และ TCP/IP Model**

**สิ่งสำคัญที่ต้องจำ:**

- OSI Model = 7 layers (โมเดลอ้างอิง)
- TCP/IP Model = 4 layers (ใช้จริง)
- Encapsulation เกิดเมื่อส่งข้อมูล (Top-Down)
- De-encapsulation เกิดเมื่อรับข้อมูล (Bottom-Up)
- IP address ไม่เปลี่ยนตลอดทาง (End-to-End)
- MAC address เปลี่ยนทุก hop (Hop-by-Hop)

**Next Module:** Module 4 - Physical Layer