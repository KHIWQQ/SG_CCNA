# CCNA 2: Module 3 - VLANs

## Virtual Local Area Networks

---

## สารบัญ

1. [วัตถุประสงค์ของ Module](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#%E0%B8%A7%E0%B8%B1%E0%B8%95%E0%B8%96%E0%B8%B8%E0%B8%9B%E0%B8%A3%E0%B8%B0%E0%B8%AA%E0%B8%87%E0%B8%84%E0%B9%8C%E0%B8%82%E0%B8%AD%E0%B8%87-module)
2. [Overview of VLANs](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#1-overview-of-vlans)
3. [VLANs Across Multiple Switches](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#2-vlans-across-multiple-switches)
4. [VLAN Configuration](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#3-vlan-configuration)
5. [VLAN Trunks](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#4-vlan-trunks)
6. [Dynamic Trunking Protocol (DTP)](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#5-dynamic-trunking-protocol-dtp)
7. [สรุป](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#%E0%B8%AA%E0%B8%A3%E0%B8%B8%E0%B8%9B)

---

## วัตถุประสงค์ของ Module

เมื่อจบ Module นี้ คุณจะสามารถ:

- ✅ อธิบายวัตถุประสงค์และประโยชน์ของ VLANs
- ✅ เข้าใจประเภทต่างๆ ของ VLANs
- ✅ สร้างและกำหนด VLANs บน switch
- ✅ กำหนดพอร์ตให้กับ VLAN
- ✅ อธิบายและ configure VLAN trunking
- ✅ เข้าใจ 802.1Q frame tagging
- ✅ Configure native VLAN
- ✅ ใช้ Dynamic Trunking Protocol (DTP)
- ✅ Troubleshoot VLAN และ trunk issues

---

## 1. Overview of VLANs

### ภาพรวมของ VLANs

### 1.1 VLAN คืออะไร?

**VLAN (Virtual Local Area Network):**

- การแบ่งสวิตช์ออกเป็นหลาย broadcast domains แบบ logical
- แต่ละ VLAN ทำงานเหมือน LAN แยกกัน
- อุปกรณ์ในคนละ VLAN ไม่สามารถสื่อสารกันได้โดยตรง (ต้องผ่าน router)
- Configure ผ่าน software (ไม่ต้องเปลี่ยนฮาร์ดแวร์)

**แนวคิด:**

```
Traditional Network (No VLANs):
[Switch] - ทุกพอร์ตอยู่ใน broadcast domain เดียวกัน
├── Sales PC
├── Engineering PC
├── HR PC
└── IT PC

= 1 Broadcast Domain
= ทุกคนเห็น broadcast traffic ของกันและกัน

With VLANs:
[Switch]
├── Sales PC        (VLAN 10)
├── Sales PC        (VLAN 10)
├── Engineering PC  (VLAN 20)
├── Engineering PC  (VLAN 20)
├── HR PC          (VLAN 30)
└── IT PC          (VLAN 40)

= 4 Broadcast Domains
= แยกกันตามแผนก, broadcast ไม่ข้าม VLANs
```

### 1.2 ประโยชน์ของ VLANs

**1. Security (ความปลอดภัย):**

```
แยกข้อมูลสำคัญออกจากกัน:
- VLAN 10: Sales (public data)
- VLAN 20: Finance (sensitive data)
- VLAN 30: HR (confidential data)

Finance ไม่เห็น traffic ของ Sales และ HR
```

**2. Cost Reduction (ลดค่าใช้จ่าย):**

- ไม่ต้องซื้อสวิตช์เพิ่ม
- ลด broadcast traffic = ลด CPU load
- ง่ายต่อการจัดการ

**3. Better Performance (ประสิทธิภาพดีขึ้น):**

- แบ่ง broadcast domains เล็กลง
- ลด broadcast traffic
- Bandwidth utilization ดีขึ้น

**4. Shrink Broadcast Domains:**

```
Without VLANs:
100 devices = 1 broadcast domain
Broadcast storms affect everyone

With VLANs:
100 devices = 5 VLANs × 20 devices
Broadcast isolated to each VLAN
```

**5. Improved IT Efficiency:**

- จัดการผ่าน software
- ย้าย users ระหว่าง VLANs ได้ง่าย
- ไม่ต้องเปลี่ยนสาย
- Easier troubleshooting

**6. Simpler Project Management:**

- แยกตามโปรเจค
- แยกตามแผนก
- แยกตามหน้าที่
- Flexible group management

### 1.3 Types of VLANs

**1. Default VLAN (VLAN 1):**

```
- VLAN เริ่มต้นของ Cisco switches
- ทุกพอร์ตอยู่ใน VLAN 1 เมื่อแรกติดตั้ง
- ไม่สามารถลบหรือเปลี่ยนชื่อได้
- Best Practice: ไม่ควรใช้ VLAN 1 สำหรับ user traffic

Switch# show vlan brief
VLAN Name                             Status    Ports
---- -------------------------------- --------- ------------------------
1    default                          active    Fa0/1, Fa0/2, ..., Fa0/24
```

**2. Data VLAN (User VLAN):**

```
- VLAN สำหรับ user traffic
- แยกตาม user groups, departments, functions
- ตัวอย่าง:
  * VLAN 10: Sales
  * VLAN 20: Engineering
  * VLAN 30: HR
  * VLAN 40: Finance

Switch(config)# vlan 10
Switch(config-vlan)# name Sales
```

**3. Native VLAN:**

```
- VLAN สำหรับ untagged traffic บน trunk link
- Default: VLAN 1
- Best Practice: เปลี่ยนเป็นค่าอื่น (security)
- ต้องตรงกันทั้งสองฝั่งของ trunk

Switch(config)# interface fa0/1
Switch(config-if)# switchport trunk native vlan 99
```

**4. Management VLAN:**

```
- VLAN สำหรับจัดการ switch (SSH, Telnet, SNMP)
- กำหนด IP address บน SVI (Switch Virtual Interface)
- แยกจาก user traffic
- Best Practice: ใช้ VLAN อื่นจาก default VLAN

Switch(config)# interface vlan 99
Switch(config-if)# ip address 192.168.99.10 255.255.255.0
Switch(config-if)# no shutdown
```

**5. Voice VLAN:**

```
- VLAN พิเศษสำหรับ VoIP traffic
- Priority สูงกว่า data (QoS)
- Separate voice และ data traffic
- ใช้ร่วมกับ data VLAN บนพอร์ตเดียวกันได้

Switch(config)# interface fa0/5
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config-if)# switchport voice vlan 50
```

**6. Black Hole VLAN:**

```
- VLAN สำหรับพอร์ตที่ไม่ใช้งาน
- Security measure
- ไม่มีอุปกรณ์ใดใช้งาน VLAN นี้

Switch(config)# vlan 999
Switch(config-vlan)# name BlackHole

Switch(config)# interface range fa0/20 - 24
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 999
Switch(config-if-range)# shutdown
```

### 1.4 VLAN ID Ranges

**Normal Range VLANs (1-1005):**

```
- VLAN 1: Default VLAN (ไม่สามารถลบได้)
- VLAN 2-1001: สร้างและลบได้
- VLAN 1002-1005: Default VLANs สำหรับ Token Ring และ FDDI
- เก็บใน vlan.dat file (flash memory)
- รองรับใน VTP (VLAN Trunking Protocol)
```

**Extended Range VLANs (1006-4094):**

```
- VLAN 1006-4094: สำหรับ service providers
- ต้อง configure เป็น transparent mode ใน VTP
- ไม่เก็บใน vlan.dat (เก็บใน running-config)
- Best practice สำหรับ large networks
```

**Reserved VLANs:**

```
- VLAN 0: Reserved (ไม่ได้ใช้)
- VLAN 1: Default VLAN
- VLAN 1002-1005: Token Ring และ FDDI defaults
- VLAN 4095: Reserved (ไม่ได้ใช้)
```

### 1.5 VLAN Example

**สถานการณ์:**

```
บริษัทมี 4 แผนก ต้องการแยก network:
- Sales (20 users)
- Engineering (30 users)
- HR (10 users)
- Management (5 users)

Without VLANs:
ต้องใช้ 4 switches แยกกัน = แพง

With VLANs:
ใช้ 1-2 switches, แบ่งเป็น 4 VLANs = ประหยัด
```

**VLAN Design:**

```
VLAN 10 - Sales
├── IP Subnet: 192.168.10.0/24
└── Ports: Fa0/1-10

VLAN 20 - Engineering
├── IP Subnet: 192.168.20.0/24
└── Ports: Fa0/11-20

VLAN 30 - HR
├── IP Subnet: 192.168.30.0/24
└── Ports: Fa0/21-24

VLAN 99 - Management
├── IP Subnet: 192.168.99.0/24
└── SVI: 192.168.99.10
```

---

## 2. VLANs Across Multiple Switches

### VLANs ข้ามหลาย Switches

### 2.1 VLAN Implementation

**Single Switch:**

```
[Switch1]
├── VLAN 10: Fa0/1-5
├── VLAN 20: Fa0/6-10
└── VLAN 30: Fa0/11-15

= ทำงานได้ดี แต่จำกัดจำนวนพอร์ต
```

**Multiple Switches (ไม่มี Trunk):**

```
[Switch1]               [Switch2]
├── VLAN 10: Fa0/1     ├── VLAN 10: Fa0/1
│                      │
└── Connect ผ่าน VLAN 10 link

ปัญหา:
- ต้องใช้ cable แยกต่าง VLAN
- ถ้ามี 10 VLANs ต้องใช้ 10 cables!
- เสียพอร์ตมาก, ไม่ practical
```

**Multiple Switches (มี Trunk):**

```
[Switch1] ═══TRUNK═══ [Switch2]
├── VLAN 10           ├── VLAN 10
├── VLAN 20           ├── VLAN 20
└── VLAN 30           └── VLAN 30

= ใช้ cable เส้นเดียวสำหรับทุก VLANs
= ประหยัดพอร์ต, ง่ายต่อการจัดการ
```

### 2.2 VLAN Tagging

**802.1Q Frame Tagging:**

```
Normal Ethernet Frame:
[Dest MAC][Src MAC][Type][Data][FCS]

802.1Q Tagged Frame:
[Dest MAC][Src MAC][802.1Q TAG][Type][Data][FCS]
                      |
                      └─ 4 bytes เพิ่มเข้ามา

802.1Q Tag (4 bytes):
[TPID][PRI][CFI][VLAN ID]
  2B   3bit 1bit  12bit

- TPID (Tag Protocol ID): 0x8100 (บอกว่าเป็น 802.1Q frame)
- PRI (Priority): 0-7 (สำหรับ QoS)
- CFI (Canonical Format Indicator): 0 หรือ 1
- VLAN ID: 0-4095 (ระบุว่าเป็น VLAN อะไร)
```

**การทำงาน:**

```
PC (VLAN 10) ────► [Switch1] ══TRUNK══ [Switch2] ────► PC (VLAN 10)
                      │                    │
                      ▼                    ▼
                 Add Tag              Remove Tag
              (VLAN 10)               (VLAN 10)

1. PC ส่ง normal frame (ไม่มี tag)
2. Switch1 เพิ่ม 802.1Q tag (VLAN 10)
3. ส่งผ่าน trunk link
4. Switch2 ตรวจสอบ tag
5. ลบ tag ก่อนส่งไป PC
6. PC ได้รับ normal frame
```

### 2.3 Native VLAN Concept

**Native VLAN คืออะไร:**

- VLAN สำหรับ frames ที่ไม่มี tag (untagged)
- Default: VLAN 1
- Frames ใน native VLAN ไม่ถูก tag บน trunk

**การทำงาน:**

```
Trunk Link:
[Switch1] ═════════ [Switch2]

VLAN 10 frames → Tagged with VLAN 10
VLAN 20 frames → Tagged with VLAN 20
Native VLAN frames → Untagged

Best Practice:
- เปลี่ยนจาก VLAN 1 เป็นค่าอื่น (security)
- ต้องตรงกันทั้งสองฝั่ง
- ไม่ควรใช้งานจริง (unused VLAN)
```

**Native VLAN Mismatch:**

```
Problem:
[Switch1]                    [Switch2]
Native VLAN 10 ──trunk─── Native VLAN 99

ผลลัพธ์:
- Traffic ถูกส่งผิด VLAN
- Broadcast floods
- Security issues
- CDP/VTP warnings

Solution:
ตั้งให้ตรงกันทั้งสองฝั่ง
```

### 2.4 Voice VLAN Implementation

**ทำไมต้องมี Voice VLAN:**

```
VoIP Requirements:
- Low latency (<150ms)
- Low jitter
- High priority (QoS)
- Guaranteed bandwidth

ถ้ารวมกับ data traffic:
- Voice quality แย่
- Delay, packet loss
- ไม่ควบคุม QoS ได้
```

**Voice VLAN Configuration:**

```
Topology:
IP Phone (VLAN 50) ──┬── PC (VLAN 10)
                     │
                 [Switch Port]

Port Configuration:
- Access VLAN: 10 (for PC)
- Voice VLAN: 50 (for IP Phone)
- IP Phone ทำหน้าที่เป็น mini-switch
- แยก voice และ data traffic
```

**ตัวอย่าง Configuration:**

```cisco
Switch(config)# interface fa0/5
Switch(config-if)# description IP Phone + PC

! ตั้งเป็น access port
Switch(config-if)# switchport mode access

! VLAN สำหรับ PC
Switch(config-if)# switchport access vlan 10

! VLAN สำหรับ IP Phone
Switch(config-if)# switchport voice vlan 50

! เปิด PoE (ถ้ารองรับ)
Switch(config-if)# power inline auto

! QoS Trust
Switch(config-if)# mls qos trust cos
```

---

## 3. VLAN Configuration

### การตั้งค่า VLAN

### 3.1 VLAN Creation

**การสร้าง VLAN:**

**Method 1: Global Configuration Mode**

```cisco
! เข้าสู่ global config
Switch(config)# vlan 10

! ตั้งชื่อ VLAN (optional แต่แนะนำ)
Switch(config-vlan)# name Sales

! ออกจาก VLAN config
Switch(config-vlan)# exit

! สร้าง VLANs เพิ่ม
Switch(config)# vlan 20
Switch(config-vlan)# name Engineering

Switch(config)# vlan 30
Switch(config-vlan)# name HR

Switch(config)# vlan 99
Switch(config-vlan)# name Management
```

**Method 2: สร้างหลาย VLANs พร้อมกัน**

```cisco
! สร้าง VLANs 10, 20, 30 พร้อมกัน
Switch(config)# vlan 10,20,30

! หรือสร้างแบบ range
Switch(config)# vlan 10-30
```

**Method 3: Interface Configuration Mode (deprecated)**

```cisco
! วิธีเก่า - ไม่แนะนำ
Switch(config)# interface fa0/1
Switch(config-if)# switchport access vlan 10
% Access VLAN does not exist. Creating vlan 10
```

### 3.2 VLAN Port Assignment

**กำหนดพอร์ตให้ VLAN:**

**Single Port:**

```cisco
! เลือก interface
Switch(config)# interface fastethernet 0/5

! ตั้งเป็น access mode
Switch(config-if)# switchport mode access

! กำหนดให้ VLAN 10
Switch(config-if)# switchport access vlan 10

! เพิ่ม description (best practice)
Switch(config-if)# description Sales Department PC
```

**Multiple Ports (Interface Range):**

```cisco
! เลือกหลายพอร์ต
Switch(config)# interface range fastethernet 0/1 - 10

! ตั้งเป็น access mode
Switch(config-if-range)# switchport mode access

! กำหนดให้ VLAN 10
Switch(config-if-range)# switchport access vlan 10

! เพิ่ม description
Switch(config-if-range)# description Sales Department

! อีกตัวอย่าง
Switch(config)# interface range fa0/11 - 20
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 20
Switch(config-if-range)# description Engineering Department
```

**Non-Contiguous Ports:**

```cisco
! เลือกพอร์ตที่ไม่ติดกัน
Switch(config)# interface range fa0/1, fa0/5, fa0/10, fa0/15
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 30
```

### 3.3 VLAN Verification

**คำสั่งตรวจสอบ VLAN:**

**1. Show VLAN Brief:**

```cisco
Switch# show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/24
10   Sales                            active    Fa0/1, Fa0/2, Fa0/3
20   Engineering                      active    Fa0/11, Fa0/12, Fa0/13
30   HR                               active    Fa0/21, Fa0/22
99   Management                       active    
```

**2. Show VLAN (รายละเอียดเต็ม):**

```cisco
Switch# show vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
10   Sales                            active    Fa0/1, Fa0/2, Fa0/3

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
10   enet  100010     1500  -      -      -        -    -        0      0
```

**3. Show VLAN ID:**

```cisco
Switch# show vlan id 10

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
10   Sales                            active    Fa0/1, Fa0/2, Fa0/3
```

**4. Show VLAN Name:**

```cisco
Switch# show vlan name Sales
```

**5. Show Interface Switchport:**

```cisco
Switch# show interface fa0/1 switchport

Name: Fa0/1
Switchport: Enabled
Administrative Mode: static access
Operational Mode: static access
Administrative Trunking Encapsulation: dot1q
Operational Trunking Encapsulation: native
Negotiation of Trunking: Off
Access Mode VLAN: 10 (Sales)
Trunking Native Mode VLAN: 1 (default)
```

**6. Show Interfaces Status:**

```cisco
Switch# show interfaces status

Port      Name               Status       Vlan       Duplex  Speed Type
Fa0/1     Sales PC1         connected    10         a-full  a-100 10/100BaseTX
Fa0/2     Sales PC2         connected    10         a-full  a-100 10/100BaseTX
Fa0/11    Eng PC1           connected    20         a-full  a-100 10/100BaseTX
```

**7. Show Running-Config:**

```cisco
Switch# show running-config | section interface Fa0/1

interface FastEthernet0/1
 description Sales PC1
 switchport mode access
 switchport access vlan 10
```

### 3.4 Management VLAN Configuration

**ตั้งค่า Management VLAN:**

```cisco
! สร้าง Management VLAN
Switch(config)# vlan 99
Switch(config-vlan)# name Management
Switch(config-vlan)# exit

! กำหนด IP address บน SVI
Switch(config)# interface vlan 99
Switch(config-if)# description Management Interface
Switch(config-if)# ip address 192.168.99.10 255.255.255.0
Switch(config-if)# no shutdown
Switch(config-if)# exit

! กำหนด default gateway
Switch(config)# ip default-gateway 192.168.99.1

! ย้ายพอร์ต management ไป VLAN 99 (optional)
Switch(config)# interface fa0/24
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 99
Switch(config-if)# description Management Access Port
```

**ตรวจสอบ Management Interface:**

```cisco
Switch# show interface vlan 99

Vlan99 is up, line protocol is up
  Hardware is EtherSVI, address is 0cd9.96d2.4800
  Description: Management Interface
  Internet address is 192.168.99.10/24
  MTU 1500 bytes, BW 1000000 Kbit/sec
```

### 3.5 Delete VLANs

**ลบ VLAN:**

```cisco
! ลบ VLAN เดียว
Switch(config)# no vlan 10

! ลบหลาย VLANs
Switch(config)# no vlan 10,20,30

! ลบ VLAN range
Switch(config)# no vlan 10-30
```

**หมายเหตุสำคัญ:**

- ไม่สามารถลบ VLAN 1 ได้
- พอร์ตที่อยู่ใน VLAN ที่ถูกลบจะกลับไป VLAN 1
- VLAN 1002-1005 ลบไม่ได้ (reserved)

**ลบ VLAN Configuration ทั้งหมด:**

```cisco
! ลบไฟล์ vlan.dat
Switch# delete flash:vlan.dat
Delete filename [vlan.dat]? [Enter]
Delete flash:/vlan.dat? [confirm] [Enter]

! หรือ
Switch# delete vlan.dat

! รีบูตสวิตช์
Switch# reload
```

---

## 4. VLAN Trunks

### VLAN Trunk Links

### 4.1 Trunk คืออะไร?

**VLAN Trunk:**

- Link ที่สามารถส่ง traffic ของหลาย VLANs
- ใช้เชื่อมต่อระหว่าง switches
- ใช้ 802.1Q tagging
- ประหยัดพอร์ตและสาย

**Access Port vs Trunk Port:**

```
Access Port:
- ส่ง traffic ของ VLAN เดียว
- ไม่มี VLAN tagging
- เชื่อมต่อกับ end devices (PC, printer)

Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10

Trunk Port:
- ส่ง traffic ของหลาย VLANs
- ใช้ 802.1Q tagging
- เชื่อมต่อระหว่าง switches หรือ router

Switch(config-if)# switchport mode trunk
```

**Topology ตัวอย่าง:**

```
         [Switch1]
        /    |    \
    VLAN10 VLAN20 VLAN30
       |
    [TRUNK] (ส่งทั้ง VLAN 10, 20, 30)
       |
    [Switch2]
        /    |    \
    VLAN10 VLAN20 VLAN30
```

### 4.2 802.1Q Trunking

**802.1Q คืออะไร:**

- มาตรฐาน IEEE สำหรับ VLAN tagging
- เพิ่ม 4-byte tag เข้าไปใน Ethernet frame
- รองรับ VLANs 1-4094
- Native VLAN สำหรับ untagged traffic

**802.1Q Frame Format:**

```
Normal Frame:
[Dest MAC][Src MAC][Type/Length][Data][FCS]
   6B        6B         2B        46-1500B  4B

Tagged Frame:
[Dest MAC][Src MAC][Tag][Type/Length][Data][FCS]
   6B        6B      4B      2B      46-1500B  4B

Tag Field (4 bytes):
[TPID: 0x8100][Priority][CFI][VLAN ID]
     2B           3bit    1bit   12bit
```

**ISL (Inter-Switch Link) - Legacy:**

```
- โปรโตคอลของ Cisco (proprietary)
- Encapsulation แทนการ insert tag
- เพิ่ม 26-byte header + 4-byte trailer
- ไม่ใช้แล้วใน modern switches
- 802.1Q เป็นมาตรฐาน
```

### 4.3 Configure Trunk

**Basic Trunk Configuration:**

```cisco
! เลือก interface
Switch(config)# interface fastethernet 0/1

! ตั้งค่าเป็น trunk mode
Switch(config-if)# switchport mode trunk

! กำหนด native VLAN (optional)
Switch(config-if)# switchport trunk native vlan 99

! กำหนด VLANs ที่อนุญาต (optional)
Switch(config-if)# switchport trunk allowed vlan 10,20,30,99

! เพิ่ม description
Switch(config-if)# description Trunk to Switch2
```

**Trunk Configuration - ทั้งสองฝั่ง:**

```cisco
! Switch 1
Switch1(config)# interface gi0/1
Switch1(config-if)# switchport mode trunk
Switch1(config-if)# switchport trunk native vlan 99
Switch1(config-if)# switchport trunk allowed vlan 10,20,30,99

! Switch 2
Switch2(config)# interface gi0/1
Switch2(config-if)# switchport mode trunk
Switch2(config-if)# switchport trunk native vlan 99
Switch2(config-if)# switchport trunk allowed vlan 10,20,30,99
```

### 4.4 Allowed VLANs on Trunk

**จัดการ Allowed VLANs:**

```cisco
! อนุญาตเฉพาะ VLANs ที่ระบุ
Switch(config-if)# switchport trunk allowed vlan 10,20,30

! อนุญาตทุก VLANs (default)
Switch(config-if)# switchport trunk allowed vlan all

! ไม่อนุญาต VLAN ใดเลย
Switch(config-if)# switchport trunk allowed vlan none

! เพิ่ม VLANs (add)
Switch(config-if)# switchport trunk allowed vlan add 40,50

! ลบ VLANs (remove)
Switch(config-if)# switchport trunk allowed vlan remove 10

! ยกเว้น VLANs บางตัว (except)
Switch(config-if)# switchport trunk allowed vlan except 1
```

**Best Practice:**

```cisco
! อนุญาตเฉพาะ VLANs ที่ใช้งานจริง
Switch(config-if)# switchport trunk allowed vlan 10,20,30,99

ข้อดี:
- ลด broadcast traffic บน trunk
- ป้องกัน VLAN hopping attacks
- ง่ายต่อการ troubleshoot
```

### 4.5 Verify Trunk Configuration

**คำสั่งตรวจสอบ Trunk:**

**1. Show Interfaces Trunk:**

```cisco
Switch# show interfaces trunk

Port        Mode             Encapsulation  Status        Native vlan
Fa0/1       on               802.1q         trunking      99

Port        Vlans allowed on trunk
Fa0/1       10,20,30,99

Port        Vlans allowed and active in management domain
Fa0/1       10,20,30,99

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       10,20,30,99
```

**2. Show Interface Switchport:**

```cisco
Switch# show interface fa0/1 switchport

Name: Fa0/1
Switchport: Enabled
Administrative Mode: trunk
Operational Mode: trunk
Administrative Trunking Encapsulation: dot1q
Operational Trunking Encapsulation: dot1q
Negotiation of Trunking: On
Access Mode VLAN: 1 (default)
Trunking Native Mode VLAN: 99 (VLAN0099)
Administrative Native VLAN tagging: enabled
Voice VLAN: none
Trunking VLANs Enabled: 10,20,30,99
```

**3. Show Running-Config:**

```cisco
Switch# show running-config interface fa0/1

interface FastEthernet0/1
 description Trunk to Switch2
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,99
 switchport mode trunk
```

### 4.6 Troubleshooting Trunks

**ปัญหาที่พบบ่อย:**

**1. Native VLAN Mismatch:**

```
Problem:
Switch1: Native VLAN 1
Switch2: Native VLAN 99

Symptoms:
- CDP warnings
- Traffic ถูกส่งผิด VLAN
- Connectivity issues

Solution:
ตั้ง native VLAN ให้เหมือนกันทั้งสองฝั่ง
```

**2. Trunk Mode Mismatch:**

```
Problem:
Switch1: mode trunk
Switch2: mode access

Symptoms:
- Trunk ไม่ขึ้น
- เห็นเป็น access port

Solution:
ตั้งทั้งสองฝั่งเป็น trunk mode
```

**3. Allowed VLANs:**

```
Problem:
VLAN 20 ไม่อยู่ใน allowed list

Symptoms:
- VLAN 20 ไม่สามารถข้าม trunk ได้

Solution:
เพิ่ม VLAN 20 ใน allowed list
Switch(config-if)# switchport trunk allowed vlan add 20
```

**4. Physical Issues:**

```
Problems:
- สายเคเบิลเสีย
- พอร์ตเสีย
- Duplex mismatch

Troubleshooting:
Switch# show interfaces fa0/1
Switch# show interfaces fa0/1 status
Switch# show interfaces fa0/1 | include error
```

---

## 5. Dynamic Trunking Protocol (DTP)

### โปรโตคอลการเจรจา Trunk อัตโนมัติ

### 5.1 DTP คืออะไร?

**Dynamic Trunking Protocol:**

- โปรโตคอลของ Cisco (proprietary)
- ใช้เจรจาการสร้าง trunk link อัตโนมัติ
- ทำงานระหว่าง Cisco switches เท่านั้น
- ส่ง DTP frames ทุก 30 วินาที

**ทำไมต้องมี DTP:**

- สะดวก: ไม่ต้อง configure trunk manual
- Flexible: adjust โหมดได้อัตโนมัติ
- Plug-and-play: ต่อแล้วใช้ได้เลย

**ข้อเสียของ DTP:**

- Security risk (VLAN hopping attacks)
- Best Practice: ปิด DTP และตั้งแบบ manual

### 5.2 Switchport Modes

**1. Access Mode:**

```cisco
Switch(config-if)# switchport mode access

- ทำงานเป็น access port เท่านั้น
- ไม่มีการเจรจา trunking
- ใช้กับ end devices
```

**2. Trunk Mode:**

```cisco
Switch(config-if)# switchport mode trunk

- ทำงานเป็น trunk port เท่านั้น
- ส่ง DTP frames บอกว่าเป็น trunk
- ถ้าอีกฝั่งเป็น trunk, dynamic desirable, หรือ dynamic auto
  → trunk ขึ้น
```

**3. Dynamic Desirable:**

```cisco
Switch(config-if)# switchport mode dynamic desirable

- พยายามสร้าง trunk
- ส่ง DTP frames actively
- จะเป็น trunk ถ้าอีกฝั่งเป็น:
  * trunk
  * dynamic desirable
  * dynamic auto
- Default บน older switches
```

**4. Dynamic Auto:**

```cisco
Switch(config-if)# switchport mode dynamic auto

- รอให้อีกฝั่งเจรจา
- ส่ง DTP frames passively
- จะเป็น trunk ถ้าอีกฝั่งเป็น:
  * trunk
  * dynamic desirable
- Default บน newer switches (2960, 3560)
```

### 5.3 DTP Negotiation Results

**ตาราง DTP Combinations:**

```
               │ Access │ Trunk  │ Dynamic│ Dynamic│
               │        │        │Desirable│ Auto  │
───────────────┼────────┼────────┼─────────┼────────┤
Access         │ Access │ Error  │ Access  │ Access │
Trunk          │ Error  │ Trunk  │ Trunk   │ Trunk  │
Dynamic        │ Access │ Trunk  │ Trunk   │ Access │
Desirable      │        │        │         │        │
Dynamic Auto   │ Access │ Trunk  │ Access  │ Access │
```

**อธิบายตาราง:**

```
Access + Access = Access (ไม่เจรจา)
Access + Trunk = ERROR (incompatible)
Access + Dynamic Desirable = Access
Access + Dynamic Auto = Access

Trunk + Trunk = Trunk (ทั้งคู่เป็น trunk)
Trunk + Dynamic Desirable = Trunk
Trunk + Dynamic Auto = Trunk

Dynamic Desirable + Dynamic Desirable = Trunk (เจรจากันได้)
Dynamic Desirable + Dynamic Auto = Access (ไม่มีฝั่งใดบังคับ trunk)

Dynamic Auto + Dynamic Auto = Access (ทั้งคู่รอกัน)
```

### 5.4 Disable DTP (Best Practice)

**ปิด DTP - Security Best Practice:**

```cisco
! Method 1: ตั้งเป็น Access Mode
Switch(config-if)# switchport mode access
Switch(config-if)# switchport nonegotiate

! Method 2: ตั้งเป็น Trunk Mode + Nonegotiate
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport nonegotiate
```

**ทำไมต้องปิด DTP:**

```
Security Risks:
1. VLAN Hopping Attacks
   - ผู้โจมตีเจรจาสร้าง trunk
   - เข้าถึง VLANs ที่ไม่ได้รับอนุญาต

2. Unwanted Trunk Formation
   - trunk ขึ้นโดยไม่ตั้งใจ
   - broadcast leaks

3. Configuration Errors
   - ยากต่อการ troubleshoot
   - ไม่แน่นอน

Best Practice:
- ตั้ง trunk แบบ manual
- ปิด DTP (nonegotiate)
- ชัดเจน, ควบคุมได้
```

### 5.5 DTP Configuration Examples

**ตัวอย่างที่ 1: Manual Trunk (Recommended)**

```cisco
! Switch 1
Switch1(config)# interface gi0/1
Switch1(config-if)# switchport mode trunk
Switch1(config-if)# switchport trunk native vlan 99
Switch1(config-if)# switchport trunk allowed vlan 10,20,30,99
Switch1(config-if)# switchport nonegotiate

! Switch 2
Switch2(config)# interface gi0/1
Switch2(config-if)# switchport mode trunk
Switch2(config-if)# switchport trunk native vlan 99
Switch2(config-if)# switchport trunk allowed vlan 10,20,30,99
Switch2(config-if)# switchport nonegotiate
```

**ตัวอย่างที่ 2: Dynamic Trunking (Not Recommended)**

```cisco
! Switch 1
Switch1(config)# interface fa0/1
Switch1(config-if)# switchport mode dynamic desirable

! Switch 2
Switch2(config)# interface fa0/1
Switch2(config-if)# switchport mode dynamic auto

! Result: Access (ไม่เป็น trunk)

! ถ้าต้องการ trunk:
Switch2(config)# interface fa0/1
Switch2(config-if)# switchport mode dynamic desirable
! หรือ
Switch2(config-if)# switchport mode trunk
```

**ตัวอย่างที่ 3: Access Port (End Devices)**

```cisco
Switch(config)# interface range fa0/2 - 24
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10
Switch(config-if-range)# switchport nonegotiate
Switch(config-if-range)# spanning-tree portfast
Switch(config-if-range)# spanning-tree bpduguard enable
```

### 5.6 Verify DTP

**คำสั่งตรวจสอบ DTP:**

```cisco
! ดู DTP status
Switch# show dtp interface fa0/1

DTP information for FastEthernet0/1:
  TOS/TAS/TNS:                        TRUNK/ON/TRUNK
  TOT/TAT/TNT:                        802.1Q/802.1Q/802.1Q
  Neighbor address 1:                 0CD9.96D2.3F01
  Neighbor address 2:                 000000000000
  Hello timer expiration (sec/state): never/STOPPED
  Access timer expiration (sec/state): never/STOPPED
  Negotiation timer expiration (sec/state): never/STOPPED

! ดู switchport status
Switch# show interface fa0/1 switchport

Name: Fa0/1
Switchport: Enabled
Administrative Mode: trunk
Operational Mode: trunk
Administrative Trunking Encapsulation: dot1q
Negotiation of Trunking: Off   ← nonegotiate
```

---

## สรุป

### สิ่งที่ได้เรียนรู้ใน Module 3:

✅ **VLAN Concepts:**

- ประโยชน์: Security, Performance, Cost Reduction
- ประเภท: Default, Data, Voice, Native, Management
- VLAN ID Ranges: 1-1005 (normal), 1006-4094 (extended)

✅ **VLAN Implementation:**

- สร้างและกำหนด VLANs
- กำหนดพอร์ตให้ VLANs
- Management VLAN configuration
- Voice VLAN สำหรับ IP Phones

✅ **VLAN Trunking:**

- 802.1Q frame tagging
- Native VLAN concepts
- Trunk configuration
- Allowed VLANs management

✅ **DTP (Dynamic Trunking Protocol):**

- Switchport modes: Access, Trunk, Dynamic
- DTP negotiation
- Best practice: Disable DTP
- Manual trunk configuration

✅ **Troubleshooting:**

- Native VLAN mismatch
- Trunk mode issues
- Allowed VLAN problems
- Verification commands

---

## คำสั่งสำคัญสรุป

### VLAN Configuration:

```cisco
! สร้าง VLAN
vlan <vlan-id>
 name <vlan-name>

! กำหนดพอร์ต
interface <interface>
 switchport mode access
 switchport access vlan <vlan-id>

! Management VLAN
interface vlan <vlan-id>
 ip address <ip> <mask>
 no shutdown
```

### Trunk Configuration:

```cisco
interface <interface>
 switchport mode trunk
 switchport trunk native vlan <vlan-id>
 switchport trunk allowed vlan <vlan-list>
 switchport nonegotiate
```

### Voice VLAN:

```cisco
interface <interface>
 switchport mode access
 switchport access vlan <data-vlan>
 switchport voice vlan <voice-vlan>
```

### Verification:

```cisco
show vlan brief
show vlan id <vlan-id>
show interfaces trunk
show interface <interface> switchport
show dtp interface <interface>
```

---

## Lab Activities

### Lab 3.1: Configure VLANs

**วัตถุประสงค์:**

- สร้าง VLANs หลายตัว
- กำหนดพอร์ตให้ VLANs
- Configure management VLAN
- Verify VLAN configuration

### Lab 3.2: Configure VLAN Trunks

**วัตถุประสงค์:**

- Configure trunk links
- Set native VLAN
- Manage allowed VLANs
- Verify trunk operation

### Lab 3.3: Troubleshoot VLANs

**วัตถุประสงค์:**

- ระบุปัญหา VLAN
- แก้ไข native VLAN mismatch
- แก้ไข trunk configuration
- Verify connectivity

---

## Packet Tracer Activities

### PT 3.2.1: Configure VLANs

**Tasks:**

1. สร้าง VLANs: 10, 20, 30, 99
2. ตั้งชื่อ VLANs
3. กำหนดพอร์ตให้ VLANs
4. Configure management IP
5. Test connectivity within VLANs

### PT 3.4.1: Configure Trunks

**Tasks:**

1. Configure trunk between switches
2. Set native VLAN to 99
3. Allow specific VLANs
4. Verify trunk operation
5. Test inter-switch VLAN connectivity

### PT 3.5.1: DTP Configuration

**Tasks:**

1. Configure different DTP modes
2. Observe trunk negotiation
3. Disable DTP (nonegotiate)
4. Document results

---

## คำถามทบทวน

1. VLAN คืออะไร? มีประโยชน์อย่างไร?
2. ประเภทของ VLANs มีอะไรบ้าง?
3. Native VLAN คืออะไร? ทำไมต้องตรงกันทั้งสองฝั่ง?
4. 802.1Q frame tagging ทำงานอย่างไร?
5. Trunk port และ Access port ต่างกันอย่างไร?
6. Voice VLAN ใช้ทำอะไร?
7. DTP modes มีอะไรบ้าง?
8. ทำไมควรปิด DTP?
9. จะ troubleshoot native VLAN mismatch ได้อย่างไร?
10. Extended range VLANs (1006-4094) ต่างจาก normal range อย่างไร?

---

## เฉลยคำถาม

1. **VLAN:** แบ่ง switch เป็นหลาย broadcast domains, ประโยชน์: security, performance, cost reduction
2. **ประเภท:** Default, Data, Voice, Native, Management, Black Hole
3. **Native VLAN:** สำหรับ untagged traffic บน trunk, ต้องตรงกันเพื่อป้องกัน traffic ถูกส่งผิด VLAN
4. **802.1Q:** เพิ่ม 4-byte tag (VLAN ID) เข้าไปใน Ethernet frame
5. **Trunk:** ส่งหลาย VLANs (tagged), **Access:** ส่ง VLAN เดียว (untagged)
6. **Voice VLAN:** แยก VoIP traffic จาก data, QoS priority, low latency
7. **DTP Modes:** access, trunk, dynamic desirable, dynamic auto
8. **ปิด DTP:** ป้องกัน VLAN hopping, ควบคุมได้ดีกว่า, security best practice
9. **Troubleshoot:** `show interfaces trunk`, ตรวจสอบ native VLAN ทั้งสองฝั่ง, ตั้งให้เหมือนกัน
10. **Extended VLANs:** 1006-4094, ไม่เก็บใน vlan.dat, ต้องเป็น VTP transparent, ใช้กับ large networks

---

## Best Practices Summary

### VLAN Design:

```
✅ ใช้ VLANs แยกตามแผนก/หน้าที่
✅ เปลี่ยน native VLAN จาก VLAN 1
✅ ใช้ Management VLAN แยกจาก user VLANs
✅ ตั้งชื่อ VLANs ให้ชัดเจน
✅ ใช้ voice VLAN สำหรับ IP Phones
```

### Trunk Configuration:

```
✅ ตั้ง trunk mode manual (ไม่ใช้ dynamic)
✅ ใช้ switchport nonegotiate
✅ จำกัด allowed VLANs (ไม่ใช้ all)
✅ Native VLAN ต้องตรงกันทั้งสองฝั่ง
✅ เพิ่ม description บน trunk ports
```

### Security:

```
✅ ปิด DTP
✅ Shutdown unused ports
✅ ใส่ unused ports ใน black hole VLAN
✅ ไม่ใช้ VLAN 1 สำหรับ user traffic
✅ แยก management VLAN
```

---

## Common Mistakes

❌ **ใช้ VLAN 1 สำหรับทุกอย่าง** ✅ แยก VLANs ตามหน้าที่

❌ **Native VLAN ไม่ตรงกัน** ✅ ตรวจสอบและตั้งให้เหมือนกัน

❌ **ใช้ dynamic trunking** ✅ ตั้ง manual + nonegotiate

❌ **Allow all VLANs บน trunk** ✅ จำกัดเฉพาะ VLANs ที่ใช้

❌ **ลืม configure voice VLAN** ✅ แยก voice และ data traffic

---

**หมายเหตุ:** VLANs เป็นพื้นฐานสำคัญของ switching ความเข้าใจในหัวข้อนี้จำเป็นสำหรับ Module ถัดไป (Inter-VLAN Routing) และการออกแบบ network ในชีวิตจริง

---

**เอกสารจัดทำโดย:** Claude (Anthropic AI)  
**Module:** CCNA 2 Module 3 - VLANs  
**Version:** v7.02 SRWE  
**วันที่อัพเดท:** พฤศจิกายน 2025