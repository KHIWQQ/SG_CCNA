# CCNA Course 1 - Module 9: Address Resolution

## การแปลงที่อยู่

---

## 9.1 MAC and IP (MAC และ IP)

### Two Addresses in Communication

**เมื่อส่งข้อมูลในเครือข่าย ต้องใช้ 2 ที่อยู่:**

#### 1. IP Address (Logical Address)

**คำจำกัดความ:**

- ที่อยู่ **Layer 3** (Network Layer)
- ใช้สำหรับ **end-to-end delivery** (จากต้นทางถึงปลายทาง)
- เปลี่ยนแปลงไม่ได้ตลอดการเดินทาง

**ลักษณะ:**

- IPv4: 32 bits (192.168.1.10)
- IPv6: 128 bits (2001:db8::1)
- **Routable** - Router สามารถ forward ข้ามเครือข่ายได้

**ตัวอย่าง:**

```
PC1 (192.168.1.10) ส่งไปยัง Server (8.8.8.8)

Packet:
  Source IP: 192.168.1.10        ← ไม่เปลี่ยนตลอดทาง
  Destination IP: 8.8.8.8        ← ไม่เปลี่ยนตลอดทาง
```

#### 2. MAC Address (Physical Address)

**คำจำกัดความ:**

- ที่อยู่ **Layer 2** (Data Link Layer)
- ใช้สำหรับ **local delivery** (hop-by-hop)
- **เปลี่ยนทุก hop** (ทุกครั้งที่ผ่าน Router)

**ลักษณะ:**

- 48 bits (00:1A:2B:3C:4D:5E)
- **Non-routable** - ใช้ได้เฉพาะ local network
- Burned into NIC

**ตัวอย่าง:**

```
PC1 → R1 → R2 → Server

Hop 1 (PC1 → R1):
  Frame: Src MAC = PC1, Dst MAC = R1
  Packet: Src IP = PC1, Dst IP = Server

Hop 2 (R1 → R2):
  Frame: Src MAC = R1, Dst MAC = R2       ← MAC เปลี่ยน
  Packet: Src IP = PC1, Dst IP = Server   ← IP ยังเดิม

Hop 3 (R2 → Server):
  Frame: Src MAC = R2, Dst MAC = Server   ← MAC เปลี่ยนอีก
  Packet: Src IP = PC1, Dst IP = Server   ← IP ยังเดิม
```

---

### Why Two Addresses?

**คำถาม:** ทำไมต้องมีทั้ง IP และ MAC?

**เหตุผล:**

#### 1. Different Scopes (ขอบเขตต่างกัน)

- **IP Address:** Global (ทั่วโลก) - หาทางข้ามเครือข่าย
- **MAC Address:** Local (ในเครือข่าย) - ส่งในเครือข่ายเดียว

#### 2. Different Layers (ชั้นต่างกัน)

- **IP:** Layer 3 - Routing, end-to-end
- **MAC:** Layer 2 - Switching, hop-by-hop

#### 3. Separation of Concerns (แยกความรับผิดชอบ)

- **IP:** รู้ว่าจะส่งไปไหน (destination)
- **MAC:** รู้ว่าจะส่งให้ใคร (next hop)

**Analogy:**

```
IP Address = ที่อยู่บ้าน (บอกว่าต้องการส่งไปไหน)
  "เลขที่ 123 ถนนสุขุมวิท กรุงเทพ"

MAC Address = ชื่อคนขับรถส่งของ (ใครรับของในแต่ละขั้น)
  "คนขับคันที่ 1 → คนขับคันที่ 2 → คนขับคันที่ 3 → ถึงบ้าน"
```

---

### Address Comparison

```
Feature          IP Address              MAC Address
------------------------------------------------------------------------
Layer            Layer 3 (Network)       Layer 2 (Data Link)
Purpose          End-to-end delivery     Hop-by-hop delivery
Scope            Global/Routable         Local/Non-routable
Format           Dotted decimal (IPv4)   Hexadecimal (6 bytes)
                 Hex (IPv6)              
Size             32 bits (IPv4)          48 bits
                 128 bits (IPv6)
Assigned by      Admin/DHCP              Manufacturer (burned-in)
Changes          During transit: NO      During transit: YES (every hop)
Used by          Router                  Switch
Example          192.168.1.10            00:1A:2B:3C:4D:5E
                 2001:db8::1
------------------------------------------------------------------------
```

---

### Communication Example

**Scenario:** PC1 ส่งข้อมูลไปยัง Web Server ผ่าน 2 routers

**Topology:**

```
[PC1] ---[SW1]--- [R1] --- [R2] --- [SW2]--- [Server]
10.1.1.10         .1    .1  .2   .2   .1      10.3.3.10
```

**IP Addresses:**

```
PC1:    10.1.1.10
R1 G0/0: 10.1.1.1
R1 G0/1: 10.2.2.1
R2 G0/0: 10.2.2.2
R2 G0/1: 10.3.3.1
Server: 10.3.3.10
```

**Segment 1: PC1 → R1**

```
Frame:
  Src MAC: AA:AA:AA:AA:AA:AA (PC1)
  Dst MAC: 11:11:11:11:11:11 (R1 G0/0)
Packet:
  Src IP: 10.1.1.10 (PC1)
  Dst IP: 10.3.3.10 (Server)
```

