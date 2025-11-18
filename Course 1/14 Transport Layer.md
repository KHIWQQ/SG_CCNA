# CCNA Course 1 - Module 14: Transport Layer

## ชั้นการขนส่ง

---

## 14.1 Transport Layer Characteristics (ลักษณะของชั้นการขนส่ง)

### Transport Layer (Layer 4) Overview

**คำจำกัดความ:**

- **ชั้นที่ 4** ของ OSI Model
- อยู่ระหว่าง **Network Layer (L3)** และ **Session Layer (L5)**
- รับผิดชอบการสื่อสาร **end-to-end** ระหว่าง applications

**PDU (Protocol Data Unit):** **Segment** (TCP) หรือ **Datagram** (UDP)

---

### Transport Layer Responsibilities

**หน้าที่หลัก:**

#### 1. Tracking Individual Conversations (ติดตาม conversations)

**คำจำกัดความ:**

- แยก traffic ของ applications ต่างๆ
- ใช้ **Port Numbers** เป็น identifier

**ตัวอย่าง:**

```
PC เปิด:
  - Web Browser → Server:80 (HTTP)
  - Email Client → Server:25 (SMTP)
  - FTP Client → Server:21 (FTP)

Transport Layer ใช้ port numbers แยก conversations:
  PC:50001 ↔ Server:80  (Web)
  PC:50002 ↔ Server:25  (Email)
  PC:50003 ↔ Server:21  (FTP)
```

#### 2. Segmenting Data (แบ่งข้อมูล)

**คำจำกัดความ:**

- แบ่งข้อมูลจาก Application Layer เป็น **segments** ขนาดเล็ก
- เพิ่ม **header** (Source/Destination port, Sequence number, etc.)

**ตัวอย่าง:**

```
Application: ไฟล์ขนาด 100 KB

Transport Layer:
  - แบ่งเป็น segments (ขนาด ~1460 bytes แต่ละ segment)
  - เพิ่ม TCP header (20 bytes)
  - ได้ TCP segment ขนาด 1480 bytes
  - ส่งไปยัง Network Layer

จำนวน segments ≈ 100,000 / 1460 ≈ 69 segments
```

**ข้อดีของการ segment:**

- ✅ **Multiplexing** - หลาย applications ใช้ network พร้อมกัน
- ✅ **Efficient** - ส่งทีละ segment, ไม่ต้องรอให้ครบทั้งหมด
- ✅ **Error Recovery** - ถ้า segment หนึ่งเสีย ส่งใหม่เฉพาะ segment นั้น

#### 3. Reassembling Segments (รวมข้อมูล)

**คำจำกัดความ:**

- รับ segments จาก Network Layer
- เรียง segments ตาม **Sequence Number**
- รวมกลับเป็นข้อมูลเดิม
- ส่งไปยัง Application Layer

**ตัวอย่าง:**

```
Receiver รับ TCP segments:
  Segment 3 (Seq: 2001-3000)
  Segment 1 (Seq: 1-1000)      ← มาไม่เรียง
  Segment 2 (Seq: 1001-2000)

Transport Layer:
  - เรียง segments ตาม Sequence Number
  - รวมเป็นข้อมูล 1-3000 bytes
  - ส่งไปยัง Application
```

#### 4. Identifying Applications (ระบุ applications)

**คำจำกัดความ:**

- ใช้ **Port Numbers** ระบุ application
- แต่ละ application มี port number เฉพาะ

**ตัวอย่าง:**

```
Port Numbers:
  HTTP:  80
  HTTPS: 443
  FTP:   21
  SSH:   22
  SMTP:  25
  DNS:   53
```

---

### Transport Layer Protocols

**2 โปรโตคอลหลัก:**

#### 1. TCP (Transmission Control Protocol)

**ลักษณะ:**

- ✅ **Connection-oriented** - สร้าง connection ก่อนส่ง
- ✅ **Reliable** - รับประกัน delivery
- ✅ **Ordered** - segments มาถึงเรียงตามลำดับ
- ✅ **Error checking** - ตรวจสอบ errors
- ✅ **Flow control** - ควบคุมอัตราการส่ง
- ✅ **Congestion control** - ปรับตาม network congestion

**ใช้สำหรับ:**

- Applications ที่ต้องการความน่าเชื่อถือ
- HTTP, HTTPS, FTP, SMTP, SSH

