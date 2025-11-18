# CCNA Course 3 - Module 2: Single-Area OSPFv2 Configuration

## การกำหนดค่า OSPF แบบ Single-Area

---

## 2.1 OSPF Router ID

### Router ID Overview

**คำจำกัดความ:**

- **Router ID** = ตัวระบุ router ใน OSPF domain
- รูปแบบ **32-bit** (เหมือน IPv4 address)
- **ต้องไม่ซ้ำกัน** ใน OSPF domain
- ใช้ใน DR/BDR election และระบุ LSAs

**ความสำคัญ:**

```
✅ ระบุ router uniquely
✅ ใช้ใน DR/BDR election (Router ID สูงกว่า = ดีกว่า)
✅ ปรากฏใน LSAs (Advertising Router field)
✅ แสดงใน OSPF neighbor table
```

---

### Router ID Selection Process

**OSPF เลือก Router ID ตามลำดับความสำคัญ:**

#### 1. Manual Configuration (แนะนำที่สุด)

**ข้อดี:**

```
✅ ควบคุมได้เอง
✅ ไม่เปลี่ยนถ้า interface down/up
✅ ชัดเจน easy to troubleshoot
```

**Configuration:**

```
Router(config)# router ospf <process-id>
Router(config-router)# router-id <router-id>
```

**ตัวอย่าง:**

```
R1(config)# router ospf 1
R1(config-router)# router-id 1.1.1.1
```

**หมายเหตุ:**

```
- Router ID ไม่จำเป็นต้องเป็น IP address ที่มีใน router
- แต่ควรใช้รูปแบบ IPv4 (x.x.x.x)
- Best practice: ใช้ pattern เช่น 1.1.1.1, 2.2.2.2, 3.3.3.3
```

#### 2. Highest Loopback IP Address

**ถ้าไม่ configure manual:**

- OSPF เลือก **IP address สูงสุด** จาก loopback interfaces
- Loopback ต้อง **up/up**

**ตัวอย่าง:**

```
R1 Loopback interfaces:
  Loopback 0: 10.0.0.1/32 (up/up)
  Loopback 1: 192.168.1.1/32 (up/up)
  Loopback 2: 172.16.1.1/32 (up/up)

Router ID = 192.168.1.1 (highest)
```

**ข้อดี:**

```
✅ Loopback interfaces ไม่ down (stable)
✅ ไม่ต้อง configure manual router-id
```

**Best Practice:**

```
สร้าง Loopback 0 สำหรับ Router ID:

R1(config)# interface loopback 0
R1(config-if)# ip address 1.1.1.1 255.255.255.255
R1(config-if)# description Router ID
```

#### 3. Highest Active Physical Interface IP

**ถ้าไม่มี manual และไม่มี loopback:**

- OSPF เลือก **IP address สูงสุด** จาก physical interfaces
- Interface ต้อง **up/up**

**ตัวอย่าง:**

```
R1 Physical interfaces:
  GigabitEthernet0/0: 192.168.1.1/24 (up/up)
  GigabitEthernet0/1: 10.1.1.1/24 (up/up)
  Serial0/0/0: 172.16.1.1/30 (down/down) ← ไม่นับ

Router ID = 192.168.1.1 (highest active)
```

**ข้อเสีย:**

```
❌ Interface down → Router ID อาจเปลี่ยน
❌ OSPF process restart → neighbor relationships ขาด
❌ DR/BDR election อาจเปลี่ยน
```

---

### Router ID Configuration Examples

**Example 1: Manual Configuration**

```
R1(config)# router ospf 10
R1(config-router)# router-id 1.1.1.1
R1(config-router)# end

R1# show ip protocols
*** IP Routing is NSF aware ***

Routing Protocol is "ospf 10"
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Router ID 1.1.1.1
  ...
```

**Example 2: Using Loopback**

```
R2(config)# interface loopback 0
R2(config-if)# ip address 2.2.2.2 255.255.255.255
R2(config-if)# exit

R2(config)# router ospf 10
R2(config-router)# end

R2# show ip protocols
  Router ID 2.2.2.2
```

**Example 3: Physical Interface (Not Recommended)**

```
R3:
  Gi0/0: 10.1.1.1/24 (up/up)
  Gi0/1: 192.168.1.1/24 (up/up)

R3(config)# router ospf 10
R3(config-router)# end

R3# show ip protocols
  Router ID 192.168.1.1
```

---

### Changing Router ID

**เปลี่ยน Router ID:**

```
Router(config-router)# router-id <new-router-id>

หลังจากนั้นต้อง reload OSPF process:
```

**Method 1: Clear OSPF Process (แนะนำ)**

```
Router# clear ip ospf process
Reset ALL OSPF processes? [no]: yes
```

**Method 2: Reload Router**

```
Router# reload
```

**ตัวอย่าง:**

```
R1(config)# router ospf 10
R1(config-router)# router-id 1.1.1.100

R1# show ip protocols
  Router ID 1.1.1.1  ← ยังเป็นค่าเดิม

R1# clear ip ospf process
Reset ALL OSPF processes? [no]: yes

R1# show ip protocols
  Router ID 1.1.1.100  ← เปลี่ยนแล้ว
```

**ผลกระทบ:**

```
❌ Neighbor relationships drop
❌ OSPF reconverges
❌ อาจมี brief packet loss
❌ DR/BDR re-election (ถ้าเป็น broadcast network)
```

**Best Practice:**

```
⚠️ เปลี่ยน Router ID ในช่วง maintenance window
⚠️ Coordinate กับ team
⚠️ Backup configuration ก่อน
```

---

### Verifying Router ID

**Commands:**

#### show ip protocols

```
R1# show ip protocols
*** IP Routing is NSF aware ***

Routing Protocol is "ospf 10"
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Router ID 1.1.1.1
  Number of areas in this router is 1. 1 normal 0 stub 0 nssa
  ...
```

#### show ip ospf

```
R1# show ip ospf
 Routing Process "ospf 10" with ID 1.1.1.1
 Start time: 00:00:01.234, Time elapsed: 00:05:23.456
 Supports only single TOS(TOS0) routes
 Supports opaque LSA
 Supports Link-local Signaling (LLS)
 ...
```