**Segment 2: R1 → R2**

```
Frame:
  Src MAC: 22:22:22:22:22:22 (R1 G0/1)
  Dst MAC: 33:33:33:33:33:33 (R2 G0/0)
Packet:
  Src IP: 10.1.1.10 (PC1)        ← ยังเดิม
  Dst IP: 10.3.3.10 (Server)     ← ยังเดิม
```

**Segment 3: R2 → Server**

```
Frame:
  Src MAC: 44:44:44:44:44:44 (R2 G0/1)
  Dst MAC: BB:BB:BB:BB:BB:BB (Server)
Packet:
  Src IP: 10.1.1.10 (PC1)        ← ยังเดิม
  Dst IP: 10.3.3.10 (Server)     ← ยังเดิม
```

**สังเกต:**

- ✅ **IP addresses ไม่เปลี่ยน** ตลอดทาง
- ✅ **MAC addresses เปลี่ยนทุก hop**

---

## 9.2 ARP (Address Resolution Protocol)

### ARP Overview

**คำจำกัดความ:**

- โปรโตคอลที่ใช้หา **MAC address** จาก **IP address**
- ทำงานที่ Layer 2-3
- ใช้ใน **IPv4** เท่านั้น (IPv6 ใช้ NDP)

**ปัญหาที่ ARP แก้:**

```
PC1 รู้ว่าต้องการส่งไปยัง 192.168.1.20
แต่ไม่รู้ว่า MAC address ของ 192.168.1.20 คืออะไร

→ ใช้ ARP หา MAC address!
```

---

### ARP Message Types

#### 1. ARP Request (การร้องขอ)

**คำจำกัดความ:**

- ถามว่า "ใครมี IP address นี้?"
- ส่งแบบ **Broadcast** (ทุกคนได้รับ)

**รูปแบบ:**

```
ARP Request:
  Sender MAC: AA:AA:AA:AA:AA:AA
  Sender IP:  192.168.1.10
  Target MAC: 00:00:00:00:00:00 (unknown - ว่างเปล่า)
  Target IP:  192.168.1.20

Frame:
  Src MAC:  AA:AA:AA:AA:AA:AA (PC1)
  Dst MAC:  FF:FF:FF:FF:FF:FF (Broadcast!)
  Type:     0x0806 (ARP)
```

**ส่งไปยังใคร:**

- **Broadcast** - ทุกอุปกรณ์ในเครือข่ายเดียวกัน

#### 2. ARP Reply (การตอบกลับ)

**คำจำกัดความ:**

- ตอบว่า "ฉันมี IP นี้ และ MAC ของฉันคือ..."
- ส่งแบบ **Unicast** (ตรงไปยังผู้ถาม)

**รูปแบบ:**

```
ARP Reply:
  Sender MAC: BB:BB:BB:BB:BB:BB
  Sender IP:  192.168.1.20
  Target MAC: AA:AA:AA:AA:AA:AA (PC1)
  Target IP:  192.168.1.10

Frame:
  Src MAC:  BB:BB:BB:BB:BB:BB (PC2)
  Dst MAC:  AA:AA:AA:AA:AA:AA (PC1) ← Unicast
  Type:     0x0806 (ARP)
```

**ส่งไปยังใคร:**

- **Unicast** - เฉพาะผู้ที่ถาม (PC1)

---

### ARP Process (กระบวนการ)

**Scenario:** PC1 (192.168.1.10) ต้องการส่งข้อมูลไปยัง PC2 (192.168.1.20)

**ขั้นตอน:**

#### Step 1: Check ARP Cache

```
PC1 ตรวจสอบ ARP cache:
  มี entry สำหรับ 192.168.1.20 หรือไม่?

ถ้ามี → ใช้ MAC จาก cache (ไม่ต้อง ARP)
ถ้าไม่มี → ต้อง ARP
```

#### Step 2: Send ARP Request

```
PC1 broadcast ARP Request:
  "ใครมี IP 192.168.1.20? บอก 192.168.1.10 (MAC: AA:AA:AA:AA:AA:AA)"

Frame:
  Src MAC: AA:AA:AA:AA:AA:AA
  Dst MAC: FF:FF:FF:FF:FF:FF (Broadcast)

Switch flood ออกทุก port (ยกเว้น port ที่รับเข้ามา)
→ PC2, PC3, PC4, ... ทุกคนได้รับ
```

#### Step 3: Process ARP Request

```
PC2 (192.168.1.20):
  "เป็นฉันเอง! IP ของฉันคือ 192.168.1.20"
  → ส่ง ARP Reply กลับ

PC3, PC4, ... (IP อื่น):
  "ไม่ใช่ฉัน" → ไม่ตอบ, discard ARP Request
```

#### Step 4: Send ARP Reply

```
PC2 unicast ARP Reply ไปยัง PC1:
  "IP 192.168.1.20 อยู่ที่ MAC: BB:BB:BB:BB:BB:BB"

Frame:
  Src MAC: BB:BB:BB:BB:BB:BB
  Dst MAC: AA:AA:AA:AA:AA:AA (Unicast)
```

#### Step 5: Update ARP Cache