**Header Size:** 20-60 bytes

#### 2. UDP (User Datagram Protocol)

**ลักษณะ:**

- ❌ **Connectionless** - ไม่สร้าง connection
- ❌ **Unreliable** - ไม่รับประกัน delivery
- ❌ **Unordered** - datagrams อาจมาไม่เรียง
- ❌ **No error recovery** - ไม่ retransmit
- ✅ **Faster** - overhead น้อย
- ✅ **Lower latency** - ไม่ต้องรอ ACK

**ใช้สำหรับ:**

- Applications ที่ต้องการความเร็ว
- DNS, DHCP, TFTP, VoIP, Video streaming

**Header Size:** 8 bytes (คงที่)

---

### TCP vs UDP Comparison

```
Feature              TCP                      UDP
------------------------------------------------------------------------
Connection           Connection-oriented      Connectionless
Reliability          Reliable                 Unreliable
Ordering             Ordered delivery         No ordering
Error Recovery       Yes (retransmit)         No
Flow Control         Yes                      No
Congestion Control   Yes                      No
Header Size          20-60 bytes              8 bytes
Speed                Slower                   Faster
Latency              Higher                   Lower
Overhead             High                     Low
Usage                HTTP, FTP, Email         DNS, VoIP, Streaming
------------------------------------------------------------------------
```

**เมื่อไหร่ใช้ TCP:**

- ✅ ต้องการ **reliability** (ข้อมูลครบถ้วน)
- ✅ ข้อมูล**สำคัญ** (files, emails, web pages)
- ✅ ยอมให้**ช้ากว่า** เพื่อความถูกต้อง

**เมื่อไหร่ใช้ UDP:**

- ✅ ต้องการ **speed** และ **low latency**
- ✅ ข้อมูล**สูญหายบ้างได้** (video frame หาย 1 frame)
- ✅ **Real-time** applications (voice, video)
- ✅ **Small transactions** (DNS query)

---

### Port Numbers

**คำจำกัดความ:**

- **16-bit number** (0-65535)
- ระบุ **application** หรือ **service**
- เป็นส่วนหนึ่งของ **socket** (IP + Port)

**ประเภท Port Numbers:**

#### 1. Well-Known Ports (0-1023)

**คำจำกัดความ:**

- กำหนดโดย **IANA**
- ใช้สำหรับ **services ที่รู้จักทั่วไป**
- ต้องมี **administrator privileges** ในการใช้

**ตัวอย่าง:**

```
Port  Protocol  Service
----------------------------------------
20    TCP       FTP Data
21    TCP       FTP Control
22    TCP       SSH
23    TCP       Telnet
25    TCP       SMTP (Email sending)
53    TCP/UDP   DNS
67    UDP       DHCP Server
68    UDP       DHCP Client
69    UDP       TFTP
80    TCP       HTTP
110   TCP       POP3 (Email receiving)
143   TCP       IMAP (Email receiving)
161   UDP       SNMP
443   TCP       HTTPS
```

#### 2. Registered Ports (1024-49151)

**คำจำกัดความ:**

- ลงทะเบียนกับ **IANA**
- ใช้โดย **applications ทั่วไป**
- สามารถใช้โดย user applications

**ตัวอย่าง:**

```
Port  Protocol  Service
----------------------------------------
1433  TCP       Microsoft SQL Server
1521  TCP       Oracle Database
3306  TCP       MySQL
3389  TCP       RDP (Remote Desktop)
5060  TCP/UDP   SIP (VoIP)
5432  TCP       PostgreSQL
8080  TCP       HTTP Alternate
8443  TCP       HTTPS Alternate
```

#### 3. Dynamic/Private Ports (49152-65535)

**คำจำกัดความ:**

- **Ephemeral ports** (ชั่วคราว)
- กำหนดโดย **client** อัตโนมัติ
- ใช้เป็น **source port**

**ตัวอย่าง:**

```
Client:50001 → Server:80 (HTTP)
       ^^^^^
    Dynamic port (Source)

Client request:
  Source: 192.168.1.10:50001
  Destination: 8.8.8.8:80

Server response:
  Source: 8.8.8.8:80
  Destination: 192.168.1.10:50001
```

---

### Socket

**คำจำกัดความ:**

