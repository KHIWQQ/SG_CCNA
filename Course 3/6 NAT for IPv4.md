# CCNA Course 3 - Module 6: NAT for IPv4

## Network Address Translation สำหรับ IPv4

---

## 6.1 NAT Characteristics (ลักษณะของ NAT)

### What is NAT?

**คำจำกัดความ:**

- **Network Address Translation (NAT)** = กระบวนการแปลง IP addresses
- แปลง **private IP addresses** เป็น **public IP addresses**
- ใช้บน router ระหว่าง private network และ Internet

**ปัญหาที่ NAT แก้:**

```
Problem: IPv4 address exhaustion
  - IPv4 addresses จำกัด (~4.3 billion)
  - Devices มากกว่า available addresses
  - ไม่เพียงพอสำหรับทุกคนมี public IP

Solution: NAT
  - ใช้ private IPs ภายใน organization
  - แปลงเป็น public IPs เมื่อออก Internet
  - หลาย devices แชร์ public IPs เดียวกัน
```

---

### Private vs Public IP Addresses

#### Private IP Address Ranges (RFC 1918)

**คำจำกัดความ:**

- ใช้ภายใน private networks
- **ไม่ routable** บน Internet
- ฟรี, ใช้ซ้ำได้

**Ranges:**

```
Class    Range                        CIDR           Hosts
------------------------------------------------------------------------
Class A  10.0.0.0 - 10.255.255.255    10.0.0.0/8     16,777,216
Class B  172.16.0.0 - 172.31.255.255  172.16.0.0/12  1,048,576
Class C  192.168.0.0 - 192.168.255.255 192.168.0.0/16 65,536
------------------------------------------------------------------------
```

**ตัวอย่าง:**

```
Company Internal Network:
  - LAN: 192.168.1.0/24 (private)
  - Servers: 10.1.1.0/24 (private)
  - Branch: 172.16.0.0/16 (private)
```

#### Public IP Addresses

**คำจำกัดความ:**

- ได้รับจาก ISP
- **Routable** บน Internet
- ต้องจ่ายเงิน, ไม่ซ้ำกัน globally

**ตัวอย่าง:**

```
Company Public IPs:
  - 203.0.113.1 - 203.0.113.10
  - ใช้สำหรับ Internet connection
```

---

### NAT Terminology

#### Inside Network

```
- Network ภายใน organization
- ใช้ private IP addresses
- ต้องการ NAT เพื่อออก Internet
```

#### Outside Network

```
- Network ภายนอก (Internet)
- ใช้ public IP addresses
```

#### Inside Local Address

```
- IP address ของ device บน inside network
- เป็น private IP
- เห็นโดย inside users

ตัวอย่าง: 192.168.1.10
```

#### Inside Global Address

```
- IP address ของ inside device หลังผ่าน NAT
- เป็น public IP
- เห็นโดย outside users

ตัวอย่าง: 203.0.113.5
```

#### Outside Local Address

```
- IP address ของ outside device เมื่อมองจาก inside
- มักเป็น public IP เดียวกับ outside global

ตัวอย่าง: 8.8.8.8
```

#### Outside Global Address

```
- IP address ของ outside device บน Internet
- เป็น public IP

ตัวอย่าง: 8.8.8.8
```

**ตัวอย่างสรุป:**

```
[PC: 192.168.1.10] --- [NAT Router] --- [Internet Server: 8.8.8.8]
    Inside Local        Public IP:           Outside Global
                        203.0.113.5
                        Inside Global

NAT Translation:
  Inside Local → Inside Global
  192.168.1.10 → 203.0.113.5

Packet flow:
  1. PC sends: Src 192.168.1.10, Dst 8.8.8.8
  2. NAT translates: Src 203.0.113.5, Dst 8.8.8.8
  3. Server sees: Src 203.0.113.5, Dst 8.8.8.8
  4. Server replies: Src 8.8.8.8, Dst 203.0.113.5
  5. NAT translates: Src 8.8.8.8, Dst 192.168.1.10
  6. PC receives: Src 8.8.8.8, Dst 192.168.1.10
```

---

### NAT Benefits

**1. Conservation of Public IP Addresses**

```
✅ หลาย devices ใช้ public IPs น้อย
✅ ประหยัด IPv4 addresses
✅ ลด IPv4 address exhaustion

Example:
  1000 devices (inside)
  → 1 public IP (outside)
  → Save 999 public IPs
```

**2. Security/Privacy**

```
✅ ซ่อน internal IP addresses
✅ Outside ไม่เห็น internal topology
✅ Protection layer

Example:
  Inside: 192.168.1.10
  Outside sees: 203.0.113.5
  → Cannot directly attack 192.168.1.10
```

**3. Flexibility**

```
✅ เปลี่ยน ISP ง่าย (เปลี่ยน public IPs)
✅ ไม่ต้อง renumber internal network
✅ Merge networks (overlapping IPs)

Example:
  Change ISP:
    Old public IP: 198.51.100.5
    New public IP: 203.0.113.5
    Internal (192.168.1.0/24) ไม่ต้องเปลี่ยน
```

