# CCNA Course 3 - Module 4: ACL Concepts

## แนวคิดเกี่ยวกับ Access Control Lists

---

## 4.1 Purpose of ACLs (วัตถุประสงค์ของ ACL)

### What is an ACL?

**คำจำกัดความ:**

- **Access Control List (ACL)** = รายการ rules/statements ที่ควบคุม traffic
- กรอง packets ตาม criteria (IP, port, protocol)
- อนุญาต (permit) หรือปฏิเสธ (deny) traffic

**เปรียบเทียบ:**

```
ACL เหมือน Security Guard:
  - ตรวจสอบ credentials (IP address, port)
  - อนุญาตให้เข้า (permit)
  - ปฏิเสธการเข้า (deny)
  - บันทึก (log)
```

---

### ACL Functions

**การใช้งาน ACLs:**

#### 1. Packet Filtering (กรอง packets)

**คำจำกัดความ:**

- กรอง traffic ที่เข้า/ออก interface
- ตัดสินใจ forward หรือ drop packets

**ตัวอย่าง:**

```
Scenario: Block Telnet access to server
ACL: deny tcp any host 192.168.1.100 eq 23

ผลลัพธ์:
  - Telnet (port 23) ไป 192.168.1.100 → Denied
  - SSH (port 22) ไป 192.168.1.100 → Allowed (ไม่ match rule)
  - Telnet ไป 192.168.1.101 → Allowed (ไม่ match rule)
```

#### 2. Network Security

**การป้องกัน:**

```
✅ Block unauthorized access
✅ Restrict services (Telnet, FTP)
✅ Prevent spoofing
✅ Limit access to sensitive networks
```

**ตัวอย่าง:**

```
Block Internet access to internal servers:
  deny ip any 10.1.1.0 0.0.0.255

Allow only management network to access routers:
  permit tcp 192.168.1.0 0.0.0.255 any eq 22
  deny tcp any any eq 22
```

#### 3. Traffic Control

**คำจำกัดความ:**

- ควบคุมประเภทของ traffic
- จำกัด bandwidth usage

**ตัวอย่าง:**

```
Block peer-to-peer file sharing:
  deny tcp any any range 6881 6889 (BitTorrent)

Limit video streaming:
  deny tcp any any eq 1935 (RTMP)
```

#### 4. QoS (Quality of Service)

**คำจำกัดความ:**

- Classify traffic
- Prioritize important traffic

**ตัวอย่าง:**

```
Identify VoIP traffic:
  permit udp any any range 16384 32767
  → Set priority สูง
```

#### 5. NAT (Network Address Translation)

**คำจำกัดความ:**

- ระบุ traffic ที่ต้องการ NAT

**ตัวอย่าง:**

```
NAT inside source list 1:
  access-list 1 permit 192.168.1.0 0.0.0.255
  → NAT เฉพาะ 192.168.1.0/24
```

#### 6. Limit Network Traffic

**Debugging/Logging:**

```
ACL สำหรับ debug:
  access-list 100 permit tcp host 10.1.1.1 host 192.168.1.100
  debug ip packet 100
  → Debug เฉพาะ traffic ระหว่าง 2 hosts
```

---

### ACL Operation

**การทำงาน:**

```
Packet arrives → Router checks ACL:

1. อ่าน ACE (Access Control Entry) แรก
2. เปรียบเทียบ packet กับ ACE
3. ถ้า match:
     - ทำตาม action (permit/deny)
     - หยุด (ไม่ตรวจสอบ ACEs ถัดไป)
4. ถ้าไม่ match:
     - ไป ACE ถัดไป
5. ถ้าไม่ match ทุก ACEs:
     - Implicit deny (deny ทุก packets)
```

**ตัวอย่าง:**