- **IP Address + Port Number**
- ระบุ **endpoint** ของ communication

**รูปแบบ:**

```
Socket = IP:Port

ตัวอย่าง:
192.168.1.10:50001
8.8.8.8:80
2001:db8::1:443
```

**Socket Pair:**

```
Connection ระหว่าง Client และ Server:

Client Socket:  192.168.1.10:50001
Server Socket:  8.8.8.8:80

Socket Pair: (192.168.1.10:50001, 8.8.8.8:80)
→ ระบุ connection เฉพาะเจาะจง
```

---

### Multiplexing and Demultiplexing

#### Multiplexing (การรวม)

**คำจำกัดความ:**

- รวม data จาก**หลาย applications** เข้าด้วยกัน
- ส่งผ่าน network เดียวกัน

**ตัวอย่าง:**

```
Applications → Transport Layer → Network

Browser (Port 50001)  ─┐
Email (Port 50002)    ─┼→ Transport Layer → Network
FTP (Port 50003)      ─┘

Transport Layer ใช้ port numbers แยก traffic
```

#### Demultiplexing (การแยก)

**คำจำกัดความ:**

- แยก data จาก network
- ส่งไปยัง **application ที่ถูกต้อง** ตาม port number

**ตัวอย่าง:**

```
Network → Transport Layer → Applications

                            ┌→ Browser (Port 50001)
Network → Transport Layer ─┼→ Email (Port 50002)
                            └→ FTP (Port 50003)

Transport Layer อ่าน destination port แล้วส่งไปยัง application
```

---

## 14.2 TCP (Transmission Control Protocol)

### TCP Overview

**คำจำกัดความ:**

- **Connection-oriented** protocol
- **Reliable** data delivery
- กำหนดใน **RFC 793**

**คุณสมบัติหลัก:**

- ✅ Connection establishment (3-way handshake)
- ✅ Sequencing และ Acknowledgment
- ✅ Flow control (Sliding window)
- ✅ Congestion control
- ✅ Error detection และ recovery

---

### TCP Header

**โครงสร้าง TCP Header (20-60 bytes):**

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Sequence Number                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Acknowledgment Number                      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Data |       |U|A|P|R|S|F|                                   |
| Offset| Rsrvd |R|C|S|S|Y|I|            Window Size            |
|       |       |G|K|H|T|N|N|                                   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           Checksum            |         Urgent Pointer        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options (if any)                           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                             Data                              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

**ฟิลด์สำคัญ:**

#### 1. Source Port (16 bits)

- Port number ของ**ผู้ส่ง**

#### 2. Destination Port (16 bits)

- Port number ของ**ปลายทาง**

#### 3. Sequence Number (32 bits)

**คำจำกัดความ:**

- บอกลำดับของ **byte แรก** ใน segment นี้
- ใช้สำหรับ **ordering** และ **reassembly**

**ตัวอย่าง:**

```
Segment 1: Seq = 1000    (Data: bytes 1000-1499)
Segment 2: Seq = 1500    (Data: bytes 1500-1999)
Segment 3: Seq = 2000    (Data: bytes 2000-2499)

ถ้า Segment 2 หาย:
  Receiver รับ Segment 1, 3
  Receiver รู้ว่าขาด Segment (Seq 1500-1999)
  Receiver ขอ retransmit Segment 2
```

#### 4. Acknowledgment Number (32 bits)

**คำจำกัดความ:**

- บอก**ลำดับ byte ถัดไป**ที่คาดหวัง
- ใช้ยืนยันว่ารับข้อมูลถึง byte ไหนแล้ว

**ตัวอย่าง:**

```
Sender ส่ง: Seq = 1000, Len = 500 bytes (1000-1499)
Receiver ตอบ: ACK = 1500

ความหมาย: "ฉันรับ bytes 1000-1499 ครบแล้ว, ส่ง byte 1500 ต่อมาเลย"
```

#### 5. Data Offset (4 bits)

**คำจำกัดความ:**

- ขนาดของ TCP header
- หน่วยเป็น **32-bit words** (4 bytes)

**คำนวณ:**

```
Header size = Data Offset × 4 bytes

ตัวอย่าง:
Data Offset = 5 → Header = 5 × 4 = 20 bytes (minimum)
Data Offset = 15 → Header = 15 × 4 = 60 bytes (maximum)
```

