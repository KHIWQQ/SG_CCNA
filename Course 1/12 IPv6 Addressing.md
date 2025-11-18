# CCNA Course 1 - Module 12: IPv6 Addressing

## การกำหนดที่อยู่ IPv6

---

## 12.1 IPv4 Issues and IPv6 Overview

### IPv4 Limitations (ข้อจำกัดของ IPv4)

#### 1. Address Exhaustion (IP หมด)

**ปัญหา:**

- IPv4 มีเพียง **4.3 billion addresses** (2^32)
- Population โลก > 8 billion
- อุปกรณ์ต่อคน > 1 เครื่อง (PC, Phone, Tablet, IoT)

**Timeline:**

```
1980s: IPv4 ดูเพียงพอ
1990s: เริ่มเห็นปัญหา
2011: IANA IPv4 pool หมด
2019: Regional registries หมดหมด
ปัจจุบัน: ต้องซื้อ IPv4 addresses (แพงมาก)
```

**Solution ชั่วคราว:**

- **NAT** (Network Address Translation)
- **Private IPs** (RFC 1918)
- **CIDR** (Classless Inter-Domain Routing)

แต่ไม่ใช่ solution ระยะยาว!

#### 2. Complex Configuration

**ปัญหา:**

- ต้องกำหนด IP manually หรือใช้ DHCP
- Subnet mask ซับซ้อน
- ไม่มี built-in auto-configuration ที่ดี

#### 3. Lack of Built-in Security

**ปัญหา:**

- IPv4 ไม่มี encryption/authentication built-in
- IPsec = optional (ไม่บังคับ)
- Vulnerable to attacks

#### 4. QoS Limitations

**ปัญหา:**

- ToS field จำกัด (8 bits)
- ไม่เพียงพอสำหรับ modern applications (VoIP, Streaming)

---

### IPv6 Overview

**คำจำกัดความ:**

- Internet Protocol version 6
- Successor ของ IPv4
- พัฒนาโดย IETF เพื่อแก้ปัญหา IPv4

**คุณสมบัติหลัก:**

#### 1. Larger Address Space

**IPv4:** 32 bits = 4.3 billion addresses **IPv6:** **128 bits** = **340 undecillion addresses**

```
340,282,366,920,938,463,463,374,607,431,768,211,456 addresses

หรือ 3.4 × 10^38

เปรียบเทียบ:
- 340 trillion trillion trillion addresses
- 670 million trillion addresses ต่อตารางมิลลิเมตรของโลก!
```

#### 2. Simplified Header

**IPv6 header:**

- ✅ Fixed 40 bytes (vs 20-60 bytes ใน IPv4)
- ✅ Fewer fields (8 vs 12-13 fields)
- ✅ Faster processing by routers
- ✅ No checksum (offload to Layer 2/4)
- ✅ No fragmentation by routers (only by source)

#### 3. Built-in Security

- ✅ **IPsec mandatory** (built-in)
- ✅ Encryption และ Authentication
- ✅ More secure by design

#### 4. Better QoS

- ✅ **Traffic Class** field (8 bits) - เหมือน ToS
- ✅ **Flow Label** (20 bits) - ระบุ flow สำหรับ QoS

#### 5. Auto-configuration

- ✅ **SLAAC** (Stateless Address Auto-Configuration)
- ✅ ไม่ต้องใช้ DHCP (optional)
- ✅ Plug-and-play

#### 6. No Broadcast

- ✅ ไม่มี broadcast (ใช้ multicast แทน)
- ✅ ลด network traffic
- ✅ More efficient

#### 7. Hierarchical Addressing

- ✅ Easier routing
- ✅ Aggregation
- ✅ Smaller routing tables

---

### IPv4 vs IPv6 Comparison

```
Feature              IPv4                    IPv6
------------------------------------------------------------------------
Address Size         32 bits                 128 bits
Address Space        4.3 billion             340 undecillion
Notation             Decimal (192.168.1.1)   Hexadecimal (2001:db8::1)
Header Size          20-60 bytes (variable)  40 bytes (fixed)
Header Fields        12-13 fields            8 fields
Checksum             Yes                     No
Fragmentation        Router + Source         Source only
IPsec                Optional                Mandatory (built-in)
Broadcast            Yes                     No (use multicast)
ARP                  Yes                     No (use NDP)
DHCP                 DHCPv4                  DHCPv6 or SLAAC
Configuration        Manual/DHCP             Auto (SLAAC) or DHCPv6
QoS                  ToS (8 bits)            Traffic Class + Flow Label
NAT                  Common (required)       Not needed (but possible)
------------------------------------------------------------------------
```

---

### IPv6 Adoption Status

**Current Status (2024):**

- **~40-45% adoption** globally (Google statistics)
- Major platforms support IPv6:
    - ✅ All modern OS (Windows, macOS, Linux, iOS, Android)
    - ✅ Major ISPs
    - ✅ Cloud providers (AWS, Google Cloud, Azure)
    - ✅ CDNs (Cloudflare, Akamai)

