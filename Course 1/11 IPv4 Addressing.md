# CCNA Course 1 - Module 11: IPv4 Addressing

## การกำหนดที่อยู่ IPv4

---

## 11.1 IPv4 Address Structure (โครงสร้างที่อยู่ IPv4)

### IPv4 Overview

**คำจำกัดความ:**

- Internet Protocol version 4
- ที่อยู่แบบ **Logical** (Layer 3)
- **32 bits** (4 bytes)
- เขียนในรูป **Dotted Decimal Notation**

**รูปแบบ:**

```
192.168.1.10

Binary:  11000000.10101000.00000001.00001010
Decimal: 192     .168     .1       .10
         ^        ^        ^        ^
       Octet 1  Octet 2  Octet 3  Octet 4
       (8 bits) (8 bits) (8 bits) (8 bits)
```

**คุณสมบัติ:**

- ✅ **Hierarchical** - แบ่งเป็น Network + Host portions
- ✅ **Unique** - ต้องไม่ซ้ำกันใน Internet
- ✅ **Routable** - Router สามารถ forward ได้

---

### Binary to Decimal Conversion

#### Binary Number System

**ฐาน 2:**

- ใช้เลข: **0** และ **1** เท่านั้น
- แต่ละหลักมีค่า: **2^position**

**Position Values (8 bits):**

```
Position:  7    6    5    4    3    2    1    0
Value:    128   64   32   16    8    4    2    1
          2^7  2^6  2^5  2^4  2^3  2^2  2^1  2^0
```

#### Binary to Decimal

**สูตร:**

```
ถ้า bit = 1 → นับค่า
ถ้า bit = 0 → ไม่นับ

Decimal = ผลรวมของค่าที่ bit = 1
```

**ตัวอย่าง 1:**

```
Binary: 11000000

Position:  128  64  32  16   8   4   2   1
Binary:     1   1   0   0   0   0   0   0
Count:    128  64   -   -   -   -   -   -

Decimal = 128 + 64 = 192
```

**ตัวอย่าง 2:**

```
Binary: 10101000

Position:  128  64  32  16   8   4   2   1
Binary:     1   0   1   0   1   0   0   0
Count:    128   -  32   -   8   -   -   -

Decimal = 128 + 32 + 8 = 168
```

**ตัวอย่าง 3:**

```
Binary: 11111111

Position:  128  64  32  16   8   4   2   1
Binary:     1   1   1   1   1   1   1   1
Count:    128  64  32  16   8   4   2   1

Decimal = 128 + 64 + 32 + 16 + 8 + 4 + 2 + 1 = 255
```

**ตัวอย่าง 4:**

```
Binary: 00000000

All bits = 0

Decimal = 0
```

#### Decimal to Binary

**วิธีที่ 1: ลบทีละค่า (แนะนำ)**

```
ตัวอย่าง: แปลง 192 เป็น Binary

Position:  128  64  32  16   8   4   2   1
           ---  --  --  --  --  --  --  --

Step 1: 192 ≥ 128? YES → bit = 1, เหลือ 192-128 = 64
        1

Step 2: 64 ≥ 64? YES → bit = 1, เหลือ 64-64 = 0
        1   1

Step 3: 0 ≥ 32? NO → bit = 0
        1   1   0

Step 4: 0 ≥ 16? NO → bit = 0
        1   1   0   0

Step 5-8: เหลือ 0 → ที่เหลือทั้งหมด = 0
        1   1   0   0   0   0   0   0

Binary = 11000000
```

**ตัวอย่าง 2: แปลง 172**

```
Position:  128  64  32  16   8   4   2   1
           ---  --  --  --  --  --  --  --

172 ≥ 128? YES → 1, เหลือ 44
 44 ≥  64? NO  → 0
 44 ≥  32? YES → 1, เหลือ 12
 12 ≥  16? NO  → 0
 12 ≥   8? YES → 1, เหลือ 4
  4 ≥   4? YES → 1, เหลือ 0
  0 ≥   2? NO  → 0
  0 ≥   1? NO  → 0

Binary = 10101100
```

**วิธีที่ 2: หารด้วย 2**

```
ตัวอย่าง: แปลง 192

192 ÷ 2 = 96  เหลือ 0  → bit ขวาสุด
 96 ÷ 2 = 48  เหลือ 0
 48 ÷ 2 = 24  เหลือ 0
 24 ÷ 2 = 12  เหลือ 0
 12 ÷ 2 = 6   เหลือ 0
  6 ÷ 2 = 3   เหลือ 0
  3 ÷ 2 = 1   เหลือ 1
  1 ÷ 2 = 0   เหลือ 1  → bit ซ้ายสุด

อ่านจากล่างขึ้นบน: 11000000
```