```
ACL 10:
  10 permit host 10.1.1.1
  20 permit 10.1.1.0 0.0.0.255
  30 deny any

Packet from 10.1.1.1:
  - Check line 10: Match! → Permit → Stop

Packet from 10.1.1.50:
  - Check line 10: No match
  - Check line 20: Match! → Permit → Stop

Packet from 192.168.1.1:
  - Check line 10: No match
  - Check line 20: No match
  - Check line 30: Match! → Deny
```

---

### Implicit Deny

**คำจำกัดความ:**

- ท้าย ACL ทุกตัวมี **implicit deny any** ซ่อนอยู่
- ถ้า packet ไม่ match ACE ใดๆ → Denied

**ตัวอย่าง:**

```
ACL 10:
  permit host 10.1.1.1

จริงๆ คือ:
  permit host 10.1.1.1
  deny any ← implicit (ไม่เห็น)

ผลลัพธ์:
  - 10.1.1.1 → Permitted
  - อื่นๆ ทั้งหมด → Denied
```

**ข้อควรระวัง:**

```
⚠️ ACL ว่างเปล่า (no ACEs) = deny all!
⚠️ ต้อง permit อย่างน้อย 1 rule
```

---

## 4.2 Wildcard Masks

### What is a Wildcard Mask?

**คำจำกัดความ:**

- ใช้ระบุ range ของ IP addresses ใน ACL
- **Inverse** ของ subnet mask
- **0** = must match, **1** = don't care

**เปรียบเทียบกับ Subnet Mask:**

```
Subnet Mask     Wildcard Mask    Meaning
------------------------------------------------------------------------
255.255.255.255 0.0.0.0          Exact match (host)
255.255.255.252 0.0.0.3          /30 network
255.255.255.0   0.0.0.255        /24 network
255.255.0.0     0.0.255.255      /16 network
255.0.0.0       0.255.255.255    /8 network
0.0.0.0         255.255.255.255  Any (all addresses)
------------------------------------------------------------------------
```

---

### Wildcard Mask Calculation

**สูตร:**

```
Wildcard Mask = 255.255.255.255 - Subnet Mask
```

**ตัวอย่าง:**

**Example 1: /24 network**

```
Subnet Mask: 255.255.255.0

Calculation:
  255.255.255.255
- 255.255.255.0
-----------------
  0.  0.  0.255

Wildcard: 0.0.0.255
```

**Example 2: /30 network**

```
Subnet Mask: 255.255.255.252

Calculation:
  255.255.255.255
- 255.255.255.252
-----------------
  0.  0.  0.  3

Wildcard: 0.0.0.3
```

**Example 3: /16 network**

```
Subnet Mask: 255.255.0.0

Calculation:
  255.255.255.255
- 255.255.0.0
-----------------
  0.  0.255.255

Wildcard: 0.0.255.255
```

---

### Wildcard Mask Logic

**Binary Logic:**

```
Wildcard bit = 0 → bit ต้องตรงกัน (must match)
Wildcard bit = 1 → bit ไม่สนใจ (don't care)

ตัวอย่าง: 192.168.1.0 0.0.0.255

IP:       192.168.1.0    = 11000000.10101000.00000001.00000000
Wildcard: 0.0.0.255      = 00000000.00000000.00000000.11111111
                           ↑↑↑↑↑↑↑↑ ↑↑↑↑↑↑↑↑ ↑↑↑↑↑↑↑↑ ↑↑↑↑↑↑↑↑
                           must     must     must     don't
                           match    match    match    care

Result: 192.168.1.0 - 192.168.1.255 (256 addresses)
```

---

### Common Wildcard Masks

#### 1. Single Host (0.0.0.0)

**ตัวอย่าง:**

```
ACL: permit host 192.168.1.10
หรือ: permit 192.168.1.10 0.0.0.0

ความหมาย: Match เฉพาะ 192.168.1.10
```

#### 2. Class C Network (0.0.0.255)

**ตัวอย่าง:**

```
ACL: permit 192.168.1.0 0.0.0.255

ความหมาย: Match 192.168.1.0 - 192.168.1.255
จำนวน: 256 addresses
```

#### 3. Class B Network (0.0.255.255)