**Challenges:**

- ❌ Legacy devices/applications
- ❌ Training/Knowledge gap
- ❌ Migration costs
- ❌ "If it ain't broke, don't fix it" mentality

**Future:**

- IPv6 adoption increasing
- Dual-stack (IPv4 + IPv6) common
- Eventually IPv6-only

---

## 12.2 IPv6 Address Representation

### IPv6 Address Format

**โครงสร้าง:**

- **128 bits** (16 bytes)
- เขียนใน **Hexadecimal** (ฐาน 16)
- แบ่งเป็น **8 groups** (hextets)
- แต่ละ group = **16 bits** (4 hex digits)
- คั่นด้วย **colon (:)**

**รูปแบบเต็ม:**

```
2001:0db8:85a3:0000:0000:8a2e:0370:7334

แบ่งเป็น:
2001 : 0db8 : 85a3 : 0000 : 0000 : 8a2e : 0370 : 7334
^^^^   ^^^^   ^^^^   ^^^^   ^^^^   ^^^^   ^^^^   ^^^^
Group1 Group2 Group3 Group4 Group5 Group6 Group7 Group8
16 bits each = 8 × 16 = 128 bits total
```

**Hexadecimal Digits:**

```
0 1 2 3 4 5 6 7 8 9 A B C D E F
(0-15 in decimal)
```

---

### IPv6 Address Shortening Rules

#### Rule 1: Omit Leading Zeros (ตัด 0 นำ)

**ตัดเลข 0 ที่อยู่หน้าแต่ละ group**

**ตัวอย่าง:**

```
Original: 2001:0db8:0000:0000:0000:0000:0000:0001

Apply Rule 1:
2001:db8:0:0:0:0:0:1
      ^   ^ ^ ^ ^ ^
    ตัด 0 นำออก
```

**หมายเหตุ:**

- ต้องเหลือเลขอย่างน้อย **1 ตัว** ใน group
- ไม่สามารถตัดหมดเป็นช่องว่างได้

**ตัวอย่างเพิ่มเติม:**

```
0db8 → db8
0000 → 0
00a3 → a3
0001 → 1
```

#### Rule 2: Double Colon (::)

**แทน groups ของ all zeros ต่อเนื่องกัน ด้วย `::`**

**กฎ:**

- ✅ ใช้ได้เฉพาะ **ครั้งเดียว** ใน address เดียว
- ✅ แทน **1 หรือมากกว่า** groups ของ all zeros
- ✅ สามารถอยู่ **ต้น กลาง หรือท้าย** address

**ตัวอย่าง:**

```
Original: 2001:0db8:0000:0000:0000:0000:0000:0001

Step 1 (Rule 1): 2001:db8:0:0:0:0:0:1

Step 2 (Rule 2): 2001:db8::1
                        ^^
                แทน 0:0:0:0:0
```

**ตัวอย่างเพิ่มเติม:**

**Example 1:**

```
Full:       2001:0db8:0000:0000:0000:ff00:0042:8329
Rule 1:     2001:db8:0:0:0:ff00:42:8329
Rule 2:     2001:db8::ff00:42:8329
```

**Example 2:**

```
Full:       fe80:0000:0000:0000:0123:4567:89ab:cdef
Rule 1:     fe80:0:0:0:123:4567:89ab:cdef
Rule 2:     fe80::123:4567:89ab:cdef
```

**Example 3:**

```
Full:       ff02:0000:0000:0000:0000:0000:0000:0001
Rule 1:     ff02:0:0:0:0:0:0:1
Rule 2:     ff02::1
```

**Example 4:**

```
Full:       0000:0000:0000:0000:0000:0000:0000:0001
Rule 2:     ::1 (Loopback address)
```

**Example 5:**

```
Full:       0000:0000:0000:0000:0000:0000:0000:0000
Rule 2:     :: (Unspecified address)
```

---

### IPv6 Address Expansion

**กระบวนการขยาย :: กลับ:**

**ตัวอย่าง: ขยาย `2001:db8::1`**

```
Step 1: นับ groups ที่มี
  2001:db8::1
  ^^^^^^^ ^
  2 groups + 1 group = 3 groups

Step 2: คำนวณ groups ที่หาย
  Total groups = 8
  Missing = 8 - 3 = 5 groups

Step 3: แทน :: ด้วย 0:0:0:0:0
  2001:db8:0:0:0:0:0:1

Step 4: เติม leading zeros (optional)
  2001:0db8:0000:0000:0000:0000:0000:0001
```

**ตัวอย่างเพิ่มเติม:**

**Example 1: `fe80::1`**

```
Groups: fe80 :: 1 = 2 groups
Missing: 8 - 2 = 6 groups

Expanded: fe80:0:0:0:0:0:0:1
Full:     fe80:0000:0000:0000:0000:0000:0000:0001
```

**Example 2: `2001:db8:1::1`**