---

### Decimal Range per Octet

**ค่าที่เป็นไปได้:**

```
Minimum: 00000000 = 0
Maximum: 11111111 = 255

Range: 0 - 255 (256 ค่า)
```

**IPv4 Address Range:**

```
Minimum: 0.0.0.0
Maximum: 255.255.255.255

Total addresses: 2^32 = 4,294,967,296 addresses
                       ≈ 4.3 billion addresses
```

---

### Network and Host Portions

**โครงสร้าง IPv4:**

```
IPv4 Address = Network Portion + Host Portion

ตัวอย่าง:
192.168.  1  .  10
^^^^^^^^^    ^^^^
Network      Host
(24 bits)    (8 bits)
```

#### Network Portion

**คำจำกัดความ:**

- ระบุ**เครือข่าย**เฉพาะ
- **เหมือนกัน**สำหรับทุกอุปกรณ์ในเครือข่ายเดียวกัน
- ใช้สำหรับ **Routing**

**ตัวอย่าง:**

```
Network: 192.168.1.0/24

Devices in network:
192.168.1.1   → Network: 192.168.1
192.168.1.10  → Network: 192.168.1
192.168.1.100 → Network: 192.168.1
              (ส่วน Network เหมือนกัน)
```

#### Host Portion

**คำจำกัดความ:**

- ระบุ**อุปกรณ์**เฉพาะในเครือข่าย
- **ต่างกัน**สำหรับแต่ละอุปกรณ์
- ใช้ระบุ **Host**

**ตัวอย่าง:**

```
Network: 192.168.1.0/24

192.168.1. 1   → Host: 1
192.168.1. 10  → Host: 10
192.168.1. 100 → Host: 100
           ^^^
         (ส่วน Host ต่างกัน)
```

---

### Subnet Mask

**คำจำกัดความ:**

- บอกว่า**ส่วนไหน**เป็น Network, ส่วนไหนเป็น Host
- **32 bits** เหมือน IP address
- Bit = 1 → Network portion
- Bit = 0 → Host portion

**รูปแบบ:**

```
Dotted Decimal: 255.255.255.0
Binary:         11111111.11111111.11111111.00000000
                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
                1 = Network     0 = Host
                (24 bits)       (8 bits)
```

**Prefix Length (CIDR Notation):**

```
/24 = จำนวน bits ที่เป็น 1 (Network bits)

255.255.255.0   = /24
255.255.0.0     = /16
255.0.0.0       = /8
255.255.255.128 = /25
```

---

### Common Subnet Masks

```
Decimal             Binary                                  CIDR  Network  Host
                                                                  Bits     Bits
----------------------------------------------------------------------------------------
255.0.0.0           11111111.00000000.00000000.00000000     /8    8        24
255.255.0.0         11111111.11111111.00000000.00000000     /16   16       16
255.255.255.0       11111111.11111111.11111111.00000000     /24   24       8
255.255.255.128     11111111.11111111.11111111.10000000     /25   25       7
255.255.255.192     11111111.11111111.11111111.11000000     /26   26       6
255.255.255.224     11111111.11111111.11111111.11100000     /27   27       5
255.255.255.240     11111111.11111111.11111111.11110000     /28   28       4
255.255.255.248     11111111.11111111.11111111.11111000     /29   29       3
255.255.255.252     11111111.11111111.11111111.11111100     /30   30       2
255.255.255.254     11111111.11111111.11111111.11111110     /31   31       1
255.255.255.255     11111111.11111111.11111111.11111111     /32   32       0 (Host route)
```

---

### Determining Network Address

**วิธีคำนวณ:**

- **AND operation** ระหว่าง IP address และ Subnet mask

**AND Operation:**

```
1 AND 1 = 1
1 AND 0 = 0
0 AND 1 = 0
0 AND 0 = 0
```

**ตัวอย่าง:**

```
IP Address:    192.168.1.10
Subnet Mask:   255.255.255.0

Binary:
IP:            11000000.10101000.00000001.00001010
Subnet Mask:   11111111.11111111.11111111.00000000
               ------------------------------------  AND
Network:       11000000.10101000.00000001.00000000
               = 192.168.1.0

Network Address = 192.168.1.0/24
```

**ตัวอย่าง 2:**

```
IP Address:    172.16.20.65
Subnet Mask:   255.255.255.192 (/26)

Binary:
IP:            10101100.00010000.00010100.01000001
Subnet Mask:   11111111.11111111.11111111.11000000
               ------------------------------------  AND
Network:       10101100.00010000.00010100.01000000
               = 172.16.20.64

Network Address = 172.16.20.64/26
```