```
PC1 รับ ARP Reply:
  - บันทึกลง ARP cache:
    192.168.1.20 → BB:BB:BB:BB:BB:BB
  - สามารถส่งข้อมูลได้แล้ว!
```

#### Step 6: Send Data

```
PC1 ส่งข้อมูลไปยัง PC2:
Frame:
  Src MAC: AA:AA:AA:AA:AA:AA
  Dst MAC: BB:BB:BB:BB:BB:BB (ได้จาก ARP)
Packet:
  Src IP: 192.168.1.10
  Dst IP: 192.168.1.20
```

---

### ARP Packet Format

**ARP Message:**

```
+------------------+------------------------+
| Ethernet Header  | ARP Packet             |
+------------------+------------------------+

Ethernet Header:
  Destination MAC: FF:FF:FF:FF:FF:FF (Request) or specific MAC (Reply)
  Source MAC: Sender's MAC
  Type: 0x0806 (ARP)

ARP Packet:
  Hardware Type: 1 (Ethernet)
  Protocol Type: 0x0800 (IPv4)
  Hardware Size: 6 (MAC = 6 bytes)
  Protocol Size: 4 (IPv4 = 4 bytes)
  Opcode: 1 (Request) or 2 (Reply)
  Sender MAC: MAC address ของผู้ส่ง
  Sender IP: IP address ของผู้ส่ง
  Target MAC: MAC address ของเป้าหมาย (00:00:00:00:00:00 ใน Request)
  Target IP: IP address ของเป้าหมาย
```

**Opcode:**

- **1** = ARP Request
- **2** = ARP Reply
- **3** = RARP Request (Reverse ARP - obsolete)
- **4** = RARP Reply

---

### ARP Cache (ARP Table)

**คำจำกัดความ:**

- ตารางที่เก็บ **mapping ระหว่าง IP กับ MAC**
- เก็บไว้ใน **RAM** (memory)
- ป้องกันต้อง ARP ทุกครั้ง

**ข้อมูลที่เก็บ:**

```
IP Address        MAC Address           Type        Age
------------------------------------------------------------------------
192.168.1.1       AA:11:22:33:44:55     Dynamic     60s
192.168.1.20      BB:AA:BB:CC:DD:EE     Dynamic     30s
192.168.1.100     CC:CC:CC:CC:CC:CC     Static      -
```

**Entry Types:**

- **Dynamic:** ได้จาก ARP, มี timeout (หมดอายุ)
- **Static:** กำหนดด้วยตนเอง, ไม่หมดอายุ

**Timeout (Aging Time):**

- **Windows:** 2 minutes (idle), 10 minutes (used)
- **Linux:** 60 seconds (default)
- **Cisco:** 4 hours (14400 seconds)

**เมื่อ entry หมดอายุ:**

- ถูกลบออกจาก cache
- ครั้งต่อไปต้อง ARP ใหม่

---

### Viewing ARP Cache

#### Windows

```cmd
arp -a
```

**Output:**

```
Interface: 192.168.1.10 --- 0x2
  Internet Address      Physical Address      Type
  192.168.1.1           aa-11-22-33-44-55     dynamic
  192.168.1.20          bb-aa-bb-cc-dd-ee     dynamic
  192.168.1.255         ff-ff-ff-ff-ff-ff     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.251           01-00-5e-00-00-fb     static
```

#### Linux/macOS

```bash
arp -n
หรือ
ip neighbor show
```

**Output:**

```
192.168.1.1 dev eth0 lladdr aa:11:22:33:44:55 REACHABLE
192.168.1.20 dev eth0 lladdr bb:aa:bb:cc:dd:ee STALE
```

#### Cisco IOS

```
Router# show ip arp
หรือ
Switch# show mac address-table
```

**Output:**

```
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  192.168.1.1             0   aa11.2233.4455  ARPA   Vlan1
Internet  192.168.1.10            5   bbaa.bbcc.ddee  ARPA   Vlan1
Internet  10.0.0.1                -   0011.2233.4455  ARPA   GigabitEthernet0/0
```

---

### Managing ARP Cache

#### Clear ARP Cache

**Windows:**

```cmd
arp -d
หรือ
netsh interface ip delete arpcache
```

**Linux:**

```bash
sudo ip -s -s neigh flush all
```

**Cisco:**

```
Router# clear ip arp
```

#### Add Static ARP Entry

**Windows:**

```cmd
arp -s 192.168.1.20 BB-AA-BB-CC-DD-EE
```

**Linux:**

```bash
sudo arp -s 192.168.1.20 BB:AA:BB:CC:DD:EE
```

**Cisco:**

```
Router(config)# arp 192.168.1.20 bbaa.bbcc.ddee ARPA
```

**เมื่อไหร่ใช้ Static ARP:**

- Security - ป้องกัน ARP spoofing
- Critical devices - อุปกรณ์สำคัญ (Gateway, Server)
- Troubleshooting

---

### ARP for Different Scenarios

#### Scenario 1: Local Destination (Same Network)

**Topology:**

```
[PC1: 192.168.1.10] ----[Switch]---- [PC2: 192.168.1.20]
```

**Process:**

```
1. PC1 ต้องการส่งไปยัง 192.168.1.20
2. PC1 เช็ค: 192.168.1.20 อยู่ local network หรือไม่?
   → YES (same network)
3. PC1 ARP for 192.168.1.20
4. PC2 reply MAC address
5. PC1 ส่งข้อมูลตรงไปยัง PC2
```