#### 6. Reserved (6 bits)

- สงวนไว้ (ต้องเป็น 0)

#### 7. Control Flags (6 bits)

**ธง (Flags):**

**URG (Urgent):**

- ข้อมูลเร่งด่วน
- ใช้ Urgent Pointer

**ACK (Acknowledgment):**

- Acknowledgment Number field ถูกต้อง
- ใช้ในทุก segment ยกเว้น SYN แรก

**PSH (Push):**

- บังคับให้ส่ง data ทันที (ไม่รอ buffer เต็ม)
- Application ต้องการ data ทันที

**RST (Reset):**

- Reset connection (บังคับปิด)
- เกิด error หรือ connection ไม่ valid

**SYN (Synchronize):**

- สร้าง connection (3-way handshake)
- Synchronize sequence numbers

**FIN (Finish):**

- ปิด connection (graceful shutdown)

#### 8. Window Size (16 bits)

**คำจำกัดความ:**

- จำนวน bytes ที่ receiver พร้อมรับ
- ใช้สำหรับ **Flow Control**

**ตัวอย่าง:**

```
Receiver ส่ง: Window = 8000

ความหมาย: "ฉันมี buffer 8000 bytes, ส่งได้สูงสุด 8000 bytes"

Sender จะส่งไม่เกิน 8000 bytes ก่อนรอ ACK
```

#### 9. Checksum (16 bits)

**คำจำกัดความ:**

- ตรวจสอบความถูกต้องของ **header + data**
- คำนวณจาก pseudo-header + TCP segment

#### 10. Urgent Pointer (16 bits)

**คำจำกัดความ:**

- ใช้เมื่อ URG flag = 1
- บอก offset ของ urgent data

#### 11. Options (0-40 bytes)

**คำจำกัดความ:**

- Options เพิ่มเติม (optional)
- ต้อง padding ให้ครบ 32-bit boundary

**Options ที่ใช้บ่อย:**

```
- Maximum Segment Size (MSS)
- Window Scale
- Timestamps
- Selective Acknowledgment (SACK)
```

---

### TCP Three-Way Handshake (การสร้าง Connection)

**กระบวนการ:**

#### Step 1: SYN (Client → Server)

```
Client ส่ง SYN segment:
  Flags: SYN = 1
  Sequence Number: X (random, เช่น 1000)
  Window Size: 8192

Message: "สวัสดี, ฉันต้องการเชื่อมต่อ, Seq ของฉันเริ่มที่ 1000"
```

#### Step 2: SYN-ACK (Server → Client)

```
Server ตอบด้วย SYN-ACK segment:
  Flags: SYN = 1, ACK = 1
  Sequence Number: Y (random, เช่น 5000)
  Acknowledgment Number: X + 1 (1001)
  Window Size: 8192

Message: "ได้เลย, Seq ของฉันเริ่มที่ 5000, ฉันรับ byte 1001 ของคุณ"
```

#### Step 3: ACK (Client → Server)

```
Client ส่ง ACK segment:
  Flags: ACK = 1
  Sequence Number: X + 1 (1001)
  Acknowledgment Number: Y + 1 (5001)

Message: "ตกลง, ฉันรับ byte 5001 ของคุณ, เริ่มส่งข้อมูลได้"
```

**ผลลัพธ์:**

- ✅ Connection established
- ✅ ทั้งสองฝั่งรู้ initial sequence numbers ของกัน
- ✅ พร้อมส่งข้อมูล

**ตัวอย่าง:**

```
Client                          Server
   |                               |
   |-------- SYN (Seq=1000) ------>|
   |                               |
   |<-- SYN-ACK (Seq=5000,Ack=1001)|
   |                               |
   |------- ACK (Ack=5001) ------->|
   |                               |
   |===== Connection Established ==|
   |                               |
   |------ Data (Seq=1001) ------->|
   |                               |
```

---

### TCP Data Transfer (การส่งข้อมูล)

**กระบวนการ:**

**1. Sender ส่ง segments:**

```
Segment 1: Seq=1001, Len=1000 (bytes 1001-2000)
Segment 2: Seq=2001, Len=1000 (bytes 2001-3000)
Segment 3: Seq=3001, Len=1000 (bytes 3001-4000)
```

**2. Receiver ส่ง ACKs:**