**ตัวอย่าง:**

```
ACL: permit 172.16.0.0 0.0.255.255

ความหมาย: Match 172.16.0.0 - 172.16.255.255
จำนวน: 65,536 addresses
```

#### 4. Any Address (255.255.255.255)

**ตัวอย่าง:**

```
ACL: permit any
หรือ: permit 0.0.0.0 255.255.255.255

ความหมาย: Match ทุก IP addresses
```

---

### Wildcard Mask Examples

**Example 1: Match subnet 192.168.1.64/26**

```
Network: 192.168.1.64/26
Subnet Mask: 255.255.255.192
Wildcard: 0.0.0.63

ACL: permit 192.168.1.64 0.0.0.63

Matches: 192.168.1.64 - 192.168.1.127 (64 addresses)
```

**Example 2: Match even subnets only**

```
ต้องการ: 192.168.0.0/24, 192.168.2.0/24, 192.168.4.0/24, ...

ACL: permit 192.168.0.0 0.0.254.255

Binary:
  192.168.0.0    = 11000000.10101000.00000000.00000000
  Wildcard       = 00000000.00000000.11111110.11111111
                                     ↑       ↑
                                     2nd bit must be 0 (even)

Matches:
  192.168.0.0/24 (00000000 = even)
  192.168.2.0/24 (00000010 = even)
  192.168.4.0/24 (00000100 = even)
  ...

Not match:
  192.168.1.0/24 (00000001 = odd)
  192.168.3.0/24 (00000011 = odd)
```

**Example 3: Match range of hosts**

```
ต้องการ: 10.1.1.32 - 10.1.1.63 (32 hosts)

Network: 10.1.1.32/27
Wildcard: 0.0.0.31

ACL: permit 10.1.1.32 0.0.0.31
```

---

### Wildcard Mask Shortcuts

**Keywords:**

#### host

```
แทน: <ip> 0.0.0.0

ตัวอย่าง:
  permit host 192.168.1.10
  = permit 192.168.1.10 0.0.0.0
```

#### any

```
แทน: 0.0.0.0 255.255.255.255

ตัวอย่าง:
  permit any
  = permit 0.0.0.0 255.255.255.255
```

---

### Calculating Wildcard for CIDR

**Method: Block Size - 1**

```
CIDR         Block Size       Wildcard
------------------------------------------------------------------------
/32 (host)   1                0.0.0.0
/31          2                0.0.0.1
/30          4                0.0.0.3
/29          8                0.0.0.7
/28          16               0.0.0.15
/27          32               0.0.0.31
/26          64               0.0.0.63
/25          128              0.0.0.127
/24          256              0.0.0.255
------------------------------------------------------------------------
```

**สูตร:**

```
Block Size = 2^(32 - CIDR)
Wildcard = Block Size - 1

ตัวอย่าง: /28
  Block Size = 2^(32-28) = 2^4 = 16
  Wildcard = 16 - 1 = 15
  → 0.0.0.15
```

---

## 4.3 Guidelines for ACL Creation

### ACL Planning

**ขั้นตอนการวางแผน:**

#### 1. Define Security Policy

```
ตัวอย่าง:
  - Allow employees access to Internet
  - Block employees from accessing social media
  - Allow IT staff to manage all devices
  - Block all other management access
```

#### 2. Identify Traffic

```
คำถาม:
  - Source: จากไหน?
  - Destination: ไปไหน?
  - Protocol: TCP/UDP/ICMP?
  - Port: Port อะไร?
  - Action: Permit หรือ Deny?
```

#### 3. Design ACL

```
- เลือก ACL type (Standard/Extended)
- เขียน ACEs
- เรียงลำดับจาก specific → general
```

#### 4. Choose Placement

```
- Inbound หรือ Outbound?
- Interface ไหน?
```

---

### ACL Best Practices

#### 1. Order of ACEs (ลำดับ)

**กฎ:**