#### show ip ospf neighbor

```
R1# show ip ospf neighbor

Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           1   FULL/DR         00:00:35    10.1.1.2        Gi0/0
3.3.3.3           1   FULL/BDR        00:00:38    10.1.1.3        Gi0/0
```

---

## 2.2 Enable OSPF on Interfaces

### OSPF Network Command

**คำจำกัดความ:**

- ใช้ **network command** enable OSPF บน interfaces
- Format: `network <network-address> <wildcard-mask> area <area-id>`

**Syntax:**

```
Router(config-router)# network <network-address> <wildcard-mask> area <area-id>
```

**พารามิเตอร์:**

**network-address:**

```
- Network address ที่ต้องการ enable OSPF
- ใช้ wildcard mask ระบุ range
```

**wildcard-mask:**

```
- Inverse ของ subnet mask
- 0 = must match, 1 = don't care
```

**area:**

```
- Area ID ที่ interface นี้อยู่
- Single-area: ใช้ area 0
```

---

### Wildcard Mask Calculation

**คำจำกัดความ:**

- Wildcard mask = **Inverse** ของ subnet mask
- การคำนวณ: **255.255.255.255 - Subnet Mask**

**ตัวอย่าง:**

```
Subnet Mask        Wildcard Mask
------------------------------------------------------------------------
255.255.255.255    0.0.0.0          (host - exact match)
255.255.255.252    0.0.0.3          (/30)
255.255.255.0      0.0.0.255        (/24)
255.255.0.0        0.0.255.255      (/16)
255.0.0.0          0.255.255.255    (/8)
0.0.0.0            255.255.255.255  (any)
------------------------------------------------------------------------
```

**วิธีคำนวณ:**

```
Subnet Mask: 255.255.255.252

255.255.255.255
-   255.255.255.252
-------------------
  0.  0.  0.  3

Wildcard: 0.0.0.3
```

**ตัวอย่างแบบ binary:**

```
Subnet /30: 255.255.255.252
Binary: 11111111.11111111.11111111.11111100

Wildcard:
Binary: 00000000.00000000.00000000.00000011
Decimal: 0.0.0.3
```

---

### Network Command Examples

**Example 1: Enable OSPF on Specific Interface**

**Scenario:**

```
R1 Gi0/0: 192.168.1.1/24
ต้องการ enable OSPF บน Gi0/0 เท่านั้น
```

**Configuration:**

```
R1(config)# router ospf 10
R1(config-router)# network 192.168.1.0 0.0.0.255 area 0

หรือ match exact IP:
R1(config-router)# network 192.168.1.1 0.0.0.0 area 0
```

**Example 2: Enable OSPF on Multiple Interfaces**

**Scenario:**

```
R1:
  Gi0/0: 192.168.1.1/24
  Gi0/1: 192.168.2.1/24
  Serial0/0/0: 10.1.1.1/30
```

**Method 1: Individual Networks**

```
R1(config)# router ospf 10
R1(config-router)# network 192.168.1.0 0.0.0.255 area 0
R1(config-router)# network 192.168.2.0 0.0.0.255 area 0
R1(config-router)# network 10.1.1.0 0.0.0.3 area 0
```

**Method 2: Match All Interfaces**

```
R1(config)# router ospf 10
R1(config-router)# network 0.0.0.0 255.255.255.255 area 0

⚠️ Enable OSPF บนทุก interfaces (ระวัง!)
```

**Method 3: Host-Specific**

```
R1(config)# router ospf 10
R1(config-router)# network 192.168.1.1 0.0.0.0 area 0
R1(config-router)# network 192.168.2.1 0.0.0.0 area 0
R1(config-router)# network 10.1.1.1 0.0.0.0 area 0

✅ แนะนำ: ชัดเจน, ควบคุมได้ดี
```

**Example 3: Enable OSPF on Subnet Range**

**Scenario:**

```
Enable OSPF บน interfaces ที่อยู่ใน 192.168.0.0/16
```

**Configuration:**

```
R1(config)# router ospf 10
R1(config-router)# network 192.168.0.0 0.0.255.255 area 0
```

---

### Network Command Matching Process

**OSPF ตรวจสอบ interfaces:**

1. อ่าน network command ตามลำดับจากบนลงล่าง
2. เปรียบเทียบ IP address ของแต่ละ interface กับ network statement
3. ถ้า match → enable OSPF บน interface นั้น
4. หยุด (ไม่ตรวจสอบ network statements ถัดไป)

**ตัวอย่าง:**

```
R1:
  Gi0/0: 10.1.1.1/24
  Gi0/1: 10.1.2.1/24

R1(config-router)# network 10.1.1.0 0.0.0.255 area 0
R1(config-router)# network 10.0.0.0 0.255.255.255 area 0

Gi0/0:
  - Match network 10.1.1.0 0.0.0.255? YES → Enable OSPF, stop checking

Gi0/1:
  - Match network 10.1.1.0 0.0.0.255? NO
  - Match network 10.0.0.0 0.255.255.255? YES → Enable OSPF
```

**Order Matters:**

```
Configuration 1:
  network 10.1.1.0 0.0.0.255 area 0
  network 10.0.0.0 0.255.255.255 area 1

  Gi0/0 (10.1.1.1) → Area 0

Configuration 2:
  network 10.0.0.0 0.255.255.255 area 1
  network 10.1.1.0 0.0.0.255 area 0

  Gi0/0 (10.1.1.1) → Area 1 (match first statement)
```

**Best Practice:**

```
✅ เรียง network statements จาก specific → general
✅ ใช้ host-specific wildcard (0.0.0.0)
```

---

### Passive Interface

**คำจำกัดความ:**

- **Passive interface** = Interface ที่ advertise network แต่**ไม่ส่ง/รับ OSPF packets**
- ใช้สำหรับ interfaces ที่เชื่อมกับ end users (ไม่มี OSPF neighbors)

**ข้อดี:**