---

### Special Addresses

#### Network Address

**คำจำกัดความ:**

- **Host portion = all 0s**
- ระบุเครือข่าย
- **ไม่สามารถ assign ให้ host ได้**

**ตัวอย่าง:**

```
Network: 192.168.1.0/24

Network Address = 192.168.1.0
                            ^
                    (Host portion = 0)
```

#### Broadcast Address

**คำจำกัดความ:**

- **Host portion = all 1s**
- ส่งไปยังทุกอุปกรณ์ในเครือข่าย
- **ไม่สามารถ assign ให้ host ได้**

**ตัวอย่าง:**

```
Network: 192.168.1.0/24

Broadcast Address = 192.168.1.255
                              ^^^
                      (Host portion = 255 = all 1s in binary)
```

#### First Usable Address

**คำจำกัดความ:**

- Network Address **+ 1**
- IP แรกที่สามารถ assign ได้

**ตัวอย่าง:**

```
Network: 192.168.1.0/24

First Usable = 192.168.1.1
```

#### Last Usable Address

**คำจำกัดความ:**

- Broadcast Address **- 1**
- IP สุดท้ายที่สามารถ assign ได้

**ตัวอย่าง:**

```
Network: 192.168.1.0/24

Last Usable = 192.168.1.254
```

---

### Number of Hosts

**สูตร:**

```
Number of hosts = 2^(Host bits) - 2

-2 เพราะ:
  - 1 = Network address (ไม่ assign ได้)
  - 1 = Broadcast address (ไม่ assign ได้)
```

**ตัวอย่าง:**

```
Network: 192.168.1.0/24
Host bits = 32 - 24 = 8 bits

Number of hosts = 2^8 - 2
                = 256 - 2
                = 254 hosts

Usable IPs: 192.168.1.1 - 192.168.1.254
```

**ตัวอย่าง 2:**

```
Network: 10.0.0.0/16
Host bits = 32 - 16 = 16 bits

Number of hosts = 2^16 - 2
                = 65,536 - 2
                = 65,534 hosts
```

**ตัวอย่าง 3:**

```
Network: 192.168.1.0/30
Host bits = 32 - 30 = 2 bits

Number of hosts = 2^2 - 2
                = 4 - 2
                = 2 hosts

Usable: 192.168.1.1, 192.168.1.2
(ใช้สำหรับ Point-to-Point links)
```

---

### Network Address Example

**ตัวอย่างที่ 1:**

```
IP: 192.168.10.65/24

Network address:    192.168.10.0
First usable:       192.168.10.1
Last usable:        192.168.10.254
Broadcast:          192.168.10.255
Number of hosts:    254
```

**ตัวอย่างที่ 2:**

```
IP: 172.16.50.139/26

Step 1: ค้นหา Subnet Mask
  /26 = 255.255.255.192

Step 2: คำนวณ Network Address (AND)
  IP:    172.16.50.139 = 10101100.00010000.00110010.10001011
  Mask:  255.255.255.192 = 11111111.11111111.11111111.11000000
         --------------------------------------------------------
  Net:   172.16.50.128 = 10101100.00010000.00110010.10000000

Network address:    172.16.50.128
First usable:       172.16.50.129
Last usable:        172.16.50.190
Broadcast:          172.16.50.191
Number of hosts:    2^6 - 2 = 62 hosts
```

**ตัวอย่างที่ 3:**

```
IP: 10.1.100.25/30

Network address:    10.1.100.24
First usable:       10.1.100.25
Last usable:        10.1.100.26
Broadcast:          10.1.100.27
Number of hosts:    2 hosts (Point-to-Point)
```

---

## 11.2 IPv4 Address Types (ประเภทของที่อยู่ IPv4)

### Unicast Address

**คำจำกัดความ:**

- ส่งไปยัง**อุปกรณ์เดียว**
- One-to-one communication
- ใช้มากที่สุด

**ลักษณะ:**

- IP address ปกติ
- Destination = specific host

**ตัวอย่าง:**

```
Source:      192.168.1.10
Destination: 192.168.1.20

→ ส่งไปยัง 192.168.1.20 เท่านั้น
```

---

### Broadcast Address

**คำจำกัดความ:**

- ส่งไปยัง**ทุกอุปกรณ์**ในเครือข่าย
- One-to-all communication

**ประเภท:**

#### 1. Limited Broadcast

**Address:** `255.255.255.255`

**ลักษณะ:**

