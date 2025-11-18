# CCNA Course 1 - Module 13: ICMP

## Internet Control Message Protocol

---

## 13.1 ICMP Messages (ข้อความ ICMP)

### ICMP Overview

**คำจำกัดความ:**

- **Internet Control Message Protocol**
- Protocol สำหรับ **error reporting** และ **diagnostics**
- ทำงานที่ **Layer 3** (Network Layer)
- เป็นส่วนหนึ่งของ **IP protocol suite**

**หมายเหตุ:**

- ICMP **ไม่ใช่** Transport Layer protocol
- ICMP messages ถูก encapsulated ใน **IP packets**
- มี 2 เวอร์ชัน:
    - **ICMPv4** - สำหรับ IPv4
    - **ICMPv6** - สำหรับ IPv6

---

### ICMP Purpose (วัตถุประสงค์)

**ฟังก์ชันหลัก:**

#### 1. Error Reporting

- แจ้ง**ปัญหา**ในการส่ง packets
- Destination unreachable
- Time exceeded
- Parameter problems

#### 2. Diagnostics

- ทดสอบการเชื่อมต่อ (**ping**)
- ตรวจสอบเส้นทาง (**traceroute**)

#### 3. Informational Messages

- Echo Request/Reply (ping)
- Router Advertisement/Solicitation
- Redirect messages

**หมายเหตุสำคัญ:**

- ICMP **ไม่ทำให้ IP reliable**
- ICMP เพียงแค่**รายงานปัญหา**
- ไม่มีการแก้ไขหรือ retransmit

---

### ICMP Characteristics (ลักษณะ)

**คุณสมบัติ:**

- ✅ **Connectionless** - ไม่มี connection setup
- ✅ **Unreliable** - ไม่รับประกัน delivery
- ✅ **No retransmission** - ส่งครั้งเดียว
- ✅ **Best effort** - พยายามส่งเท่านั้น

**ICMP ไม่ส่งสำหรับ:**

- ❌ ICMP error messages (ไม่ error ซ้อน error)
- ❌ Fragments (เฉพาะ fragment แรก)
- ❌ Broadcasts/Multicasts
- ❌ Link-layer broadcasts

---

### ICMPv4 Packet Format

**โครงสร้าง:**

```
+----------+----------+----------+------------------+
|   Type   |   Code   | Checksum |   Rest of Header |
| (8 bits) | (8 bits) |(16 bits) |   (32 bits)      |
+----------+----------+----------+------------------+
|              Data (Variable)                      |
+---------------------------------------------------+
```

**ฟิลด์:**

#### 1. Type (8 bits)

- บอก**ประเภท**ของ ICMP message
- ตัวอย่าง: 0 = Echo Reply, 8 = Echo Request

#### 2. Code (8 bits)

- รายละเอียดเพิ่มเติมของ Type
- ตัวอย่าง: Type 3 (Destination Unreachable) มีหลาย Codes

#### 3. Checksum (16 bits)

- ตรวจสอบความถูกต้องของ ICMP message

#### 4. Rest of Header (32 bits)

- ขึ้นกับ Type และ Code
- อาจเป็น Identifier, Sequence Number, Gateway Address, etc.

#### 5. Data (Variable)

- ข้อมูลเพิ่มเติม
- สำหรับ error messages: มักเป็น IP header + 8 bytes ของ original packet

---

### ICMPv4 Message Types

**Common ICMP Types:**

```
Type  Name                          Description
------------------------------------------------------------------------
0     Echo Reply                    ตอบกลับ ping
3     Destination Unreachable       ไปไม่ถึงปลายทาง
4     Source Quench                 Congestion control (obsolete)
5     Redirect                      แนะนำเส้นทางที่ดีกว่า
8     Echo Request                  ping
9     Router Advertisement          Router บอก "ฉันคือ Router"
10    Router Solicitation           Host ถาม "มี Router ไหม?"
11    Time Exceeded                 TTL = 0 หรือ Fragment timeout
12    Parameter Problem             IP header ผิดพลาด
13    Timestamp Request             ขอเวลา
14    Timestamp Reply               ตอบเวลา
------------------------------------------------------------------------
```

---

## 13.2 ICMP Error Messages (ข้อความแจ้งข้อผิดพลาด)

### Type 3: Destination Unreachable

**คำจำกัดความ:**

- ไม่สามารถส่ง packet ไปยังปลายทางได้
- Router หรือ Host ส่ง ICMP Type 3 กลับ

**Codes (รายละเอียด):**

```
Code  Name                          Meaning
------------------------------------------------------------------------
0     Network Unreachable           ไม่มีเส้นทางไปยัง network
1     Host Unreachable              ไม่มีเส้นทางไปยัง host
2     Protocol Unreachable          Protocol ไม่รองรับ (เช่น ไม่มี TCP)
3     Port Unreachable              Port ปิดอยู่
4     Fragmentation Needed but      Packet ใหญ่เกินไป + DF flag set
      DF Set
5     Source Route Failed           Source routing ไม่สำเร็จ
6     Destination Network Unknown   ไม่รู้จัก network
7     Destination Host Unknown      ไม่รู้จัก host
9     Network Administratively      Network ถูกบล็อก (firewall/ACL)
      Prohibited
10    Host Administratively         Host ถูกบล็อก
      Prohibited
11    Network Unreachable for ToS   ToS ไม่รองรับ
12    Host Unreachable for ToS      ToS ไม่รองรับ
13    Communication                 Filtered by firewall/ACL
      Administratively Prohibited
------------------------------------------------------------------------
```