```
✅ ประหยัด bandwidth (ไม่ส่ง Hello packets)
✅ ประหยัด CPU (ไม่ process OSPF packets)
✅ Security (ไม่ expose OSPF ไปยัง users)
```

**Configuration:**

```
Router(config-router)# passive-interface <interface-id>

ตัวอย่าง:
R1(config-router)# passive-interface gigabitEthernet 0/0
```

**Set All Interfaces Passive (แล้วเปิดที่ต้องการ):**

```
Router(config-router)# passive-interface default
Router(config-router)# no passive-interface <interface-id>

ตัวอย่าง:
R1(config-router)# passive-interface default
R1(config-router)# no passive-interface serial 0/0/0
R1(config-router)# no passive-interface serial 0/0/1

✅ แนะนำ: ปลอดภัยกว่า (default deny)
```

**Example:**

**Scenario:**

```
R1:
  Gi0/0: 192.168.1.0/24 (LAN - users)
  Gi0/1: 192.168.2.0/24 (LAN - users)
  Serial0/0/0: 10.1.1.0/30 (WAN - ต่อ R2)
  Serial0/0/1: 10.1.2.0/30 (WAN - ต่อ R3)

ต้องการ:
  - Advertise ทุก networks
  - แต่ OSPF neighbors เฉพาะ Serial interfaces
```

**Configuration:**

```
R1(config)# router ospf 10
R1(config-router)# network 0.0.0.0 255.255.255.255 area 0
R1(config-router)# passive-interface gigabitEthernet 0/0
R1(config-router)# passive-interface gigabitEthernet 0/1

หรือ:

R1(config-router)# passive-interface default
R1(config-router)# no passive-interface serial 0/0/0
R1(config-router)# no passive-interface serial 0/0/1
```

**Verification:**

```
R1# show ip protocols
Routing Protocol is "ospf 10"
  ...
  Passive Interface(s):
    GigabitEthernet0/0
    GigabitEthernet0/1
  Routing for Networks:
    0.0.0.0 255.255.255.255 area 0
  ...
```

---

### OSPF Process ID

**คำจำกัดความ:**

- **Process ID** = ตัวเลขระบุ OSPF process บน router
- Range: **1-65535**
- **Locally significant** (แต่ละ router ใช้ค่าต่างกันได้)

**ไม่จำเป็นต้องเหมือนกัน:**

```
R1: router ospf 10
R2: router ospf 20
R3: router ospf 100

→ ยังสามารถเป็น neighbors ได้ (Process ID ไม่เกี่ยวกับ neighbor formation)
```

**ต้องเหมือนกัน (Neighbor requirements):**

```
✅ Area ID
✅ Subnet/Mask
✅ Hello/Dead timers
✅ Authentication (ถ้ามี)

❌ Process ID ไม่ต้องเหมือนกัน
```

**ใช้เมื่อไหร่:**

```
Multiple OSPF processes บน router เดียว:

R1(config)# router ospf 10
R1(config-router)# network 10.0.0.0 0.255.255.255 area 0

R1(config)# router ospf 20
R1(config-router)# network 192.168.0.0 0.0.255.255 area 0

→ แยก OSPF domains (แต่ใช้น้อย)
```

**Best Practice:**

```
✅ ใช้ Process ID เดียวกันทุก routers (ง่ายต่อการจัดการ)
✅ เช่น: ทุก routers ใช้ process 1 หรือ 10
```

---

## 2.3 OSPF Configuration Example

### Sample Topology

```
        Area 0
        
[PC1]     [PC2]     [PC3]
  |         |         |
192.168.1.0/24    192.168.2.0/24    192.168.3.0/24
  |         |         |
[R1]------[R2]------[R3]
   10.1.12.0/30  10.2.23.0/30
```

**Network Information:**

```
R1:
  Gi0/0: 192.168.1.1/24 (LAN)
  Serial0/0/0: 10.1.12.1/30 (WAN to R2)
  Loopback 0: 1.1.1.1/32 (Router ID)

R2:
  Gi0/0: 192.168.2.1/24 (LAN)
  Serial0/0/0: 10.1.12.2/30 (WAN to R1)
  Serial0/0/1: 10.2.23.1/30 (WAN to R3)
  Loopback 0: 2.2.2.2/32 (Router ID)

R3:
  Gi0/0: 192.168.3.1/24 (LAN)
  Serial0/0/1: 10.2.23.2/30 (WAN to R2)
  Loopback 0: 3.3.3.3/32 (Router ID)
```

---

### Step-by-Step Configuration

#### Router R1 Configuration

```
! Configure hostname
R1(config)# hostname R1

! Configure Loopback 0 (Router ID)
R1(config)# interface loopback 0
R1(config-if)# ip address 1.1.1.1 255.255.255.255
R1(config-if)# description Router ID
R1(config-if)# exit

! Configure GigabitEthernet 0/0 (LAN)
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# description LAN
R1(config-if)# no shutdown
R1(config-if)# exit

! Configure Serial 0/0/0 (WAN to R2)
R1(config)# interface serial 0/0/0
R1(config-if)# ip address 10.1.12.1 255.255.255.252
R1(config-if)# description Link to R2
R1(config-if)# clock rate 128000
R1(config-if)# no shutdown
R1(config-if)# exit

! Configure OSPF
R1(config)# router ospf 10
R1(config-router)# router-id 1.1.1.1
R1(config-router)# network 192.168.1.0 0.0.0.255 area 0
R1(config-router)# network 10.1.12.0 0.0.0.3 area 0
R1(config-router)# passive-interface gigabitEthernet 0/0
R1(config-router)# end

! Save configuration
R1# copy running-config startup-config
```

#### Router R2 Configuration

