# CCNA Course 1 - Module 10: Basic Router Configuration

## การตั้งค่าเราเตอร์เบื้องต้น

---

## 10.1 Configure Initial Router Settings (ตั้งค่าเราเตอร์เบื้องต้น)

### Router vs Switch

**ความแตกต่างหลัก:**

```
Feature              Router                    Switch
------------------------------------------------------------------------
Layer                Layer 3 (Network)         Layer 2 (Data Link)
Primary Function     Routing (between networks) Switching (within network)
Forwards based on    IP address                MAC address
Broadcasts           Blocks broadcasts         Forwards broadcasts (same VLAN)
Interfaces           Fewer (2-100+)            Many (24-48+)
Default state        Down (shutdown)           Up (no shutdown)
IP address           Required on interfaces    Optional (management only)
Routing table        Yes                       No (except L3 switches)
MAC table            No                        Yes
------------------------------------------------------------------------
```

**ฟังก์ชันของ Router:**

- ✅ **Routing** - เลือกเส้นทางที่ดีที่สุด
- ✅ **Interconnect networks** - เชื่อมต่อเครือข่ายต่างๆ
- ✅ **Broadcast domain separation** - แบ่ง broadcast domains
- ✅ **WAN access** - เชื่อมต่อ Internet/WAN
- ✅ **Security** - ACLs, Firewall features
- ✅ **NAT/PAT** - แปลง IP addresses
- ✅ **QoS** - จัดการ traffic priority

---

### Initial Router Setup

#### Console Connection

**อุปกรณ์ที่ต้องการ:**

- **Console cable** (RJ-45 to DB-9 หรือ USB)
- **Terminal emulation software**:
    - PuTTY (Windows)
    - Tera Term (Windows)
    - SecureCRT
    - screen (Linux/macOS)

**Console Settings:**

```
Baud rate: 9600
Data bits: 8
Parity: None
Stop bits: 1
Flow control: None
```

**เชื่อมต่อ:**

```
1. เสียบ Console cable:
   Router Console port → PC COM/USB port

2. เปิด Terminal software:
   Windows (PuTTY):
     - Connection type: Serial
     - Serial line: COM3 (ตรวจสอบใน Device Manager)
     - Speed: 9600

   Linux/macOS:
     screen /dev/ttyUSB0 9600

3. เปิด Router → จะเห็น boot messages
```

---

### Router Boot Sequence

**ลำดับการ boot (เหมือน Switch):**

#### 1. POST (Power-On Self-Test)

```
Testing hardware:
  - CPU
  - Memory (RAM)
  - Flash
  - Interfaces
  - NVRAM

LED:
  - During POST: Amber/Yellow
  - POST OK: Green
```

#### 2. Load Bootstrap

```
- โหลด Bootstrap program จาก ROM
- Initialize flash file system
- Locate Cisco IOS
```

#### 3. Load Cisco IOS

```
- โหลด IOS จาก flash memory
- Decompress และโหลดเข้า RAM
- Run IOS
```

#### 4. Load Configuration

```
- ค้นหา startup-config ใน NVRAM
- ถ้ามี: Load เป็น running-config
- ถ้าไม่มี: เข้า Setup Mode
```

---

### Setup Mode

**เมื่อไหร่เข้า Setup Mode:**

- ไม่มี startup-config (Router ใหม่)
- หรือ config-register = 0x2142 (bypass config)

**ข้อความ:**

```
--- System Configuration Dialog ---

Would you like to enter the initial configuration dialog? [yes/no]: no

% Please answer 'yes' or 'no'.
Would you like to enter the initial configuration dialog? [yes/no]: no


Press RETURN to get started!
```

**คำแนะนำ:**

- พิมพ์ **no**
- ตั้งค่าด้วย CLI (ยืดหยุ่นกว่า)

---

### Router CLI Modes

**เหมือนกับ Switch:**

```
Router>                                    User EXEC Mode
  enable ↓
Router#                                    Privileged EXEC Mode
  configure terminal ↓
Router(config)#                            Global Configuration Mode
  interface gigabitethernet 0/0 ↓
Router(config-if)#                         Interface Configuration Mode
  line console 0 ↓
Router(config-line)#                       Line Configuration Mode
  router ospf 1 ↓
Router(config-router)#                     Router Configuration Mode
```