---

### NAT Limitations

**1. Performance**

```
❌ CPU overhead (translate addresses)
❌ Latency เพิ่มขึ้น
❌ Throughput ลดลง (บน high traffic)
```

**2. End-to-End Connectivity**

```
❌ บาง protocols ไม่ทำงานกับ NAT
❌ Peer-to-peer applications มีปัญหา
❌ Protocols ที่ embed IP ใน payload

Examples:
  - FTP (active mode)
  - SIP (VoIP)
  - IPsec
  - Some online games
```

**3. Port Forwarding Complexity**

```
❌ ต้อง configure manually สำหรับ inbound connections
❌ ซับซ้อนสำหรับหลาย services
```

**4. Logging/Troubleshooting**

```
❌ ยากต่อการ trace connections
❌ Logs ต้อง correlate inside/outside addresses
❌ Troubleshooting ซับซ้อน
```

**5. Loss of Address Traceability**

```
❌ หลาย users แชร์ public IP เดียวกัน
❌ ยากต่อการระบุ specific user
❌ Legal/forensic issues
```

---

## 6.2 Types of NAT

### NAT Types Overview

**3 ประเภทหลัก:**

```
1. Static NAT
   - 1:1 mapping (permanent)
   - Inside local ↔ Inside global

2. Dynamic NAT
   - 1:1 mapping (temporary)
   - Inside local → Pool of inside global

3. PAT (Port Address Translation)
   - Many:1 mapping
   - หลาย inside local → 1 inside global (different ports)
```

---

### Static NAT

**คำจำกัดความ:**

- **One-to-one** mapping
- Inside local ↔ Inside global (คงที่)
- Permanent translation
- ใช้สำหรับ servers ที่ต้องการ access จาก Internet

**Characteristics:**

```
✅ Permanent mapping
✅ Bidirectional (inbound/outbound)
✅ Same public IP เสมอ
✅ ใช้ 1 public IP per inside host
```

**Use Cases:**

```
- Web servers
- Mail servers
- FTP servers
- ทุก servers ที่ต้องการ inbound access
```

**Example:**

```
Topology:
[Web Server: 192.168.1.100] --- [NAT Router] --- [Internet]
   Inside Local                  Public: 203.0.113.10
                                 Inside Global

Static NAT Mapping:
  192.168.1.100 ↔ 203.0.113.10

Translation:
  Inside → Outside:
    Src: 192.168.1.100 → 203.0.113.10
    
  Outside → Inside:
    Dst: 203.0.113.10 → 192.168.1.100

Internet users access: http://203.0.113.10
→ Reaches web server at 192.168.1.100
```

**Configuration (Preview):**

```
Router(config)# ip nat inside source static 192.168.1.100 203.0.113.10

Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip nat inside

Router(config)# interface gigabitEthernet 0/1
Router(config-if)# ip nat outside
```

---

### Dynamic NAT

**คำจำกัดความ:**

- **One-to-one** mapping (temporary)
- Inside local → Inside global จาก pool
- First-come, first-served
- Automatic mapping (ไม่ต้อง configure แต่ละ host)

**Characteristics:**

```
✅ Temporary mapping
✅ ใช้เฉพาะเมื่อ inside host ติดต่อ outside
✅ Release เมื่อ connection idle
✅ ใช้ pool of public IPs
❌ ถ้า pool เต็ม → ไม่สามารถ translate
```

**Use Cases:**

```
- Organizations มี public IPs น้อยกว่า inside hosts
- ไม่ต้องการ permanent mappings
- Outbound traffic only
```

**Example:**

```
Topology:
[PC1: 192.168.1.10] ┐
[PC2: 192.168.1.20] ├─ [NAT Router] ─── [Internet]
[PC3: 192.168.1.30] ┘  Pool: 203.0.113.1-3

Dynamic NAT Pool:
  203.0.113.1
  203.0.113.2
  203.0.113.3

Translation (when PCs access Internet):
  PC1 (192.168.1.10) → 203.0.113.1
  PC2 (192.168.1.20) → 203.0.113.2
  PC3 (192.168.1.30) → 203.0.113.3

ถ้า PC4 ต้องการ access Internet:
  → Pool เต็ม → ไม่สามารถ translate → Failed!
```

**Configuration (Preview):**

```
! Define pool
Router(config)# ip nat pool MYPOOL 203.0.113.1 203.0.113.3 netmask 255.255.255.0

! Define ACL (inside addresses)
Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255

! NAT configuration
Router(config)# ip nat inside source list 1 pool MYPOOL

! Apply to interfaces
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip nat inside

Router(config)# interface gigabitEthernet 0/1
Router(config-if)# ip nat outside
```

---

### PAT (Port Address Translation)

**คำจำกัดความ:**

- **Many-to-one** mapping
- หลาย inside locals → 1 inside global
- แยกด้วย **port numbers**
- เรียกอีกชื่อว่า **NAT Overload**

**Characteristics:**