```
! Configure hostname
R2(config)# hostname R2

! Configure Loopback 0 (Router ID)
R2(config)# interface loopback 0
R2(config-if)# ip address 2.2.2.2 255.255.255.255
R2(config-if)# exit

! Configure GigabitEthernet 0/0 (LAN)
R2(config)# interface gigabitEthernet 0/0
R2(config-if)# ip address 192.168.2.1 255.255.255.0
R2(config-if)# no shutdown
R2(config-if)# exit

! Configure Serial 0/0/0 (WAN to R1)
R2(config)# interface serial 0/0/0
R2(config-if)# ip address 10.1.12.2 255.255.255.252
R2(config-if)# no shutdown
R2(config-if)# exit

! Configure Serial 0/0/1 (WAN to R3)
R2(config)# interface serial 0/0/1
R2(config-if)# ip address 10.2.23.1 255.255.255.252
R2(config-if)# clock rate 128000
R2(config-if)# no shutdown
R2(config-if)# exit

! Configure OSPF
R2(config)# router ospf 10
R2(config-router)# router-id 2.2.2.2
R2(config-router)# network 192.168.2.0 0.0.0.255 area 0
R2(config-router)# network 10.1.12.0 0.0.0.3 area 0
R2(config-router)# network 10.2.23.0 0.0.0.3 area 0
R2(config-router)# passive-interface gigabitEthernet 0/0
R2(config-router)# end

R2# copy running-config startup-config
```

#### Router R3 Configuration

```
! Configure hostname
R3(config)# hostname R3

! Configure Loopback 0 (Router ID)
R3(config)# interface loopback 0
R3(config-if)# ip address 3.3.3.3 255.255.255.255
R3(config-if)# exit

! Configure GigabitEthernet 0/0 (LAN)
R3(config)# interface gigabitEthernet 0/0
R3(config-if)# ip address 192.168.3.1 255.255.255.0
R3(config-if)# no shutdown
R3(config-if)# exit

! Configure Serial 0/0/1 (WAN to R2)
R3(config)# interface serial 0/0/1
R3(config-if)# ip address 10.2.23.2 255.255.255.252
R3(config-if)# no shutdown
R3(config-if)# exit

! Configure OSPF
R3(config)# router ospf 10
R3(config-router)# router-id 3.3.3.3
R3(config-router)# network 192.168.3.0 0.0.0.255 area 0
R3(config-router)# network 10.2.23.0 0.0.0.3 area 0
R3(config-router)# passive-interface gigabitEthernet 0/0
R3(config-router)# end

R3# copy running-config startup-config
```

---

## 2.4 Verify OSPF Configuration

### Verification Commands

#### show ip ospf neighbor

**คำจำกัดความ:**

- แสดง OSPF neighbors
- แสดง neighbor state, priority, DR/BDR

**Output:**

```
R1# show ip ospf neighbor

Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           0   FULL/  -        00:00:35    10.1.12.2       Serial0/0/0
```

**Columns:**

```
Neighbor ID:  Router ID ของ neighbor
Pri:          OSPF priority (0-255)
State:        OSPF state (FULL = fully adjacent)
Dead Time:    เวลาที่เหลือก่อนประกาศ neighbor down
Address:      IP address ของ neighbor
Interface:    Local interface ที่เชื่อมกับ neighbor
```

**States:**

```
FULL/  -      Point-to-point (no DR/BDR)
FULL/DR       Neighbor คือ DR
FULL/BDR      Neighbor คือ BDR
FULL/DROTHER  Neighbor คือ DROther
2WAY/DROTHER  DROther (normal บน broadcast)
```

#### show ip protocols

**คำจำกัดความ:**

- แสดง routing protocols ทั้งหมด
- แสดง OSPF configuration summary

**Output:**

```
R1# show ip protocols
*** IP Routing is NSF aware ***

Routing Protocol is "ospf 10"
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Router ID 1.1.1.1
  Number of areas in this router is 1. 1 normal 0 stub 0 nssa
  Maximum path: 4
  Routing for Networks:
    192.168.1.0 0.0.0.255 area 0
    10.1.12.0 0.0.0.3 area 0
  Passive Interface(s):
    GigabitEthernet0/0
  Routing Information Sources:
    Gateway         Distance      Last Update
    2.2.2.2              110      00:05:23
  Distance: (default is 110)
```

**ข้อมูลสำคัญ:**

```
- Router ID
- Areas
- Network statements
- Passive interfaces
- Neighbors (Gateway)
- Administrative Distance
```

#### show ip ospf

**คำจำกัดความ:**

- แสดงรายละเอียด OSPF process
- แสดง timers, areas, SPF statistics

**Output:**

```
R1# show ip ospf
 Routing Process "ospf 10" with ID 1.1.1.1
 Start time: 00:00:12.345, Time elapsed: 00:15:43.678
 Supports only single TOS(TOS0) routes
 Supports opaque LSA
 Supports Link-local Signaling (LLS)
 Supports area transit capability
 Supports NSSA (compatible with RFC 3101)
 Event-log enabled, Maximum number of events: 1000, Mode: cyclic
 Router is not originating router-LSAs with maximum metric
 Initial SPF schedule delay 5000 msecs
 Minimum hold time between two consecutive SPFs 10000 msecs
 Maximum wait time between two consecutive SPFs 10000 msecs
 Incremental-SPF disabled
 Minimum LSA interval 5 secs
 Minimum LSA arrival 1000 msecs
 LSA group pacing timer 240 secs
 Interface flood pacing timer 33 msecs
 Retransmission pacing timer 66 msecs
 Number of external LSA 0. Checksum Sum 0x000000
 Number of opaque AS LSA 0. Checksum Sum 0x000000
 Number of DCbitless external and opaque AS LSA 0
 Number of DoNotAge external and opaque AS LSA 0
 Number of areas in this router is 1. 1 normal 0 stub 0 nssa
 Number of areas transit capable is 0
 External flood list length 0
 IETF NSF helper support enabled
 Cisco NSF helper support enabled
 Reference bandwidth unit is 100 mbps
    Area BACKBONE(0)
        Number of interfaces in this area is 2
        Area has no authentication
        SPF algorithm last executed 00:05:12.345 ago
        SPF algorithm executed 3 times
        Area ranges are
        Number of LSA 3. Checksum Sum 0x01ABCD
        Number of opaque link LSA 0. Checksum Sum 0x000000
        Number of DCbitless LSA 0
        Number of indication LSA 0
        Number of DoNotAge LSA 0
        Flood list length 0
```