```
Groups: 2001:db8:1 :: 1 = 4 groups
Missing: 8 - 4 = 4 groups

Expanded: 2001:db8:1:0:0:0:0:1
Full:     2001:0db8:0001:0000:0000:0000:0000:0001
```

---

### IPv6 Prefix Length

**คำจำกัดความ:**

- เหมือน subnet mask ใน IPv4
- บอกจำนวน **network bits**
- เขียนเป็น **/prefix-length**

**รูปแบบ:**

```
2001:db8:acad:1::1/64
                   ^^^
                   Prefix length (64 bits = network)

Network bits: 64 bits
Host bits:    128 - 64 = 64 bits
```

**Common Prefix Lengths:**

```
/64  - Standard LAN segment (most common)
/128 - Single host (like /32 in IPv4)
/48  - Site prefix (organization)
/32  - ISP allocation
/127 - Point-to-Point links (RFC 6164)
```

**ตัวอย่าง:**

```
Address: 2001:db8:acad:1::1/64

Network Portion (64 bits):
  2001:0db8:acad:0001:0000:0000:0000:0000
  ^^^^^^^^^^^^^^^^^^^^^^^^
  Network (64 bits)

Host Portion (64 bits):
                          ^^^^^^^^^^^^^^^^^^^^^^^^
                          Host/Interface ID (64 bits)
```

---

## 12.3 IPv6 Address Types

### IPv6 Address Types Overview

**IPv6 มี 3 ประเภทหลัก:**

1. **Unicast** - One-to-one
2. **Multicast** - One-to-many
3. **Anycast** - One-to-nearest

**ไม่มี Broadcast!** (ใช้ multicast แทน)

---

### 1. Unicast Addresses

**คำจำกัดความ:**

- ส่งไปยัง**อุปกรณ์เดียว**
- ใช้มากที่สุด

**ประเภทของ Unicast:**

#### Global Unicast Address (GUA)

**คำจำกัดความ:**

- เทียบเท่า **Public IPv4**
- **Routable** on the Internet
- **Globally unique**

**Range:** `2000::/3` (2000:: - 3fff:ffff:ffff:ffff:ffff:ffff:ffff:ffff)

**โครงสร้าง:**

```
|      48 bits     | 16 bits |      64 bits        |
|------------------|---------|---------------------|
| Global Routing   | Subnet  | Interface ID        |
| Prefix           | ID      |                     |
```

**ตัวอย่าง:**

```
2001:db8:acad:1::1/64

2001:db8:acad = Global Routing Prefix (ได้จาก ISP/RIR)
1             = Subnet ID (กำหนดเอง)
::1           = Interface ID (EUI-64 หรือกำหนดเอง)
```

**Current Allocation:**

```
2001::/32  - IANA (ใช้ตอนนี้)
2001:db8::/32 - Documentation (ห้ามใช้จริง)
2002::/16  - 6to4 transition
```

**การใช้งาน:**

- Internet-facing devices
- Public servers
- End-user devices (via ISP)

#### Link-Local Address

**คำจำกัดความ:**

- ใช้ภายใน **local link** เท่านั้น
- **Non-routable** (Router ไม่ forward)
- **Required** ทุก IPv6 interface
- เทียบเท่า APIPA (169.254.x.x) แต่ใน IPv6 **ใช้เสมอ**

**Range:** `fe80::/10` (fe80:: - febf:ffff:ffff:ffff:ffff:ffff:ffff:ffff)

**แต่ในทางปฏิบัติ:** `fe80::/64`

**รูปแบบ:**

```
fe80:0000:0000:0000:<Interface ID>/64
^^^^^^^^^^^^^^^^^^^^^^^^^
Fixed prefix (64 bits)
                    ^^^^^^^^^^^^^^^^
                    Interface ID (64 bits)
```

**ตัวอย่าง:**

```
fe80::1
fe80::a8bb:ccff:fe11:2233
fe80::1234:5678:90ab:cdef
```

**การใช้งาน:**

- ✅ Neighbor Discovery Protocol (NDP)
- ✅ Router Discovery
- ✅ Auto-configuration
- ✅ Default gateway (ใช้ link-local ของ router)
- ✅ Routing protocols

**หมายเหตุ:**

- ทุก IPv6 interface มี link-local **อัตโนมัติ**
- ไม่ต้องกำหนดเอง (แต่สามารถกำหนดได้)
- ต้องระบุ **Zone ID** (interface) เมื่อใช้
    - ตัวอย่าง: `fe80::1%eth0`

#### Unique Local Address (ULA)

**คำจำกัดความ:**

- เทียบเท่า **Private IPv4** (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16)
- ใช้ภายใน **site/organization**
- **Non-routable** on the Internet
- แต่ **routable ภายใน organization**

**Range:** `fc00::/7` (fc00:: - fdff:ffff:ffff:ffff:ffff:ffff:ffff:ffff)

**แต่ในทางปฏิบัติใช้:** `fd00::/8`

**โครงสร้าง:**