```
✅ Many devices → 1 public IP
✅ ใช้ port numbers แยก connections
✅ ประหยัด public IPs มากที่สุด
✅ Most common NAT type
✅ Supports 64,000+ connections (65535 ports)
```

**Use Cases:**

```
- Home routers
- Small businesses
- Organizations มี public IP เดียว
- ทุกที่ที่ต้องการประหยัด public IPs
```

**Example:**

```
Topology:
[PC1: 192.168.1.10] ┐
[PC2: 192.168.1.20] ├─ [NAT Router] ─── [Internet: 8.8.8.8]
[PC3: 192.168.1.30] ┘  Public: 203.0.113.5

PAT Translation:

PC1 browsing (HTTP):
  Inside: 192.168.1.10:50001 → 8.8.8.8:80
  After PAT: 203.0.113.5:50001 → 8.8.8.8:80

PC2 browsing (HTTP):
  Inside: 192.168.1.20:50002 → 8.8.8.8:80
  After PAT: 203.0.113.5:50002 → 8.8.8.8:80

PC3 SSH:
  Inside: 192.168.1.30:50003 → 8.8.8.8:22
  After PAT: 203.0.113.5:50003 → 8.8.8.8:22

NAT Table:
  Inside Local          Inside Global        Outside
  192.168.1.10:50001    203.0.113.5:50001    8.8.8.8:80
  192.168.1.20:50002    203.0.113.5:50002    8.8.8.8:80
  192.168.1.30:50003    203.0.113.5:50003    8.8.8.8:22

ทั้ง 3 devices ใช้ public IP เดียวกัน (203.0.113.5)
แต่แยกด้วย port numbers ต่างกัน
```

**PAT with Pool:**

```
! สามารถใช้ PAT กับ pool ได้
! หลาย public IPs + PAT = connections มากขึ้น

Pool: 203.0.113.1-3
  203.0.113.1 → รองรับ ~65000 connections
  203.0.113.2 → รองรับ ~65000 connections
  203.0.113.3 → รองรับ ~65000 connections
  Total: ~195,000 connections
```

**Configuration (Preview):**

```
! PAT using interface IP
Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255
Router(config)# ip nat inside source list 1 interface gigabitEthernet 0/1 overload

! PAT using pool
Router(config)# ip nat pool MYPOOL 203.0.113.1 203.0.113.3 netmask 255.255.255.0
Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255
Router(config)# ip nat inside source list 1 pool MYPOOL overload
```

---

### NAT Types Comparison

```
Feature          Static NAT       Dynamic NAT         PAT
------------------------------------------------------------------------
Mapping          1:1 permanent    1:1 temporary       Many:1
Public IPs       1 per host       Pool                1 or Pool
Direction        Bidirectional    Outbound mainly     Outbound mainly
Use Case         Servers          Few public IPs      Minimal IPs
Inbound Access   Yes              No                  Port forwarding
Cost             High             Medium              Low
Scalability      Low              Medium              High
Common           Servers          Medium orgs         Home/SOHO
------------------------------------------------------------------------
```

**Decision Tree:**

```
Need inbound access to server?
  Yes → Static NAT

Have many public IPs?
  Yes → Dynamic NAT
  No  → PAT

Default recommendation:
  Servers → Static NAT
  Users → PAT
```

---

## 6.3 Configure Static NAT

### Static NAT Configuration Steps

**Step 1: Configure Static Mapping**

**Syntax:**

```
Router(config)# ip nat inside source static <inside-local> <inside-global>
```

**Example:**

```
Router(config)# ip nat inside source static 192.168.1.100 203.0.113.10
```

**Step 2: Define Inside Interface**

**Syntax:**

```
Router(config)# interface <interface-id>
Router(config-if)# ip nat inside
```

**Example:**

```
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip nat inside
```

**Step 3: Define Outside Interface**

**Syntax:**

```
Router(config)# interface <interface-id>
Router(config-if)# ip nat outside
```

**Example:**

```
Router(config)# interface gigabitEthernet 0/1
Router(config-if)# ip nat outside
```

---

### Static NAT Complete Example

**Scenario:**

```
Topology:
[Web Server: 192.168.1.100] ─┬─ [Gi0/0: 192.168.1.1]
[Mail Server: 192.168.1.200] ─┘           R1
                                 [Gi0/1: 203.0.113.1]
                                          |
                                     [Internet]

Requirements:
  - Web Server: 192.168.1.100 → 203.0.113.10
  - Mail Server: 192.168.1.200 → 203.0.113.20
  - Internet users can access servers via public IPs
```

**Configuration:**

```
! Step 1: Configure static NAT mappings
R1(config)# ip nat inside source static 192.168.1.100 203.0.113.10
R1(config)# ip nat inside source static 192.168.1.200 203.0.113.20

! Step 2: Define inside interface
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# ip nat inside
R1(config-if)# no shutdown
R1(config-if)# exit

! Step 3: Define outside interface
R1(config)# interface gigabitEthernet 0/1
R1(config-if)# ip address 203.0.113.1 255.255.255.0
R1(config-if)# ip nat outside
R1(config-if)# no shutdown
R1(config-if)# end

! Step 4: Configure default route (to Internet)
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.254
```