---

### Basic Router Configuration Steps

#### Step 1: Enter Privileged EXEC Mode

```
Router> enable
Router#
```

#### Step 2: Enter Global Configuration Mode

```
Router# configure terminal
Router(config)#
```

#### Step 3: Hostname

```
Router(config)# hostname R1
R1(config)#
```

**ชื่อที่ดี:**

- สั้น กะทัดรัด
- บ่งบอกตำแหน่ง/หน้าที่
- ไม่มีช่องว่าง
- เริ่มด้วยตัวอักษร

**ตัวอย่าง:**

```
R1-HQ-Core
R2-Branch-Edge
Router-Floor3
```

#### Step 4: Disable DNS Lookup

```
R1(config)# no ip domain-lookup
```

**เหตุผล:**

- ป้องกัน Router พยายาม resolve typos เป็น hostnames
- ลดเวลารอเมื่อพิมพ์ผิด

**ตัวอย่าง:**

```
Without "no ip domain-lookup":
R1# shwo ip route
Translating "shwo"...domain server (255.255.255.255)
% Unknown command or computer name, or unable to find computer address
[รอ 30+ seconds]

With "no ip domain-lookup":
R1# shwo ip route
% Invalid input detected at '^' marker.
[ตอบทันที]
```

#### Step 5: Secure Privileged EXEC Mode

**Enable Secret:**

```
R1(config)# enable secret class
```

**เปรียบเทียบ enable password vs enable secret:**

```
Command                     Encryption         Recommended
------------------------------------------------------------------------
enable password cisco       Type 7 (weak)      ❌ No
enable secret cisco         MD5 (strong)       ✅ Yes
------------------------------------------------------------------------

หมายเหตุ:
- ถ้ามีทั้งสอง → ใช้ enable secret
- แนะนำใช้ enable secret เท่านั้น
```

#### Step 6: Secure User EXEC Mode

**Console Password:**

```
R1(config)# line console 0
R1(config-line)# password cisco
R1(config-line)# login
R1(config-line)# logging synchronous
R1(config-line)# exec-timeout 5 0
R1(config-line)# exit
```

**คำอธิบาย:**

- `password cisco` - ตั้งรหัสผ่าน
- `login` - เปิดใช้งานการตรวจสอบรหัสผ่าน
- `logging synchronous` - ข้อความ log ไม่ขัดจังหวะการพิมพ์
- `exec-timeout 5 0` - Timeout 5 นาที 0 วินาที (default: 10 minutes)
    - `exec-timeout 0 0` - ไม่มี timeout (ไม่แนะนำใน production)

**VTY Lines (Telnet/SSH):**

```
R1(config)# line vty 0 4
R1(config-line)# password cisco
R1(config-line)# login
R1(config-line)# transport input ssh telnet
R1(config-line)# exec-timeout 5 0
R1(config-line)# exit
```

**หมายเหตุ:**

- `line vty 0 4` - รองรับ 5 connections พร้อมกัน (0-4)
- Router บางรุ่น: `line vty 0 15` (16 connections)

#### Step 7: Encrypt Passwords

```
R1(config)# service password-encryption
```

**ก่อน encryption:**

```
R1# show running-config
!
line console 0
 password cisco         ← Plaintext
 login
!
line vty 0 4
 password cisco         ← Plaintext
 login
```

**หลัง encryption:**

```
R1# show running-config
!
line console 0
 password 7 0822455D0A16     ← Encrypted (Type 7)
 login
!
line vty 0 4
 password 7 0822455D0A16     ← Encrypted
 login
!
enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0  ← MD5 (Type 5)
```

**Password Types:**

```
Type    Encryption       Security       Used for
------------------------------------------------------------------------
0       Plaintext        ❌ None        Clear text (debugging)
5       MD5              ✅ Strong      enable secret
7       Cisco Type 7     ⚠️ Weak        line passwords (service password-encryption)
8       PBKDF2 (SHA-256) ✅ Strong      enable secret (newer IOS)
9       Scrypt           ✅ Very Strong enable secret (newest IOS)
------------------------------------------------------------------------
```

**หมายเหตุ:**

- Type 7 **ง่ายต่อการ crack** (tools online มีเยอะ)
- ใช้เพื่อป้องกันการ "แอบมอง" เท่านั้น
- Type 5/8/9 ปลอดภัยกว่ามาก