- ส่งไปยังทุกอุปกรณ์ใน **local network**
- Router **ไม่ forward** (blocked by router)

**การใช้งาน:**

- DHCP Discovery
- ไม่รู้ Gateway หรือ network information

**ตัวอย่าง:**

```
DHCP Client broadcast:
  Source: 0.0.0.0
  Destination: 255.255.255.255

→ ทุกคนใน local network ได้รับ
→ Router ไม่ forward ออกนอก network
```

#### 2. Directed Broadcast

**Address:** **Network address + all 1s in host portion**

**ตัวอย่าง:**

```
Network: 192.168.1.0/24
Directed Broadcast: 192.168.1.255
                              ^^^
                        Host portion = all 1s
```

**ลักษณะ:**

- ส่งไปยังทุกอุปกรณ์ใน**เครือข่ายเฉพาะ**
- Router สามารถ forward ได้ (แต่ปกติปิด)

**การใช้งาน:**

- Wake-on-LAN
- Network scanning (แต่ไม่แนะนำ)

**Security:**

- **Default: Disabled** บน Router (ป้องกัน DDoS)
- Smurf attack (ใช้ directed broadcast โจมตี)

---

### Multicast Address

**คำจำกัดความ:**

- ส่งไปยัง**กลุ่มอุปกรณ์** (group)
- One-to-many communication
- อุปกรณ์ต้อง **subscribe** ไปยัง multicast group

**Range:** `224.0.0.0` ถึง `239.255.255.255`

**Class D addresses**

**Multicast Groups:**

```
224.0.0.0 - 224.0.0.255    Reserved (Local network control)
224.0.1.0 - 238.255.255.255 Globally scoped (Internetwork control)
239.0.0.0 - 239.255.255.255 Limited scope (Organization-local)
```

**Well-Known Multicast Addresses:**

```
Address          Purpose
------------------------------------------------------------------------
224.0.0.1        All hosts on this subnet
224.0.0.2        All routers on this subnet
224.0.0.5        OSPF routers (OSPFv2)
224.0.0.6        OSPF designated routers
224.0.0.9        RIPv2 routers
224.0.0.10       EIGRP routers
224.0.0.13       PIM routers
224.0.0.18       VRRP routers
224.0.0.102      HSRP routers (v2)
239.255.255.250  SSDP (UPnP)
------------------------------------------------------------------------
```

**การใช้งาน:**

- **Routing protocols** (OSPF, EIGRP, RIP)
- **Video streaming** (IPTV)
- **Online gaming**
- **Stock market data**
- **Video conferencing**

**ตัวอย่าง:**

```
OSPF routers ส่ง Hello messages:
  Destination: 224.0.0.5 (All OSPF routers)

→ เฉพาะ Router ที่รัน OSPF จะรับและ process
→ อุปกรณ์อื่นๆ ignore
```

---

## 11.3 IPv4 Address Classes (คลาสของที่อยู่ IPv4)

### Classful Addressing (ล้าสมัย)

**คำจำกัดความ:**

- การแบ่ง IP addresses เป็น **5 classes** (A, B, C, D, E)
- แบ่งตาม **octet แรก**
- ใช้ก่อน CIDR (Classless Inter-Domain Routing)
- **ปัจจุบันไม่ใช้แล้ว** แต่ยังเรียนเพื่อความเข้าใจ

---

### Class A

**Range:** `0.0.0.0` - `127.255.255.255`

**First Octet:** 0-127 (Binary เริ่มด้วย `0`)

**Default Subnet Mask:** `255.0.0.0` (/8)

**Network/Host bits:**

```
N.H.H.H
^
Network (8 bits)
  ^^^^^^
  Host (24 bits)
```

**Number of:**

- **Networks:** 2^7 = 128 networks (0-127)
- **Hosts per network:** 2^24 - 2 = 16,777,214 hosts

**ใช้สำหรับ:**

- องค์กรขนาด**ใหญ่มาก**
- ISPs

**ตัวอย่าง:**

```
10.0.0.0/8
  Network: 10.0.0.0
  Hosts: 10.0.0.1 - 10.255.255.254
  Broadcast: 10.255.255.255
```

**หมายเหตุ:**

- `0.0.0.0` - Reserved
- `127.0.0.0/8` - Loopback (127.0.0.1)

---

### Class B

**Range:** `128.0.0.0` - `191.255.255.255`

**First Octet:** 128-191 (Binary เริ่มด้วย `10`)

**Default Subnet Mask:** `255.255.0.0` (/16)

**Network/Host bits:**

```
N.N.H.H
^^^^
Network (16 bits)
    ^^^^
    Host (16 bits)
```