**Verification:**

```
R1# show ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
--- 203.0.113.10       192.168.1.100      ---                ---
--- 203.0.113.20       192.168.1.200      ---                ---

R1# show ip nat statistics
Total active translations: 2 (2 static, 0 dynamic; 0 extended)
Peak translations: 2, occurred 00:05:23 ago
Outside interfaces:
  GigabitEthernet0/1
Inside interfaces:
  GigabitEthernet0/0
...
```

**Testing:**

```
! From Internet
Internet# ping 203.0.113.10
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 203.0.113.10, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5)

Internet# telnet 203.0.113.10 80
Trying 203.0.113.10, 80 ... Open
→ Connected to Web Server (192.168.1.100)

! From Web Server
WebServer# ping 8.8.8.8
!!!!!
Success rate is 100 percent (5/5)
```

---

### Static NAT with Port Forwarding

**คำจำกัดความ:**

- Static NAT แบบระบุ port
- แปลง specific port ไป inside host
- เรียกว่า **Port Forwarding** หรือ **Static PAT**

**Use Case:**

```
- หลาย services บน inside, แต่มี public IP เดียว
- Forward different ports ไป different servers
```

**Syntax:**

```
Router(config)# ip nat inside source static {tcp|udp} 
                <inside-local> <inside-port> <inside-global> <global-port>
```

**Example:**

```
Scenario:
  Public IP: 203.0.113.5 (เดียว)
  Web Server: 192.168.1.100:80
  SSH Server: 192.168.1.200:22
  FTP Server: 192.168.1.300:21

Configuration:
! Web (HTTP port 80)
R1(config)# ip nat inside source static tcp 192.168.1.100 80 203.0.113.5 80

! SSH (port 22)
R1(config)# ip nat inside source static tcp 192.168.1.200 22 203.0.113.5 22

! FTP (port 21)
R1(config)# ip nat inside source static tcp 192.168.1.300 21 203.0.113.5 21

Result:
  Internet → 203.0.113.5:80 → 192.168.1.100:80 (Web)
  Internet → 203.0.113.5:22 → 192.168.1.200:22 (SSH)
  Internet → 203.0.113.5:21 → 192.168.1.300:21 (FTP)
```

**Advanced Port Forwarding:**

```
! Forward external port → different internal port
! Example: External 8080 → Internal 80

R1(config)# ip nat inside source static tcp 192.168.1.100 80 203.0.113.5 8080

Result:
  Internet → 203.0.113.5:8080 → 192.168.1.100:80
  Users access http://203.0.113.5:8080
```

---

## 6.4 Configure Dynamic NAT

### Dynamic NAT Configuration Steps

**Step 1: Define NAT Pool**

**Syntax:**

```
Router(config)# ip nat pool <pool-name> <start-ip> <end-ip> 
                netmask <netmask>
```

**Example:**

```
Router(config)# ip nat pool MYPOOL 203.0.113.10 203.0.113.20 netmask 255.255.255.0
```

**Step 2: Define Access List (Inside Addresses)**

**Syntax:**

```
Router(config)# access-list <number> permit <network> <wildcard>
```

**Example:**

```
Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255
```

**Step 3: Enable Dynamic NAT**

**Syntax:**

```
Router(config)# ip nat inside source list <acl> pool <pool-name>
```

**Example:**

```
Router(config)# ip nat inside source list 1 pool MYPOOL
```

**Step 4: Define Inside/Outside Interfaces**

```
Router(config)# interface <inside-interface>
Router(config-if)# ip nat inside

Router(config)# interface <outside-interface>
Router(config-if)# ip nat outside
```

---

### Dynamic NAT Complete Example

**Scenario:**

```
Topology:
[LAN: 192.168.1.0/24] ─── [R1] ─── [Internet]
   Gi0/0: 192.168.1.1      Gi0/1: 203.0.113.1

Requirements:
  - LAN hosts access Internet via dynamic NAT
  - Public IP pool: 203.0.113.10-20 (11 addresses)
  - Up to 11 concurrent connections
```

**Configuration:**

```
! Step 1: Define NAT pool
R1(config)# ip nat pool INTERNET-POOL 203.0.113.10 203.0.113.20 netmask 255.255.255.0

! Step 2: Define ACL (inside addresses to translate)
R1(config)# access-list 1 remark === LAN Users ===
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255

! Step 3: Enable dynamic NAT
R1(config)# ip nat inside source list 1 pool INTERNET-POOL

! Step 4: Define inside interface
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# ip nat inside
R1(config-if)# no shutdown
R1(config-if)# exit

! Step 5: Define outside interface
R1(config)# interface gigabitEthernet 0/1
R1(config-if)# ip address 203.0.113.1 255.255.255.0
R1(config-if)# ip nat outside
R1(config-if)# no shutdown
R1(config-if)# end

! Step 6: Configure default route
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.254
```

**Verification:**