#### Step 8: Legal Notification Banner

**MOTD (Message of the Day) Banner:**

```
R1(config)# banner motd #
Enter TEXT message. End with the character '#'.
***********************************************
*                                             *
*  UNAUTHORIZED ACCESS IS PROHIBITED         *
*  Violators will be prosecuted              *
*  All activities are monitored and logged   *
*                                             *
***********************************************
#
```

**Delimiter:**

- ใช้อักขระที่**ไม่ปรากฏใน message** เป็น delimiter
- ตัวอย่าง: `#`, `$`, `%`, `@`

**Login Banner (ไม่บ่อย):**

```
R1(config)# banner login #
Please enter your credentials
#
```

**EXEC Banner (ไม่บ่อย):**

```
R1(config)# banner exec #
Welcome to R1!
#
```

**ลำดับการแสดง:**

```
1. MOTD banner (ก่อน login prompt)
2. Login banner (พร้อม login prompt)
3. EXEC banner (หลัง login สำเร็จ)
```

#### Step 9: Save Configuration

**บันทึกจาก running-config ไป startup-config:**

```
R1(config)# exit
R1# copy running-config startup-config
Destination filename [startup-config]? [Enter]
Building configuration...
[OK]
```

**Short version:**

```
R1# write memory
หรือ
R1# wr
```

**Verify:**

```
R1# show startup-config
R1# show running-config
```

---

### Complete Basic Configuration Example

```
Router> enable
Router# configure terminal

! Hostname
Router(config)# hostname R1

! Disable DNS lookup
R1(config)# no ip domain-lookup

! Enable secret
R1(config)# enable secret class

! Console line
R1(config)# line console 0
R1(config-line)# password cisco
R1(config-line)# login
R1(config-line)# logging synchronous
R1(config-line)# exec-timeout 5 0
R1(config-line)# exit

! VTY lines
R1(config)# line vty 0 4
R1(config-line)# password cisco
R1(config-line)# login
R1(config-line)# transport input ssh telnet
R1(config-line)# exec-timeout 5 0
R1(config-line)# exit

! Encrypt passwords
R1(config)# service password-encryption

! Banner
R1(config)# banner motd #
***********************************************
*  UNAUTHORIZED ACCESS IS PROHIBITED         *
***********************************************
#

! Save
R1(config)# exit
R1# copy running-config startup-config
```

---

## 10.2 Configure Interfaces (ตั้งค่า Interfaces)

### Router Interfaces

**ประเภท Interfaces:**

#### 1. LAN Interfaces (Ethernet)

```
Types:
- FastEthernet (100 Mbps) - เก่า
- GigabitEthernet (1 Gbps) - ปัจจุบัน
- TenGigabitEthernet (10 Gbps)
- FortyGigabitEthernet (40 Gbps)

Naming:
FastEthernet 0/0
GigabitEthernet 0/0/0
TenGigabitEthernet 0/1/0
```

#### 2. WAN Interfaces (Serial)

```
Types:
- Serial (T1, E1, T3, E3)
- Integrated Services Router (ISR)

Naming:
Serial 0/0/0
Serial 0/1/0
```

#### 3. Management Interface

```
- ไม่มี physical interface แยก (ต่างจาก Switch VLAN 1)
- ใช้ LAN interface ใดก็ได้สำหรับ management
```

---

### Interface Numbering

**รูปแบบ:**

```
type slot/module/port

ตัวอย่าง:
GigabitEthernet 0/0/0
│              │ │ │
│              │ │ └─ Port number
│              │ └─── Module number (เสริม)
│              └───── Slot number
└──────────────────── Interface type

Older routers (no modules):
GigabitEthernet 0/0
│              │ │
│              │ └─── Port number
│              └───── Slot number
└──────────────────── Interface type
```

---

### Configure LAN Interface

#### Default State

**สำคัญ:**

- Router interfaces เป็น **administratively down** (shutdown) โดย default
- ต่างจาก Switch (up โดย default)

#### Configuration Steps

**1. Enter Interface Configuration Mode:**

```
R1(config)# interface gigabitethernet 0/0
R1(config-if)#

! Short version
R1(config)# int g0/0
R1(config-if)#
```

**2. Description:**

```
R1(config-if)# description Link to LAN 1
```