```
รับ Segment 1 → ส่ง ACK=2001 (ต้องการ byte 2001 ต่อไป)
รับ Segment 2 → ส่ง ACK=3001
รับ Segment 3 → ส่ง ACK=4001
```

**3. ถ้า Segment หาย:**

```
Sender ส่ง:
  Segment 1: Seq=1001
  Segment 2: Seq=2001  ← หาย!
  Segment 3: Seq=3001

Receiver รับ:
  Segment 1 → ส่ง ACK=2001
  Segment 3 (out of order) → ส่ง ACK=2001 อีกครั้ง (duplicate ACK)

Sender รับ duplicate ACK:
  → รู้ว่า Segment 2 หาย
  → Retransmit Segment 2

Receiver รับ Segment 2:
  → รวม Segment 2, 3
  → ส่ง ACK=4001
```

---

### TCP Flow Control (การควบคุมการไหล)

**คำจำกัดความ:**

- ควบคุมอัตราการส่งให้เหมาะสมกับ **receiver**
- ป้องกัน receiver **overwhelm** (รับไม่ทัน)
- ใช้ **Sliding Window** mechanism

**การทำงาน:**

**1. Receiver แจ้ง Window Size:**

```
Receiver buffer = 8000 bytes
Receiver ส่ง: Window = 8000

Sender สามารถส่งได้สูงสุด 8000 bytes ก่อนรอ ACK
```

**2. Sender ส่งข้อมูลตาม Window:**

```
Sender ส่ง 8000 bytes (ไม่รอ ACK)
```

**3. Receiver รับข้อมูล:**

```
Buffer ลดลง: 8000 - 8000 = 0 bytes ว่าง
Receiver ส่ง ACK: Window = 0

Sender หยุดส่ง (รอ Window > 0)
```

**4. Application อ่านข้อมูล:**

```
Application อ่าน 4000 bytes จาก buffer
Buffer ว่าง: 0 + 4000 = 4000 bytes

Receiver ส่ง: Window = 4000

Sender ส่งต่อได้ 4000 bytes
```

**ตัวอย่าง Sliding Window:**

```
Data to send: [1000 bytes] [2000 bytes] [3000 bytes] [4000 bytes]

Window = 8000 bytes:

Step 1: Send 1000, 2000, 3000 (total 6000 < 8000)
Window: [============================        ]
        ^                           ^
        Sent, waiting ACK           Can send

Step 2: รับ ACK for 1000
Window: [============================        ]
        Slide →

Step 3: Send 4000
Window: [====================================]
        Full, wait for ACK
```

---

### TCP Congestion Control (การควบคุม Congestion)

**คำจำกัดความ:**

- ควบคุมอัตราการส่งตาม**สภาพ network**
- ป้องกัน network congestion

**Mechanisms:**

#### 1. Slow Start

**คำจำกัดความ:**

- เริ่มส่งช้าๆ แล้วเพิ่มทีละนิด
- ใช้ **Congestion Window (cwnd)**

**การทำงาน:**

```
Initial: cwnd = 1 MSS (Maximum Segment Size)

Round 1: ส่ง 1 segment, รับ ACK → cwnd = 2
Round 2: ส่ง 2 segments, รับ ACK → cwnd = 4
Round 3: ส่ง 4 segments, รับ ACK → cwnd = 8
...
(cwnd เพิ่มแบบ exponential)

จนกว่าถึง ssthresh (Slow Start Threshold) หรือเกิด packet loss
```

#### 2. Congestion Avoidance

**เมื่อ cwnd ≥ ssthresh:**

- เปลี่ยนจาก exponential → **linear increase**
- cwnd เพิ่มทีละ 1 MSS ต่อ RTT

```
cwnd = ssthresh
Round 1: cwnd = ssthresh + 1
Round 2: cwnd = ssthresh + 2
Round 3: cwnd = ssthresh + 3
...
```

#### 3. Fast Retransmit

**เมื่อรับ 3 duplicate ACKs:**

- ถือว่า packet loss (ไม่รอ timeout)
- Retransmit ทันที

#### 4. Fast Recovery

**หลัง Fast Retransmit:**

- ssthresh = cwnd / 2
- cwnd = ssthresh
- เข้า Congestion Avoidance

---

### TCP Connection Termination (การปิด Connection)

**4-Way Handshake:**