```
| 8 bits |  40 bits   | 16 bits |   64 bits      |
|--------|------------|---------|----------------|
|  fd    | Global ID  | Subnet  | Interface ID   |
|        | (random)   | ID      |                |
```

**ตัวอย่าง:**

```
fd00:1234:5678:1::1/64

fd                = Prefix (fixed)
00:1234:5678      = Global ID (random, pseudo-random)
1                 = Subnet ID
::1               = Interface ID
```

**การใช้งาน:**

- Internal networks (ไม่ต้อง Internet)
- Site-to-site VPN
- Lab/Test environments

**ข้อดีเหนือ Link-Local:**

- ✅ Routable ภายใน organization
- ✅ สามารถใช้ข้าม subnets

#### Loopback Address

**Address:** `::1/128`

**คำจำกัดความ:**

- เทียบเท่า **127.0.0.1** ใน IPv4
- ส่งกลับไปยัง**ตัวเอง**
- ทดสอบ TCP/IP stack

**การใช้งาน:**

```
ping ::1
→ ทดสอบว่า IPv6 stack ทำงาน
```

#### Unspecified Address

**Address:** `::/128`

**คำจำกัดความ:**

- เทียบเท่า **0.0.0.0** ใน IPv4
- แสดงว่า**ยังไม่มี IP address**
- ไม่สามารถใช้เป็น destination

**การใช้งาน:**

```
Source IP ในขณะทำ DHCPv6 Discovery
หรือ DAD (Duplicate Address Detection)

Source: ::
Destination: ff02::1
```

---

### 2. Multicast Addresses

**คำจำกัดความ:**

- ส่งไปยัง**กลุ่มอุปกรณ์**
- One-to-many
- แทน broadcast ใน IPv4

**Range:** `ff00::/8` (ff00:: - ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff)

**โครงสร้าง:**

```
| 8 bits | 4 bits | 4 bits |        112 bits          |
|--------|--------|--------|--------------------------|
|  ff    | Flags  | Scope  | Group ID                 |
```

**Flags (4 bits):**

```
0 = Permanent (well-known)
1 = Temporary (transient)
```

**Scope (4 bits):**

```
1 = Interface-local (loopback)
2 = Link-local (same subnet)
4 = Admin-local
5 = Site-local (same site)
8 = Organization-local
E = Global
```

**Well-Known Multicast Addresses:**

```
Address          Scope       Purpose
------------------------------------------------------------------------
ff02::1          Link-local  All nodes (like broadcast)
ff02::2          Link-local  All routers
ff02::5          Link-local  OSPF routers
ff02::6          Link-local  OSPF designated routers
ff02::9          Link-local  RIPng routers
ff02::a          Link-local  EIGRP routers
ff02::d          Link-local  PIM routers
ff02::1:2        Link-local  DHCP agents/relay
ff02::1:ff00:0/104 Link-local Solicited-Node multicast
------------------------------------------------------------------------
```

**Solicited-Node Multicast:**

```
Format: ff02::1:ff + last 24 bits of IPv6 address

Example:
IPv6: 2001:db8::a8bb:ccff:fe11:2233
                     ^^^^^^^^ Last 24 bits = 11:2233

Solicited-Node: ff02::1:ff11:2233
```

**การใช้งาน:**

- Neighbor Discovery (NS/NA)
- Routing protocols
- Service discovery
- Efficient group communication

---

### 3. Anycast Addresses

**คำจำกัดความ:**

- ส่งไปยัง**อุปกรณ์ที่ใกล้ที่สุด** ในกลุ่ม
- One-to-nearest
- Same address กำหนดให้**หลายอุปกรณ์**

**ลักษณะ:**

- เหมือน unicast address (รูปแบบเดียวกัน)
- แต่กำหนดให้หลาย interfaces
- Routing protocol เลือกเส้นทางที่ใกล้ที่สุด

**การใช้งาน:**

- **DNS root servers** (root DNS ใช้ anycast)
- **CDN** (Content Delivery Networks)
- **Load balancing**
- **Redundancy**

**ตัวอย่าง:**

```
Root DNS Server (a.root-servers.net):
IPv6: 2001:503:ba3e::2:30

กระจายไปทั่วโลก:
- Server in US: 2001:503:ba3e::2:30
- Server in EU: 2001:503:ba3e::2:30
- Server in Asia: 2001:503:ba3e::2:30

User query → ถูกส่งไปยัง server ที่ใกล้ที่สุด
```

**Subnet-Router Anycast:**

```
Format: <Prefix>::/128

Example:
Subnet: 2001:db8:acad:1::/64
Anycast: 2001:db8:acad:1::/128
         (all zeros in Interface ID)

กำหนดให้ทุก router ใน subnet นี้
```

---

### IPv6 Address Type Summary

```
Type              Prefix       Example                      Description
----------------------------------------------------------------------------------------
Global Unicast    2000::/3     2001:db8::1                  Public, routable
Link-Local        fe80::/10    fe80::1                      Local link only
Unique Local      fc00::/7     fd12:3456:789a:1::1          Private, site-local
Loopback          ::1/128      ::1                          Self (localhost)
Unspecified       ::/128       ::                           No address yet
Multicast         ff00::/8     ff02::1                      One-to-many
Anycast           (varies)     2001:db8::                   One-to-nearest
----------------------------------------------------------------------------------------
```