**ข้อมูลสำคัญ:**

```
- Router ID
- Process ID
- Uptime
- Reference bandwidth
- Areas
- SPF statistics
- LSA counts
```

#### show ip ospf interface

**คำจำกัดความ:**

- แสดงรายละเอียด OSPF interfaces
- แสดง timers, cost, DR/BDR, network type

**Output:**

```
R1# show ip ospf interface

GigabitEthernet0/0 is up, line protocol is up
  Internet Address 192.168.1.1/24, Area 0, Attached via Network Statement
  Process ID 10, Router ID 1.1.1.1, Network Type BROADCAST, Cost: 1
  Topology-MTID    Cost    Disabled    Shutdown      Topology Name
        0           1         no          no            Base
  Transmit Delay is 1 sec, State DR, Priority 1
  Designated Router (ID) 1.1.1.1, Interface address 192.168.1.1
  No backup designated router on this network
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    oob-resync timeout 40
    No Hellos (Passive interface)
  Supports Link-local Signaling (LLS)
  ...

Serial0/0/0 is up, line protocol is up
  Internet Address 10.1.12.1/30, Area 0, Attached via Network Statement
  Process ID 10, Router ID 1.1.1.1, Network Type POINT_TO_POINT, Cost: 64
  Topology-MTID    Cost    Disabled    Shutdown      Topology Name
        0           64        no          no            Base
  Transmit Delay is 1 sec, State POINT_TO_POINT
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    oob-resync timeout 40
    Hello due in 00:00:03
  Supports Link-local Signaling (LLS)
  ...
```

**ข้อมูลสำคัญ:**

```
- Interface status
- IP address
- Area
- Network Type (BROADCAST, POINT_TO_POINT)
- Cost
- DR/BDR (ถ้ามี)
- Priority
- Timers (Hello, Dead)
- Passive interface status
```

**Show Specific Interface:**

```
R1# show ip ospf interface serial 0/0/0
```

**Show Brief:**

```
R1# show ip ospf interface brief

Interface    PID   Area            IP Address/Mask    Cost  State Nbrs F/C
Gi0/0        10    0               192.168.1.1/24     1     DR    0/0
Se0/0/0      10    0               10.1.12.1/30       64    P2P   1/1
```

#### show ip route ospf

**คำจำกัดความ:**

- แสดง OSPF routes ใน routing table
- แสดง learned routes จาก OSPF

**Output:**

```
R1# show ip route ospf
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       + - replicated route, % - next hop override

Gateway of last resort is not set

      10.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
O        10.2.23.0/30 [110/128] via 10.1.12.2, 00:05:23, Serial0/0/0
O     192.168.2.0/24 [110/65] via 10.1.12.2, 00:05:23, Serial0/0/0
O     192.168.3.0/24 [110/129] via 10.1.12.2, 00:05:23, Serial0/0/0
```

**Legend:**

```
O     = OSPF intra-area route
IA    = OSPF inter-area route
E1/E2 = OSPF external routes
N1/N2 = OSPF NSSA external routes

Format:
O     <network> [AD/metric] via <next-hop>, <time>, <interface>

ตัวอย่าง:
O     192.168.2.0/24 [110/65] via 10.1.12.2, 00:05:23, Serial0/0/0
      ↑                ↑   ↑        ↑             ↑            ↑
   Network            AD  Cost   Next-hop      Time      Interface
```

#### show ip route

**คำจำกัดความ:**

- แสดง routing table ทั้งหมด

**Output:**

```
R1# show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       ...

Gateway of last resort is not set

      1.0.0.0/32 is subnetted, 1 subnets
C        1.1.1.1 is directly connected, Loopback0
      10.0.0.0/8 is variably subnetted, 4 subnets, 2 masks
C        10.1.12.0/30 is directly connected, Serial0/0/0
L        10.1.12.1/32 is directly connected, Serial0/0/0
O        10.2.23.0/30 [110/128] via 10.1.12.2, 00:05:23, Serial0/0/0
      192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.1.0/24 is directly connected, GigabitEthernet0/0
L        192.168.1.1/32 is directly connected, GigabitEthernet0/0
O     192.168.2.0/24 [110/65] via 10.1.12.2, 00:05:23, Serial0/0/0
O     192.168.3.0/24 [110/129] via 10.1.12.2, 00:05:23, Serial0/0/0
```

**Route Codes:**

```
C = Directly connected
L = Local (interface IP)
O = OSPF
S = Static
```

---

### Verification Workflow

**1. ตรวจสอบ OSPF process running:**

```
R1# show ip protocols | include ospf
Routing Protocol is "ospf 10"
```

**2. ตรวจสอบ Router ID:**

```
R1# show ip protocols | include Router ID
  Router ID 1.1.1.1
```

**3. ตรวจสอบ OSPF-enabled interfaces:**

```
R1# show ip ospf interface brief
```

**4. ตรวจสอบ neighbors:**

```
R1# show ip ospf neighbor
```

**5. ตรวจสอบ OSPF routes:**

```
R1# show ip route ospf
```

**6. ทดสอบ connectivity:**

```
R1# ping 192.168.2.1
R1# ping 192.168.3.1
```

---

## 2.5 OSPF Cost Modification

### Default OSPF Cost

**คำนวณ:**

```
Cost = Reference Bandwidth / Interface Bandwidth

Default Reference Bandwidth = 100 Mbps
```

**ตาราง Default Cost:**

```
Interface           Bandwidth       Cost
------------------------------------------------------------------------
Serial              64 Kbps         1562
T1 (1.544 Mbps)     1.544 Mbps      64
Ethernet            10 Mbps         10
Fast Ethernet       100 Mbps        1
Gigabit Ethernet    1 Gbps          1
10 Gig Ethernet     10 Gbps         1
------------------------------------------------------------------------
```

**ปัญหา:**

```
Fast Ethernet, Gigabit Ethernet, 10 Gig → Cost เท่ากัน (1)
→ OSPF ไม่แยก high-speed links
```

---

### Modify Reference Bandwidth

**คำจำกัดความ:**