```
✅ เรียง ACEs จาก specific → general
✅ Permit specific traffic ก่อน
✅ Deny general traffic ทีหลัง

เหตุผล: First match wins
```

**ตัวอย่างที่ถูก:**

```
access-list 100 permit tcp host 10.1.1.1 any eq 80
access-list 100 deny tcp 10.1.1.0 0.0.0.255 any eq 80
access-list 100 permit ip any any

ผลลัพธ์:
  - 10.1.1.1 → Internet (HTTP) = Permit (line 1)
  - 10.1.1.2 → Internet (HTTP) = Deny (line 2)
  - 10.1.1.1 → Internet (SSH) = Permit (line 3)
```

**ตัวอย่างที่ผิด:**

```
access-list 100 deny tcp 10.1.1.0 0.0.0.255 any eq 80
access-list 100 permit tcp host 10.1.1.1 any eq 80
access-list 100 permit ip any any

ผลลัพธ์:
  - 10.1.1.1 → Internet (HTTP) = Deny (line 1) ← ผิด!
    (match line 1 ก่อน, หยุด, ไม่ถึง line 2)
```

#### 2. Place ACL Close to Source/Destination

**Standard ACL:**

```
✅ วางใกล้ destination
เหตุผล: Standard ACL กรองเฉพาะ source IP
       → วางใกล้ source = block traffic เร็วเกินไป

Topology:
[Branch] ------ [HQ] ------ [Server]

Goal: Block Branch จาก Server เฉพาะ
Standard ACL: วางที่ HQ (ใกล้ Server)
```

**Extended ACL:**

```
✅ วางใกล้ source
เหตุผล: Extended ACL กรองละเอียด (src, dst, port)
       → วางใกล้ source = ประหยัด bandwidth

Topology:
[Branch] ------ [HQ] ------ [Server]

Goal: Block Branch จาก Server:Telnet เฉพาะ
Extended ACL: วางที่ Branch (ใกล้ source)
```

#### 3. Document ACLs

```
✅ ใช้ remark (ความคิดเห็น)
✅ ใช้ชื่อที่มีความหมาย
✅ เก็บ external documentation

ตัวอย่าง:
access-list 100 remark === Block Telnet to Server ===
access-list 100 deny tcp any host 192.168.1.100 eq 23
access-list 100 remark === Allow all other traffic ===
access-list 100 permit ip any any
```

#### 4. Test ACLs

```
✅ Test บน lab environment ก่อน
✅ ใช้ extended ping test
✅ Verify ด้วย show commands
✅ ใช้ logging ดู matches
```

#### 5. Use Named ACLs

```
✅ ง่ายต่อการจดจำ
✅ Insert/Delete ACEs ได้
✅ Resequence ACEs ได้

ตัวอย่าง:
ip access-list extended BLOCK-TELNET
 deny tcp any any eq 23
 permit ip any any
```

#### 6. Implicit Deny

```
⚠️ อย่าลืม implicit deny any ท้าย ACL
✅ ใส่ permit statement สุดท้าย (ถ้าต้องการ allow traffic อื่น)

ตัวอย่าง:
access-list 100 deny tcp any host 192.168.1.100 eq 23
access-list 100 permit ip any any ← สำคัญ!
```

---

### ACL Configuration Workflow

**ขั้นตอน:**

```
1. Plan:
   - Define security policy
   - Identify traffic patterns
   - Choose ACL type

2. Create:
   - Write ACL statements
   - Use descriptive names/remarks
   - Order from specific to general

3. Apply:
   - Select interface
   - Choose direction (in/out)
   - Apply ACL

4. Test:
   - Test connectivity
   - Verify with show commands
   - Check counters

5. Monitor:
   - Review logs
   - Analyze matches
   - Adjust as needed

6. Document:
   - Update network documentation
   - Record changes
   - Maintain ACL inventory
```

---

## 4.4 Types of ACLs

### ACL Types Overview

**2 ประเภทหลัก:**