**ตัวอย่าง Scenarios:**

#### Code 0: Network Unreachable

```
PC1 (192.168.1.10) → Server (10.0.0.100)

Router ไม่มี route ไปยัง 10.0.0.0/8
→ Router ส่ง ICMP Type 3 Code 0 กลับ PC1
→ "Network 10.0.0.0 unreachable"
```

#### Code 1: Host Unreachable

```
PC1 → 192.168.1.100

Router มี route ไปยัง 192.168.1.0/24
แต่ไม่มี ARP entry สำหรับ .100 (host ไม่มีจริง)
→ Router ส่ง ICMP Type 3 Code 1 กลับ
→ "Host 192.168.1.100 unreachable"
```

#### Code 3: Port Unreachable

```
PC1 ส่ง UDP packet ไปยัง Server:53 (DNS)

Server ไม่มี DNS service running
→ Server ส่ง ICMP Type 3 Code 3 กลับ
→ "Port 53 unreachable"
```

#### Code 4: Fragmentation Needed

```
PC1 ส่ง packet ขนาด 2000 bytes พร้อม DF flag
Router interface MTU = 1500 bytes

Router ต้อง fragment แต่ DF flag = 1 (Don't Fragment)
→ Router ส่ง ICMP Type 3 Code 4 กลับ
→ "Fragmentation needed but DF set"
→ พร้อม MTU ที่รองรับ

PC1 ลด packet size แล้วส่งใหม่ (Path MTU Discovery)
```

#### Code 13: Administratively Prohibited

```
PC1 → Server (blocked by ACL)

Router มี ACL ปิดกั้น traffic
→ Router ส่ง ICMP Type 3 Code 13 กลับ
→ "Communication administratively prohibited"

(บาง ACL config ไม่ส่ง ICMP เพื่อ security)
```

---

### Type 11: Time Exceeded

**คำจำกัดความ:**

- Packet หมดเวลา (TTL = 0)
- หรือ Fragment reassembly timeout

**Codes:**

```
Code  Name                          Meaning
------------------------------------------------------------------------
0     TTL Exceeded in Transit       TTL ถูกลดเป็น 0 ระหว่างทาง
1     Fragment Reassembly           Fragment timeout (ไม่ครบภายในเวลา)
      Time Exceeded
------------------------------------------------------------------------
```

**Code 0: TTL Exceeded (ใช้บ่อย)**

**Scenario:**

```
PC1 ส่ง packet พร้อม TTL = 3

PC1 → R1: TTL = 3
R1 → R2: TTL = 2 (R1 ลด 1)
R2 → R3: TTL = 1 (R2 ลด 1)
R3: TTL - 1 = 0 (หมดแล้ว!)

→ R3 drop packet
→ R3 ส่ง ICMP Type 11 Code 0 กลับ PC1
→ "TTL exceeded in transit"
```

**วัตถุประสงค์:**

- ป้องกัน **routing loop** (packet วนไปมา)
- ใช้สำหรับ **traceroute**

**Code 1: Fragment Reassembly Timeout**

**Scenario:**

```
Packet ถูก fragment เป็น 3 pieces

Destination รับ:
  Fragment 1: OK
  Fragment 2: OK
  Fragment 3: Lost!

หลังจากรอ timeout (default 60s):
→ Destination ทิ้ง Fragment 1, 2
→ ส่ง ICMP Type 11 Code 1 กลับ Source
```

---

### Type 5: Redirect

**คำจำกัดความ:**

- Router แจ้ง Host ว่ามี**เส้นทางที่ดีกว่า**
- บอกให้ใช้ Gateway อื่น

**Codes:**

```
Code  Name                          Description
------------------------------------------------------------------------
0     Redirect for Network          Redirect ทั้ง network
1     Redirect for Host             Redirect specific host
2     Redirect for ToS & Network    Redirect network + ToS
3     Redirect for ToS & Host       Redirect host + ToS
------------------------------------------------------------------------
```

**Scenario:**

```
Topology:
[PC1] --[R1]-- [R2] -- [Server]
  |
  └------[R3 (direct to Server)]

PC1 Default Gateway: R1 (192.168.1.1)

1. PC1 ส่ง packet → Server ผ่าน R1
2. R1 รับ packet และ forward → R2
3. R1 รู้ว่า R3 ใกล้กว่า (อยู่ same subnet กับ PC1)
4. R1 ส่ง ICMP Type 5 Redirect กลับ PC1:
   "ใช้ R3 (192.168.1.254) แทน สำหรับ Server นี้"
5. PC1 update routing table (temporary)
6. PC1 ส่ง packet ถัดไปผ่าน R3 โดยตรง
```

**หมายเหตุ:**