```
! Before any translations
R1# show ip nat translations
(empty)

! After PC1 (192.168.1.10) pings Internet
PC1# ping 8.8.8.8

R1# show ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
icmp 203.0.113.10:1    192.168.1.10:1     8.8.8.8:1          8.8.8.8:1

! After PC2, PC3 also access Internet
R1# show ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
icmp 203.0.113.10:1    192.168.1.10:1     8.8.8.8:1          8.8.8.8:1
icmp 203.0.113.11:2    192.168.1.20:2     8.8.8.8:2          8.8.8.8:2
tcp  203.0.113.12:1024 192.168.1.30:1024  1.1.1.1:80         1.1.1.1:80
```

**Statistics:**

```
R1# show ip nat statistics
Total active translations: 3 (0 static, 3 dynamic; 3 extended)
Peak translations: 5, occurred 00:10:23 ago
Outside interfaces:
  GigabitEthernet0/1
Inside interfaces:
  GigabitEthernet0/0
Hits: 150  Misses: 0
CEF Translated packets: 150, CEF Punted packets: 0
Expired translations: 10
Dynamic mappings:
-- Inside Source
[Id: 1] access-list 1 pool INTERNET-POOL refcount 3
 pool INTERNET-POOL: netmask 255.255.255.0
        start 203.0.113.10 end 203.0.113.20
        type generic, total addresses 11, allocated 3 (27%), misses 0
```

---

### Dynamic NAT Pool Exhaustion

**ปัญหา:**

```
Pool มี public IPs จำกัด
ถ้า devices มากกว่า pool → ไม่สามารถ translate

Example:
  Pool: 203.0.113.10-20 (11 IPs)
  Devices: 50 devices
  → เฉพาะ 11 devices แรกสามารถ access Internet พร้อมกัน
  → Device ที่ 12 onwards ต้องรอ
```

**Verification:**

```
R1# show ip nat statistics
...
Dynamic mappings:
-- Inside Source
 pool INTERNET-POOL: netmask 255.255.255.0
        start 203.0.113.10 end 203.0.113.20
        type generic, total addresses 11, allocated 11 (100%), misses 5
                                                                    ↑↑↑↑↑
                                                          Pool exhausted!
```

**Solution:**

```
1. เพิ่ม public IPs ใน pool
   R1(config)# ip nat pool INTERNET-POOL 203.0.113.10 203.0.113.30 netmask 255.255.255.0

2. ใช้ PAT (แนะนำ)
   R1(config)# ip nat inside source list 1 pool INTERNET-POOL overload
```

---

## 6.5 Configure PAT

### PAT Configuration Methods

**2 methods:**

```
Method 1: PAT using Interface IP
  - ใช้ IP address ของ outside interface
  - Single public IP
  - Common for SOHO/Home

Method 2: PAT using Pool
  - ใช้ pool ของ public IPs
  - PAT บน IP แรก, ถ้าเต็มไป IP ถัดไป
  - Scalability สูง
```

---

### Method 1: PAT Using Interface IP

**Syntax:**

```
Router(config)# ip nat inside source list <acl> interface <interface> overload
```

**คำอธิบาย:**

```
- ใช้ IP address ของ outside interface
- "overload" = PAT (many-to-one + ports)
```

**Complete Example:**

```
! Step 1: Define ACL (inside addresses)
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255

! Step 2: Enable PAT using interface
R1(config)# ip nat inside source list 1 interface gigabitEthernet 0/1 overload

! Step 3: Define inside interface
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip nat inside

! Step 4: Define outside interface
R1(config)# interface gigabitEthernet 0/1
R1(config-if)# ip nat outside
```

**Verification:**

```
R1# show ip nat translations
Pro Inside global        Inside local       Outside local      Outside global
tcp 203.0.113.1:1024     192.168.1.10:1024  8.8.8.8:80         8.8.8.8:80
tcp 203.0.113.1:1025     192.168.1.20:1025  8.8.8.8:80         8.8.8.8:80
tcp 203.0.113.1:1026     192.168.1.30:1026  1.1.1.1:443        1.1.1.1:443
                ↑↑↑↑
        Same IP, different ports
```

---

### Method 2: PAT Using Pool

**Syntax:**

```
Router(config)# ip nat pool <pool-name> <start-ip> <end-ip> netmask <netmask>
Router(config)# ip nat inside source list <acl> pool <pool-name> overload
```

**Complete Example:**

```
! Step 1: Define NAT pool
R1(config)# ip nat pool PAT-POOL 203.0.113.10 203.0.113.12 netmask 255.255.255.0

! Step 2: Define ACL
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255

! Step 3: Enable PAT using pool
R1(config)# ip nat inside source list 1 pool PAT-POOL overload

! Step 4: Define interfaces
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip nat inside

R1(config)# interface gigabitEthernet 0/1
R1(config-if)# ip nat outside
```

**การทำงาน:**

```
Pool: 203.0.113.10-12 (3 IPs)

1. ใช้ 203.0.113.10 + PAT → ~65000 connections
2. ถ้า 203.0.113.10 ports เต็ม → ใช้ 203.0.113.11 + PAT
3. ถ้า 203.0.113.11 ports เต็ม → ใช้ 203.0.113.12 + PAT

Total capacity: ~195,000 connections
```