**Number of:**

- **Networks:** 2^14 = 16,384 networks
- **Hosts per network:** 2^16 - 2 = 65,534 hosts

**ใช้สำหรับ:**

- องค์กรขนาด**กลาง-ใหญ่**

**ตัวอย่าง:**

```
172.16.0.0/16
  Network: 172.16.0.0
  Hosts: 172.16.0.1 - 172.16.255.254
  Broadcast: 172.16.255.255
```

---

### Class C

**Range:** `192.0.0.0` - `223.255.255.255`

**First Octet:** 192-223 (Binary เริ่มด้วย `110`)

**Default Subnet Mask:** `255.255.255.0` (/24)

**Network/Host bits:**

```
N.N.N.H
^^^^^^
Network (24 bits)
      ^
      Host (8 bits)
```

**Number of:**

- **Networks:** 2^21 = 2,097,152 networks
- **Hosts per network:** 2^8 - 2 = 254 hosts

**ใช้สำหรับ:**

- องค์กรขนาด**เล็ก**
- Home networks

**ตัวอย่าง:**

```
192.168.1.0/24
  Network: 192.168.1.0
  Hosts: 192.168.1.1 - 192.168.1.254
  Broadcast: 192.168.1.255
```

---

### Class D (Multicast)

**Range:** `224.0.0.0` - `239.255.255.255`

**First Octet:** 224-239 (Binary เริ่มด้วย `1110`)

**ลักษณะ:**

- ไม่มี subnet mask
- ไม่มี network/host division
- ใช้สำหรับ **Multicast** เท่านั้น

**การใช้งาน:**

- Routing protocols
- Streaming media
- Group communication

---

### Class E (Experimental)

**Range:** `240.0.0.0` - `255.255.255.255`

**First Octet:** 240-255 (Binary เริ่มด้วย `1111`)

**ลักษณะ:**

- **Reserved** สำหรับ experimental/research
- **ไม่สามารถใช้** ใน production

---

### Class Summary

```
Class  Range                      First    Default  Network  Host   Hosts
                                  Octet    Mask     Bits     Bits   per Network
----------------------------------------------------------------------------------------
A      0.0.0.0 - 127.255.255.255   0-127   /8       8        24     16,777,214
B      128.0.0.0 - 191.255.255.255 128-191 /16      16       16     65,534
C      192.0.0.0 - 223.255.255.255 192-223 /24      24       8      254
D      224.0.0.0 - 239.255.255.255 224-239 N/A      N/A      N/A    Multicast
E      240.0.0.0 - 255.255.255.255 240-255 N/A      N/A      N/A    Experimental
----------------------------------------------------------------------------------------
```

**Binary Patterns:**

```
Class   First Bits   Example
-----------------------------------------
A       0xxxxxxx     01111111 = 127
B       10xxxxxx     10111111 = 191
C       110xxxxx     11011111 = 223
D       1110xxxx     11101111 = 239
E       1111xxxx     11111111 = 255
```

---

## 11.4 Private and Public IPv4 Addresses

### Public IPv4 Addresses

**คำจำกัดความ:**

- IP addresses ที่**สามารถใช้ได้ใน Internet**
- **Unique globally** - ไม่ซ้ำกันทั่วโลก
- **Routable** on the Internet
- มอบหมายโดย **IANA/RIRs**

**RIRs (Regional Internet Registries):**

```
ARIN   - American Registry for Internet Numbers (North America)
RIPE   - Réseaux IP Européens (Europe, Middle East, Central Asia)
APNIC  - Asia-Pacific Network Information Centre (Asia Pacific)
LACNIC - Latin America and Caribbean Network Information Centre
AFRINIC - African Network Information Centre
```

**ลักษณะ:**

- ต้อง**จ่ายเงิน** (lease from ISP)
- **จำกัด** (IPv4 หมดแล้ว)
- ราคาแพง

---

### Private IPv4 Addresses (RFC 1918)

**คำจำกัดความ:**

- IP addresses ที่**ไม่สามารถใช้ใน Internet**
- **Non-routable** on the Internet
- ใช้ภายใน**เครือข่ายส่วนตัว** (LAN)
- **ฟรี** - ใช้ได้โดยไม่ต้องขอจาก IANA

**Private Ranges:**

```
Class   Range                              CIDR           Hosts
------------------------------------------------------------------------
A       10.0.0.0 - 10.255.255.255          10.0.0.0/8     16,777,216
B       172.16.0.0 - 172.31.255.255        172.16.0.0/12  1,048,576
C       192.168.0.0 - 192.168.255.255      192.168.0.0/16 65,536
------------------------------------------------------------------------
```