- Redirect **ไม่เปลี่ยน Default Gateway**
- เพียงแค่เพิ่ม host route ชั่วคราว
- ใช้สำหรับ **optimization** เท่านั้น

---

### Type 12: Parameter Problem

**คำจำกัดความ:**

- IP header มีปัญหา
- Field ผิดพลาด

**Codes:**

```
Code  Name                          Description
------------------------------------------------------------------------
0     Pointer Indicates Error       ระบุตำแหน่ง byte ที่ผิด
1     Missing Required Option       ขาด option ที่จำเป็น
2     Bad Length                    ความยาวผิด
------------------------------------------------------------------------
```

**ตัวอย่าง:**

```
IP header field ผิด:
- Version ไม่ถูกต้อง (ไม่ใช่ 4)
- Header Length ผิด
- Total Length ไม่ตรงกับความยาวจริง
- Options ผิดพลาด

→ Router/Host ส่ง ICMP Type 12 กลับ
```

---

## 13.3 ICMP Diagnostic Tools (เครื่องมือวินิจฉัย)

### Ping (Packet Internet Groper)

**คำจำกัดความ:**

- เครื่องมือทดสอบ**การเชื่อมต่อ** (connectivity)
- ส่ง **ICMP Echo Request** (Type 8)
- รอรับ **ICMP Echo Reply** (Type 0)

**การทำงาน:**

**Step 1: ส่ง Echo Request**

```
ICMP Type 8 (Echo Request):
  Type: 8
  Code: 0
  Identifier: <process ID>
  Sequence Number: 1, 2, 3, ...
  Data: 32-56 bytes (default, ขึ้นกับ OS)
```

**Step 2: รอรับ Echo Reply**

```
ICMP Type 0 (Echo Reply):
  Type: 0
  Code: 0
  Identifier: <same as request>
  Sequence Number: <same as request>
  Data: <same as request>
```

**Step 3: คำนวณ RTT**

```
RTT (Round Trip Time) = Time Reply - Time Request
```

---

### Ping Command

**Syntax:**

```
ping <destination>
ping <IP address>
ping <hostname>
```

**Windows:**

```cmd
C:\> ping 8.8.8.8

Pinging 8.8.8.8 with 32 bytes of data:
Reply from 8.8.8.8: bytes=32 time=15ms TTL=118
Reply from 8.8.8.8: bytes=32 time=14ms TTL=118
Reply from 8.8.8.8: bytes=32 time=16ms TTL=118
Reply from 8.8.8.8: bytes=32 time=15ms TTL=118

Ping statistics for 8.8.8.8:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 14ms, Maximum = 16ms, Average = 15ms
```

**Linux/macOS:**

```bash
$ ping 8.8.8.8

PING 8.8.8.8 (8.8.8.8): 56 data bytes
64 bytes from 8.8.8.8: icmp_seq=0 ttl=118 time=15.2 ms
64 bytes from 8.8.8.8: icmp_seq=1 ttl=118 time=14.8 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=118 time=16.1 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=118 time=15.0 ms
^C
--- 8.8.8.8 ping statistics ---
4 packets transmitted, 4 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 14.8/15.3/16.1/0.5 ms
```

**Cisco IOS:**

```
Router# ping 8.8.8.8

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 8.8.8.8, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 12/14/16 ms
```

**Cisco Symbols:**

```
!  = Success (Echo Reply received)
.  = Timeout (no reply within timeout)
U  = Destination Unreachable
C  = Congestion experienced
I  = User interrupted
?  = Unknown packet type
&  = Packet lifetime exceeded
```

---

### Ping Options

**Windows:**

```cmd
ping -t 8.8.8.8               # Continuous ping (Ctrl+C to stop)
ping -n 10 8.8.8.8            # Send 10 packets
ping -l 1000 8.8.8.8          # Packet size 1000 bytes
ping -i 64 8.8.8.8            # TTL = 64
ping -w 5000 8.8.8.8          # Timeout 5000ms
ping -a 8.8.8.8               # Resolve hostname
```

**Linux/macOS:**

```bash
ping -c 10 8.8.8.8            # Count: send 10 packets
ping -s 1000 8.8.8.8          # Size: 1000 bytes
ping -i 0.5 8.8.8.8           # Interval: 0.5 seconds
ping -t 64 8.8.8.8            # TTL = 64
ping -W 5 8.8.8.8             # Timeout 5 seconds
```

**Cisco IOS Extended Ping:**

```
Router# ping
Protocol [ip]:
Target IP address: 8.8.8.8
Repeat count [5]: 100
Datagram size [100]: 1500
Timeout in seconds [2]: 5
Extended commands [n]: y
Source address or interface: 192.168.1.1
Type of service [0]:
Set DF bit in IP header? [no]: yes
Validate reply data? [no]:
Data pattern [0xABCD]:
Loose, Strict, Record, Timestamp, Verbose[none]:
Sweep range of sizes [n]:
```

---

### Ping Use Cases

#### 1. Test Local TCP/IP Stack

```
ping 127.0.0.1
หรือ
ping localhost

→ ทดสอบว่า TCP/IP stack ทำงานหรือไม่
```