**3. IP Address:**

```
R1(config-if)# ip address 192.168.10.1 255.255.255.0
```

**4. Enable Interface:**

```
R1(config-if)# no shutdown
R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up
```

**ความหมายของ messages:**

```
%LINK-5-CHANGED: Interface ... changed state to up
  → Physical layer (Layer 1) is up

%LINEPROTO-5-UPDOWN: Line protocol ... changed state to up
  → Data Link layer (Layer 2) is up
```

**Complete Example:**

```
R1(config)# interface gigabitethernet 0/0
R1(config-if)# description Link to LAN 1 - 192.168.10.0/24
R1(config-if)# ip address 192.168.10.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)#
```

---

### Configure Multiple Interfaces

**Interface Range:**

```
R1(config)# interface range gigabitethernet 0/0 - 1
R1(config-if-range)# description LAN Interfaces
R1(config-if-range)# no shutdown
R1(config-if-range)# exit
```

**Configure Individual Interfaces:**

```
! Interface G0/0
R1(config)# interface g0/0
R1(config-if)# description LAN 1 - Sales Department
R1(config-if)# ip address 192.168.10.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit

! Interface G0/1
R1(config)# interface g0/1
R1(config-if)# description LAN 2 - Engineering Department
R1(config-if)# ip address 192.168.20.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit

! Interface G0/2
R1(config)# interface g0/2
R1(config-if)# description Link to R2
R1(config-if)# ip address 10.0.0.1 255.255.255.252
R1(config-if)# no shutdown
R1(config-if)# exit
```

---

### Configure Loopback Interface

**คำจำกัดความ:**

- **Virtual interface** (software)
- ไม่มี physical interface
- **Always up** (ไม่เสียง่าย)

**การใช้งาน:**

- **Router ID** - OSPF, EIGRP
- **Testing** - Ping, Telnet
- **Management** - Remote access

**Configuration:**

```
R1(config)# interface loopback 0
R1(config-if)# description Loopback for Management
R1(config-if)# ip address 10.1.1.1 255.255.255.255
R1(config-if)# exit

! Loopback automatically "no shutdown"
```

**Subnet Mask:**

- แนะนำใช้ `/32` (255.255.255.255) สำหรับ Loopback
- ประหยัด IP addresses

**ตัวอย่าง Multiple Loopbacks:**

```
R1(config)# interface loopback 0
R1(config-if)# ip address 10.1.1.1 255.255.255.255
R1(config-if)# exit

R1(config)# interface loopback 1
R1(config-if)# ip address 10.2.2.2 255.255.255.255
R1(config-if)# exit
```

---

### Verification Commands

#### Show IP Interface Brief

**คำสั่ง:**

```
R1# show ip interface brief
```

**Output:**

```
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0     192.168.10.1    YES manual up                    up
GigabitEthernet0/1     192.168.20.1    YES manual up                    up
GigabitEthernet0/2     10.0.0.1        YES manual up                    up
Serial0/0/0            unassigned      YES unset  administratively down down
Serial0/0/1            unassigned      YES unset  administratively down down
Loopback0              10.1.1.1        YES manual up                    up
```

**อธิบาย columns:**

- **Interface:** ชื่อ interface
- **IP-Address:** IP address (unassigned = ไม่มี IP)
- **OK?:** Configuration OK
- **Method:**
    - `manual` - กำหนดด้วยตนเอง
    - `DHCP` - ได้จาก DHCP
    - `unset` - ไม่ได้กำหนด
- **Status:** Physical layer (Layer 1)
    - `up` - สาย/hardware OK
    - `down` - ไม่มีสาย/hardware เสีย
    - `administratively down` - shutdown (ปิดโดย admin)
- **Protocol:** Data Link layer (Layer 2)
    - `up` - Layer 2 OK
    - `down` - Layer 2 ไม่ OK

**Interface States:**

```
Status                   Protocol    Meaning
------------------------------------------------------------------------
up                       up          ✅ Interface working (สายเสียบ, config OK)
up                       down        ⚠️ Layer 1 OK, Layer 2 problem
down                     down        ❌ No cable / hardware failure
administratively down    down        ⚠️ shutdown (ปิดโดย admin)
------------------------------------------------------------------------
```

#### Show Interfaces

**คำสั่ง:**