```
1. Standard ACLs
   - กรอง source IP address เท่านั้น
   - Number: 1-99, 1300-1999

2. Extended ACLs
   - กรอง source, destination, protocol, port
   - Number: 100-199, 2000-2699
```

---

### Standard ACLs

**คำจำกัดความ:**

- กรองตาม **source IP address** เท่านั้น
- ไม่สามารถกรอง destination, protocol, port
- ใช้สำหรับ simple filtering

**Syntax:**

```
access-list <number> {permit|deny} <source> [<wildcard>] [log]

Number: 1-99 หรือ 1300-1999
```

**ตัวอย่าง:**

```
access-list 10 permit host 192.168.1.10
access-list 10 permit 192.168.1.0 0.0.0.255
access-list 10 deny any
```

**ข้อจำกัด:**

```
❌ ไม่สามารถระบุ destination
❌ ไม่สามารถระบุ protocol
❌ ไม่สามารถระบุ port
❌ All-or-nothing (block ทั้งหมดหรือ allow ทั้งหมด)
```

**Use Cases:**

```
✅ Restrict management access (SSH, Telnet to router)
✅ NAT inside source
✅ Simple network filtering
✅ Route filtering (distribute-list)
```

---

### Extended ACLs

**คำจำกัดความ:**

- กรองตาม **source, destination, protocol, port**
- ละเอียดกว่า Standard ACL มาก
- ยืดหยุ่น

**Syntax:**

```
access-list <number> {permit|deny} <protocol> <source> [<src-port>] <destination> [<dst-port>] [options]

Number: 100-199 หรือ 2000-2699
Protocol: ip, tcp, udp, icmp, ...
```

**ตัวอย่าง:**

```
! Block Telnet to specific server
access-list 100 deny tcp any host 192.168.1.100 eq 23

! Allow HTTP and HTTPS to Internet
access-list 100 permit tcp 10.1.1.0 0.0.0.255 any eq 80
access-list 100 permit tcp 10.1.1.0 0.0.0.255 any eq 443

! Block ICMP ping
access-list 100 deny icmp any any echo

! Allow all other traffic
access-list 100 permit ip any any
```

**Port Operators:**

```
eq    = equal (เท่ากับ)
      access-list 100 permit tcp any any eq 80

ne    = not equal (ไม่เท่ากับ)
      access-list 100 deny tcp any any ne 80

gt    = greater than (มากกว่า)
      access-list 100 permit tcp any any gt 1023

lt    = less than (น้อยกว่า)
      access-list 100 permit tcp any any lt 1024

range = range (ช่วง)
      access-list 100 permit tcp any any range 20 21
```

**Use Cases:**

```
✅ Granular traffic control
✅ Block specific services (Telnet, FTP)
✅ QoS classification
✅ Firewall policies
```

---

### Numbered vs Named ACLs

#### Numbered ACLs

**ข้อดี:**

```
✅ ง่าย, รวดเร็ว
✅ ใช้ได้กับ router เก่า
```

**ข้อเสีย:**

```
❌ ไม่สามารถลบ ACE เฉพาะได้ (ต้องลบทั้ง ACL)
❌ ไม่สามารถ insert ACE ระหว่างได้
❌ จำยาก (number ไม่มีความหมาย)
```

**ตัวอย่าง:**

```
access-list 10 permit 192.168.1.0 0.0.0.255
access-list 10 deny any
```

#### Named ACLs

**ข้อดี:**

```
✅ ชื่อมีความหมาย
✅ แก้ไข ACEs ได้ (insert, delete)
✅ Resequence ได้
✅ แนะนำสำหรับ configurations ใหม่
```

**ข้อเสีย:**

```
❌ ซับซ้อนกว่า numbered
```

**Syntax:**

```
Standard Named ACL:
ip access-list standard <name>
 {permit|deny} <source> [<wildcard>]

Extended Named ACL:
ip access-list extended <name>
 {permit|deny} <protocol> <source> <destination> [options]
```

**ตัวอย่าง:**