#### 2. Test Local NIC

```
ping <own IP address>

ตัวอย่าง:
ping 192.168.1.10

→ ทดสอบว่า NIC ทำงานหรือไม่
```

#### 3. Test Default Gateway

```
ping <gateway IP>

ตัวอย่าง:
ping 192.168.1.1

→ ทดสอบว่าเข้าถึง Gateway ได้หรือไม่
```

#### 4. Test Remote Host

```
ping 8.8.8.8

→ ทดสอบการเชื่อมต่อ Internet
```

#### 5. Test DNS

```
ping google.com

→ ถ้า ping IP ได้ แต่ ping hostname ไม่ได้ = DNS problem
```

#### 6. Test Path MTU

```
ping -l 1500 -f 8.8.8.8  (Windows, DF flag)
ping -s 1500 -M do 8.8.8.8  (Linux, DF flag)

→ ทดสอบ MTU ในเส้นทาง
→ ถ้าได้ ICMP Type 3 Code 4 = Packet ใหญ่เกิน MTU
```

---

### Ping Response Analysis

**Success:**

```
Reply from 8.8.8.8: bytes=32 time=15ms TTL=118

✅ Connectivity OK
✅ TTL=118 → มีประมาณ 137-118 = 19 hops (default TTL ~128-255)
✅ time=15ms → Latency ต่ำ, connection ดี
```

**Request Timeout:**

```
Request timed out.

Possible causes:
❌ Destination down/unreachable
❌ Firewall blocking ICMP
❌ Network congestion
❌ Routing problem
```

**Destination Host Unreachable:**

```
Reply from 192.168.1.1: Destination host unreachable.

ICMP Type 3 Code 1
❌ Host ไม่มีจริง หรือ down
❌ Router ไม่มี route
```

**Destination Network Unreachable:**

```
Reply from 192.168.1.1: Destination net unreachable.

ICMP Type 3 Code 0
❌ Router ไม่มี route ไปยัง network
```

**TTL Expired in Transit:**

```
Reply from 192.168.1.1: TTL expired in transit.

ICMP Type 11 Code 0
❌ TTL = 0 ก่อนถึงปลายทาง
❌ อาจมี routing loop
❌ หรือ destination ไกลเกินไป (เพิ่ม TTL)
```

**General Failure:**

```
General failure.

❌ No route to host (ไม่มี Default Gateway)
❌ Network adapter disabled
❌ IP configuration ผิด
```

---

### Traceroute (tracert/traceroute)

**คำจำกัดความ:**

- เครื่องมือติดตาม**เส้นทาง** (path) ไปยังปลายทาง
- แสดง**ทุก hop** (router) ที่ผ่าน
- วัด **latency** แต่ละ hop

**หลักการทำงาน:**

- ส่ง packets พร้อม **TTL เพิ่มขึ้นทีละ 1**
- TTL = 1 → Router แรกส่ง ICMP Time Exceeded กลับ
- TTL = 2 → Router ที่สองส่ง ICMP Time Exceeded กลับ
- ...
- จนกว่าถึงปลายทาง → ICMP Echo Reply

---

### Traceroute Implementations

**ความแตกต่าง:**

#### Windows: tracert (ICMP)

```
ใช้ ICMP Echo Request
TTL เพิ่มทีละ 1
```

#### Linux/macOS: traceroute (UDP)

```
ใช้ UDP packets (port 33434+)
TTL เพิ่มทีละ 1
Destination ส่ง ICMP Port Unreachable กลับ
```

#### Cisco IOS: traceroute (UDP)

```
ใช้ UDP packets (port 33434+)
เหมือน Linux/macOS
```

---

### Traceroute Command

**Windows (tracert):**

```cmd
C:\> tracert google.com

Tracing route to google.com [142.250.185.46]
over a maximum of 30 hops:

  1    <1 ms    <1 ms    <1 ms  192.168.1.1
  2     2 ms     2 ms     2 ms  10.0.0.1
  3     5 ms     5 ms     5 ms  203.0.113.1
  4    10 ms    11 ms    10 ms  72.14.221.1
  5    15 ms    14 ms    16 ms  172.253.65.159
  6    15 ms    15 ms    15 ms  142.250.185.46

Trace complete.
```

**Linux/macOS:**

```bash
$ traceroute google.com

traceroute to google.com (142.250.185.46), 30 hops max, 60 byte packets
 1  192.168.1.1 (192.168.1.1)  0.823 ms  0.756 ms  0.712 ms
 2  10.0.0.1 (10.0.0.1)  2.145 ms  2.098 ms  2.054 ms
 3  203.0.113.1 (203.0.113.1)  5.234 ms  5.189 ms  5.142 ms
 4  72.14.221.1 (72.14.221.1)  10.456 ms  10.412 ms  10.367 ms
 5  172.253.65.159 (172.253.65.159)  15.678 ms  15.634 ms  15.589 ms
 6  142.250.185.46 (142.250.185.46)  15.123 ms  15.089 ms  15.045 ms
```

**Cisco IOS:**