---

### PAT Complete Scenario

**Scenario:**

```
Topology:
[PC1: 192.168.1.10] ┐
[PC2: 192.168.1.20] ├─ [Gi0/0: 192.168.1.1]
[PC3: 192.168.1.30] ┘           R1
                        [Gi0/1: 203.0.113.5]
                                 |
                            [Internet]

Requirements:
  - All LAN devices access Internet
  - Single public IP (203.0.113.5)
  - Use PAT
```

**Configuration:**

```
! Step 1: Define ACL (inside network)
R1(config)# ip access-list standard LAN-NAT
R1(config-std-nacl)# remark === LAN Users ===
R1(config-std-nacl)# permit 192.168.1.0 0.0.0.255
R1(config-std-nacl)# exit

! Step 2: Enable PAT using interface
R1(config)# ip nat inside source list LAN-NAT interface gigabitEthernet 0/1 overload

! Step 3: Configure inside interface
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# description === LAN ===
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# ip nat inside
R1(config-if)# no shutdown
R1(config-if)# exit

! Step 4: Configure outside interface
R1(config)# interface gigabitEthernet 0/1
R1(config-if)# description === Internet ===
R1(config-if)# ip address 203.0.113.5 255.255.255.0
R1(config-if)# ip nat outside
R1(config-if)# no shutdown
R1(config-if)# end

! Step 5: Default route
R1(config)# ip route 0.0.0.0 0.0.0.0 gigabitEthernet 0/1 203.0.113.254

! Save configuration
R1# copy running-config startup-config
```

**Testing:**

```
! PC1 ping Internet
PC1# ping 8.8.8.8
!!!!!
Success rate is 100 percent (5/5)

! PC2 browse web
PC2# telnet 1.1.1.1 80
Trying 1.1.1.1, 80 ... Open

! PC3 SSH
PC3# ssh admin@8.8.4.4
```

**Verification:**

```
R1# show ip nat translations
Pro Inside global        Inside local       Outside local      Outside global
icmp 203.0.113.5:1       192.168.1.10:1     8.8.8.8:1          8.8.8.8:1
tcp  203.0.113.5:50001   192.168.1.20:50001 1.1.1.1:80         1.1.1.1:80
tcp  203.0.113.5:50002   192.168.1.30:50002 8.8.4.4:22         8.8.4.4:22

R1# show ip nat statistics
Total active translations: 3 (0 static, 3 dynamic; 3 extended)
Peak translations: 10, occurred 00:05:23 ago
Outside interfaces:
  GigabitEthernet0/1
Inside interfaces:
  GigabitEthernet0/0
Hits: 250  Misses: 0
...
Dynamic mappings:
-- Inside Source
[Id: 1] access-list LAN-NAT interface GigabitEthernet0/1 refcount 3
```

---

## 6.6 Verify NAT

### NAT Verification Commands

#### show ip nat translations

**Purpose:** แสดง NAT translation table

**Syntax:**

```
Router# show ip nat translations [verbose]
```

**Output:**

```
R1# show ip nat translations
Pro Inside global        Inside local       Outside local      Outside global
tcp 203.0.113.5:50001   192.168.1.10:50001 8.8.8.8:80         8.8.8.8:80
tcp 203.0.113.5:50002   192.168.1.20:50002 1.1.1.1:443        1.1.1.1:443
icmp 203.0.113.5:1      192.168.1.30:1     8.8.4.4:1          8.8.4.4:1
---  203.0.113.10       192.168.1.100      ---                ---
                        ↑↑↑↑↑↑↑↑↑↑↑↑↑↑
                    Static NAT (no protocol/port)
```

**Verbose Output:**

```
R1# show ip nat translations verbose
Pro Inside global        Inside local       Outside local      Outside global
tcp 203.0.113.5:50001   192.168.1.10:50001 8.8.8.8:80         8.8.8.8:80
    create 00:00:05, use 00:00:02 timeout:86400000, left 23:59:55
    Flags: extended
```

---

#### show ip nat statistics

**Purpose:** แสดง NAT statistics

**Syntax:**

```
Router# show ip nat statistics
```

**Output:**

```
R1# show ip nat statistics
Total active translations: 3 (1 static, 2 dynamic; 2 extended)
Peak translations: 10, occurred 00:15:23 ago
Outside interfaces:
  GigabitEthernet0/1
Inside interfaces:
  GigabitEthernet0/0
Hits: 500  Misses: 5
CEF Translated packets: 500, CEF Punted packets: 0
Expired translations: 20
Dynamic mappings:
-- Inside Source
[Id: 1] access-list 1 interface GigabitEthernet0/1 refcount 2
```

**ฟิลด์สำคัญ:**

```
Total active translations: จำนวน translations ปัจจุบัน
  - Static: Static NAT mappings
  - Dynamic: Dynamic/PAT mappings
  - Extended: Includes port numbers

Peak translations: จำนวน translations สูงสุดที่เคยมี

Hits: จำนวน packets ที่ match NAT และถูก translate
Misses: จำนวน packets ที่ควร NAT แต่ไม่สามารถ (pool exhausted)

Expired translations: จำนวน translations ที่ timeout และถูกลบ
```