- เปลี่ยน **Reference Bandwidth** ให้เหมาะสมกับ network
- ต้อง configure **ทุก routers** ใน OSPF domain

**Syntax:**

```
Router(config-router)# auto-cost reference-bandwidth <mbps>
```

**ตัวอย่าง:**

```
R1(config)# router ospf 10
R1(config-router)# auto-cost reference-bandwidth 10000

% OSPF: Reference bandwidth is changed.
        Please ensure reference bandwidth is consistent across all routers.
```

**คำนวณ Cost ใหม่:**

```
Reference Bandwidth = 10,000 Mbps (10 Gbps)

Interface           Bandwidth       New Cost
------------------------------------------------------------------------
Fast Ethernet       100 Mbps        10,000/100 = 100
Gigabit Ethernet    1 Gbps          10,000/1,000 = 10
10 Gig Ethernet     10 Gbps         10,000/10,000 = 1
------------------------------------------------------------------------
```

**Best Practice:**

```
✅ ใช้ reference bandwidth สูงกว่า fastest link ใน network
✅ เช่น: ถ้ามี 10 Gig → ใช้ 10000 หรือ 100000
✅ Configure ทุก routers เหมือนกัน
```

**Verify:**

```
R1# show ip ospf interface gigabitEthernet 0/0
...
  Process ID 10, Router ID 1.1.1.1, Network Type BROADCAST, Cost: 10
...
```

---

### Modify Interface Cost Manually

**คำจำกัดความ:**

- ตั้ง cost ของ interface เฉพาะเจาะจง
- Override automatic cost calculation

**Syntax:**

```
Router(config-if)# ip ospf cost <cost-value>
```

**Range:** 1-65535

**ตัวอย่าง:**

```
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip ospf cost 50
R1(config-if)# end

R1# show ip ospf interface gigabitEthernet 0/0
...
  Process ID 10, Router ID 1.1.1.1, Network Type BROADCAST, Cost: 50
...
```

**Use Cases:**

**1. Path manipulation:**

```
Topology:
R1 ---- (Gi0/0, Cost 1) ---- R2
 |                            |
 +--- (Serial, Cost 64) ------+

ต้องการ: Force traffic ผ่าน Serial link

Solution:
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip ospf cost 100
```

**2. Load balancing:**

```
R1 มี 2 paths ไป R3:
  Path 1: R1 → R2 → R3 (Cost 10)
  Path 2: R1 → R4 → R3 (Cost 20)

ปรับให้ load balance:
ลด cost ของ Path 2 เป็น 10
```

---

### Modify Bandwidth

**อีกวิธี: เปลี่ยน bandwidth ของ interface**

**Syntax:**

```
Router(config-if)# bandwidth <kilobits>
```

**ตัวอย่าง:**

```
R1(config)# interface serial 0/0/0
R1(config-if)# bandwidth 128

R1# show ip ospf interface serial 0/0/0
...
  Cost: 781 (calculated from bandwidth 128)
...
```

**หมายเหตุ:**

```
⚠️ bandwidth command ไม่เปลี่ยนความเร็วจริงของ interface
⚠️ มีผลกับ routing protocols เท่านั้น (OSPF, EIGRP)
⚠️ การเปลี่ยนความเร็วจริงใช้ clock rate (serial) หรือ speed (Ethernet)
```

**เปรียบเทียบ:**

```
Method                          Effect
------------------------------------------------------------------------
auto-cost reference-bandwidth   เปลี่ยน reference ทุก interfaces
ip ospf cost                    เปลี่ยน cost interface เฉพาะเจาะจง
bandwidth                       เปลี่ยน bandwidth → OSPF คำนวณ cost

Recommendation:
✅ ใช้ auto-cost reference-bandwidth (global)
✅ ใช้ ip ospf cost สำหรับ specific tuning
```

---

## 2.6 Modify OSPF Timers

### Hello and Dead Intervals

**Default Values:**

```
Network Type         Hello Interval    Dead Interval
------------------------------------------------------------------------
Broadcast            10 seconds        40 seconds
Point-to-Point       10 seconds        40 seconds
NBMA                 30 seconds        120 seconds
------------------------------------------------------------------------
```

**คำจำกัดความ:**

- **Hello Interval** = ความถี่ส่ง Hello packets
- **Dead Interval** = เวลารอก่อนประกาศ neighbor down
- Dead = 4 × Hello (default)

---

### Modify Timers

**Syntax:**

```
Router(config-if)# ip ospf hello-interval <seconds>
Router(config-if)# ip ospf dead-interval <seconds>
```

**ตัวอย่าง:**

```
R1(config)# interface serial 0/0/0
R1(config-if)# ip ospf hello-interval 5
R1(config-if)# ip ospf dead-interval 20
R1(config-if)# end
```

**Verify:**

```
R1# show ip ospf interface serial 0/0/0
...
  Timer intervals configured, Hello 5, Dead 20, Wait 20, Retransmit 5
...
```

---

### Requirements

**⚠️ Hello และ Dead intervals ต้องตรงกันระหว่าง neighbors**

**ตัวอย่าง:**

```
R1: Hello 5, Dead 20
R2: Hello 10, Dead 40

→ R1 และ R2 ไม่เป็น neighbors!
```

**Verification:**

```
R1# debug ip ospf hello
OSPF-1 HELLO Se0/0/0: Send hello to 224.0.0.5
OSPF-1 HELLO Se0/0/0: Rcv hello from 2.2.2.2
  Dead timer mismatch: Configured 20, Received 40

R1# show ip ospf neighbor
(empty - no neighbors)
```

**Fix:**

```
Configure ให้เหมือนกันทั้ง 2 routers:

R2(config)# interface serial 0/0/0
R2(config-if)# ip ospf hello-interval 5
R2(config-if)# ip ospf dead-interval 20
```

---

### Use Cases

**1. Fast Convergence:**

```
ลด Hello/Dead intervals → detect failures เร็วขึ้น

R1(config-if)# ip ospf hello-interval 1
R1(config-if)# ip ospf dead-interval 4

Failure detection: 4 seconds (instead of 40)

⚠️ Trade-off: CPU/bandwidth overhead เพิ่มขึ้น
```