```
Router# traceroute google.com

Type escape sequence to abort.
Tracing the route to google.com (142.250.185.46)
VRF info: (vrf in name/id, vrf out name/id)
  1 192.168.1.1 4 msec 4 msec 4 msec
  2 10.0.0.1 8 msec 8 msec 8 msec
  3 203.0.113.1 12 msec 12 msec 12 msec
  4 72.14.221.1 16 msec 16 msec 16 msec
  5 172.253.65.159 20 msec 20 msec 20 msec
  6 142.250.185.46 24 msec 24 msec 24 msec
```

---

### Traceroute Process Example

**ตัวอย่าง: PC → Server ผ่าน 3 routers**

```
Topology:
[PC] → [R1] → [R2] → [R3] → [Server]

TTL=1:
PC → R1: TTL=1
R1: TTL - 1 = 0 → Drop packet
R1 → PC: ICMP Time Exceeded (Type 11)
PC records: Hop 1 = R1 IP, RTT

TTL=2:
PC → R1 → R2: TTL=2
R1 → R2: TTL=1
R2: TTL - 1 = 0 → Drop packet
R2 → PC: ICMP Time Exceeded (Type 11)
PC records: Hop 2 = R2 IP, RTT

TTL=3:
PC → R1 → R2 → R3: TTL=3
R3: TTL - 1 = 0 → Drop packet
R3 → PC: ICMP Time Exceeded (Type 11)
PC records: Hop 3 = R3 IP, RTT

TTL=4:
PC → R1 → R2 → R3 → Server: TTL=4
Server: รับ packet

Windows tracert: ICMP Echo Request
  → Server ส่ง ICMP Echo Reply กลับ

Linux/macOS traceroute: UDP (port unreachable)
  → Server ส่ง ICMP Port Unreachable กลับ

PC records: Hop 4 = Server IP, RTT
Done!
```

---

### Traceroute Output Analysis

**Normal Output:**

```
 1  192.168.1.1   1 ms   1 ms   1 ms
 2  10.0.0.1      5 ms   5 ms   5 ms
 3  203.0.113.1  10 ms  10 ms  10 ms

✅ ทุก hop ตอบกลับ
✅ Latency เพิ่มขึ้นตามระยะทาง
```

**Timeouts (*):**

```
 1  192.168.1.1   1 ms   1 ms   1 ms
 2  *  *  *
 3  203.0.113.1  15 ms  15 ms  15 ms

⚠️ Hop 2 ไม่ตอบ ICMP (หรือ timeout)

Possible causes:
- Router configured ไม่ให้ส่ง ICMP Time Exceeded
- Firewall blocking ICMP
- High latency/packet loss

แต่ traffic ยังผ่านได้ (เห็น Hop 3)
```

**Request Timed Out:**

```
 1  192.168.1.1   1 ms   1 ms   1 ms
 2  10.0.0.1      5 ms   5 ms   5 ms
 3  Request timed out.
 4  Request timed out.
...

❌ Hop 3+ ไม่ตอบกลับเลย

Possible causes:
- ปลายทางไม่ reachable
- Firewall blocking
- Destination down
```

**High Latency Spike:**

```
 1  192.168.1.1     1 ms    1 ms    1 ms
 2  10.0.0.1        5 ms    5 ms    5 ms
 3  203.0.113.1   250 ms  248 ms  252 ms
 4  8.8.8.8        15 ms   15 ms   15 ms

⚠️ Hop 3 latency สูงมาก แต่ Hop 4 ต่ำ

Possible causes:
- Router ที่ Hop 3 ให้ priority ต่ำกับ ICMP
- Congestion ที่ Hop 3 (แต่ไม่น่าเพราะ Hop 4 ต่ำ)
- Router rate-limiting ICMP
```

**Asymmetric Routing:**

```
Traceroute อาจแสดง path ไป (forward)
แต่ path กลับ (return) อาจต่างกัน
```

---

### Traceroute Options

**Windows (tracert):**

```cmd
tracert -d google.com          # ไม่ resolve hostnames (เร็วขึ้น)
tracert -h 50 google.com       # Max hops = 50
tracert -w 5000 google.com     # Timeout 5000ms
```

**Linux/macOS:**

```bash
traceroute -n google.com       # ไม่ resolve hostnames
traceroute -m 50 google.com    # Max hops = 50
traceroute -w 5 google.com     # Timeout 5s
traceroute -I google.com       # ใช้ ICMP แทน UDP
traceroute -T google.com       # ใช้ TCP
traceroute -p 80 google.com    # Specify port
```

**Cisco IOS:**

```
Router# traceroute google.com
หรือ Extended:

Router# traceroute
Protocol [ip]:
Target IP address: 8.8.8.8
Source address:
Numeric display [n]:
Timeout in seconds [3]:
Probe count [3]:
Minimum Time to Live [1]:
Maximum Time to Live [30]:
Port Number [33434]:
Loose, Strict, Record, Timestamp, Verbose[none]:
```

---

## 13.4 ICMPv6 Messages

### ICMPv6 Overview

**คำจำกัดความ:**

- ICMP สำหรับ **IPv6**
- มีความสามารถ**มากกว่า** ICMPv4
- รวม functionality ของ:
    - ICMPv4
    - ARP (ใน IPv4)
    - IGMP (ใน IPv4)