---

#### show running-config | include nat

**Purpose:** แสดง NAT configuration

**Syntax:**

```
Router# show running-config | include nat
Router# show running-config | section nat
```

**Output:**

```
R1# show running-config | include nat
ip nat inside source list 1 interface GigabitEthernet0/1 overload
ip nat inside source static 192.168.1.100 203.0.113.10
interface GigabitEthernet0/0
 ip nat inside
interface GigabitEthernet0/1
 ip nat outside
```

---

#### debug ip nat

**Purpose:** Debug NAT translations real-time

**⚠️ Warning: CPU intensive - ใช้ระวัง**

**Syntax:**

```
Router# debug ip nat
Router# debug ip nat detailed
```

**Output:**

```
R1# debug ip nat
IP NAT debugging is on

*Mar  1 00:00:12.345: NAT: s=192.168.1.10->203.0.113.5, d=8.8.8.8 [123]
*Mar  1 00:00:12.346: NAT*: s=8.8.8.8, d=203.0.113.5->192.168.1.10 [123]
```

**ปิด debug:**

```
R1# no debug ip nat
R1# undebug all
```

---

### Clear NAT Translations

**Purpose:** ลบ NAT translations จาก table

**Syntax:**

```
! Clear all dynamic translations
Router# clear ip nat translation *

! Clear specific translation
Router# clear ip nat translation inside <global-ip> <local-ip>
Router# clear ip nat translation outside <global-ip> <local-ip>

! Clear specific protocol/port
Router# clear ip nat translation protocol inside <global-ip> <global-port> <local-ip> <local-port>
```

**ตัวอย่าง:**

```
! Clear all
R1# clear ip nat translation *

! Clear specific
R1# clear ip nat translation inside 203.0.113.5 192.168.1.10

! Clear TCP connection
R1# clear ip nat translation tcp inside 203.0.113.5 50001 192.168.1.10 50001
```

**⚠️ Warning:**

```
- ลบ translations → connections ตัด
- ใช้เฉพาะเมื่อจำเป็น (troubleshooting)
- Static NAT ไม่สามารถ clear
```

---

### NAT Troubleshooting Workflow

**1. Verify NAT configuration:**

```
R1# show running-config | include nat
! ตรวจสอบ:
  - ACL correct?
  - Pool defined?
  - Inside/Outside interfaces correct?
```

**2. Check translations exist:**

```
R1# show ip nat translations
! ควรเห็น translations เมื่อ inside hosts access outside
! ถ้าไม่มี → NAT ไม่ทำงาน
```

**3. Check statistics:**

```
R1# show ip nat statistics
! ตรวจสอบ:
  - Hits (should increase)
  - Misses (should be 0 or low)
  - Pool allocation (not 100%)
```

**4. Test connectivity:**

```
! From inside host
PC# ping 8.8.8.8

! Extended ping from router
R1# ping
Source address: 192.168.1.10
Target: 8.8.8.8
```

**5. Enable debug (if needed):**

```
R1# debug ip nat
! Test traffic
! Observe translations
R1# undebug all
```

**6. Check routing:**

```
R1# show ip route
! Default route to Internet?
! Route to inside networks?
```

---

### Common NAT Problems

#### Problem 1: No Translations

**Symptom:**

```
show ip nat translations = empty
Inside hosts ไม่สามารถ access Internet
```

**Causes:**

```
1. ACL ไม่ match inside addresses
2. Inside/Outside interfaces กำหนดผิด
3. NAT pool ไม่ถูกต้อง
```

**Solutions:**

```
! Check ACL
R1# show access-lists
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255

! Check interfaces
R1# show ip interface brief | include nat
R1(config)# interface gi0/0
R1(config-if)# ip nat inside

! Check pool
R1# show ip nat statistics
```

#### Problem 2: Pool Exhausted

**Symptom:**

```
show ip nat statistics: misses > 0
Some hosts cannot access Internet
```

**Solution:**

```
! Option 1: Enlarge pool
R1(config)# ip nat pool MYPOOL 203.0.113.10 203.0.113.50 netmask 255.255.255.0

! Option 2: Use PAT (แนะนำ)
R1(config)# ip nat inside source list 1 pool MYPOOL overload
```

#### Problem 3: Wrong Interface

**Symptom:**

```
Translations เกิดบน interface ผิด
```

**Solution:**

```
R1# show ip nat statistics
! ดู Inside/Outside interfaces

R1(config)# interface gi0/0
R1(config-if)# no ip nat outside  ! ลบผิด
R1(config-if)# ip nat inside      ! ใส่ถูก
```

#### Problem 4: Static NAT Not Working

**Symptom:**

```
Cannot access inside server from Internet
```

**Causes:**

```
1. Static mapping ผิด
2. Firewall/ACL block
3. Routing problem
```

**Solutions:**