#### Step 1: FIN (Client → Server)

```
Client ส่ง FIN segment:
  Flags: FIN = 1
  Sequence Number: X

Message: "ฉันส่งข้อมูลเสร็จแล้ว (ไม่มีข้อมูลใหม่), แต่ยังรับได้"
```

#### Step 2: ACK (Server → Client)

```
Server ส่ง ACK:
  Flags: ACK = 1
  Acknowledgment Number: X + 1

Message: "รับทราบ, แต่ฉันอาจมีข้อมูลเหลือส่ง"
```

#### Step 3: FIN (Server → Client)

```
Server ส่ง FIN segment:
  Flags: FIN = 1
  Sequence Number: Y

Message: "ฉันส่งข้อมูลเสร็จแล้ว"
```

#### Step 4: ACK (Client → Server)

```
Client ส่ง ACK:
  Flags: ACK = 1
  Acknowledgment Number: Y + 1

Message: "รับทราบ, ปิด connection"
```

**ผลลัพธ์:**

- ✅ Connection closed gracefully
- ✅ ทั้งสองฝั่งส่งข้อมูลครบ

**ตัวอย่าง:**

```
Client                          Server
   |                               |
   |-------- FIN (Seq=X) --------->|
   |                               |
   |<------ ACK (Ack=X+1) ---------|
   |                               |
   |<------ FIN (Seq=Y) -----------|
   |                               |
   |------- ACK (Ack=Y+1) -------->|
   |                               |
   |===== Connection Closed =======|
```

**หมายเหตุ:**

- บางครั้ง Step 2 และ 3 รวมกัน (FIN+ACK) → 3-way termination
- RST flag = บังคับปิดทันที (abnormal termination)

---

## 14.3 UDP (User Datagram Protocol)

### UDP Overview

**คำจำกัดความ:**

- **Connectionless** protocol
- **Unreliable** (best effort)
- กำหนดใน **RFC 768**

**คุณสมบัติ:**

- ❌ No connection setup
- ❌ No acknowledgment
- ❌ No flow control
- ❌ No congestion control
- ❌ No retransmission
- ✅ **Simple** และ **fast**
- ✅ **Low overhead**

---

### UDP Header

**โครงสร้าง UDP Header (8 bytes คงที่):**

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|            Length             |           Checksum            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                             Data                              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

**ฟิลด์:**

#### 1. Source Port (16 bits)

- Port number ของผู้ส่ง
- Optional (สามารถเป็น 0)

#### 2. Destination Port (16 bits)

- Port number ของปลายทาง
- **Required**

#### 3. Length (16 bits)

- ความยาวของ UDP datagram (header + data)
- หน่วยเป็น bytes
- Minimum = 8 bytes (header only)

#### 4. Checksum (16 bits)

- ตรวจสอบความถูกต้อง (header + data)
- Optional ใน IPv4 (แต่แนะนำ)
- **Mandatory** ใน IPv6

---

### UDP Characteristics

#### 1. Connectionless

**ไม่มี connection setup:**

```
TCP:
  Client → SYN → Server
  Client ← SYN-ACK ← Server
  Client → ACK → Server
  (3-way handshake)
  → ส่งข้อมูล

UDP:
  Client → Data → Server
  (ส่งทันที, ไม่มี handshake)
```

#### 2. Unreliable

**ไม่มี acknowledgment:**

```
Sender ส่ง UDP datagram
Sender ไม่รู้ว่า:
  - Datagram ถึงปลายทางหรือไม่
  - หายระหว่างทางหรือไม่
  - มาถูกลำดับหรือไม่

Sender ไม่ retransmit
→ Application ต้องจัดการเอง (ถ้าต้องการ reliability)
```

#### 3. No Flow Control

**ไม่มี window mechanism:**

```
Sender ส่งเร็วเท่าที่ต้องการ
Receiver อาจรับไม่ทัน → datagram หาย

UDP ไม่สน
→ Application ต้องจัดการเอง
```

#### 4. Low Overhead

**Header เล็ก:**

```
UDP header = 8 bytes
TCP header = 20-60 bytes

UDP overhead น้อยกว่า → เร็วกว่า
```

---

### UDP vs TCP Comparison (สรุป)