---

## 12.4 IPv6 Address Configuration

### Static Configuration

#### Manual Configuration (Cisco Router)

**Enable IPv6 Routing:**

```
Router(config)# ipv6 unicast-routing
```

- **ต้องเปิดก่อนใช้ IPv6 routing**
- Router จะส่ง RA (Router Advertisement)

**Configure Interface:**

```
Router(config)# interface gigabitethernet 0/0
Router(config-if)# ipv6 address 2001:db8:acad:1::1/64
Router(config-if)# no shutdown
Router(config-if)# exit
```

**Configure Link-Local (Optional):**

```
Router(config)# interface gigabitethernet 0/0
Router(config-if)# ipv6 address fe80::1 link-local
Router(config-if)# exit
```

- ถ้าไม่กำหนด link-local จะถูก generate อัตโนมัติ

**ตัวอย่างเต็ม:**

```
Router(config)# ipv6 unicast-routing

Router(config)# interface g0/0
Router(config-if)# description LAN 1
Router(config-if)# ipv6 address 2001:db8:acad:1::1/64
Router(config-if)# ipv6 address fe80::1 link-local
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# interface g0/1
Router(config-if)# description LAN 2
Router(config-if)# ipv6 address 2001:db8:acad:2::1/64
Router(config-if)# ipv6 address fe80::1 link-local
Router(config-if)# no shutdown
Router(config-if)# exit
```

**Verification:**

```
Router# show ipv6 interface brief
Router# show ipv6 interface g0/0
Router# show ipv6 route
```

---

### Dynamic Configuration

#### 1. SLAAC (Stateless Address Auto-Configuration)

**คำจำกัดความ:**

- Host สร้าง IPv6 address **อัตโนมัติ**
- ไม่ต้องใช้ DHCP server
- ใช้ **RA** (Router Advertisement) จาก Router

**กระบวนการ:**

**Step 1: Host boot up**

```
Interface up → generate Link-Local address
```

**Step 2: DAD (Duplicate Address Detection)**

```
ตรวจสอบว่า Link-Local ซ้ำหรือไม่
ส่ง NS (Neighbor Solicitation)
ถ้าไม่มีใครตอบ → OK, ใช้ได้
```

**Step 3: Send Router Solicitation (RS)**

```
Host ส่ง RS:
  Source: Link-Local
  Destination: ff02::2 (All-Routers)
  Message: "มี Router ไหม? ส่ง RA มาเลย"
```

**Step 4: Receive Router Advertisement (RA)**

```
Router ส่ง RA:
  Source: Link-Local ของ Router
  Destination: ff02::1 (All-Nodes) หรือ Unicast
  Information:
    - Prefix: 2001:db8:acad:1::/64
    - Prefix Length: 64
    - Default Gateway: fe80::<router>
    - Flags: A=1 (Auto-config), M=0, O=0
    - MTU, Hop Limit, etc.
```

**Step 5: Generate Global Unicast Address**

```
Host สร้าง GUA:
  Prefix (from RA): 2001:db8:acad:1::
  Interface ID: <generated>

Methods:
  - EUI-64 (Modified EUI-64)
  - Random (Privacy Extensions)
  - Manual
```

**Step 6: DAD for GUA**

```
ตรวจสอบว่า GUA ซ้ำหรือไม่
ถ้า OK → ใช้ได้
```

**Step 7: Communication**

```
Host มี:
  - Link-Local: fe80::<interface-id>
  - Global Unicast: 2001:db8:acad:1::<interface-id>/64
  - Default Gateway: fe80::<router>
```

**RA Flags:**

```
M (Managed) flag:
  0 = ไม่ต้องใช้ DHCPv6 สำหรับ address
  1 = ใช้ DHCPv6 (Stateful)

O (Other) flag:
  0 = ไม่ต้องใช้ DHCPv6 สำหรับ other info
  1 = ใช้ DHCPv6 สำหรับ DNS, etc. (Stateless)

A (Autonomous) flag (in Prefix Information):
  1 = ใช้ prefix นี้สำหรับ SLAAC
  0 = ไม่ใช้ SLAAC
```

**SLAAC Scenarios:**

```
Flags    Method                Description
------------------------------------------------------------------------
M=0,O=0  SLAAC only           Host creates address, no DHCPv6
M=0,O=1  SLAAC + DHCPv6       Address from SLAAC, DNS from DHCPv6
         (Stateless DHCPv6)
M=1,O=0  DHCPv6 only          All from DHCPv6 (rare)
         (Stateful DHCPv6)
M=1,O=1  DHCPv6 + Others      All from DHCPv6
------------------------------------------------------------------------
```

**Enable SLAAC (Cisco Router):**