```
ip access-list standard ADMIN-ACCESS
 permit 192.168.1.0 0.0.0.255
 deny any

ip access-list extended BLOCK-TELNET
 deny tcp any any eq 23
 permit ip any any
```

**Edit Named ACL:**

```
! Show sequence numbers
show ip access-lists BLOCK-TELNET

! Delete specific ACE
ip access-list extended BLOCK-TELNET
 no 10

! Insert ACE at specific sequence
ip access-list extended BLOCK-TELNET
 5 permit tcp host 10.1.1.1 any eq 23

! Resequence
ip access-list resequence BLOCK-TELNET 10 10
```

---

### Standard vs Extended Comparison

```
Feature              Standard             Extended
------------------------------------------------------------------------
Number Range         1-99, 1300-1999      100-199, 2000-2699
Filter by Source     Yes                  Yes
Filter by Dest       No                   Yes
Filter by Protocol   No                   Yes
Filter by Port       No                   Yes
Granularity          Low                  High
Complexity           Simple               Complex
Placement            Near destination     Near source
Use Case             Simple filtering     Complex policies
------------------------------------------------------------------------
```

---

## 4.5 ACL Processing

### Inbound vs Outbound

**คำจำกัดความ:**

- **Inbound ACL**: กรอง traffic ที่เข้า interface
- **Outbound ACL**: กรอง traffic ที่ออก interface

#### Inbound ACL Processing

```
Packet arrives → Check Inbound ACL:
  - If Permit → Route packet → Check Outbound ACL
  - If Deny → Drop packet

Direction: From network → Into router
```

**ตัวอย่าง:**

```
     Internet
        |
    [Router Gi0/0] ← Inbound ACL
        |
   Internal Network

Inbound ACL บน Gi0/0:
  - กรอง traffic จาก Internet เข้า router
  - ก่อนที่ router จะ route
```

#### Outbound ACL Processing

```
Packet routed → Check Outbound ACL:
  - If Permit → Forward packet out interface
  - If Deny → Drop packet

Direction: From router → Out to network
```

**ตัวอย่าง:**

```
   Internal Network
        |
    [Router Gi0/1] → Outbound ACL
        |
     Internet

Outbound ACL บน Gi0/1:
  - กรอง traffic ที่ออกจาก router
  - หลังจาก router route แล้ว
```

---

### ACL Processing Logic

**Flow Chart:**

```
INBOUND ACL:
Packet arrives
    ↓
Check Inbound ACL
    ↓
Match ACE? 
    ├─ Yes → Permit? 
    │         ├─ Yes → Route packet
    │         └─ No → Drop packet
    │
    └─ No → Next ACE
           ↓
        More ACEs?
           ├─ Yes → Check next ACE
           └─ No → Implicit Deny → Drop packet

OUTBOUND ACL:
Route packet
    ↓
Check Outbound ACL
    ↓
Match ACE?
    ├─ Yes → Permit?
    │         ├─ Yes → Forward out interface
    │         └─ No → Drop packet
    │
    └─ No → Next ACE
           ↓
        More ACEs?
           ├─ Yes → Check next ACE
           └─ No → Implicit Deny → Drop packet
```

---

### ACL Application

**Syntax:**

```
interface <interface-id>
 ip access-group <acl-number-or-name> {in|out}
```

**ตัวอย่าง:**

```
! Apply Standard ACL 10 inbound
interface GigabitEthernet0/0
 ip access-group 10 in

! Apply Extended ACL 100 outbound
interface GigabitEthernet0/1
 ip access-group 100 out

! Apply Named ACL inbound
interface Serial0/0/0
 ip access-group BLOCK-TELNET in
```

**Rules:**

```
⚠️ แต่ละ interface สามารถมี ACL ได้:
   - 1 Inbound ACL (per protocol)
   - 1 Outbound ACL (per protocol)

⚠️ ไม่สามารถใช้ ACL เดียวกันทั้ง inbound และ outbound บน interface เดียว
```

---

### ACL Examples