```
Aspect               TCP                   UDP
------------------------------------------------------------------------
Connection           Yes (3-way handshake) No
Reliability          Reliable              Unreliable
Ordering             Yes                   No
Retransmission       Yes                   No
Flow Control         Yes                   No
Congestion Control   Yes                   No
Error Detection      Yes                   Yes (optional IPv4)
Header Size          20-60 bytes           8 bytes
Speed                Slower                Faster
Overhead             High                  Low
Latency              Higher                Lower
Use Case             File transfer,        Streaming, VoIP,
                     Email, Web            DNS, DHCP
------------------------------------------------------------------------
```

---

### UDP Applications

**เมื่อไหร่ใช้ UDP:**

#### 1. Real-Time Applications

**VoIP (Voice over IP):**

- ต้องการ **low latency**
- Packet หาย 1-2% ยอมรับได้
- เสียง แว้บนิดไม่เป็นไร (retransmit ช้าเกินไป)

**Video Streaming:**

- ต้องการ **high throughput**
- Frame หาย 1 frame → ข้ามไป (ไม่เห็นอยู่ดี)
- Retransmit = เสีย real-time

**Online Gaming:**

- ต้องการ **low latency**
- Position update ล่าช้า = เล่นกระตุก
- ข้อมูลเก่าไม่สำคัญ (มี update ใหม่แล้ว)

#### 2. Simple Request-Response

**DNS (Domain Name System):**

```
Query: DNS request ขนาดเล็ก (< 512 bytes)
Response: DNS response ขนาดเล็ก

UDP เหมาะ:
  - Transaction เล็ก (1 request, 1 response)
  - TCP overhead เปลือง (3-way handshake ใช้เวลา)
  - ถ้าไม่ได้รับ response → query ใหม่
```

**DHCP:**

```
DHCP Discovery, Offer, Request, Ack
แต่ละ message เล็ก
UDP เหมาะ (TCP overhead เปลือง)
```

**SNMP (Simple Network Management Protocol):**

```
Query: ขอข้อมูล MIB
Response: ส่งข้อมูลกลับ

Transaction เล็ก → ใช้ UDP
```

#### 3. Broadcast/Multicast

**DHCP Discovery:**

```
Client ส่ง broadcast (255.255.255.255)
TCP ไม่รองรับ broadcast → ต้องใช้ UDP
```

**Routing Protocols:**

```
RIP ใช้ UDP multicast (224.0.0.9)
```

---

### UDP Example: DNS Query

**Scenario:** คอมพิวเตอร์ query DNS สำหรับ google.com

```
1. Client สร้าง DNS query:
   Source: 192.168.1.10:50001
   Destination: 8.8.8.8:53 (DNS)
   Protocol: UDP
   Data: "What is IP of google.com?"

2. Client ส่ง UDP datagram (ไม่มี handshake)

3. DNS Server รับและ process

4. DNS Server ส่ง response:
   Source: 8.8.8.8:53
   Destination: 192.168.1.10:50001
   Protocol: UDP
   Data: "google.com = 142.250.185.46"

5. Client รับ response → ใช้ IP address

Total time: ~10-50ms (ไม่มี handshake overhead)

ถ้า response หาย:
  → Client timeout (ประมาณ 2-5 วินาที)
  → Client query ใหม่
```

**เปรียบเทียบกับ TCP:**

```
UDP:
  - ส่งทันที
  - Total time: ~10-50ms

TCP:
  - 3-way handshake (20-30ms)
  - ส่งข้อมูล (10-20ms)
  - 4-way termination (20-30ms)
  - Total time: ~50-80ms

→ UDP เร็วกว่า แต่ไม่ reliable
```

---

## 14.4 Port Numbers (หมายเลขพอร์ต)

### Common Port Numbers

**Well-Known Ports (0-1023):**