#### Scenario 2: Remote Destination (Different Network)

**Topology:**

```
[PC1: 192.168.1.10] ----[Router: 192.168.1.1]---- [Internet]
Gateway: 192.168.1.1
```

**Process:**

```
1. PC1 ต้องการส่งไปยัง 8.8.8.8
2. PC1 เช็ค: 8.8.8.8 อยู่ local network หรือไม่?
   → NO (different network)
3. PC1 ARP for Default Gateway (192.168.1.1)
4. Router reply MAC address
5. PC1 ส่ง frame ไปยัง Router (MAC ของ Router)
   แต่ Packet ยัง Dst IP = 8.8.8.8
```

**สำคัญ:**

- **Remote destination:** ARP for **Gateway** (ไม่ใช่ destination)
- Packet Dst IP = Destination
- Frame Dst MAC = Gateway

---

### Gratuitous ARP (GARP)

**คำจำกัดความ:**

- ARP Request ที่ส่ง**โดยไม่มีใครถาม**
- Sender IP = Target IP (ตัวเอง)

**วัตถุประสงค์:**

#### 1. Update ARP Caches

```
เมื่ออุปกรณ์:
- เปลี่ยน MAC address (เปลี่ยน NIC)
- เปลี่ยน IP address
- Interface up after down

→ ส่ง GARP เพื่อบอกทุกคนว่า "IP นี้เป็นของฉัน, MAC ใหม่คือนี่"
```

#### 2. Duplicate IP Detection

```
อุปกรณ์ boot up:
- ก่อนใช้ IP, ส่ง GARP
- ถ้ามีใครตอบ → IP ซ้ำ! (Duplicate IP)
- ถ้าไม่มีใครตอบ → OK, ใช้ได้
```

#### 3. High Availability (VRRP, HSRP)

```
Standby Router เป็น Active:
- ส่ง GARP with Virtual IP
- Update ทุกคนว่า Virtual IP ตอนนี้อยู่ที่ MAC ของตัวเอง
```

**ตัวอย่าง GARP:**

```
ARP Request:
  Sender MAC: AA:AA:AA:AA:AA:AA
  Sender IP:  192.168.1.10
  Target MAC: 00:00:00:00:00:00
  Target IP:  192.168.1.10 (ตัวเอง!)

Dst MAC: FF:FF:FF:FF:FF:FF (Broadcast)
```

---

### ARP Issues and Security

#### ARP Spoofing (ARP Poisoning)

**คำจำกัดความ:**

- ผู้โจมตีส่ง **fake ARP messages**
- หลอกให้เหยื่อส่ง traffic มาที่ attacker

**วิธีการ:**

```
Normal:
PC1 ARP cache: 192.168.1.1 → Gateway_MAC

Attack:
Attacker ส่ง fake ARP Reply:
  "192.168.1.1 (Gateway) อยู่ที่ Attacker_MAC"

PC1 ARP cache: 192.168.1.1 → Attacker_MAC (ถูกหลอก!)

PC1 ส่งข้อมูล → Attacker (แทน Gateway)
Attacker สามารถ:
- ดัก traffic (Man-in-the-Middle)
- แก้ไข traffic
- Forward ต่อไป Gateway (เหยื่อไม่รู้)
```

**ป้องกัน:**

- ✅ **Static ARP entries** - กำหนด Gateway MAC แบบ static
- ✅ **Dynamic ARP Inspection (DAI)** - Switch ตรวจสอบ ARP messages
- ✅ **Port Security** - จำกัด MAC address ต่อ port
- ✅ **ARP monitoring tools** - ตรวจจับ ARP anomalies

#### ARP Flooding

**คำจำกัดความ:**

- ส่ง ARP messages จำนวนมหาศาล
- ทำให้ Switch ARP table เต็ม

**ผลกระทบ:**

- Switch ต้อง flood ทุก frame (performance ลด)
- DoS (Denial of Service)

**ป้องกัน:**

- ✅ **Rate limiting** - จำกัดจำนวน ARP messages
- ✅ **Storm control** - จำกัด broadcast traffic

---

## 9.3 IPv6 Neighbor Discovery (การค้นหาเพื่อนบ้าน IPv6)

### IPv6 Neighbor Discovery Protocol (NDP)

**คำจำกัดความ:**

- ทำหน้าที่เหมือน **ARP ใน IPv4**
- แต่มีความสามารถมากกว่า
- ใช้ **ICMPv6** messages

**ฟังก์ชัน:**

1. **Address Resolution** - หา MAC จาก IPv6 (เหมือน ARP)
2. **Router Discovery** - หา Default Gateway
3. **Prefix Discovery** - หา Network prefix
4. **Parameter Discovery** - หา MTU, hop limit, etc.
5. **Duplicate Address Detection (DAD)** - ตรวจจับ IP ซ้ำ
6. **Neighbor Reachability** - ตรวจสอบว่า neighbor ยัง reachable
7. **Redirect** - แจ้งเส้นทางที่ดีกว่า

---

### NDP Message Types

**ใช้ ICMPv6 (Type 133-137):**