```
Router(config)# ipv6 unicast-routing
Router(config)# interface g0/0
Router(config-if)# ipv6 address 2001:db8:acad:1::1/64
Router(config-if)# no shutdown
```

- Router จะส่ง RA อัตโนมัติ (ทุก 200 วินาที)

#### 2. EUI-64 (Modified EUI-64)

**คำจำกัดความ:**

- สร้าง Interface ID (64 bits) จาก **MAC address** (48 bits)

**กระบวนการ:**

**Step 1: แบ่ง MAC เป็น 2 ส่วน**

```
MAC: 00:1A:2B:3C:4D:5E

แบ่ง:
  00:1A:2B | 3C:4D:5E
  ^^^^^^^^   ^^^^^^^^
  OUI        NIC Specific
```

**Step 2: แทรก FF:FE ตรงกลาง**

```
00:1A:2B:FF:FE:3C:4D:5E
        ^^^^^
        Insert FFFE
```

**Step 3: Flip bit ที่ 7 (U/L bit)**

```
First Byte: 00 (hex) = 00000000 (binary)
                       ^
                       bit 7 (from right, start at 0)

Flip bit 7:
  00000000 → 00000010 = 02 (hex)

Result: 02:1A:2B:FF:FE:3C:4D:5E
        ^^
        Flipped
```

**Step 4: เขียนใน IPv6 format**

```
021A:2BFF:FE3C:4D5E
```

**Step 5: รวมกับ Prefix**

```
Prefix: 2001:db8:acad:1::/64
Interface ID: 021A:2BFF:FE3C:4D5E

IPv6 Address: 2001:db8:acad:1:021a:2bff:fe3c:4d5e/64
หรือย่อ:      2001:db8:acad:1:21a:2bff:fe3c:4d5e/64
```

**ทำไมต้อง Flip bit 7?**

- U/L bit (Universal/Local bit)
- 0 = Globally unique (universal)
- 1 = Locally administered
- MAC addresses: U/L = 0 (globally unique)
- EUI-64: Flip เป็น 1 → บอกว่า "modified"

**Configure EUI-64 (Cisco):**

```
Router(config)# interface g0/0
Router(config-if)# ipv6 address 2001:db8:acad:1::/64 eui-64
Router(config-if)# no shutdown
```

- Router จะสร้าง Interface ID จาก MAC อัตโนมัติ

**ตัวอย่าง:**

```
Router interface G0/0:
  MAC: 0001.9717.6501

EUI-64:
  Split: 0001.97 | 17.6501
  Insert FFFE: 0001.97FF.FE17.6501
  Flip bit 7: 0201.97FF.FE17.6501
  IPv6 format: 0201:97ff:fe17:6501

Full address:
  2001:db8:acad:1:201:97ff:fe17:6501/64
```

#### 3. Privacy Extensions (Random Interface ID)

**คำจำกัดความ:**

- สร้าง Interface ID แบบ**สุ่ม**
- เปลี่ยนทุกครั้งที่เชื่อมต่อ (หรือตามระยะเวลา)
- ป้องกัน tracking

**เหตุผล:**

- EUI-64 ใช้ MAC address → สามารถ track ได้
- Privacy concern

**การทำงาน:**

- Interface ID = random 64 bits
- เปลี่ยนทุก 24 hours (default) หรือทุกครั้งที่ connect

**Enable (Windows):**

```
netsh interface ipv6 set privacy state=enabled
```

**Enable (Linux):**

```
sysctl -w net.ipv6.conf.all.use_tempaddr=2
```

**ตัวอย่าง:**

```
Connection 1: 2001:db8:acad:1:a4b3:7c2d:91e8:5f6a/64
Connection 2: 2001:db8:acad:1:1a8e:92c7:4b3f:d5e2/64
              (Interface ID เปลี่ยน)
```

#### 4. DHCPv6 (Dynamic Host Configuration Protocol for IPv6)

**คำจำกัดความ:**

- เหมือน DHCP ใน IPv4
- แต่มี 2 modes:
    - **Stateful DHCPv6** - เหมือน DHCPv4 (จัดการ address)
    - **Stateless DHCPv6** - ให้เฉพาะ DNS, domain name (ไม่ให้ address)

**Stateless DHCPv6:**

```
Host:
  - Address: จาก SLAAC
  - DNS, Domain: จาก DHCPv6 server

RA Flags: M=0, O=1
```

**Stateful DHCPv6:**

```
Host:
  - Address: จาก DHCPv6 server
  - DNS, Domain: จาก DHCPv6 server

RA Flags: M=1, O=1
```

**Configure Stateless DHCPv6 (Router as DHCPv6 Server):**

```
! Create DHCPv6 pool
Router(config)# ipv6 dhcp pool IPV6POOL
Router(config-dhcpv6)# dns-server 2001:db8:acad:1::100
Router(config-dhcpv6)# domain-name example.com
Router(config-dhcpv6)# exit

! Apply to interface
Router(config)# interface g0/0
Router(config-if)# ipv6 address 2001:db8:acad:1::1/64
Router(config-if)# ipv6 nd other-config-flag
Router(config-if)# ipv6 dhcp server IPV6POOL
Router(config-if)# no shutdown
```