**การใช้งาน:**

- **Home networks** - 192.168.x.x
- **Small businesses** - 192.168.x.x, 10.x.x.x
- **Large organizations** - 10.x.x.x, 172.16.x.x

**ตัวอย่าง:**

```
Home Router:
  WAN (Public): 203.0.113.5        (จาก ISP)
  LAN (Private): 192.168.1.1       (Private)
  
PCs in Home:
  PC1: 192.168.1.10                (Private)
  PC2: 192.168.1.20                (Private)

→ ภายในบ้านใช้ Private IPs
→ ออก Internet ใช้ Public IP (ผ่าน NAT)
```

---

### NAT (Network Address Translation)

**คำจำกัดความ:**

- แปลง **Private IP** เป็น **Public IP**
- ทำให้อุปกรณ์ที่ใช้ Private IP เข้าถึง Internet ได้

**การทำงาน:**

```
Inside (Private)          NAT Router          Outside (Public)
                          
PC1: 192.168.1.10 ───────→ Translate ───────→ 203.0.113.5
                           Src: 192.168.1.10
                           →
                           Src: 203.0.113.5
```

**ประโยชน์ของ NAT:**

- ✅ ประหยัด Public IPs (หลาย Private → Public เดียว)
- ✅ Security (ซ่อน internal structure)
- ✅ Flexibility (เปลี่ยน ISP ไม่ต้องเปลี่ยน internal IPs)

---

### Special-Use IPv4 Addresses

**นอกจาก Private addresses ยังมี Special addresses:**

#### 1. Loopback Address

**Range:** `127.0.0.0/8`

**ใช้บ่อย:** `127.0.0.1`

**ลักษณะ:**

- ส่งกลับไปยัง**ตัวเอง**
- ใช้ทดสอบ TCP/IP stack

**การใช้งาน:**

```
ping 127.0.0.1
→ ทดสอบว่า NIC ทำงานหรือไม่
```

#### 2. Link-Local Addresses (APIPA)

**Range:** `169.254.0.0/16`

**คำจำกัดความ:**

- **Automatic Private IP Addressing**
- ใช้เมื่อไม่ได้รับ IP จาก DHCP

**การทำงาน:**

```
1. PC boot up
2. ส่ง DHCP Discovery
3. ไม่ได้รับ DHCP Offer (ไม่มี DHCP server)
4. PC assign IP จาก 169.254.0.0/16 ให้ตัวเอง
5. ตรวจสอบ duplicate (ARP)
6. ใช้ IP นี้ (local network เท่านั้น)
```

**ตัวอย่าง:**

```
ipconfig

Ethernet adapter:
  IP Address: 169.254.10.50
  Subnet Mask: 255.255.0.0

→ บอกว่า: ไม่ได้ IP จาก DHCP
```

**การแก้ปัญหา:**

- ตรวจสอบ DHCP server
- ตรวจสอบสาย network
- Renew IP: `ipconfig /renew`

#### 3. TEST-NET Addresses (Documentation)

**Ranges:**

```
192.0.2.0/24       TEST-NET-1
198.51.100.0/24    TEST-NET-2
203.0.113.0/24     TEST-NET-3
```

**ลักษณะ:**

- ใช้สำหรับ**ตัวอย่างใน documentation**
- **Non-routable** (ไม่ใช้จริงใน Internet)

**การใช้งาน:**

- ตัวอย่างใน textbooks
- Training materials
- RFCs

---

### Legacy Classful Boundaries

**ปัญหาของ Classful:**

- ✅ Class A: เยอะเกินไป (16M hosts) - เปลือง IPs
- ✅ Class C: น้อยเกินไป (254 hosts) - ไม่พอ
- ✅ ไม่ flexible

**ตัวอย่างปัญหา:**

```
องค์กรต้องการ 500 hosts:

Class C (/24): 254 hosts → ไม่พอ! ต้องใช้ 2 Class C
Class B (/16): 65,534 hosts → เกินมาก! เปลือง 65,034 IPs
```

**Solution: CIDR (Classless)**

- ไม่ติดกับ class boundaries
- ใช้ subnet mask ได้**อิสระ**
- ตัวอย่าง: `/23` (510 hosts) - พอดีสำหรับ 500 hosts!

---

## 11.5 Subnetting (การแบ่งเครือข่ายย่อย)

### Why Subnetting?

**เหตุผล:**