#### 1. Router Solicitation (RS) - Type 133

**คำจำกัดความ:**

- Host ถาม "มี Router ไหม?"
- ส่งแบบ **Multicast**

**ส่งไปยัง:**

- `ff02::2` (All-Routers multicast address)

**เมื่อไหร่ส่ง:**

- Interface up ครั้งแรก
- ต้องการหา Gateway เร็ว (ไม่รอ RA)

#### 2. Router Advertisement (RA) - Type 134

**คำจำกัดความ:**

- Router ตอบ "ฉันคือ Router"
- บอก prefix, Gateway, MTU, hop limit, etc.

**ส่งไปยัง:**

- `ff02::1` (All-Nodes multicast) - periodic
- หรือ Unicast ตอบ RS

**ส่งเมื่อไหร่:**

- **Periodic** - ทุก 200 seconds (default)
- **Response to RS** - เมื่อได้รับ RS

**ข้อมูลใน RA:**

```
- Router Lifetime
- Reachable Time
- Retransmit Timer
- Prefix Information
- MTU
- Hop Limit
```

#### 3. Neighbor Solicitation (NS) - Type 135

**คำจำกัดความ:**

- เหมือน **ARP Request**
- หา MAC address จาก IPv6 address

**ส่งไปยัง:**

- **Solicited-Node Multicast Address**
- รูปแบบ: `ff02::1:ff + (last 24 bits of target IPv6)`

**ตัวอย่าง:**

```
Target IPv6: 2001:db8::1234:5678:9abc:def0

Solicited-Node Multicast:
  ff02::1:ff + 9abc:def0
  = ff02::1:ff9a:bcde:f0
```

**ใช้สำหรับ:**

- Address Resolution (หา MAC)
- Duplicate Address Detection (DAD)
- Neighbor Reachability (ยัง alive?)

#### 4. Neighbor Advertisement (NA) - Type 136

**คำจำกัดความ:**

- เหมือน **ARP Reply**
- ตอบพร้อม MAC address

**ส่งไปยัง:**

- **Unicast** (ตอบ NS)
- **Multicast** `ff02::1` (unsolicited - GARP-like)

**Flags:**

- **R** (Router): ผู้ส่งเป็น Router
- **S** (Solicited): ตอบจาก NS
- **O** (Override): อัปเดต existing cache entry

#### 5. Redirect - Type 137

**คำจำกัดความ:**

- Router บอก Host ว่า "มีเส้นทางที่ดีกว่า"
- เหมือน ICMP Redirect ใน IPv4

---

### NDP Address Resolution Process

**Scenario:** PC1 (2001:db8::1) ต้องการส่งไปยัง PC2 (2001:db8::2)

**ขั้นตอน:**

#### Step 1: Check Neighbor Cache

```
PC1 เช็ค Neighbor cache:
  มี entry สำหรับ 2001:db8::2 หรือไม่?

ถ้ามี → ใช้ MAC จาก cache
ถ้าไม่มี → ส่ง NS
```

#### Step 2: Send Neighbor Solicitation (NS)

```
PC1 ส่ง NS:
  Target: 2001:db8::2
  Dst IPv6: ff02::1:ff00:2 (Solicited-Node Multicast)
  Dst MAC: 33:33:ff:00:00:02 (Multicast MAC)
  ICMPv6 Type: 135 (NS)
  Message: "ใครมี 2001:db8::2? MAC ของฉันคือ AA:AA:AA:AA:AA:AA"
```

#### Step 3: Receive and Process NS

```
PC2 (2001:db8::2):
  "เป็นฉันเอง!"
  → ส่ง NA กลับ

PC3, PC4 (IPv6 อื่น):
  "ไม่ใช่ฉัน" → ไม่ตอบ
```

#### Step 4: Send Neighbor Advertisement (NA)

```
PC2 ส่ง NA (Unicast):
  Target: 2001:db8::2
  Dst IPv6: 2001:db8::1 (PC1)
  Dst MAC: AA:AA:AA:AA:AA:AA (PC1)
  ICMPv6 Type: 136 (NA)
  Message: "MAC ของ 2001:db8::2 คือ BB:BB:BB:BB:BB:BB"
  Flags: S=1 (Solicited), O=1 (Override)
```

#### Step 5: Update Neighbor Cache

```
PC1 บันทึก:
  2001:db8::2 → BB:BB:BB:BB:BB:BB

พร้อมส่งข้อมูล!
```

---

### IPv6 Neighbor Cache

**คำจำกัดความ:**

- เหมือน ARP cache ใน IPv4
- เก็บ mapping ระหว่าง IPv6 และ MAC

**States of Neighbor Cache Entry:**

#### 1. INCOMPLETE

- NS ถูกส่งแล้ว แต่ยังไม่ได้ NA
- รอ response

#### 2. REACHABLE

- NA ได้รับแล้ว
- Entry ใช้ได้ (valid)
- Timeout: 30 seconds (default)

#### 3. STALE

- Entry หมดอายุ (เกิน REACHABLE time)
- ยังใช้ได้ แต่ควรตรวจสอบ

#### 4. DELAY

- กำลังรอการยืนยัน reachability
- Timeout: 5 seconds

#### 5. PROBE