---

### Verification Commands

**Show IPv6 Interface Brief:**

```
Router# show ipv6 interface brief

GigabitEthernet0/0     [up/up]
    FE80::1
    2001:DB8:ACAD:1::1
GigabitEthernet0/1     [up/up]
    FE80::1
    2001:DB8:ACAD:2::1
```

**Show IPv6 Interface:**

```
Router# show ipv6 interface g0/0

GigabitEthernet0/0 is up, line protocol is up
  IPv6 is enabled, link-local address is FE80::1
  No Virtual link-local address(es):
  Global unicast address(es):
    2001:DB8:ACAD:1::1, subnet is 2001:DB8:ACAD:1::/64
  Joined group address(es):
    FF02::1
    FF02::2
    FF02::1:FF00:1
  MTU is 1500 bytes
  ICMP error messages limited to one every 100 milliseconds
  ICMP redirects are enabled
  ICMP unreachables are sent
  ND DAD is enabled, number of DAD attempts: 1
  ND reachable time is 30000 milliseconds
  ND advertised reachable time is 0 (unspecified)
  ND advertised retransmit interval is 0 (unspecified)
  ND router advertisements are sent every 200 seconds
  ND router advertisements live for 1800 seconds
  ND advertised default router preference is Medium
  Hosts use stateless autoconfig for addresses.
```

**Show IPv6 Route:**

```
Router# show ipv6 route

C   2001:DB8:ACAD:1::/64 [0/0]
     via GigabitEthernet0/0, directly connected
L   2001:DB8:ACAD:1::1/128 [0/0]
     via GigabitEthernet0/0, receive
C   2001:DB8:ACAD:2::/64 [0/0]
     via GigabitEthernet0/1, directly connected
L   2001:DB8:ACAD:2::1/128 [0/0]
     via GigabitEthernet0/1, receive
L   FF00::/8 [0/0]
     via Null0, receive
```

**Show IPv6 Neighbors (IPv6 ARP equivalent):**

```
Router# show ipv6 neighbors

IPv6 Address                      Age Link-layer Addr State Interface
2001:DB8:ACAD:1::10                 0 0011.2233.4455  REACH Gi0/0
FE80::211:22FF:FE33:4455            5 0011.2233.4455  STALE Gi0/0
```

**Ping IPv6:**

```
Router# ping 2001:db8:acad:1::10
Router# ping fe80::1%g0/0
       (ต้องระบุ interface สำหรับ link-local)
```

**Traceroute IPv6:**

```
Router# traceroute 2001:db8:acad:2::10
```

---

## 12.5 IPv6 Transition Mechanisms

### Why Transition Needed?

**ปัญหา:**

- Internet ยังใช้ IPv4 เป็นหลัก
- ไม่สามารถเปลี่ยนเป็น IPv6 ทันที (Big Bang)
- IPv4 และ IPv6 **incompatible** (ไม่สื่อสารกันโดยตรง)

**Solution:**

- Transition mechanisms
- Coexistence (IPv4 + IPv6)

---

### 1. Dual Stack

**คำจำกัดความ:**

- อุปกรณ์รัน**ทั้ง IPv4 และ IPv6** พร้อมกัน
- **แนะนำที่สุด** และใช้มากที่สุด

**การทำงาน:**

```
Device มี 2 stacks:
  - IPv4 stack + IPv4 address
  - IPv6 stack + IPv6 address

เลือกใช้ตาม destination:
  - Destination มี IPv6 → ใช้ IPv6
  - Destination มีเฉพาะ IPv4 → ใช้ IPv4
```

**ตัวอย่าง:**

```
PC1 (Dual Stack):
  IPv4: 192.168.1.10/24
  IPv6: 2001:db8:acad:1::10/64

Server1:
  IPv6: 2001:db8:acad:2::100

Server2:
  IPv4: 203.0.113.50

PC1 → Server1: ใช้ IPv6
PC1 → Server2: ใช้ IPv4
```

**Configure Dual Stack (Cisco):**

```
Router(config)# interface g0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# ipv6 address 2001:db8:acad:1::1/64
Router(config-if)# no shutdown
```

**ข้อดี:**

- ✅ Simple
- ✅ Native support (ไม่มี tunnel)
- ✅ Best performance
- ✅ Recommended

**ข้อเสีย:**

- ❌ ต้องจัดการ 2 protocols
- ❌ ใช้ทรัพยากรมากกว่า

---

### 2. Tunneling

**คำจำกัดความ:**

- ห่อหุ้ม IPv6 packet ใน IPv4 packet (หรือกลับกัน)
- ส่งผ่าน network ที่ไม่รองรับ

**ประเภท:**

#### Manual Tunnel (IPv6 over IPv4)

**ลักษณะ:**

- กำหนด tunnel endpoints ด้วยตนเอง
- Point-to-point
- ใช้สำหรับ site-to-site