**2. Slow Links:**

```
เพิ่ม intervals สำหรับ slow/unstable links

R1(config-if)# ip ospf hello-interval 30
R1(config-if)# ip ospf dead-interval 120

ลด overhead บน slow links
```

---

## 2.7 Default Route Propagation

### Default Route in OSPF

**คำจำกัดความ:**

- **Default route** (0.0.0.0/0) = route ไปยังทุก destinations
- OSPF ไม่ advertise default route โดยอัตโนมัติ
- ต้อง configure manual

---

### Configure Static Default Route

**Step 1: สร้าง static default route บน edge router**

```
R1(config)# ip route 0.0.0.0 0.0.0.0 <next-hop-or-exit-interface>

ตัวอย่าง:
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.1
หรือ
R1(config)# ip route 0.0.0.0 0.0.0.0 serial 0/1/0
```

---

### Propagate Default Route in OSPF

**Step 2: Advertise default route ไปยัง OSPF neighbors**

**Syntax:**

```
Router(config-router)# default-information originate
```

**ตัวอย่าง:**

```
R1(config)# router ospf 10
R1(config-router)# default-information originate
R1(config-router)# end
```

**Verify:**

```
R1# show ip route | include 0.0.0.0
S*    0.0.0.0/0 [1/0] via 203.0.113.1

Other routers:
R2# show ip route | include 0.0.0.0
O*E2  0.0.0.0/0 [110/1] via 10.1.12.1, 00:01:23, Serial0/0/0
```

**Legend:**

```
O*E2 = OSPF Default Route, External Type 2
```

---

### Default-Information Originate Options

**always keyword:**

```
Router(config-router)# default-information originate always
```

**คำจำกัดความ:**

- Advertise default route เสมอ
- แม้ router ไม่มี default route ใน routing table

**ตัวอย่าง:**

```
R1 ไม่มี default route:

Without "always":
  R1 ไม่ advertise default route

With "always":
  R1 advertise default route (0.0.0.0/0) ต่อไป
  → Routers อื่นใช้ R1 เป็น gateway
```

**Use Case:**

```
ISP router ที่เป็น default gateway เสมอ
```

---

### Complete Example

**Scenario:**

```
Internet
   |
[ISP Router] 203.0.113.1
   |
[R1] --- [R2] --- [R3]
```

**Configuration:**

**R1 (Edge Router):**

```
! Static default route ไป ISP
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.1

! Propagate default route in OSPF
R1(config)# router ospf 10
R1(config-router)# default-information originate
R1(config-router)# end
```

**R2, R3 (Internal Routers):**

```
ไม่ต้อง configure อะไร
→ จะ learn default route จาก R1 โดยอัตโนมัติ
```

**Verification:**

```
R1# show ip route | include 0.0.0.0
S*    0.0.0.0/0 [1/0] via 203.0.113.1

R2# show ip route | include 0.0.0.0
O*E2  0.0.0.0/0 [110/1] via 10.1.12.1, 00:05:23, Serial0/0/0

R3# show ip route | include 0.0.0.0
O*E2  0.0.0.0/0 [110/65] via 10.2.23.1, 00:05:23, Serial0/0/1

R3# ping 8.8.8.8
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 8.8.8.8, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/2/4 ms
```

---

## 2.8 Troubleshooting OSPF

### Common OSPF Problems

#### Problem 1: Neighbors Not Forming

**Symptoms:**

```
R1# show ip ospf neighbor
(empty)
```

**Possible Causes:**

**1. Network statements incorrect:**

```
Check:
R1# show ip protocols | section Network
  Routing for Networks:
    192.168.1.0 0.0.0.255 area 0
    10.1.12.0 0.0.0.3 area 0

Solution: เพิ่ม/แก้ไข network statements
```

**2. Interface down:**

```
Check:
R1# show ip interface brief
Interface         IP-Address      Status    Protocol
Serial0/0/0       10.1.12.1       down      down

Solution: no shutdown
```

**3. Hello/Dead timers mismatch:**

```
Check:
R1# show ip ospf interface serial 0/0/0
  Timer intervals configured, Hello 10, Dead 40

R2# show ip ospf interface serial 0/0/0
  Timer intervals configured, Hello 5, Dead 20

Solution: ปรับให้เหมือนกัน
```

**4. Area ID mismatch:**

```
Check:
R1# show ip ospf interface brief
Interface    PID   Area     ...
Se0/0/0      10    0        ...

R2# show ip ospf interface brief
Interface    PID   Area     ...
Se0/0/0      10    1        ...

Solution: ใช้ area เดียวกัน (area 0 สำหรับ single-area)
```

**5. Subnet/Mask mismatch:**

```
Check:
R1: 10.1.12.1/30 (mask 255.255.255.252)
R2: 10.1.12.2/24 (mask 255.255.255.0)

Solution: ใช้ mask เดียวกัน
```

**6. Authentication mismatch:**

```
Check:
R1# show ip ospf interface serial 0/0/0
  Simple password authentication enabled

R2# show ip ospf interface serial 0/0/0
  No authentication

Solution: Configure authentication ให้เหมือนกัน หรือปิด
```

**7. Passive interface:**

```
Check:
R1# show ip protocols | include Passive
  Passive Interface(s):
    Serial0/0/0

Solution: no passive-interface serial 0/0/0
```

---

#### Problem 2: Routes Missing

**Symptoms:**

```
R1# show ip route ospf
(ไม่มี routes บางตัว)
```

**Possible Causes:**

**1. Network ไม่ได้ advertise:**

```
Check บน source router:
R2# show ip protocols | section Network
  Routing for Networks:
    192.168.2.0 0.0.0.255 area 0

ถ้าไม่มี:
Solution: เพิ่ม network statement
R2(config)# router ospf 10
R2(config-router)# network 192.168.3.0 0.0.0.255 area 0
```

**2. Interface passive:**

```
Check:
R2# show ip protocols | include Passive
  Passive Interface(s):
    GigabitEthernet0/1

ถ้า Gi0/1 ควร advertise:
Solution: no passive-interface gigabitEthernet 0/1
```