- ส่ง NS เพื่อตรวจสอบ reachability
- ถ้าไม่ได้ NA → ลบ entry

**State Transition:**

```
         NS sent
INCOMPLETE ────────→ REACHABLE
                          ↓
                      (timeout)
                          ↓
                       STALE
                          ↓
                   (send traffic)
                          ↓
                       DELAY
                          ↓
                    (no confirm)
                          ↓
                       PROBE
                          ↓
                    (no response)
                          ↓
                       DELETE
```

---

### Viewing IPv6 Neighbor Cache

#### Windows

```cmd
netsh interface ipv6 show neighbors
```

**Output:**

```
Interface 11: Ethernet

Internet Address                   Physical Address   Type
---------------------------------------------------------
2001:db8::1                        aa-11-22-33-44-55  Reachable
fe80::1                            aa-11-22-33-44-55  Reachable
2001:db8::2                        bb-aa-bb-cc-dd-ee  Stale
```

#### Linux

```bash
ip -6 neighbor show
```

**Output:**

```
2001:db8::1 dev eth0 lladdr aa:11:22:33:44:55 REACHABLE
2001:db8::2 dev eth0 lladdr bb:aa:bb:cc:dd:ee STALE
fe80::1 dev eth0 lladdr aa:11:22:33:44:55 router REACHABLE
```

#### Cisco IOS

```
Router# show ipv6 neighbors
```

**Output:**

```
IPv6 Address                      Age Link-layer Addr State Interface
2001:db8::1                         0 aa11.2233.4455  REACH Gi0/0
2001:db8::2                         5 bbaa.bbcc.ddee  STALE Gi0/0
fe80::1                             - aa11.2233.4455  REACH Gi0/0
```

---

### Duplicate Address Detection (DAD)

**คำจำกัดความ:**

- ตรวจสอบว่า IPv6 address ซ้ำหรือไม่
- ทำก่อนใช้ IPv6 address **ทุกครั้ง**

**กระบวนการ:**

#### Step 1: Assign Tentative Address

```
Host กำหนด IPv6 address (SLAAC, DHCPv6, Manual)
  → Status: Tentative (ยังใช้ไม่ได้)
```

#### Step 2: Send NS for DAD

```
Host ส่ง NS:
  Source IPv6: :: (unspecified - ยังไม่มี IP)
  Target IPv6: Tentative address (IP ที่ต้องการใช้)
  Dst IPv6: Solicited-Node Multicast of tentative address

ถาม: "มีใครใช้ IP นี้อยู่หรือไม่?"
```

#### Step 3: Wait for Response

```
ถ้าได้รับ NA:
  → Duplicate! IP ซ้ำ
  → ไม่สามารถใช้ address นี้ได้
  → ต้องเลือก IP ใหม่

ถ้าไม่ได้รับ NA (หลัง 1 second):
  → OK! ไม่ซ้ำ
  → Address เปลี่ยนเป็น Valid
  → ใช้ได้แล้ว
```

**ตัวอย่าง DAD:**

```
NS for DAD:
  Source IPv6: ::
  Dest IPv6: ff02::1:ff00:1
  Target: 2001:db8::1
  Message: "มีใครใช้ 2001:db8::1 อยู่หรือไม่?"

ถ้าไม่มีใครตอบ → OK, ใช้ได้!
ถ้ามีคนตอบ → Duplicate address detected!
```

---

### Router Discovery Process

**เมื่อ Host boot up:**

#### Method 1: Host ส่ง RS (Proactive)

```
Step 1: Host ส่ง Router Solicitation (RS)
  Dst: ff02::2 (All-Routers)
  Message: "มี Router ไหม?"

Step 2: Router ส่ง Router Advertisement (RA)
  Dst: PC (Unicast) or ff02::1 (Multicast)
  Message: "ฉันคือ Router, นี่คือข้อมูล..."
    - Prefix: 2001:db8::/64
    - Gateway: fe80::1
    - MTU: 1500
    - Hop Limit: 64
    - Flags: A (Auto-configuration), O (Other config)
```

#### Method 2: รอ RA (Passive)

```
Router ส่ง RA ทุก 200 seconds (default)
  Dst: ff02::1 (All-Nodes)

Host รับ RA:
  - รู้ว่ามี Router
  - ได้ Default Gateway
  - ได้ Prefix (สามารถ auto-config IP)
```

---

### SLAAC (Stateless Address Auto-Configuration)

**คำจำกัดความ:**

- Host **สร้าง IPv6 address อัตโนมัติ**
- ไม่ต้อง DHCPv6 server
- ใช้ **RA prefix + Interface ID**

**กระบวนการ:**

#### Step 1: รับ RA

```
Router ส่ง RA:
  Prefix: 2001:db8:1:1::/64
  Flag A=1 (Auto-configuration allowed)
```

#### Step 2: สร้าง Interface ID

```
Method 1: EUI-64
  - ใช้ MAC address (48 bits)
  - แปลงเป็น 64 bits
  - Flip bit ที่ 7

  ตัวอย่าง:
  MAC: AA:BB:CC:DD:EE:FF
  EUI-64: A8BB:CCFF:FEDD:EEFF

Method 2: Random (Privacy Extensions)
  - สุ่ม 64 bits
  - เปลี่ยนทุกครั้งที่ต่อเครือข่ายใหม่
```