**Example 1: Block Telnet to Server**

**Scenario:**

```
[Users 10.1.1.0/24] --- [Router] --- [Server 192.168.1.100]

Goal: Block Telnet (port 23) จาก Users ไป Server
```

**Extended ACL (ใกล้ source):**

```
! Create ACL
access-list 100 deny tcp 10.1.1.0 0.0.0.255 host 192.168.1.100 eq 23
access-list 100 permit ip any any

! Apply inbound on user-facing interface
interface GigabitEthernet0/0
 ip access-group 100 in
```

**Example 2: Restrict Management Access**

**Scenario:**

```
Goal: อนุญาตเฉพาะ 192.168.1.0/24 SSH เข้า router
```

**Standard ACL:**

```
! Create ACL
access-list 10 remark === SSH Access ===
access-list 10 permit 192.168.1.0 0.0.0.255
access-list 10 deny any

! Apply to VTY lines
line vty 0 4
 access-class 10 in
 transport input ssh
```

**Example 3: Allow Specific Services**

**Scenario:**

```
[Branch] --- [Router] --- [Internet]

Goal:
  - Allow HTTP, HTTPS, DNS
  - Block all other traffic
```

**Extended ACL:**

```
ip access-list extended INTERNET-ACCESS
 permit tcp 10.1.1.0 0.0.0.255 any eq 80
 permit tcp 10.1.1.0 0.0.0.255 any eq 443
 permit udp 10.1.1.0 0.0.0.255 any eq 53
 deny ip any any

interface GigabitEthernet0/0
 ip access-group INTERNET-ACCESS in
```

---

### Troubleshooting ACLs

**Common Problems:**

#### 1. Wrong Order of ACEs

```
Problem: Specific rule หลัง general rule

Symptom: Traffic ที่ควร permit ถูก deny

Example:
access-list 100 deny tcp any any eq 23
access-list 100 permit tcp host 10.1.1.1 any eq 23
→ 10.1.1.1 ยังถูก deny (match line 1 ก่อน)

Solution: สลับลำดับ
access-list 100 permit tcp host 10.1.1.1 any eq 23
access-list 100 deny tcp any any eq 23
```

#### 2. Missing Permit Statement

```
Problem: ลืม permit statement

Symptom: Traffic ทั้งหมดถูก deny (implicit deny)

Example:
access-list 100 deny tcp any any eq 23
→ Block Telnet + Block all other traffic!

Solution: เพิ่ม permit
access-list 100 deny tcp any any eq 23
access-list 100 permit ip any any
```

#### 3. Wrong Direction

```
Problem: ACL ใช้ direction ผิด (in vs out)

Symptom: ACL ไม่ทำงาน

Solution: ตรวจสอบ direction
show ip interface <interface>
```

#### 4. Wrong Interface

```
Problem: ACL ใช้ interface ผิด

Symptom: ACL ไม่กรอง traffic ที่ต้องการ

Solution: ตรวจสอบ topology และ traffic flow
```

#### 5. Wildcard Mask Error

```
Problem: Wildcard mask ผิด

Symptom: Match addresses ผิด

Example:
Goal: Match 192.168.1.0/24
Wrong: access-list 10 permit 192.168.1.0 255.255.255.0 (subnet mask)
Correct: access-list 10 permit 192.168.1.0 0.0.0.255 (wildcard)
```

---

### Verification Commands

#### show access-lists

```
R1# show access-lists
Standard IP access list 10
    10 permit 192.168.1.0, wildcard bits 0.0.0.255 (5 matches)
    20 deny   any (12 matches)

Extended IP access list 100
    10 deny tcp any host 192.168.1.100 eq telnet (3 matches)
    20 permit ip any any (150 matches)
```

#### show ip access-lists

```
R1# show ip access-lists
Standard IP access list 10
    10 permit 192.168.1.0, wildcard bits 0.0.0.255 (5 matches)
    20 deny   any (log) (12 matches)

Extended IP access list BLOCK-TELNET
    10 deny tcp any any eq telnet (3 matches)
    20 permit ip any any (150 matches)
```