```
R1# show interfaces
R1# show interfaces gigabitethernet 0/0
R1# show interfaces g0/0
```

**Output (ตัวอย่าง):**

```
R1# show interfaces g0/0

GigabitEthernet0/0 is up, line protocol is up
  Hardware is iGbE, address is 0001.9717.6501 (bia 0001.9717.6501)
  Description: Link to LAN 1 - 192.168.10.0/24
  Internet address is 192.168.10.1/24
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive set (10 sec)
  Full Duplex, 1Gbps, media type is RJ45
  output flow-control is unsupported, input flow-control is unsupported
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:00, output 00:00:00, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     1234 packets input, 567890 bytes, 0 no buffer
     Received 100 broadcasts (50 IP multicasts)
     0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 50 multicast, 0 pause input
     2468 packets output, 987654 bytes, 0 underruns
     0 output errors, 0 collisions, 1 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
```

**ข้อมูลสำคัญ:**

- **Hardware address:** MAC address
- **Internet address:** IP address
- **MTU:** Maximum Transmission Unit (1500 bytes default)
- **BW:** Bandwidth (1000000 Kbit/sec = 1 Gbps)
- **Encapsulation:** ARPA (Ethernet II)
- **Duplex, Speed:** Full Duplex, 1Gbps
- **Input/Output stats:** Packets, bytes, errors

#### Show Running-Config

**ดู Interface Configuration:**

```
R1# show running-config interface g0/0

Building configuration...

Current configuration : 155 bytes
!
interface GigabitEthernet0/0
 description Link to LAN 1 - 192.168.10.0/24
 ip address 192.168.10.1 255.255.255.0
end
```

**ดู Running-Config ทั้งหมด:**

```
R1# show running-config
หรือ
R1# sh run
```

#### Show IP Route

**ดู Routing Table:**

```
R1# show ip route
```

**Output:**

```
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       [... more codes ...]

Gateway of last resort is not set

     10.0.0.0/8 is variably subnetted, 3 subnets, 3 masks
C       10.0.0.0/30 is directly connected, GigabitEthernet0/2
L       10.0.0.1/32 is directly connected, GigabitEthernet0/2
L       10.1.1.1/32 is directly connected, Loopback0
     192.168.10.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.10.0/24 is directly connected, GigabitEthernet0/0
L       192.168.10.1/32 is directly connected, GigabitEthernet0/0
     192.168.20.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.20.0/24 is directly connected, GigabitEthernet0/1
L       192.168.20.1/32 is directly connected, GigabitEthernet0/1
```

**อธิบาย:**

- **C (Connected):** เครือข่ายที่เชื่อมต่อโดยตรง
- **L (Local):** IP address ของ interface นั้นเอง (/32)

---

### IPv6 on Router Interfaces

#### Enable IPv6 Routing

**สำคัญมาก:**

```
R1(config)# ipv6 unicast-routing
```

- **ต้องเปิด** ก่อนใช้ IPv6 routing
- Router จะส่ง Router Advertisements (RA)
- Router จะ forward IPv6 packets

#### Configure IPv6 Address

**Manual Configuration:**

```
R1(config)# interface g0/0
R1(config-if)# ipv6 address 2001:db8:acad:1::1/64
R1(config-if)# no shutdown
R1(config-if)# exit
```

**Link-Local Address (เพิ่มเติม):**

```
R1(config)# interface g0/0
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# exit
```

**IPv6 Auto-Configuration (EUI-64):**

```
R1(config)# interface g0/0
R1(config-if)# ipv6 address 2001:db8:acad:1::/64 eui-64
R1(config-if)# exit
```

**ตัวอย่างเต็ม:**

```
R1(config)# ipv6 unicast-routing

R1(config)# interface g0/0
R1(config-if)# description IPv6 LAN 1
R1(config-if)# ipv6 address 2001:db8:acad:1::1/64
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# no shutdown
R1(config-if)# exit

R1(config)# interface g0/1
R1(config-if)# description IPv6 LAN 2
R1(config-if)# ipv6 address 2001:db8:acad:2::1/64
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# no shutdown
R1(config-if)# exit
```

#### Verification

**Show IPv6 Interface Brief:**