```
! Check static mapping
R1# show ip nat translations
R1# show running-config | include static

! Check ACL บน outside interface
R1# show ip interface gi0/1

! Test from outside
Internet# telnet 203.0.113.10 80
```

---

## 6.7 Configure NAT for IPv6

### NAT for IPv6 Overview

**คำจำกัดความ:**

- IPv6 มี addresses มากพอ (340 undecillion)
- ไม่จำเป็นต้องใช้ NAT เหมือน IPv4
- แต่ยังมี use cases บางอย่าง

**IPv6 NAT Types:**

#### 1. NAT64

**คำจำกัดความ:**

- แปลง IPv6 ↔ IPv4
- ใช้เมื่อ IPv6-only hosts ต้องการ access IPv4 Internet
- ใช้ DNS64 + NAT64

**Use Case:**

```
[IPv6-only LAN] --- [NAT64 Router] --- [IPv4 Internet]

Example:
  IPv6 host: 2001:db8::10
  Want to access: 8.8.8.8 (IPv4)
  NAT64 translates between IPv6/IPv4
```

#### 2. NPTv6 (Network Prefix Translation)

**คำจำกัดความ:**

- แปลง IPv6 prefix → IPv6 prefix
- เหมือน Static NAT สำหรับ IPv6
- ใช้เมื่อเปลี่ยน ISP (renumber prefixes)

**Use Case:**

```
Inside: 2001:db8:1::/48 → Outside: 2001:db9:2::/48
```

**หมายเหตุ:**

```
⚠️ NAT64 และ NPTv6 ไม่ใช่หัวข้อหลักใน CCNA
⚠️ CCNA มุ่งเน้น NAT สำหรับ IPv4
⚠️ IPv6 ออกแบบให้ไม่ต้องใช้ NAT
```

---

## Summary (สรุป)

Module 6 นี้เราได้เรียนรู้:

1. ✅ **NAT Characteristics**:
    
    - แปลง private → public IPs
    - ประหยัด IPv4 addresses
    - Inside/Outside terminology
    - Inside Local, Inside Global, Outside Local, Outside Global
2. ✅ **NAT Types**:
    
    - **Static NAT**: 1:1 permanent (servers)
    - **Dynamic NAT**: 1:1 temporary (pool)
    - **PAT**: Many:1 (overload, most common)
3. ✅ **Static NAT Configuration**:
    
    ```
    ip nat inside source static <inside-local> <inside-global>
    interface <inside>
     ip nat inside
    interface <outside>
     ip nat outside
    ```
    
4. ✅ **Dynamic NAT Configuration**:
    
    ```
    ip nat pool <n> <start> <end> netmask <mask>
    access-list <n> permit <network> <wildcard>
    ip nat inside source list <n> pool <n>
    ```
    
5. ✅ **PAT Configuration**:
    
    ```
    Method 1: Using Interface
    ip nat inside source list <n> interface <interface> overload
    
    Method 2: Using Pool
    ip nat inside source list <n> pool <n> overload
    ```
    
6. ✅ **Verification**:
    
    - show ip nat translations
    - show ip nat statistics
    - clear ip nat translation *
    - debug ip nat (ระวัง CPU)
7. ✅ **Port Forwarding**:
    
    ```
    ip nat inside source static tcp <inside-ip> <inside-port> 
                                      <outside-ip> <outside-port>
    ```
    

**สิ่งสำคัญที่ต้องจำ:**

- NAT แก้ IPv4 address exhaustion
- Private IPs: 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16
- Static NAT = Servers (inbound access)
- Dynamic NAT = Pool of IPs (outbound)
- PAT = Most efficient (many → 1 + ports)
- "overload" keyword = PAT
- Inside interface = LAN (private IPs)
- Outside interface = Internet (public IPs)
- ACL defines ใครที่ต้องการ NAT
- Pool exhausted → Use PAT!

**NAT Decision Tree:**

```
Need inbound access?
  Yes → Static NAT (with port forwarding if needed)
  
How many public IPs available?
  Many → Dynamic NAT
  Few → PAT with Pool
  One → PAT with Interface (overload)
  
Default recommendation:
  Servers → Static NAT
  Users → PAT (overload)
```

**Configuration Template:**

```
! PAT (Most Common)
access-list 1 permit 192.168.0.0 0.0.255.255
ip nat inside source list 1 interface <outside-if> overload

interface <inside-if>
 ip nat inside

interface <outside-if>
 ip nat outside

! Static NAT (Servers)
ip nat inside source static <inside-ip> <outside-ip>

! Port Forwarding
ip nat inside source static tcp <inside-ip> <port> <outside-ip> <port>
```

**Troubleshooting Checklist:**

```
☐ ACL matches inside addresses?
☐ Inside/Outside interfaces correct?
☐ Pool defined correctly?
☐ Translations appear in table?
☐ Statistics show hits (not misses)?
☐ Default route to Internet?
☐ Inside hosts can reach router?
```

**Next Module:** Module 7 - WAN Concepts

---

**[ไฟล์ Module 6 สมบูรณ์แล้ว!]**