#### Step 3: รวม Prefix + Interface ID

```
Prefix: 2001:db8:1:1::/64
Interface ID: A8BB:CCFF:FEDD:EEFF

IPv6 Address: 2001:db8:1:1:a8bb:ccff:fedd:eeff/64
```

#### Step 4: DAD

```
ตรวจสอบว่า IP ซ้ำหรือไม่ (DAD)
ถ้าไม่ซ้ำ → ใช้ได้!
```

#### Step 5: Register Link-Local

```
Host มี 2 IPv6 addresses:
1. Global Unicast: 2001:db8:1:1:a8bb:ccff:fedd:eeff/64
2. Link-Local: fe80::a8bb:ccff:fedd:eeff
```

---

### NDP vs ARP Comparison

```
Feature              ARP (IPv4)           NDP (IPv6)
------------------------------------------------------------------------
Protocol             ARP                  ICMPv6
Layer                Layer 2/3            Layer 3 (ICMPv6)
Request/Reply        Broadcast/Unicast    Multicast/Unicast
Request message      ARP Request          Neighbor Solicitation (NS)
Reply message        ARP Reply            Neighbor Advertisement (NA)
Duplicate detection  GARP (optional)      DAD (mandatory)
Router discovery     DHCP/Manual          RS/RA (built-in)
Auto-configuration   DHCP                 SLAAC (built-in)
Security             None (vulnerable)    IPsec support, SEND
Efficiency           Broadcast            Multicast (better)
------------------------------------------------------------------------
```

**ข้อดีของ NDP:**

- ✅ **Multicast** แทน Broadcast (efficient กว่า)
- ✅ **Built-in Router Discovery** (ไม่ต้อง DHCP)
- ✅ **Mandatory DAD** (ลด duplicate IP)
- ✅ **SLAAC** - Auto-configuration (ไม่ต้อง DHCP server)
- ✅ **Neighbor Reachability** (ตรวจสอบว่ายัง alive)
- ✅ **Security** - SEND (Secure Neighbor Discovery)

---

## 9.4 Address Resolution Issues (ปัญหาการแปลงที่อยู่)

### Common Issues

#### 1. Incorrect IP Configuration

**ปัญหา:**

- IP address, Subnet mask, หรือ Default Gateway ผิด

**อาการ:**

- ไม่สามารถติดต่อ remote networks
- ARP for wrong gateway (ไม่มีใครตอบ)

**ตัวอย่าง:**

```
PC1:
  IP: 192.168.1.10/24
  Gateway: 192.168.2.1 (ผิด! ควรเป็น 192.168.1.1)

PC1 ARP for 192.168.2.1:
  → ไม่มีใครตอบ (192.168.2.1 อยู่คนละ network)
  → Cannot reach remote networks
```

**แก้ไข:**

```
ตรวจสอบ:
- IP address ถูกต้อง
- Subnet mask ถูกต้อง
- Default Gateway ถูกต้อง (อยู่ same network)

Windows:
  ipconfig /all

Linux:
  ip addr show
  ip route show
```

#### 2. Duplicate IP Address

**ปัญหา:**

- 2 devices ใช้ IP address เดียวกัน

**อาการ:**

- Intermittent connectivity (ต่อได้บ้าง ไม่ได้บ้าง)
- ARP cache ถูกอัปเดตสลับกันไปมา
- Error message: "IP address conflict"

**วิเคราะห์:**

```
PC1 และ PC2 ใช้ IP เดียวกัน: 192.168.1.10

Router ARP cache สลับไปมา:
  192.168.1.10 → MAC_PC1
  (หลังจากนั้น)
  192.168.1.10 → MAC_PC2

Traffic สลับส่งไป PC1 และ PC2
```

**แก้ไข:**

```
1. ตรวจหา duplicate IP:
   Windows: ipconfig /all
   Linux: ip addr show

2. เปลี่ยน IP ของ device หนึ่ง

3. Clear ARP cache:
   arp -d

4. ป้องกัน:
   - ใช้ DHCP (มีการตรวจสอบ duplicate)
   - IP Address Management (IPAM)
```

#### 3. ARP Cache Timeout

**ปัญหา:**

- ARP entry หมดอายุ
- ต้อง ARP ใหม่ทุกครั้ง → delay

**อาการ:**

- First packet delay (packet แรกช้า)
- Especially with short ARP timeout

**ตัวอย่าง:**

```
PC1 ping Server:
  Packet 1: ARP first → delay 5-10ms
  Packet 2-5: Use cache → delay 1ms

ถ้า ARP timeout สั้น → ต้อง ARP บ่อย
```

**แก้ไข:**

```
เพิ่ม ARP timeout:

Linux:
  echo 3600 > /proc/sys/net/ipv4/neigh/eth0/base_reachable_time_ms

หรือใช้ Static ARP:
  arp -s 192.168.1.100 AA:BB:CC:DD:EE:FF
```

#### 4. Incorrect Gateway MAC

**ปัญหา:**

- Host มี MAC address ผิดสำหรับ Gateway
- อาจเกิดจาก ARP spoofing หรือ configuration ผิด

**อาการ:**

- ไม่สามารถเข้าถึง remote networks
- Local network OK, Internet ไม่ได้