**Configuration:**

```
Router1(config)# interface tunnel 0
Router1(config-if)# ipv6 address 2001:db8:1:1::1/64
Router1(config-if)# tunnel source 192.168.1.1
Router1(config-if)# tunnel destination 192.168.2.1
Router1(config-if)# tunnel mode ipv6ip
Router1(config-if)# no shutdown
```

#### 6to4 Tunnel

**ลักษณะ:**

- อัตโนมัติมากกว่า Manual
- ใช้ IPv4 address ใน IPv6 address
- Prefix: `2002::/16`

**Format:**

```
2002:<IPv4 in hex>::/48

Example:
IPv4: 192.0.2.1 = C000:0201 (hex)
IPv6: 2002:c000:201::/48
```

#### ISATAP (Intra-Site Automatic Tunnel Addressing Protocol)

**ลักษณะ:**

- ภายใน site
- IPv6 over IPv4
- Link-local addresses

#### Teredo

**ลักษณะ:**

- IPv6 over IPv4 UDP
- ผ่าน NAT ได้
- Client behind NAT

---

### 3. Translation (NAT64)

**คำจำกัดความ:**

- แปลง IPv6 packet เป็น IPv4 (และกลับกัน)
- ไม่ใช่ tunnel (แปลงจริง)

**การทำงาน:**

```
IPv6 Client → NAT64 Gateway → IPv4 Server

IPv6 packet:
  Src: 2001:db8::1
  Dst: 64:ff9b::192.0.2.1 (embedded IPv4)

NAT64 แปลงเป็น IPv4 packet:
  Src: 203.0.113.5 (pool)
  Dst: 192.0.2.1

Server ตอบกลับ:
  Src: 192.0.2.1
  Dst: 203.0.113.5

NAT64 แปลงกลับเป็น IPv6:
  Src: 64:ff9b::192.0.2.1
  Dst: 2001:db8::1
```

**Well-Known Prefix:**

```
64:ff9b::/96 (for NAT64)

Example:
IPv4: 192.0.2.1
Embedded in IPv6: 64:ff9b::192.0.2.1
                  64:ff9b::c000:201
```

**DNS64:**

- ทำงานร่วมกับ NAT64
- Synthesize AAAA records จาก A records
- IPv6-only client query DNS64
- DNS64 return IPv6 address (embedded IPv4)

---

### Transition Summary

```
Mechanism    Description                     Use Case
------------------------------------------------------------------------
Dual Stack   Run IPv4 + IPv6 together        Best, Recommended
Tunneling    Encapsulate IPv6 in IPv4        IPv6 islands over IPv4
             (or vice versa)                 backbone
NAT64        Translate IPv6 ↔ IPv4          IPv6-only to IPv4-only
------------------------------------------------------------------------
```

**Current Best Practice:**

- **Dual Stack** - Run both
- Gradually migrate to IPv6-only
- Keep IPv4 for legacy

---

## Summary (สรุป)

Module 12 นี้เราได้เรียนรู้:

1. ✅ **IPv4 Issues** - Address exhaustion, Complex configuration
2. ✅ **IPv6 Overview** - 128 bits, Larger address space, Simplified header
3. ✅ **IPv6 Representation** - Hexadecimal, Shortening rules (:: and leading zeros)
4. ✅ **IPv6 Address Types**:
    - Unicast (GUA, Link-Local, ULA, Loopback)
    - Multicast
    - Anycast
5. ✅ **IPv6 Configuration**:
    - Static
    - SLAAC
    - EUI-64
    - DHCPv6
6. ✅ **Transition Mechanisms** - Dual Stack, Tunneling, NAT64

**สิ่งสำคัญที่ต้องจำ:**

- IPv6 = 128 bits = 8 hextets (16 bits each)
- :: ใช้ได้ครั้งเดียว ใน address เดียว
- ตัด leading zeros ได้ (0db8 → db8)
- GUA = 2000::/3 (Public, routable)
- Link-Local = fe80::/10 (Local link, required)
- ULA = fc00::/7 (Private, fd00::/8 ใช้จริง)
- Loopback = ::1/128
- All nodes multicast = ff02::1
- All routers multicast = ff02::2
- SLAAC = Stateless auto-configuration (ไม่ต้อง DHCP)
- EUI-64 = สร้าง Interface ID จาก MAC (flip bit 7)
- Dual Stack = แนะนำที่สุด (รันทั้ง IPv4 + IPv6)
- No broadcast ใน IPv6 (ใช้ multicast แทน)
- NDP แทน ARP
- DAD = Duplicate Address Detection (mandatory)

**Key Commands:**

```
ipv6 unicast-routing
ipv6 address <address>/<prefix>
ipv6 address <address> link-local
ipv6 address <prefix>::/64 eui-64
show ipv6 interface brief
show ipv6 route
show ipv6 neighbors
```

**Next Module:** Module 13 - ICMP

---

**[ไฟล์ Module 12 สมบูรณ์แล้ว!]**