**คุณสมบัติเพิ่มเติม:**

- ✅ **Neighbor Discovery Protocol (NDP)**
- ✅ **Multicast Listener Discovery (MLD)**
- ✅ **Path MTU Discovery**
- ✅ **Router Advertisement/Solicitation**

---

### ICMPv6 Message Types

**Common ICMPv6 Types:**

```
Type  Name                          Description
------------------------------------------------------------------------
1     Destination Unreachable       ไปไม่ถึงปลายทาง
2     Packet Too Big                Packet ใหญ่เกิน MTU
3     Time Exceeded                 Hop Limit = 0
4     Parameter Problem             Header ผิดพลาด
128   Echo Request                  ping
129   Echo Reply                    ตอบ ping
133   Router Solicitation (RS)      Host ขอ Router info
134   Router Advertisement (RA)     Router แจ้งข้อมูล
135   Neighbor Solicitation (NS)    หา MAC (เหมือน ARP Request)
136   Neighbor Advertisement (NA)   ตอบ MAC (เหมือน ARP Reply)
137   Redirect                      แนะนำเส้นทางที่ดีกว่า
------------------------------------------------------------------------
```

---

### ICMPv6 Error Messages

#### Type 1: Destination Unreachable

**Codes:**

```
Code  Name
------------------------------------------------------------------------
0     No route to destination
1     Communication administratively prohibited
2     Beyond scope of source address
3     Address unreachable
4     Port unreachable
5     Source address failed ingress/egress policy
6     Reject route to destination
------------------------------------------------------------------------
```

#### Type 2: Packet Too Big

**ลักษณะ:**

- เหมือน ICMPv4 Type 3 Code 4 (Fragmentation Needed)
- แต่ใน IPv6: **Router ไม่ fragment**
- เฉพาะ source ที่ fragment

**การทำงาน:**

```
Host ส่ง packet ขนาด 1500 bytes
Router interface MTU = 1280 bytes

Router ไม่สามารถ forward
→ Router drop packet
→ Router ส่ง ICMPv6 Type 2 กลับ Host
→ บอก MTU = 1280

Host ลด packet size เป็น 1280 bytes
Host ส่งใหม่
```

#### Type 3: Time Exceeded

**Codes:**

```
Code  Name
------------------------------------------------------------------------
0     Hop limit exceeded in transit
1     Fragment reassembly time exceeded
------------------------------------------------------------------------
```

**Code 0: ใช้สำหรับ traceroute**

---

### ICMPv6 Neighbor Discovery (NDP)

**คำจำกัดความ:**

- แทน **ARP** ใน IPv4
- มีความสามารถมากกว่า ARP

**ฟังก์ชัน:**

1. Address Resolution (หา MAC จาก IPv6)
2. Router Discovery (หา Default Gateway)
3. Prefix Discovery (หา network prefix)
4. Parameter Discovery (MTU, Hop Limit)
5. Duplicate Address Detection (DAD)
6. Neighbor Unreachability Detection (NUD)
7. Redirect

---

### ICMPv6 NDP Messages

#### Type 133: Router Solicitation (RS)

**ลักษณะ:**

- Host ส่งถาม "มี Router ไหม?"
- ส่งไปยัง `ff02::2` (All-Routers multicast)

**เมื่อไหร่ส่ง:**

- Interface up ครั้งแรก
- ต้องการหา Gateway เร็ว

#### Type 134: Router Advertisement (RA)

**ลักษณะ:**

- Router ตอบ "ฉันคือ Router"
- ส่งไปยัง `ff02::1` (All-Nodes) หรือ Unicast
- บอกข้อมูล:
    - Prefix (2001:db8:acad:1::/64)
    - Flags (M, O, A)
    - MTU
    - Hop Limit
    - Default Gateway (Link-Local ของ Router)

**ส่งเมื่อไหร่:**

- Periodic (ทุก 200 วินาที)
- ตอบ RS

#### Type 135: Neighbor Solicitation (NS)

**ลักษณะ:**

- เหมือน **ARP Request** ใน IPv4
- หา MAC address จาก IPv6 address
- ส่งไปยัง **Solicited-Node Multicast**

**การใช้งาน:**

1. Address Resolution (หา MAC)
2. Duplicate Address Detection (DAD)
3. Neighbor Unreachability Detection (ตรวจสอบว่ายัง reachable)

**ตัวอย่าง:**

```
Host ต้องการหา MAC ของ 2001:db8::10

NS:
  Source: Link-Local ของตัวเอง
  Destination: ff02::1:ff00:10 (Solicited-Node Multicast)
  Target: 2001:db8::10
  Message: "ใครมี 2001:db8::10? MAC ของฉันคือ XX:XX:XX:XX:XX:XX"
```

#### Type 136: Neighbor Advertisement (NA)

**ลักษณะ:**

- เหมือน **ARP Reply** ใน IPv4
- ตอบพร้อม MAC address
- ส่งแบบ Unicast (ตอบ NS) หรือ Multicast (unsolicited)

**ตัวอย่าง:**