#### show ip interface

```
R1# show ip interface gigabitEthernet 0/0
GigabitEthernet0/0 is up, line protocol is up
  Internet address is 192.168.1.1/24
  ...
  Outgoing access list is not set
  Inbound  access list is 100
  ...
```

#### show running-config

```
R1# show running-config | include access-list
access-list 10 permit 192.168.1.0 0.0.0.255
access-list 100 deny tcp any host 192.168.1.100 eq 23
access-list 100 permit ip any any

R1# show running-config interface gigabitEthernet 0/0
interface GigabitEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 ip access-group 100 in
```

---

### ACL Statistics

**Matches Counter:**

```
R1# show access-lists 100
Extended IP access list 100
    10 deny tcp any host 192.168.1.100 eq telnet (3 matches)
    20 permit ip any any (150 matches)
                             ↑↑↑
                         จำนวน packets ที่ match
```

**Clear Counters:**

```
R1# clear access-list counters 100
```

**Enable Logging:**

```
access-list 100 deny tcp any host 192.168.1.100 eq 23 log
access-list 100 permit ip any any

Syslog output:
%SEC-6-IPACCESSLOGDP: list 100 denied tcp 10.1.1.5(12345) -> 192.168.1.100(23)
```

---

## Summary (สรุป)

Module 4 นี้เราได้เรียนรู้:

1. ✅ **Purpose of ACLs**:
    
    - Packet filtering
    - Network security
    - Traffic control
    - QoS, NAT
2. ✅ **Wildcard Masks**:
    
    - Inverse ของ subnet mask
    - 0 = must match, 1 = don't care
    - Calculation: 255.255.255.255 - Subnet Mask
    - Keywords: host (0.0.0.0), any (255.255.255.255)
3. ✅ **ACL Guidelines**:
    
    - Order: Specific → General
    - Standard ACL: ใกล้ destination
    - Extended ACL: ใกล้ source
    - Document ด้วย remarks
    - Test ก่อน deploy
4. ✅ **Types of ACLs**:
    
    - **Standard**: Source IP only (1-99, 1300-1999)
    - **Extended**: Src, Dst, Protocol, Port (100-199, 2000-2699)
    - **Numbered**: เก่า, ไม่ flexible
    - **Named**: ใหม่, flexible, แนะนำ
5. ✅ **ACL Processing**:
    
    - Inbound: ก่อน routing
    - Outbound: หลัง routing
    - First match wins
    - Implicit deny any ท้าย ACL
6. ✅ **Verification**:
    
    - show access-lists
    - show ip access-lists
    - show ip interface
    - Matches counter
    - Logging

**สิ่งสำคัญที่ต้องจำ:**

- ACL = กรอง traffic ตาม rules
- Wildcard mask ≠ Subnet mask
- Wildcard = 255.255.255.255 - Subnet Mask
- Standard ACL: Source IP only → ใกล้ destination
- Extended ACL: Src/Dst/Protocol/Port → ใกล้ source
- Order matters: Specific ก่อน, General ทีหลัง
- First match wins (หยุดทันที)
- Implicit deny any ท้ายทุก ACL
- Named ACL > Numbered ACL (flexibility)
- Test ACL ก่อน deploy!
- 1 interface = 1 Inbound + 1 Outbound ACL

**ACL Decision Tree:**

```
Need to filter?
    ├─ Source IP only? → Standard ACL → Near destination
    └─ Src/Dst/Port? → Extended ACL → Near source

Use numbers or names?
    ├─ Simple, legacy → Numbered ACL
    └─ Modern, flexible → Named ACL (แนะนำ)

Direction?
    ├─ Filter before routing → Inbound
    └─ Filter after routing → Outbound
```

**Next Module:** Module 5 - ACLs for IPv4 Configuration

---

**[ไฟล์ Module 4 สมบูรณ์แล้ว!]**