```
Port  TCP/UDP  Protocol      Description
------------------------------------------------------------------------
20    TCP      FTP-DATA      FTP Data Transfer
21    TCP      FTP           FTP Control
22    TCP      SSH           Secure Shell
23    TCP      Telnet        Telnet (insecure)
25    TCP      SMTP          Email Sending
53    TCP/UDP  DNS           Domain Name System
67    UDP      DHCP Server   DHCP Server
68    UDP      DHCP Client   DHCP Client
69    UDP      TFTP          Trivial FTP
80    TCP      HTTP          Web (unencrypted)
110   TCP      POP3          Email Receiving
123   UDP      NTP           Network Time Protocol
143   TCP      IMAP          Email Receiving
161   UDP      SNMP          Network Management
179   TCP      BGP           Border Gateway Protocol
443   TCP      HTTPS         Web (encrypted)
445   TCP      SMB           Windows File Sharing
514   UDP      Syslog        System Logging
520   UDP      RIP           Routing Information Protocol
1433  TCP      MS-SQL        Microsoft SQL Server
1521  TCP      Oracle        Oracle Database
3306  TCP      MySQL         MySQL Database
3389  TCP      RDP           Remote Desktop
5060  TCP/UDP  SIP           VoIP Signaling
8080  TCP      HTTP-Alt      HTTP Alternate
------------------------------------------------------------------------
```

---

### Viewing Active Connections

**Windows:**

```cmd
netstat -an
  -a = all connections and listening ports
  -n = numerical addresses (ไม่ resolve hostnames)

Output:
Proto  Local Address          Foreign Address        State
TCP    192.168.1.10:50001     8.8.8.8:80            ESTABLISHED
TCP    192.168.1.10:50002     1.1.1.1:443           ESTABLISHED
TCP    0.0.0.0:445            0.0.0.0:0             LISTENING
UDP    192.168.1.10:50003     *:*
```

**Linux:**

```bash
netstat -tunap
  -t = TCP
  -u = UDP
  -n = numerical
  -a = all
  -p = program

ss -tunap  (faster, modern alternative)
```

**Cisco:**

```
Router# show ip sockets
```

---

### Port Security Considerations

**Port Scanning:**

```
Attacker scan ports เพื่อหา services ที่เปิดอยู่
Tools: nmap, masscan

Example:
nmap -p 1-1000 192.168.1.10
→ หา ports 1-1000 ที่เปิดอยู่
```

**Best Practices:**

- ✅ ปิด ports ที่ไม่ใช้
- ✅ Firewall filtering (อนุญาตเฉพาะ ports ที่จำเป็น)
- ✅ เปลี่ยน default ports (เช่น SSH จาก 22 → 2222)
- ✅ IDS/IPS monitoring

---

## Summary (สรุป)

Module 14 นี้เราได้เรียนรู้:

1. ✅ **Transport Layer** - Layer 4, Segmentation, Multiplexing
2. ✅ **TCP** - Connection-oriented, Reliable, 3-way handshake
3. ✅ **TCP Features**:
    - Sequencing & Acknowledgment
    - Flow Control (Sliding Window)
    - Congestion Control (Slow Start, Congestion Avoidance)
    - Error Recovery (Retransmission)
4. ✅ **UDP** - Connectionless, Unreliable, Fast, Low overhead
5. ✅ **Port Numbers** - Well-Known (0-1023), Registered (1024-49151), Dynamic (49152-65535)
6. ✅ **Socket** - IP:Port, ระบุ endpoint

**สิ่งสำคัญที่ต้องจำ:**

- Transport Layer = Layer 4, PDU = Segment (TCP) / Datagram (UDP)
- TCP = Reliable, Connection-oriented (3-way handshake)
- UDP = Unreliable, Connectionless (no handshake)
- TCP header = 20-60 bytes, UDP header = 8 bytes
- TCP flags: SYN (establish), ACK (acknowledge), FIN (close), RST (reset)
- 3-way handshake: SYN → SYN-ACK → ACK
- 4-way termination: FIN → ACK → FIN → ACK
- Flow Control = Window Size (ควบคุม receiver)
- Congestion Control = cwnd (ควบคุม network)
- Sequence Number = ordering, Acknowledgment Number = confirm
- Port Numbers: HTTP=80, HTTPS=443, DNS=53, SSH=22, FTP=21
- Well-Known Ports = 0-1023 (services)
- Dynamic Ports = 49152-65535 (client source ports)
- TCP ใช้สำหรับ: HTTP, FTP, Email, SSH
- UDP ใช้สำหรับ: DNS, DHCP, VoIP, Streaming, Gaming

**TCP vs UDP:**

```
TCP = Reliable, Slow, High overhead
UDP = Fast, Unreliable, Low overhead
```

**Next Module:** Module 15 - Application Layer

---

**[ไฟล์ Module 14 สมบูรณ์แล้ว!]**