```
NA:
  Source: 2001:db8::10
  Destination: Link-Local ของ Host ที่ถาม
  Target: 2001:db8::10
  Target Link-Layer Address: AA:BB:CC:DD:EE:FF
  Flags: S=1 (Solicited), O=1 (Override)
```

---

### ICMPv6 DAD (Duplicate Address Detection)

**กระบวนการ:**

**Step 1: Tentative Address**

```
Host กำหนด IPv6 address
Status: Tentative (ยังใช้ไม่ได้)
```

**Step 2: ส่ง NS for DAD**

```
NS:
  Source: :: (unspecified)
  Destination: Solicited-Node Multicast of tentative address
  Target: Tentative address

Message: "มีใครใช้ address นี้อยู่หรือไม่?"
```

**Step 3: รอ Response**

```
ถ้าได้รับ NA:
  → Duplicate! Address ซ้ำ
  → ไม่สามารถใช้ได้
  → ต้องเลือก address ใหม่
  → Windows/Linux จะแสดง error

ถ้าไม่ได้รับ NA (รอ 1 second):
  → OK! Address ไม่ซ้ำ
  → เปลี่ยนเป็น Valid
  → ใช้ได้
```

**หมายเหตุ:**

- DAD เป็น **mandatory** ใน IPv6
- ทำทุกครั้งก่อนใช้ address
- รวมทั้ง Link-Local และ Global Unicast

---

### ICMPv6 vs ICMPv4

```
Feature              ICMPv4                ICMPv6
------------------------------------------------------------------------
Protocol Number      1                     58
Address Resolution   ARP (separate)        NDP (integrated)
Router Discovery     DHCP/manual           NDP (RS/RA)
Multicast            IGMP (separate)       MLD (integrated)
Fragmentation        Router + Source       Source only
Error Message        Destination           Destination Unreachable,
                     Unreachable,          Packet Too Big,
                     Time Exceeded, etc.   Time Exceeded, etc.
Echo Request/Reply   Type 8/0              Type 128/129
Duplicate Detection  GARP (optional)       DAD (mandatory)
Path MTU Discovery   Optional              Required
------------------------------------------------------------------------
```

---

### ICMPv6 Ping and Traceroute

**Ping IPv6:**

```
Windows:
  ping 2001:db8::1
  ping -6 hostname

Linux/macOS:
  ping6 2001:db8::1

Cisco:
  Router# ping 2001:db8::1
  Router# ping ipv6 2001:db8::1
```

**Ping Link-Local (ต้องระบุ interface):**

```
Windows:
  ping fe80::1%12
       (12 = interface index)

Linux/macOS:
  ping6 fe80::1%eth0

Cisco:
  Router# ping fe80::1%GigabitEthernet0/0
```

**Traceroute IPv6:**

```
Windows:
  tracert 2001:db8::1
  tracert -6 hostname

Linux/macOS:
  traceroute6 2001:db8::1

Cisco:
  Router# traceroute 2001:db8::1
  Router# traceroute ipv6 2001:db8::1
```

---

## 13.5 ICMP Security Considerations

### ICMP Attacks

#### 1. ICMP Flood (Ping Flood)

**คำจำกัดความ:**

- ส่ง ICMP Echo Request **จำนวนมหาศาล**
- เป้าหมาย: **DoS** (Denial of Service)

**การทำงาน:**

```
Attacker ส่ง ping packets หลายล้าน packets
→ Target ต้อง process และตอบกลับทั้งหมด
→ Consume bandwidth และ CPU
→ Target ช้า หรือ down
```

**ป้องกัน:**

- ✅ Rate limiting ICMP
- ✅ Firewall rules
- ✅ IDS/IPS

#### 2. Ping of Death

**คำจำกัดความ:**

- ส่ง ICMP packet **ขนาดใหญ่ผิดปกติ**
- เป้าหมาย: Crash system

**การทำงาน:**

```
ส่ง ICMP Echo Request > 65,535 bytes (fragmented)
→ เมื่อ reassemble: buffer overflow
→ System crash หรือ reboot
```

**หมายเหตุ:**

- Obsolete attack (ไม่ทำงานใน modern OS)
- OS ปัจจุบันป้องกันได้

#### 3. Smurf Attack

**คำจำกัดความ:**

- ใช้ **broadcast** เพื่อ amplify ICMP attack
- เป้าหมาย: **DDoS**

**การทำงาน:**

```
1. Attacker ส่ง ICMP Echo Request:
   Source IP: Victim IP (spoofed)
   Destination: Broadcast address (255.255.255.255 or 192.168.1.255)

2. Router forward ไปยังทุกอุปกรณ์ใน network

3. ทุกอุปกรณ์ส่ง ICMP Echo Reply ไปยัง Victim

4. Victim ถูก flood ด้วย ICMP Echo Reply

Amplification:
  1 packet → 100+ packets (ถ้ามี 100 devices)
```

**ป้องกัน:**

- ✅ **Disable IP directed-broadcast** (Cisco default)
    
    ```
    Router(config)# interface g0/0Router(config-if)# no ip directed-broadcast
    ```
    
- ✅ Filter broadcast at firewall