1. ✅ **ประหยัด IP addresses** - ใช้ IP อย่างมีประสิทธิภาพ
2. ✅ **ลด broadcast traffic** - แบ่ง broadcast domains
3. ✅ **เพิ่มความปลอดภัย** - แยก departments
4. ✅ **ง่ายต่อการจัดการ** - กลุ่มตาม location/function
5. ✅ **Performance** - ลด network congestion

**ตัวอย่างปัญหา:**

```
องค์กรได้ IP: 192.168.1.0/24 (254 hosts)

มี 4 departments:
- Sales: 50 hosts
- Engineering: 100 hosts
- Marketing: 30 hosts
- Management: 20 hosts

ถ้าไม่ subnet:
  → ทุกคนอยู่ broadcast domain เดียว (254 devices)
  → Broadcast มาก, ช้า
  → ไม่มี security separation

ถ้า subnet:
  → แบ่งเป็น 4 subnets
  → แต่ละ department = subnet แยก
  → Broadcast น้อยลง, เร็วขึ้น
  → แยกได้ตาม ACLs
```

---

### Subnetting Process

**ขั้นตอน:**

#### Step 1: Determine Requirements

**คำถาม:**

- ต้องการกี่ subnets?
- แต่ละ subnet ต้องการกี่ hosts?

#### Step 2: Calculate Subnet Bits

**สูตร:**

```
Number of subnets = 2^n
n = จำนวน bits ที่ยืมมาจาก host portion

ตัวอย่าง:
ต้องการ 4 subnets → 2^2 = 4 → ต้องยืม 2 bits
ต้องการ 8 subnets → 2^3 = 8 → ต้องยืม 3 bits
```

#### Step 3: Calculate New Subnet Mask

**Original:** 192.168.1.0/24

```
255.255.255.00000000
                ^^^^
               Host bits

ยืม 2 bits:
255.255.255.11000000
            ^^^^^^^^
            Network (รวมยืม)

New mask: 255.255.255.192 (/26)
```

#### Step 4: Calculate Subnet Information

**สำหรับแต่ละ subnet:**

- Network address
- First usable
- Last usable
- Broadcast address
- Number of hosts

---

### Subnetting Example 1

**Given:** `192.168.1.0/24`

**Requirement:** แบ่งเป็น **4 subnets**

**Solution:**

**Step 1:** จำนวน bits ที่ต้องยืม

```
4 subnets = 2^2
→ ยืม 2 bits
```

**Step 2:** New Subnet Mask

```
Original: /24 (255.255.255.0)
+ 2 bits → /26 (255.255.255.192)

Binary:
255.255.255.11000000
            ^^^^^^^^
            26 bits = 1
```

**Step 3:** Subnet increment

```
256 - 192 = 64
Subnets เพิ่มทีละ 64
```

**Step 4:** Subnet Table

```
Subnet  Network       First       Last          Broadcast     Hosts
-------------------------------------------------------------------------
1       192.168.1.0   192.168.1.1   192.168.1.62  192.168.1.63   62
2       192.168.1.64  192.168.1.65  192.168.1.126 192.168.1.127  62
3       192.168.1.128 192.168.1.129 192.168.1.190 192.168.1.191  62
4       192.168.1.192 192.168.1.193 192.168.1.254 192.168.1.255  62
```

**Verification:**

```
Hosts per subnet = 2^(Host bits) - 2
                 = 2^6 - 2
                 = 64 - 2
                 = 62 hosts ✅
```

---

### Subnetting Example 2

**Given:** `172.16.0.0/16`

**Requirement:** แบ่งเป็น **8 subnets**

**Solution:**

**Step 1:** Bits ที่ต้องยืม

```
8 subnets = 2^3
→ ยืม 3 bits
```

**Step 2:** New Subnet Mask

```
Original: /16 (255.255.0.0)
+ 3 bits → /19 (255.255.224.0)

Binary:
255.255.11100000.00000000
        ^^^^^^^^
        19 bits = 1
```

**Step 3:** Increment (Octet 3)

```
256 - 224 = 32
Subnets เพิ่มทีละ 32 (ใน octet 3)
```

**Step 4:** Subnet Table

```
Subnet  Network         First          Last            Broadcast       Hosts
------------------------------------------------------------------------------------
1       172.16.0.0/19   172.16.0.1     172.16.31.254   172.16.31.255   8190
2       172.16.32.0/19  172.16.32.1    172.16.63.254   172.16.63.255   8190
3       172.16.64.0/19  172.16.64.1    172.16.95.254   172.16.95.255   8190
4       172.16.96.0/19  172.16.96.1    172.16.127.254  172.16.127.255  8190
5       172.16.128.0/19 172.16.128.1   172.16.159.254  172.16.159.255  8190
6       172.16.160.0/19 172.16.160.1   172.16.191.254  172.16.191.255  8190
7       172.16.192.0/19 172.16.192.1   172.16.223.254  172.16.223.255  8190
8       172.16.224.0/19 172.16.224.1   172.16.255.254  172.16.255.255  8190
```