**3. OSPF not running on interface:**

```
Check:
R2# show ip ospf interface brief
(ไม่มี interface ที่ต้องการ)

Solution: เพิ่ม network statement
```

---

#### Problem 3: Wrong DR/BDR

**Symptoms:**

```
Router ที่มี priority สูงไม่ได้เป็น DR
```

**Cause:**

```
DR/BDR election เป็น non-preemptive
→ Router ที่มา first เป็น DR
→ Router ที่มาทีหลัง (แม้ priority สูงกว่า) ไม่แย่ง
```

**Solution:**

```
Method 1: Reload ทุก routers (ไม่แนะนำ)

Method 2: Clear OSPF process ทีละตัว
R1# clear ip ospf process

Method 3: Shutdown/No shutdown DR
R2(config)# interface gigabitEthernet 0/0
R2(config-if)# shutdown
R2(config-if)# no shutdown
```

---

#### Problem 4: High CPU Usage

**Symptoms:**

```
R1# show processes cpu sorted
CPU utilization for five seconds: 90%/80%; one minute: 85%; five minutes: 80%

PID Runtime(ms)   Invoked      uSecs   5Sec   1Min   5Min TTY Process
  1    12345678   1234567      10000   75%    70%    65%   0 OSPF-1-Hello
```

**Possible Causes:**

**1. SPF calculations ถี่เกินไป:**

```
Check:
R1# show ip ospf | include SPF
  SPF algorithm executed 1000 times

Cause: Network unstable (flapping links)

Solution:
  - ตรวจสอบ physical links
  - ปรับ SPF timers:
    R1(config-router)# timers throttle spf 5000 10000 20000
```

**2. LSA flooding มากเกินไป:**

```
Cause: Topology changes ถี่

Solution:
  - Stabilize network
  - ใช้ areas (multi-area OSPF)
```

**3. Hellos ถี่เกินไป:**

```
Check:
R1# show ip ospf interface serial 0/0/0
  Timer intervals configured, Hello 1, Dead 4

Solution: เพิ่ม Hello interval
R1(config-if)# ip ospf hello-interval 10
```

---

### Troubleshooting Commands

**Debug Commands:**

```
R1# debug ip ospf hello
R1# debug ip ospf adj
R1# debug ip ospf events
R1# debug ip ospf packet

⚠️ ใช้ระวัง: debug กินทรัพยากร CPU
⚠️ ปิดเมื่อเสร็จ: undebug all
```

**Show Commands:**

```
show ip ospf neighbor
show ip ospf interface
show ip ospf interface brief
show ip ospf
show ip protocols
show ip route ospf
show ip ospf database
```

---

### Troubleshooting Workflow

**1. ตรวจสอบ Physical Layer:**

```
show ip interface brief
show controllers
```

**2. ตรวจสอบ OSPF Configuration:**

```
show running-config | section ospf
show ip protocols
```

**3. ตรวจสอบ Neighbors:**

```
show ip ospf neighbor
debug ip ospf adj
```

**4. ตรวจสอบ LSDB:**

```
show ip ospf database
```

**5. ตรวจสอบ Routing Table:**

```
show ip route ospf
show ip route
```

**6. ทดสอบ Connectivity:**

```
ping <destination>
traceroute <destination>
```

---

## Summary (สรุป)

Module 2 นี้เราได้เรียนรู้:

1. ✅ **Router ID Configuration**:
    
    - Manual: `router-id <id>`
    - Loopback (highest IP)
    - Physical interface (highest active IP)
    - Best practice: Manual configuration
2. ✅ **Enable OSPF**:
    
    - `network <address> <wildcard> area <id>`
    - Wildcard mask = inverse ของ subnet mask
    - Best practice: host-specific (0.0.0.0)
3. ✅ **Passive Interface**:
    
    - `passive-interface <interface>`
    - `passive-interface default`
    - Advertise network แต่ไม่ส่ง/รับ OSPF packets
4. ✅ **Verification Commands**:
    
    - `show ip ospf neighbor`
    - `show ip protocols`
    - `show ip ospf interface`
    - `show ip route ospf`
5. ✅ **Cost Modification**:
    
    - `auto-cost reference-bandwidth <mbps>` (global)
    - `ip ospf cost <value>` (interface)
    - `bandwidth <kbps>` (interface)
6. ✅ **Timer Modification**:
    
    - `ip ospf hello-interval <seconds>`
    - `ip ospf dead-interval <seconds>`
    - ต้องตรงกันระหว่าง neighbors
7. ✅ **Default Route Propagation**:
    
    - สร้าง static default: `ip route 0.0.0.0 0.0.0.0 <next-hop>`
    - Propagate: `default-information originate`
    - Optional: `default-information originate always`
8. ✅ **Troubleshooting**:
    
    - Neighbors not forming
    - Routes missing
    - Wrong DR/BDR
    - High CPU usage
    - Debug และ show commands

**สิ่งสำคัญที่ต้องจำ:**

- Router ID: Manual > Loopback > Physical
- Network command: ใช้ wildcard mask (inverse subnet mask)
- Wildcard 0.0.0.0 = exact match (แนะนำ)
- Passive interface: advertise แต่ไม่ send/receive OSPF
- Cost = Reference BW / Interface BW
- Default reference BW = 100 Mbps (ควรเปลี่ยน)
- Neighbor requirements: Area, Subnet, Timers, Authentication
- Default route: static + default-information originate
- Troubleshooting: Physical → Config → Neighbors → Routes

**Configuration Template:**

```
! Router ID
router ospf <process-id>
 router-id <x.x.x.x>

! Network statements (host-specific)
 network <ip> 0.0.0.0 area 0

! Passive interfaces
 passive-interface default
 no passive-interface <wan-interface>

! Cost (optional)
 auto-cost reference-bandwidth 10000

! Default route (edge router)
 default-information originate
```

**Next Module:** Module 3 - Network Security Concepts

---

**[ไฟล์ Module 2 สมบูรณ์แล้ว!]**