#### 4. ICMP Redirect Attack

**คำจำกัดความ:**

- ส่ง **fake ICMP Redirect**
- เป้าหมาย: **Man-in-the-Middle**

**การทำงาน:**

```
1. Attacker ส่ง ICMP Redirect ไปยัง Victim:
   "ใช้ Attacker IP เป็น Gateway สำหรับ destination X"

2. Victim update routing table

3. Victim ส่ง traffic → Attacker

4. Attacker intercept และ modify traffic

5. Attacker forward ไปยัง real destination

Victim ไม่รู้ว่าถูกดัก
```

**ป้องกัน:**

- ✅ Ignore ICMP Redirects
    
    ```
    Linux:  sysctl -w net.ipv4.conf.all.accept_redirects=0Windows:  Default: ไม่ accept redirects จาก non-gateway
    ```
    
- ✅ Cisco: ปิดการส่ง redirects
    
    ```
    Router(config)# interface g0/0Router(config-if)# no ip redirects
    ```
    

#### 5. ICMP Tunneling

**คำจำกัดความ:**

- ซ่อนข้อมูล/traffic ใน **ICMP packets**
- เป้าหมาย: Bypass firewall

**การทำงาน:**

```
Attacker ซ่อนข้อมูล (command, file) ใน ICMP Echo Request/Reply
→ ส่งผ่าน firewall (ปกติอนุญาต ICMP)
→ ปลายทาง extract ข้อมูล
→ Covert channel
```

**ป้องกัน:**

- ✅ Deep packet inspection (DPI)
- ✅ IDS/IPS
- ✅ Rate limit ICMP

---

### ICMP Best Practices

#### For Security:

**1. Rate Limiting**

```
Cisco:
  Router(config)# ip icmp rate-limit unreachable 1000
  (อนุญาต 1 packet per 1000ms = 1 packet/second)
```

**2. Filter ICMP (ระมัดระวัง)**

```
❌ ไม่แนะนำ block ICMP ทั้งหมด (จะทำให้ troubleshoot ยาก)

✅ แนะนำ:
  - อนุญาต Echo Request/Reply (ping) แต่ rate limit
  - อนุญาต Time Exceeded, Destination Unreachable (troubleshooting)
  - Block Redirect, Timestamp, Address Mask
```

**3. Disable ICMP Redirect**

```
Router(config-if)# no ip redirects
```

**4. Disable IP Directed Broadcast**

```
Router(config-if)# no ip directed-broadcast
```

#### For Operations:

**1. อนุญาต ICMP สำหรับ Troubleshooting**

```
- Echo Request/Reply (Type 8/0)
- Destination Unreachable (Type 3)
- Time Exceeded (Type 11)
```

**2. Monitor ICMP Traffic**

```
- ดูว่ามี ICMP traffic ผิดปกติหรือไม่
- High volume = possible attack
```

**3. Log ICMP Events**

```
- Log ICMP unreachable, redirects
- ช่วย troubleshooting
```

---

## Summary (สรุป)

Module 13 นี้เราได้เรียนรู้:

1. ✅ **ICMP Overview** - Error reporting และ Diagnostics protocol
2. ✅ **ICMP Message Types**:
    - Error: Destination Unreachable, Time Exceeded, Redirect, Parameter Problem
    - Informational: Echo Request/Reply
3. ✅ **Ping** - ทดสอบ connectivity, ใช้ ICMP Echo Request/Reply
4. ✅ **Traceroute** - ติดตามเส้นทาง, ใช้ TTL และ ICMP Time Exceeded
5. ✅ **ICMPv6** - ICMP สำหรับ IPv6, รวม NDP, DAD
6. ✅ **NDP Messages** - RS/RA, NS/NA (แทน ARP)
7. ✅ **ICMP Security** - Attacks และ Best Practices

**สิ่งสำคัญที่ต้องจำ:**

- ICMP = Layer 3 protocol สำหรับ error reporting
- ICMP ไม่ทำให้ IP reliable (แค่รายงานปัญหา)
- Ping = ICMP Type 8 (Request) / Type 0 (Reply)
- Traceroute = ใช้ TTL + ICMP Type 11 (Time Exceeded)
- Type 3 = Destination Unreachable (หลาย Codes)
- Type 11 Code 0 = TTL Exceeded (routing loop หรือ traceroute)
- ICMPv6 = รวม NDP (แทน ARP), DAD (mandatory)
- NDP: RS/RA (Router Discovery), NS/NA (Address Resolution)
- DAD = Duplicate Address Detection (ใช้ NS)
- ICMP attacks: Flood, Ping of Death, Smurf, Redirect, Tunneling
- Best Practice: Rate limit, ไม่ block ทั้งหมด, Monitor

**Key Commands:**

```
ping <destination>
tracert <destination>  (Windows)
traceroute <destination>  (Linux/macOS/Cisco)
ping6 <IPv6 address>  (Linux/macOS)
traceroute6 <IPv6 address>  (Linux/macOS)
```

**Next Module:** Module 14 - Transport Layer

---

**[ไฟล์ Module 13 สมบูรณ์แล้ว!]**