```
R1# show ipv6 interface brief

GigabitEthernet0/0     [up/up]
    FE80::1
    2001:DB8:ACAD:1::1
GigabitEthernet0/1     [up/up]
    FE80::1
    2001:DB8:ACAD:2::1
Serial0/0/0            [administratively down/down]
    unassigned
Loopback0              [up/up]
    FE80::1
    2001:DB8:ACAD:10::1
```

**Show IPv6 Interface:**

```
R1# show ipv6 interface g0/0

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
  ND reachable time is 30000 milliseconds (using 30000)
  ND NS retransmit interval is 1000 milliseconds
```

**Show IPv6 Route:**

```
R1# show ipv6 route

IPv6 Routing Table - default - 7 entries
Codes: C - Connected, L - Local, S - Static, [...]

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

---

## 10.3 Configure Default Gateway (ตั้งค่า Default Gateway)

### Default Gateway Concept

**คำจำกัดความ:**

- เส้นทาง**สำหรับทุก destination ที่ไม่รู้จัก**
- เรียกว่า **Gateway of Last Resort**
- Network = `0.0.0.0/0` (IPv4) หรือ `::/0` (IPv6)

---

### Configure Default Static Route

#### IPv4 Default Route

**Syntax:**

```
Router(config)# ip route 0.0.0.0 0.0.0.0 <next-hop-ip | exit-interface>
```

**ตัวอย่าง 1: Next-hop IP**

```
R1(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.2
```

**ตัวอย่าง 2: Exit Interface**

```
R1(config)# ip route 0.0.0.0 0.0.0.0 gigabitethernet 0/2
```

**ตัวอย่าง 3: Next-hop + Exit Interface (แนะนำ)**

```
R1(config)# ip route 0.0.0.0 0.0.0.0 g0/2 10.0.0.2
```

**Verify:**

```
R1# show ip route

Gateway of last resort is 10.0.0.2 to network 0.0.0.0

S*   0.0.0.0/0 [1/0] via 10.0.0.2
     ^
     * = Candidate default route
```

#### IPv6 Default Route

**Syntax:**

```
Router(config)# ipv6 route ::/0 <next-hop-ipv6 | exit-interface>
```

**ตัวอย่าง:**

```
R1(config)# ipv6 route ::/0 2001:db8:acad:2::2
หรือ
R1(config)# ipv6 route ::/0 g0/1 fe80::2
```

**Verify:**

```
R1# show ipv6 route

S   ::/0 [1/0]
     via 2001:DB8:ACAD:2::2
```

---

### Static Route Example

**Topology:**

```
[LAN 1]---[R1]---[WAN]---[R2]---[LAN 2]
10.1.1.0/24  |            |    10.2.2.0/24
          10.0.0.1    10.0.0.2
```

**R1 Configuration:**

```
! LAN Interface
R1(config)# interface g0/0
R1(config-if)# ip address 10.1.1.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit

! WAN Interface
R1(config)# interface g0/1
R1(config-if)# ip address 10.0.0.1 255.255.255.252
R1(config-if)# no shutdown
R1(config-if)# exit

! Static Route to LAN 2
R1(config)# ip route 10.2.2.0 255.255.255.0 10.0.0.2

! Or Default Route
R1(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.2
```

**R2 Configuration:**

```
! WAN Interface
R2(config)# interface g0/0
R2(config-if)# ip address 10.0.0.2 255.255.255.252
R2(config-if)# no shutdown
R2(config-if)# exit

! LAN Interface
R2(config)# interface g0/1
R2(config-if)# ip address 10.2.2.1 255.255.255.0
R2(config-if)# no shutdown
R2(config-if)# exit

! Static Route to LAN 1
R2(config)# ip route 10.1.1.0 255.255.255.0 10.0.0.1

! Or Default Route
R2(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.1
```

**Verify Connectivity:**

```
R1# ping 10.2.2.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.2.2.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/2/4 ms
```

---

## 10.4 Packet Tracer - Configure Initial Router Settings

### Lab Objectives

**ในแบบฝึกหัด Packet Tracer จะฝึก:**

#### Part 1: Basic Router Configuration

1. Configure hostname
2. Disable DNS lookup
3. Secure privileged EXEC mode (enable secret)
4. Secure console access
5. Secure VTY access
6. Encrypt passwords
7. Configure MOTD banner
8. Save configuration

#### Part 2: Configure Interfaces

1. Configure and activate router interfaces
2. Assign IP addresses
3. Configure descriptions
4. Verify interface status

#### Part 3: Verify Connectivity

1. Test connectivity between devices
2. Use ping and traceroute
3. Verify routing table

---

### Sample Lab Configuration

**Addressing Table:**

```
Device  Interface  IP Address       Subnet Mask       Default Gateway
------------------------------------------------------------------------
R1      G0/0       192.168.0.1      255.255.255.0     N/A
        G0/1       192.168.1.1      255.255.255.0     N/A
        S0/0/0     10.0.0.1         255.255.255.252   N/A
PC-A    NIC        192.168.0.10     255.255.255.0     192.168.0.1
PC-B    NIC        192.168.1.10     255.255.255.0     192.168.1.1
```

**R1 Configuration:**

```
Router> enable
Router# configure terminal

! Part 1: Basic Settings
Router(config)# hostname R1
R1(config)# no ip domain-lookup
R1(config)# enable secret class
R1(config)# line console 0
R1(config-line)# password cisco
R1(config-line)# login
R1(config-line)# logging synchronous
R1(config-line)# exit
R1(config)# line vty 0 4
R1(config-line)# password cisco
R1(config-line)# login
R1(config-line)# transport input ssh telnet
R1(config-line)# exit
R1(config)# service password-encryption
R1(config)# banner motd #Unauthorized access prohibited!#

! Part 2: Configure Interfaces
R1(config)# interface g0/0
R1(config-if)# description Connection to PC-A
R1(config-if)# ip address 192.168.0.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit

R1(config)# interface g0/1
R1(config-if)# description Connection to PC-B
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit

R1(config)# interface s0/0/0
R1(config-if)# description WAN Link
R1(config-if)# ip address 10.0.0.1 255.255.255.252
R1(config-if)# no shutdown
R1(config-if)# exit

! Part 3: Save Configuration
R1(config)# exit
R1# copy running-config startup-config
```

**Verification:**

```
! Verify interfaces
R1# show ip interface brief

! Verify running config
R1# show running-config

! Test connectivity
R1# ping 192.168.0.10
R1# ping 192.168.1.10

! Show routing table
R1# show ip route
```

---

## Summary (สรุป)

Module 10 นี้เราได้เรียนรู้:

1. ✅ **Router vs Switch** - ความแตกต่าง, ฟังก์ชัน
2. ✅ **Initial Router Setup** - Console connection, Boot sequence
3. ✅ **Basic Configuration** - Hostname, Passwords, Banner
4. ✅ **Secure Access** - Console, VTY, Enable secret
5. ✅ **Password Encryption** - service password-encryption, Types 0/5/7
6. ✅ **Interface Configuration** - LAN interfaces, IP address, Description
7. ✅ **Interface States** - up/up, up/down, down/down, administratively down
8. ✅ **Loopback Interface** - Virtual interface, Always up
9. ✅ **IPv6 Configuration** - ipv6 unicast-routing, IPv6 addresses
10. ✅ **Default Gateway** - Static default route, 0.0.0.0/0
11. ✅ **Verification Commands** - show ip interface brief, show interfaces, show ip route

**สิ่งสำคัญที่ต้องจำ:**

- Router interfaces = **shutdown โดย default** (ต่างจาก Switch)
- ต้อง **no shutdown** เพื่อเปิด interface
- Enable secret > Enable password (ใช้ enable secret)
- service password-encryption = Type 7 (weak, แต่ดีกว่าไม่มี)
- Interface states: up/up = ✅, administratively down = shutdown
- Connected routes (C) = เครือข่ายที่เชื่อมต่อโดยตรง
- Local routes (L) = IP address ของ interface (/32)
- Default route = 0.0.0.0/0 (IPv4), ::/0 (IPv6)
- IPv6 routing ต้องเปิด: ipv6 unicast-routing

**Configuration Checklist:**

```
☐ Hostname
☐ no ip domain-lookup
☐ enable secret
☐ line console 0 (password, login)
☐ line vty 0 4 (password, login)
☐ service password-encryption
☐ banner motd
☐ Interface IP addresses
☐ no shutdown (interfaces)
☐ copy run start
```

**Next Module:** Module 11 - IPv4 Addressing

---

**[ไฟล์ Module 10 สมบูรณ์แล้ว!]**