**Verification:**

```
Hosts per subnet = 2^13 - 2 = 8190 hosts ✅
```

---

### VLSM (Variable Length Subnet Mask)

**คำจำกัดความ:**

- ใช้ **Subnet Masks ต่างกัน** ในเครือข่ายเดียวกัน
- **Efficient** - ใช้ IP addresses อย่างมีประสิทธิภาพ

**ตัวอย่างปัญหา:**

```
Network: 192.168.1.0/24

Requirements:
- Department A: 100 hosts
- Department B: 50 hosts
- Department C: 25 hosts
- Router-to-Router links (3 links): 2 hosts each
```

**Fixed-Length Subnetting:**

```
แบ่งเป็น 8 subnets (/27)
→ แต่ละ subnet: 30 hosts

Department A (100 hosts): ไม่พอ! ❌
```

**VLSM Solution:**

```
1. Department A (100 hosts):
   192.168.1.0/25 (126 hosts) ✅

2. Department B (50 hosts):
   192.168.1.128/26 (62 hosts) ✅

3. Department C (25 hosts):
   192.168.1.192/27 (30 hosts) ✅

4. Router links (2 hosts each):
   192.168.1.224/30 (2 hosts) ✅
   192.168.1.228/30 (2 hosts) ✅
   192.168.1.232/30 (2 hosts) ✅
```

**ข้อดีของ VLSM:**

- ✅ ประหยัด IP addresses
- ✅ Flexible
- ✅ Efficient

**ข้อกำหนด:**

- ต้องใช้ **Classless routing protocols** (OSPF, EIGRP, BGP)
- ไม่รองรับ Classful protocols (RIPv1, IGRP)

---

### Subnet Mask Cheat Sheet

```
CIDR  Subnet Mask       Block Size  Hosts    Networks
------------------------------------------------------------------------
/30   255.255.255.252   4           2        64 (Point-to-Point)
/29   255.255.255.248   8           6        32
/28   255.255.255.240   16          14       16
/27   255.255.255.224   32          30       8
/26   255.255.255.192   64          62       4
/25   255.255.255.128   128         126      2
/24   255.255.255.0     256         254      1 (Class C)
/23   255.255.254.0     512         510      
/22   255.255.252.0     1024        1022     
/21   255.255.248.0     2048        2046     
/20   255.255.240.0     4096        4094     
/19   255.255.224.0     8192        8190     
/18   255.255.192.0     16384       16382    
/17   255.255.128.0     32768       32766    
/16   255.255.0.0       65536       65534    (Class B)
```

---

## Summary (สรุป)

Module 11 นี้เราได้เรียนรู้:

1. ✅ **IPv4 Structure** - 32 bits, Dotted decimal, Network + Host portions
2. ✅ **Binary Conversion** - Binary ↔ Decimal
3. ✅ **Subnet Mask** - กำหนด Network/Host boundary, CIDR notation
4. ✅ **Address Types** - Unicast, Broadcast, Multicast
5. ✅ **Address Classes** - A, B, C, D, E (Classful - obsolete)
6. ✅ **Public vs Private** - RFC 1918, NAT
7. ✅ **Special Addresses** - Loopback, Link-Local, TEST-NET
8. ✅ **Subnetting** - แบ่งเครือข่ายย่อย, ประหยัด IPs
9. ✅ **VLSM** - Variable Length Subnet Mask, Flexible subnetting

**สิ่งสำคัญที่ต้องจำ:**

- IPv4 = 32 bits = 4 octets (0-255 each)
- Subnet Mask = บอก Network/Host boundary
- Network Address = Host portion all 0s (ไม่ assign ได้)
- Broadcast Address = Host portion all 1s (ไม่ assign ได้)
- Usable hosts = 2^(Host bits) - 2
- Private IPs = 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16
- Loopback = 127.0.0.1
- APIPA = 169.254.0.0/16 (DHCP failed)
- Subnetting = ยืม bits จาก host portion
- VLSM = ใช้ subnet masks ต่างกันในเครือข่ายเดียวกัน

**Key Formulas:**

```
Subnets = 2^(Borrowed bits)
Hosts per subnet = 2^(Host bits) - 2
Block size = 256 - Subnet mask value
```

**Next Module:** Module 12 - IPv6 Addressing

---

**[ไฟล์ Module 11 สมบูรณ์แล้ว!]**