**ตรวจสอบ:**

```
Windows:
  arp -a
  ดู MAC ของ Default Gateway (192.168.1.1)
  เทียบกับ MAC จริงของ Router

ถ้าไม่ตรง → Clear ARP และ ARP ใหม่:
  arp -d 192.168.1.1
  ping 192.168.1.1
  arp -a
```

#### 5. VLAN Mismatch

**ปัญหา:**

- PC และ Gateway อยู่คนละ VLAN
- ARP broadcast ไม่ถึง Gateway

**ตัวอย่าง:**

```
PC1: Port Fa0/1, VLAN 10
Gateway: Port Fa0/24, VLAN 20

PC1 broadcast ARP:
  → Switch forward เฉพาะใน VLAN 10
  → Gateway (VLAN 20) ไม่ได้รับ
  → ไม่มี ARP Reply
```

**แก้ไข:**

```
ตรวจสอบ VLAN:
  Switch# show vlan brief

ตรวจสอบ port membership:
  Switch# show interfaces fa0/1 switchport

แก้ไข:
  Switch(config)# interface fa0/1
  Switch(config-if)# switchport access vlan 20
```

---

### Troubleshooting Tools

#### 1. ping

**การใช้งาน:**

```
# Test connectivity
ping 192.168.1.1

# Test with specific size
ping -s 1400 192.168.1.1

# Continuous ping
ping -t 192.168.1.1  (Windows)
ping 192.168.1.1     (Linux - Ctrl+C to stop)
```

#### 2. arp

**การใช้งาน:**

```
# View ARP cache
arp -a

# Clear specific entry
arp -d 192.168.1.1

# Clear all entries
arp -d  (Windows)
ip -s -s neigh flush all  (Linux)

# Add static entry
arp -s 192.168.1.1 AA:BB:CC:DD:EE:FF
```

#### 3. ipconfig / ifconfig / ip

**การใช้งาน:**

```
# View IP configuration
ipconfig /all  (Windows)
ifconfig       (Linux - old)
ip addr show   (Linux - new)

# Release and renew DHCP
ipconfig /release
ipconfig /renew

# Flush DNS cache (bonus)
ipconfig /flushdns
```

#### 4. Wireshark / tcpdump

**การใช้งาน:**

```
Capture and analyze:
- ARP messages
- ICMP messages
- Packet flow

Wireshark filters:
  arp
  icmp
  ip.addr == 192.168.1.10

tcpdump:
  tcpdump -i eth0 arp
  tcpdump -i eth0 icmp
```

---

### Best Practices

#### 1. IP Address Management

- ✅ ใช้ DHCP สำหรับ dynamic hosts
- ✅ จอง IP แบบ static สำหรับ servers, printers
- ✅ Document IP allocation
- ✅ Monitor for duplicate IPs

#### 2. ARP Security

- ✅ Enable **Dynamic ARP Inspection (DAI)**
- ✅ ใช้ **Static ARP** สำหรับ critical devices
- ✅ Monitor ARP traffic for anomalies
- ✅ Implement **Port Security**

#### 3. Network Design

- ✅ VLAN design ที่ดี (แบ่งตาม function)
- ✅ Redundant gateways (HSRP, VRRP)
- ✅ Proper subnetting
- ✅ Document network topology

#### 4. Monitoring

- ✅ Monitor ARP cache size
- ✅ Alert on duplicate IPs
- ✅ Log ARP changes
- ✅ Regular network scans

---

## Summary (สรุป)

Module 9 นี้เราได้เรียนรู้:

1. ✅ **MAC and IP Addresses** - 2 ที่อยู่ที่ใช้ร่วมกัน, IP = end-to-end, MAC = hop-by-hop
2. ✅ **ARP (IPv4)** - หา MAC จาก IP, ARP Request/Reply, ARP Cache
3. ✅ **ARP Process** - Broadcast request, Unicast reply, Cache update
4. ✅ **Gratuitous ARP** - Update caches, Duplicate detection
5. ✅ **ARP Issues** - Spoofing, Flooding, Duplicate IP
6. ✅ **IPv6 NDP** - Neighbor Discovery, NS/NA, RS/RA
7. ✅ **DAD** - Duplicate Address Detection (mandatory ใน IPv6)
8. ✅ **SLAAC** - Stateless auto-configuration
9. ✅ **Troubleshooting** - Tools และวิธีแก้ปัญหา

**สิ่งสำคัญที่ต้องจำ:**

- IP Address = Layer 3 (ไม่เปลี่ยนตลอดทาง)
- MAC Address = Layer 2 (เปลี่ยนทุก hop)
- ARP (IPv4) = หา MAC จาก IP, ใช้ Broadcast
- NDP (IPv6) = เหมือน ARP แต่ใช้ Multicast, มีความสามารถมากกว่า
- Local destination → ARP for destination IP
- Remote destination → ARP for Gateway IP
- ARP cache = เก็บ IP-to-MAC mapping, มี timeout
- DAD = ตรวจสอบ IP ซ้ำ (IPv6 mandatory)
- SLAAC = Auto-configure IPv6 address (ไม่ต้อง DHCP)

**Next Module:** Module 10 - Basic Router Configuration

---

**[ไฟล์ Module 9 สมบูรณ์แล้ว!]**