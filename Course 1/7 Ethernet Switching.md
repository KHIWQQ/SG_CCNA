# CCNA Course 1 - Module 7: Ethernet Switching

## การสวิตช์อีเทอร์เน็ต

---

## 7.1 Ethernet Frame (เฟรมอีเทอร์เน็ต)

### Ethernet Overview (ภาพรวมอีเทอร์เน็ต)

**คำจำกัดความ:**

- เทคโนโลยี LAN ที่ใช้มากที่สุดในโลก
- กำหนดโดย **IEEE 802.3**
- ทำงานที่ Layer 1 (Physical) และ Layer 2 (Data Link)

**ประวัติ:**

- พัฒนาโดย **Xerox** (1973)
- Standardized โดย IEEE (1983)
- ปรับปรุงเรื่อยมา: 10 Mbps → 100 Mbps → 1 Gbps → 10 Gbps → 100 Gbps → 400 Gbps

**ลักษณะสำคัญ:**

- ใช้ **CSMA/CD** (Carrier Sense Multiple Access with Collision Detection) - ใน Half-duplex
- ใช้ **MAC addressing** (48-bit)
- **Frame-based** transmission
- **Connectionless** - ไม่มีการสร้าง connection ก่อนส่ง
- **Unreliable** - ไม่มี acknowledgment ที่ Layer 2

---

### Ethernet Frame Structure (โครงสร้างเฟรมอีเทอร์เน็ต)

**Ethernet II Frame (ใช้มากที่สุด):**

```
+----------+----------+---------+---------+------+-----+
|Preamble &| Dest.    | Source  | Type/   | Data | FCS |
|   SFD    |   MAC    |   MAC   | Length  |      |     |
+----------+----------+---------+---------+------+-----+
| 8 bytes  | 6 bytes  | 6 bytes | 2 bytes |46-1500| 4 B|
           |                                            |
           |<------------- 64-1518 bytes ------------->|
           |              (Frame size)                  |
```

**รายละเอียดแต่ละส่วน:**

#### 1. Preamble and SFD (Start Frame Delimiter)

**Preamble (7 bytes):**

- Pattern: `10101010 10101010 ... (7 ครั้ง)`
- หน้าที่: **Synchronization** - ให้อุปกรณ์รับซิงค์สัญญาณ
- แจ้งว่ามี frame กำลังมา

**SFD (1 byte):**

- Pattern: `10101011`
- หน้าที่: บอกว่า frame **เริ่มจริงๆ** แล้ว
- บิตสุดท้าย (11) บอกว่าข้อมูลจริงตามมา

**รวม: 8 bytes**

- ไม่นับรวมใน frame size (64-1518 bytes)
- ไม่ปรากฏใน frame capture (Wireshark)

#### 2. Destination MAC Address (6 bytes)

**หน้าที่:**

- ระบุ**ผู้รับ** (ปลายทาง)

**ประเภท:**

- **Unicast:** MAC address ปกติ (เครื่องเดียว)
- **Multicast:** 01:00:5E:xx:xx:xx (กลุ่มอุปกรณ์)
- **Broadcast:** FF:FF:FF:FF:FF:FF (ทุกเครื่อง)

#### 3. Source MAC Address (6 bytes)

**หน้าที่:**

- ระบุ**ผู้ส่ง** (ต้นทาง)
- Switch ใช้ข้อมูลนี้ **เรียนรู้** MAC address
- **ต้องเป็น Unicast เท่านั้น** (ไม่มี broadcast/multicast source)

#### 4. Type/Length Field (2 bytes)

**Ethernet II (Type):**

- ค่า ≥ **0x0600** (1536)
- บอกประเภทของ protocol ใน Data field

**ตัวอย่าง EtherType:**

```
0x0800  = IPv4
0x0806  = ARP
0x86DD  = IPv6
0x8100  = 802.1Q (VLAN tag)
0x88CC  = LLDP (Link Layer Discovery Protocol)
0x8847  = MPLS Unicast
0x8863  = PPPoE Discovery
0x8864  = PPPoE Session
```

**IEEE 802.3 (Length):**

- ค่า ≤ **0x05DC** (1500)
- บอกความยาวของ Data field
- ใช้กับ 802.3 frame (ไม่ค่อยใช้แล้ว)

#### 5. Data and Pad (46-1500 bytes)

**Data (Payload):**

- ข้อมูลจาก Layer 3 (IP packet, ARP, etc.)
- **Minimum:** 46 bytes
- **Maximum:** 1500 bytes (MTU - Maximum Transmission Unit)

**Padding:**

- ถ้า data น้อยกว่า 46 bytes
- เติม **0x00** ให้ครบ 46 bytes
- ทำไม? เพื่อให้ frame ขนาดขั้นต่ำ 64 bytes (สำหรับ collision detection)

**ตัวอย่าง:**

```
Data = 30 bytes
Padding = 16 bytes (เติมเป็น 0x00)
Total = 46 bytes
```

#### 6. Frame Check Sequence - FCS (4 bytes)

**คำจำกัดความ:**

- **Error detection** field
- คำนวณด้วย **CRC-32** (Cyclic Redundancy Check)

**การทำงาน:**

1. **ผู้ส่ง:**
    
    - คำนวณ CRC จาก Destination MAC ถึง Data (ไม่รวม Preamble และ FCS)
    - ใส่ผลลัพธ์ใน FCS field
2. **ผู้รับ:**
    
    - คำนวณ CRC ใหม่จากข้อมูลที่ได้รับ
    - เปรียบเทียบกับ FCS ที่ได้รับ
    - ถ้าเหมือนกัน → **ถูกต้อง** (Accept)
    - ถ้าไม่เหมือนกัน → **ผิดพลาด** (Discard frame)

**หมายเหตุ:**

- FCS ตรวจจับได้เฉพาะ **error**
- **ไม่แก้ไข error** (No error correction)
- Error correction ทำที่ Layer 4 (TCP)

---

### Ethernet Frame Size (ขนาดเฟรม)

#### Minimum Frame Size

**คำนวณ:**

```
Destination MAC:  6 bytes
Source MAC:       6 bytes
Type/Length:      2 bytes
Data:            46 bytes (minimum)
FCS:              4 bytes
-----------------------------------
Total:           64 bytes (minimum)
```

**ทำไมต้องมี minimum 64 bytes?**

- สำหรับ **CSMA/CD collision detection**
- Frame ต้องยาวพอที่จะ:
    - ส่งออกไปก่อนที่ collision จะเกิด
    - ให้ผู้ส่งตรวจจับ collision ได้ทัน
- คำนวณจาก: **Slot Time** และ **Maximum cable length**

**Slot Time:**

- **512 bit times** (64 bytes)
- เวลาที่ใช้ในการตรวจจับ collision
- ที่ 10 Mbps: 51.2 microseconds

**Frame < 64 bytes:**

- เรียกว่า **Runt frame**
- Switch/NIC จะ **discard** (ทิ้ง)

#### Maximum Frame Size

**Standard Ethernet:**

```
Destination MAC:  6 bytes
Source MAC:       6 bytes
Type/Length:      2 bytes
Data:          1500 bytes (maximum - MTU)
FCS:              4 bytes
-----------------------------------
Total:         1518 bytes (maximum)
```

**Frame > 1518 bytes:**

- เรียกว่า **Giant frame** หรือ **Jumbo frame**
- Jumbo frame: 1519-9216 bytes (ไม่ใช่มาตรฐาน)

#### Jumbo Frames

**คำจำกัดความ:**

- Frame ที่ใหญ่กว่า 1518 bytes
- ขนาดสูงสุด: **9000-9216 bytes** (ขึ้นกับอุปกรณ์)

**ข้อดี:**

- ✅ **ลด overhead** - fewer frames สำหรับ data เดียวกัน
- ✅ **เพิ่ม throughput** - especially for large file transfers
- ✅ **ลด CPU usage** - process fewer frames
- ✅ เหมาะสำหรับ: Storage networks (iSCSI, NFS), Data centers, Backup

**ข้อเสีย:**

- ❌ **ไม่ใช่มาตรฐาน IEEE**
- ❌ ทุกอุปกรณ์ต้อง**รองรับ** (Switch, NIC, Router)
- ❌ ถ้า error → **retransmit ข้อมูลเยอะ**
- ❌ **ไม่เหมาะ** สำหรับ WAN หรือ Internet

**การใช้งาน:**

```
# Enable Jumbo Frame บน Cisco Switch
Switch(config)# system mtu jumbo 9000

# Verify
Switch# show system mtu
```

---

### Ethernet Frame Types (ประเภทเฟรม)

#### 1. Ethernet II (DIX Ethernet)

**คำจำกัดความ:**

- พัฒนาโดย Digital, Intel, Xerox (DIX)
- **ใช้มากที่สุดในปัจจุบัน**
- ใช้ **Type field** (2 bytes) แทน Length

**ลักษณะ:**

- Type field ≥ **0x0600** (1536)
- รองรับ multiple protocols (IPv4, IPv6, ARP, etc.)

**Format:**

```
| Preamble | Dest MAC | Src MAC | Type | Data | FCS |
                                 ^^^^^^
                                  0x0800 (IPv4)
                                  0x0806 (ARP)
                                  0x86DD (IPv6)
```

#### 2. IEEE 802.3 (ไม่ค่อยใช้แล้ว)

**คำจำกัดความ:**

- มาตรฐาน IEEE
- ใช้ **Length field** แทน Type
- ต้องมี **LLC header** (802.2)

**ลักษณะ:**

- Length field ≤ **0x05DC** (1500)
- ซับซ้อนกว่า Ethernet II

**Format:**

```
| Preamble | Dest MAC | Src MAC | Length | LLC | Data | FCS |
                                           ^^^^^
                                           802.2 header
```

**LLC Header (802.2):**

```
| DSAP | SSAP | Control |
  1 B    1 B     1-2 B

DSAP = Destination Service Access Point
SSAP = Source Service Access Point
```

#### 3. 802.1Q Tagged Frame (VLAN Tagging)

**คำจำกัดความ:**

- เพิ่ม **VLAN tag** (4 bytes) เข้าไปใน frame
- ใช้สำหรับ **VLAN** (Virtual LAN)

**Format:**

```
| Dest MAC | Src MAC | 802.1Q Tag | Type | Data | FCS |
                      ^^^^^^^^^^^^
                      4 bytes VLAN tag
```

**802.1Q Tag (4 bytes):**

```
+------+-----+-----+--------------+
| TPID | PCP | DEI | VLAN ID      |
+------+-----+-----+--------------+
| 16 b | 3 b | 1 b | 12 bits      |

TPID (Tag Protocol Identifier): 0x8100
PCP (Priority Code Point): QoS priority (0-7)
DEI (Drop Eligible Indicator): Can drop if congestion
VLAN ID: 0-4095 (4096 VLANs possible)
```

**VLAN ID:**

- **0:** Priority tagged frame (no VLAN)
- **1:** Default VLAN (often used)
- **2-1001:** Normal range VLANs
- **1002-1005:** Reserved (legacy)
- **1006-4094:** Extended range VLANs
- **4095:** Reserved

**Frame Size:**

- Tagged frame = **1522 bytes** (untagged 1518 + 4 bytes tag)
- Jumbo + VLAN = **9220 bytes**

**Double Tagging (QinQ - 802.1ad):**

```
| Dest MAC | Src MAC | Outer Tag | Inner Tag | Type | Data | FCS |
                      0x88a8      0x8100
                      (S-Tag)     (C-Tag)
```

- ใช้สำหรับ **Service Provider**
- Customer VLAN + Service Provider VLAN

---

## 7.2 Ethernet MAC Address Table (ตาราง MAC Address)

### MAC Address Table Overview

**คำจำกัดความ:**

- ตารางที่ Switch ใช้เก็บ **MAC address** และ **port** ที่เชื่อมต่อ
- เรียกอีกชื่อว่า:
    - **CAM Table** (Content Addressable Memory)
    - **Forwarding Table**
    - **Bridge Table**

**ข้อมูลที่เก็บ:**

```
+-------------------+------+------+-----------+
| MAC Address       | Port | VLAN | Age (sec) |
+-------------------+------+------+-----------+
| 0011.2233.4455    | Fa0/1|  1   |    120    |
| AABB.CCDD.EEFF    | Fa0/2|  1   |     60    |
| 1122.3344.5566    | Fa0/3|  10  |    300    |
+-------------------+------+------+-----------+
```

**ฟิลด์:**

- **MAC Address:** ที่อยู่ของอุปกรณ์
- **Port:** Port ที่อุปกรณ์เชื่อมต่ออยู่
- **VLAN:** VLAN membership
- **Type:** Dynamic หรือ Static
- **Age:** เวลานับตั้งแต่ล่าสุดที่เห็น frame จาก MAC นี้

---

### Switch Learning Process (กระบวนการเรียนรู้)

#### 1. Learning (การเรียนรู้)

**กระบวนการ:**

1. Switch รับ frame เข้า port
2. อ่าน **Source MAC address**
3. บันทึกใน MAC address table:
    - MAC address
    - Port ที่รับเข้ามา
    - VLAN
    - Timestamp (reset aging timer)

**ตัวอย่าง:**

```
Frame received on Fa0/1:
  Source MAC: 0011.2233.4455
  Dest MAC: AABB.CCDD.EEFF

Switch learns:
  MAC 0011.2233.4455 is on Fa0/1, VLAN 1
```

#### 2. Forwarding (การส่งต่อ)

**กระบวนการ:**

1. อ่าน **Destination MAC address**
2. ค้นหาใน MAC address table

**กรณีที่ 1: Known Unicast (รู้จัก)**

```
Destination MAC found in table
  → Forward ออก port ที่ระบุเท่านั้น (unicast)
  → Efficient! ไม่รบกวนอุปกรณ์อื่น
```

**กรณีที่ 2: Unknown Unicast (ไม่รู้จัก)**

```
Destination MAC NOT in table
  → Flood ออกทุก port (ยกเว้น port ที่รับเข้ามา)
  → เมื่อปลายทางตอบกลับ จะเรียนรู้ MAC ปลายทาง
```

**กรณีที่ 3: Broadcast**

```
Destination MAC = FF:FF:FF:FF:FF:FF
  → Flood ออกทุก port ในเดียวกัน (ยกเว้น port ที่รับเข้ามา)
```

**กรณีที่ 4: Multicast**

```
Destination MAC = 01:00:5E:xx:xx:xx
  → Default: Flood เหมือน broadcast
  → ถ้ามี IGMP snooping: Forward เฉพาะสมาชิกของ multicast group
```

**ตัวอย่างการ Forward:**

```
Topology:
[PC1]---Fa0/1---+
                 |
[PC2]---Fa0/2---[Switch]---Fa0/4---[PC4]
                 |
[PC3]---Fa0/3---+

MAC Table:
MAC (PC1): AAAA → Fa0/1
MAC (PC2): BBBB → Fa0/2
MAC (PC4): DDDD → Fa0/4
MAC (PC3): CCCC → NOT YET LEARNED

Scenario 1: PC1 → PC2 (Known unicast)
  Frame: Src=AAAA, Dst=BBBB
  Action: Forward ONLY to Fa0/2 ✅

Scenario 2: PC1 → PC3 (Unknown unicast)
  Frame: Src=AAAA, Dst=CCCC
  Action: Flood to Fa0/2, Fa0/3, Fa0/4 🌊

Scenario 3: PC1 → Broadcast
  Frame: Src=AAAA, Dst=FFFF.FFFF.FFFF
  Action: Flood to Fa0/2, Fa0/3, Fa0/4 🌊
```

#### 3. Filtering (การกรอง)

**คำจำกัดความ:**

- Switch **ไม่ส่ง** frame ออก port ที่ไม่จำเป็น

**ตัวอย่าง:**

```
PC1 (Fa0/1) → PC2 (Fa0/2)

Switch:
  - รับจาก Fa0/1
  - ส่งออก Fa0/2 เท่านั้น
  - ไม่ส่งออก Fa0/3, Fa0/4, ... (Filtered)
```

**ประโยชน์:**

- ✅ ลด traffic ที่ไม่จำเป็น
- ✅ เพิ่มประสิทธิภาพ
- ✅ เพิ่มความปลอดภัย (อุปกรณ์อื่นไม่เห็น traffic)

#### 4. Aging (การหมดอายุ)

**คำจำกัดความ:**

- MAC address entries **หมดอายุ**หลังไม่ใช้งาน
- ป้องกันตารางเต็มด้วย MAC เก่าๆ

**Aging Time:**

- **Default: 300 seconds** (5 minutes)
- ถ้าไม่เห็น frame จาก MAC นี้ภายใน 300 วินาที
- Entry จะถูก**ลบ**ออกจากตาราง

**การทำงาน:**

1. เมื่อเห็น frame จาก Source MAC
2. **Reset aging timer** เป็น 0
3. Timer นับขึ้นเรื่อยๆ
4. ถ้าถึง 300 seconds → ลบ entry

**ตัวอย่าง:**

```
T=0s:   PC1 sends frame → MAC AAAA learned, Age=0
T=60s:  Age=60
T=100s: PC1 sends frame → Age reset to 0
T=300s: Age=300 → Entry deleted (if no more frames)
```

**ทำไมต้องมี aging?**

- อุปกรณ์อาจเปลี่ยน port (ย้าย, เสียบใหม่)
- MAC address อาจเปลี่ยน (NIC เปลี่ยน)
- ป้องกันข้อมูลผิดพลาดใน table

---

### MAC Address Table Management

#### Viewing MAC Address Table

**คำสั่ง Cisco:**

```
Switch# show mac address-table
```

**ตัวอย่าง output:**

```
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    0011.2233.4455    DYNAMIC     Fa0/1
   1    aabb.ccdd.eeff    DYNAMIC     Fa0/2
   1    1122.3344.5566    DYNAMIC     Fa0/3
  10    aaaa.bbbb.cccc    DYNAMIC     Fa0/5
   1    0050.56c0.0001    DYNAMIC     Fa0/1
All    0100.0ccc.cccc    STATIC      CPU
All    0100.0ccc.cccd    STATIC      CPU
All    0180.c200.0000    STATIC      CPU
```

**Filtering:**

```
# ดู specific MAC
Switch# show mac address-table address 0011.2233.4455

# ดู specific port
Switch# show mac address-table interface fastethernet 0/1

# ดู specific VLAN
Switch# show mac address-table vlan 10

# ดูเฉพาะ dynamic entries
Switch# show mac address-table dynamic

# ดูเฉพาะ static entries
Switch# show mac address-table static

# นับจำนวน
Switch# show mac address-table count
```

#### Clearing MAC Address Table

**คำสั่ง:**

```
# ลบทั้งหมด (dynamic only)
Switch# clear mac address-table dynamic

# ลบ specific MAC
Switch# clear mac address-table dynamic address 0011.2233.4455

# ลบ specific port
Switch# clear mac address-table dynamic interface fa0/1

# ลบ specific VLAN
Switch# clear mac address-table dynamic vlan 10
```

**เมื่อไหร่ต้อง clear?**

- Troubleshooting connectivity issues
- หลังเปลี่ยน topology
- หลังย้ายอุปกรณ์
- Test/Verify operation

#### Configuring Aging Time

**คำสั่ง:**

```
# Set aging time (10-1000000 seconds)
Switch(config)# mac address-table aging-time 200

# Set per VLAN
Switch(config)# mac address-table aging-time 200 vlan 10

# Disable aging (not recommended)
Switch(config)# no mac address-table aging-time

# Verify
Switch# show mac address-table aging-time
```

**แนะนำ:**

- **Default (300s)** เหมาะสำหรับ environment ส่วนใหญ่
- **ลด (120-180s)** สำหรับ dynamic environment (wireless, DHCP)
- **เพิ่ม (600-1200s)** สำหรับ stable environment

#### Static MAC Address

**คำจำกัดความ:**

- กำหนด MAC address กับ port **ด้วยตนเอง**
- **ไม่หมดอายุ** (aging = 0)
- ใช้สำหรับ **security** หรือ **critical devices**

**คำสั่ง:**

```
# Add static MAC
Switch(config)# mac address-table static 0011.2233.4455 vlan 1 interface fa0/1

# Remove static MAC
Switch(config)# no mac address-table static 0011.2233.4455 vlan 1

# Verify
Switch# show mac address-table static
```

**ข้อดี:**

- ✅ **Security** - ป้องกัน MAC spoofing
- ✅ **Predictable** - ไม่เปลี่ยนแปลง
- ✅ Critical devices มั่นใจได้

**ข้อเสีย:**

- ❌ **ต้องจัดการด้วยตนเอง**
- ❌ ไม่ flexible
- ❌ ถ้าอุปกรณ์ย้าย port ต้องเปลี่ยน config

---

### MAC Address Table Size

**ขนาดตาราง:**

- ขึ้นกับ**รุ่น Switch** และ **memory**
- ยิ่งแพงยิ่ง table ใหญ่

**ตัวอย่าง:**

```
Cisco Catalyst 2960:   8,192 MAC addresses
Cisco Catalyst 3650:  32,768 MAC addresses
Cisco Catalyst 9300:  55,000+ MAC addresses
Cisco Nexus 9000:    128,000+ MAC addresses
```

**คำสั่งดูขนาด:**

```
Switch# show mac address-table count

Dynamic Address Count:  245
Static Address Count:   3
Total MAC Addresses:    248
Total MAC Address Space Available: 7944
```

**เมื่อตารางเต็ม:**

- Switch **ไม่สามารถ learn** MAC ใหม่
- **Flood** ทุก unknown unicast (performance ลด)
- ต้อง clear หรือเพิ่ม aging time

---

## 7.3 Switch Forwarding Methods (วิธีการส่งต่อของ Switch)

### Forwarding Methods Overview

Switch มี **3 วิธี**ในการ forward frames:

#### 1. Store-and-Forward Switching

**คำจำกัดความ:**

- รับ**ทั้ง frame** ก่อน
- ตรวจสอบ **FCS** (error check)
- แล้วจึง forward

**กระบวนการ:**

```
1. รับ frame ทั้งหมด → buffer
2. ตรวจสอบ FCS (CRC)
3. ถ้า OK → forward
4. ถ้า error → discard
```

**ข้อดี:**

- ✅ **Error checking** - ไม่ forward frame ที่เสีย
- ✅ **ความน่าเชื่อถือสูง**
- ✅ รองรับ **different speeds** (10/100/1000 Mbps)
- ✅ รองรับ **QoS** (priority)

**ข้อเสีย:**

- ❌ **Latency สูงกว่า** (ต้องรอรับทั้ง frame)

**Latency:**

- ขึ้นกับ**ขนาด frame**
- Frame ใหญ่ = latency สูง

**การใช้งาน:**

- **Default** ใน switch ทุกรุ่นในปัจจุบัน
- **แนะนำ** สำหรับ production networks

#### 2. Cut-Through Switching

**คำจำกัดความ:**

- อ่านเฉพาะ **Destination MAC** (6 bytes แรก)
- Forward **ทันที** ไม่รอ frame ทั้งหมด
- **ไม่ตรวจสอบ FCS**

**กระบวนการ:**

```
1. รับ Destination MAC (6 bytes)
2. Lookup MAC table
3. Forward ทันที (frame ยังมาไม่หมด)
4. ไม่ check error
```

**ข้อดี:**

- ✅ **Latency ต่ำมาก** (microseconds)
- ✅ **เร็ว** - เหมาะสำหรับ time-sensitive applications
- ✅ Latency **คงที่** (ไม่ขึ้นกับขนาด frame)

**ข้อเสีย:**

- ❌ **ไม่ตรวจสอบ error** - อาจ forward frame ที่เสีย
- ❌ **ต้องใช้ speed เดียวกัน** (ไม่รองรับ different speeds ได้ดี)
- ❌ ไม่รองรับ QoS ได้เต็มที่

**Latency:**

- **Fixed** ≈ 10 microseconds (ไม่ขึ้นกับขนาด frame)

**การใช้งาน:**

- High-performance computing
- Low-latency trading systems
- Gaming
- Real-time applications

**ประเภทของ Cut-Through:**

##### Fast-Forward Switching

- Forward ทันทีที่อ่าน **Destination MAC** (6 bytes)
- **Fastest** method
- Latency ต่ำสุด

##### Fragment-Free Switching

- รอรับ **64 bytes แรก**
- ตรวจสอบ **collision** (runts)
- แล้วจึง forward

**Fragment-Free คือ:**

- **Hybrid** ระหว่าง Store-and-Forward กับ Cut-Through
- ตรวจสอบ **collision fragments** (< 64 bytes)
- Frame ≥ 64 bytes มักไม่มี collision
- Latency: ระหว่าง Store-and-Forward กับ Cut-Through

#### 3. Adaptive Cut-Through (Automatic Error Detection)

**คำจำกัดความ:**

- Switch **เปลี่ยนโหมด**อัตโนมัติ
- เริ่มด้วย **Cut-Through**
- ถ้า error สูง → เปลี่ยนเป็น **Store-and-Forward**
- ถ้า error ลดลง → กลับเป็น Cut-Through

**การทำงาน:**

```
1. Default: Cut-Through mode
2. Monitor error rate
3. ถ้า error > threshold → Store-and-Forward
4. ถ้า error < threshold (after period) → Cut-Through
```

**ข้อดี:**

- ✅ **Best of both worlds**
- ✅ เร็ว + reliable
- ✅ Adaptive

**การใช้งาน:**

- Cisco high-end switches
- Data centers

---

### Comparison of Forwarding Methods

```
Feature              Store-and-Forward  Cut-Through    Fragment-Free
------------------------------------------------------------------------
Error Checking       ✅ Yes (FCS)       ❌ No          ⚠️  Partial
Latency              High               Low            Medium
Forward bad frames   No                 Yes            Rarely
Different speeds     ✅ Yes             ❌ Limited     ⚠️  Limited
QoS support          ✅ Full            ⚠️  Limited    ⚠️  Limited
Collision detection  N/A                ❌ No          ✅ Yes
Current usage        ✅ Default         Specialty      Rare
Reliability          High               Low            Medium
------------------------------------------------------------------------
```

**Latency Comparison (1518-byte frame @ 1 Gbps):**

```
Store-and-Forward:  ≈ 12 microseconds
Fragment-Free:      ≈ 5 microseconds
Cut-Through:        ≈ 2-3 microseconds
```

---

## 7.4 Switching Domains (โดเมนการสวิตช์)

### Collision Domain (โดเมนการชน)

**คำจำกัดความ:**

- พื้นที่เครือข่ายที่ **collision สามารถเกิดได้**
- อุปกรณ์ที่**แชร์ bandwidth** เดียวกัน
- ใช้กับ **Half-duplex** เท่านั้น

**ลักษณะ:**

- **CSMA/CD** ทำงานภายใน collision domain
- **Hub:** collision domain เดียว (ทุก port)
- **Switch:** แต่ละ port = collision domain แยก
- **Full-duplex:** ไม่มี collision domain

**ตัวอย่าง Hub:**

```
[PC1]─┐
      ├─[Hub]─[PC4]
[PC2]─┤
      │
[PC3]─┘

Collision domain = 1 (ทั้งหมดอยู่ใน domain เดียว)
ถ้า PC1 และ PC2 ส่งพร้อมกัน → Collision!
```

**ตัวอย่าง Switch:**

```
[PC1]─┐
      ├─[Switch]─[PC4]
[PC2]─┤
      │
[PC3]─┘

Collision domains = 4 (แต่ละ port = 1 domain)
PC1 และ PC2 ส่งพร้อมกัน → ไม่ collision (แยก domain)
```

**Switch vs Hub:**

```
Device    Collision Domains    Notes
--------------------------------------------------
Hub       1 (shared)          ทุก port อยู่ใน domain เดียว
Switch    1 per port          แยก domain, ไม่มี collision
```

**Full-Duplex:**

- **ไม่มี collision domain**
- ส่งและรับพร้อมกัน
- ไม่ต้อง CSMA/CD

**ประโยชน์ของการแยก Collision Domain:**

- ✅ **ไม่มี collision**
- ✅ **เพิ่ม performance**
- ✅ Dedicated bandwidth ต่อ port

---

### Broadcast Domain (โดเมนบรอดคาสต์)

**คำจำกัดความ:**

- พื้นที่เครือข่ายที่ **broadcast frame ถึงได้ทุกอุปกรณ์**
- อุปกรณ์ที่**รับ broadcast** เดียวกัน

**ลักษณะ:**

- **Hub:** broadcast domain เดียว
- **Switch:** broadcast domain เดียว (ถ้าไม่มี VLAN)
- **Router:** แบ่ง broadcast domain (ไม่ forward broadcast)
- **VLAN:** แบ่ง broadcast domain บน Switch

**ตัวอย่าง Switch (no VLAN):**

```
[PC1]─┐
      ├─[Switch]─[PC4]
[PC2]─┤
      │
[PC3]─┘

Broadcast domain = 1
PC1 broadcast → PC2, PC3, PC4 ได้รับทั้งหมด
```

**ตัวอย่าง Router:**

```
[PC1]─┐
      ├─[Switch]─[Router]─[Switch]─[PC4]
[PC2]─┤             |               |
      │             └───────────────┘
[PC3]─┘

Broadcast domains = 2
  Domain 1: PC1, PC2, PC3
  Domain 2: PC4
PC1 broadcast → เฉพาะ PC2, PC3 (Router ไม่ forward)
```

**ตัวอย่าง VLAN:**

```
[PC1]─Fa0/1─┐
            ├─[Switch]─Fa0/4─[PC4]
[PC2]─Fa0/2─┤
            │
[PC3]─Fa0/3─┘

VLAN 10: Fa0/1, Fa0/2  → Broadcast domain 1
VLAN 20: Fa0/3, Fa0/4  → Broadcast domain 2

PC1 (VLAN 10) broadcast → เฉพาะ PC2 (VLAN 10)
```

**ปัญหาของ Broadcast Domain ใหญ่:**

- ❌ **Broadcast storm** - broadcast มากเกินไป
- ❌ **Performance ลด** - CPU ทุกเครื่องต้อง process broadcast
- ❌ **Security** - ทุกคนเห็น broadcast
- ❌ **Scalability** จำกัด

**วิธีแบ่ง Broadcast Domain:**

1. **Router** - Layer 3 device
2. **VLAN** - Virtual LAN
3. **Layer 3 Switch** - Switch ที่มี routing

**แนะนำ:**

- Broadcast domain ควรมี **< 500 devices**
- ใช้ **VLAN** แบ่งตาม:
    - Department
    - Function
    - Security requirements
    - Traffic patterns

---

### Comparison: Collision vs Broadcast Domain

```
Aspect               Collision Domain         Broadcast Domain
------------------------------------------------------------------------
Definition           Area where collision     Area that receives
                     can occur                broadcast
------------------------------------------------------------------------
Separated by         Switch port              Router, VLAN
                     Bridge                   Layer 3 switch
                     Full-duplex
------------------------------------------------------------------------
Hub                  1 (all ports)            1 (all ports)
Switch (no VLAN)     1 per port               1 (all ports)
Switch (with VLAN)   1 per port               1 per VLAN
Router               1 per port               1 per interface
------------------------------------------------------------------------
Layer                Layer 1 & 2              Layer 2 & 3
------------------------------------------------------------------------
Relevant in          Half-duplex only         Always
------------------------------------------------------------------------
Problem              Collision                Broadcast storm
                     Reduced bandwidth        CPU overhead
------------------------------------------------------------------------
Modern LAN           ไม่มี (Full-duplex)      ยังมี (ใช้ VLAN แบ่ง)
------------------------------------------------------------------------
```

---

## 7.5 Switch Boot Sequence (ลำดับการบูตของ Switch)

### Boot Process Overview

**ลำดับการ boot:**

#### 1. Power-On Self-Test (POST)

**คำจำกัดความ:**

- ตรวจสอบ**ฮาร์ดแวร์**ทั้งหมด
- ทำงานทันทีเมื่อเปิดเครื่อง

**ตรวจสอบ:**

- ✅ CPU
- ✅ DRAM (Memory)
- ✅ Flash memory
- ✅ Ports/Interfaces
- ✅ LEDs
- ✅ Power supply

**ผลลัพธ์:**

- **PASS:** ไป step ถัดไป
- **FAIL:** แสดง error, หยุดการ boot

**LED Status:**

```
During POST:
  SYST LED: Amber (กระพริบ)
  
After POST:
  Success: Green
  Fail:    Amber (ติดค้าง) หรือ Off
```

#### 2. Boot Loader

**คำจำกัดความ:**

- โปรแกรมเล็กๆ ใน **ROM**
- ใช้ initialize flash file system
- โหลด IOS

**หน้าที่:**

- โหลด Cisco IOS จาก flash
- ให้บริการ **recovery mode** (ROMMON mode)

**ROMMON Mode (ROM Monitor):**

- Emergency mode
- ใช้เมื่อ IOS corrupt หรือหาไม่เจอ
- สามารถ:
    - ติดตั้ง IOS ใหม่
    - เปลี่ยน configuration register
    - Password recovery

**เข้า ROMMON:**

```
กด Ctrl+Break ในช่วง boot (15 วินาทีแรก)

rommon 1 >
```

#### 3. Cisco IOS

**คำจำกัดความ:**

- Operating System ของ Switch/Router
- โหลดจาก **flash memory**

**การโหลด:**

```
1. Boot loader อ่าน flash
2. หา IOS image file (ส่วนใหญ่ .bin)
3. โหลด IOS เข้า RAM
4. รัน IOS
```

**IOS File naming:**

```
c2960-lanbasek9-mz.150-2.SE.bin

c2960:         Platform (Catalyst 2960)
lanbasek9:     Feature set (LAN Base with crypto)
mz:            File format (m=runs from RAM, z=compressed)
150-2:         Version (15.0(2))
SE:            Release train
.bin:          Binary file
```

#### 4. Configuration File

**คำจำกัดความ:**

- ไฟล์ config ที่บันทึกการตั้งค่า
- โหลดจาก **NVRAM** (Non-Volatile RAM)

**ประเภท:**

**startup-config:**

- เก็บใน **NVRAM**
- config ที่ใช้ตอน boot
- **ไม่หายเมื่อปิดเครื่อง**

**running-config:**

- เก็บใน **RAM**
- config ที่ใช้งานอยู่ขณะนี้
- **หายเมื่อปิดเครื่อง** (ถ้าไม่ save)

**การโหลด:**

```
1. IOS boot เสร็จ
2. อ่าน startup-config จาก NVRAM
3. Copy ไปเป็น running-config ใน RAM
4. Apply configuration

ถ้าไม่มี startup-config:
  → เข้า Setup mode (Initial configuration dialog)
```

---

### Switch Memory Types

#### 1. ROM (Read-Only Memory)

**คำจำกัดความ:**

- **ไม่สามารถเขียนได้** (หรือยากมาก)
- **ไม่หายเมื่อปิดเครื่อง**

**เก็บ:**

- **POST** (Power-On Self-Test)
- **Boot loader** (ROMMON)
- Mini IOS (limited features - for recovery)

**ขนาด:** ≈ 1-2 MB

#### 2. Flash Memory (Non-Volatile)

**คำจำกัดความ:**

- **เขียนได้** (Writable)
- **ไม่หายเมื่อปิดเครื่อง**
- เหมือน USB drive หรือ SSD

**เก็บ:**

- **Cisco IOS image** (.bin file)
- Configuration files (สำรอง)
- Log files
- Other files

**ขนาด:** 32 MB - 256 MB+ (ขึ้นกับรุ่น)

**คำสั่ง:**

```
Switch# show flash

Directory of flash:/

    1  -rw-    11832320   Mar 1 1993 00:04:42  c2960-lanbasek9-mz.150-2.SE.bin
    2  -rw-        2072   Mar 1 1993 00:05:14  config.text
    3  -rw-        1038   Mar 1 1993 00:05:20  vlan.dat

64016384 bytes total (52183936 bytes free)
```

#### 3. NVRAM (Non-Volatile RAM)

**คำจำกัดความ:**

- **เขียนได้**
- **ไม่หายเมื่อปิดเครื่อง**
- เร็วกว่า Flash แต่เล็กกว่า

**เก็บ:**

- **startup-config** - configuration file
- **Configuration register** value

**ขนาด:** ≈ 512 KB

**คำสั่ง:**

```
Switch# show startup-config
```

#### 4. RAM (Random Access Memory)

**คำจำกัดความ:**

- **เขียนได้**
- **หายเมื่อปิดเครื่อง** (Volatile)
- เร็วมาก

**เก็บ:**

- **running-config** - configuration ที่ใช้อยู่
- **MAC address table**
- **ARP table** (Router)
- **Routing table** (Router)
- IOS (ตอนรัน)
- Packet buffers

**ขนาด:** 64 MB - 8 GB+ (ขึ้นกับรุ่น)

**คำสั่ง:**

```
Switch# show running-config
Switch# show mac address-table
Switch# show version  (แสดง RAM size)
```

---

### Memory Comparison

```
Type      Volatile  Writable  Speed     Size        Stores
--------------------------------------------------------------------------
ROM       No        No        Slow      1-2 MB      POST, Boot loader
Flash     No        Yes       Medium    32-256 MB+  IOS image
NVRAM     No        Yes       Medium    512 KB      startup-config
RAM       Yes       Yes       Fast      64MB-8GB+   running-config,
                                                    MAC table, IOS(running)
--------------------------------------------------------------------------
```

---

### Configuration Files

#### startup-config

**คำจำกัดความ:**

- Configuration ที่**บันทึกไว้**
- โหลดตอน**boot**
- เก็บใน **NVRAM**

**ดู:**

```
Switch# show startup-config
```

**บันทึก:**

```
# Method 1: copy running-config to startup-config
Switch# copy running-config startup-config
Destination filename [startup-config]? [Enter]
Building configuration...
[OK]

# Method 2: Short version
Switch# write memory
หรือ
Switch# wr

# Method 3 (ไม่แนะนำ):
Switch# copy running-config nvram:startup-config
```

**ลบ:**

```
Switch# erase startup-config
Erasing the nvram filesystem will remove all configuration files! Continue? [confirm] [Enter]
[OK]
Erase of nvram: complete

หรือ

Switch# write erase
```

#### running-config

**คำจำกัดความ:**

- Configuration ที่**ใช้อยู่ขณะนี้**
- เก็บใน **RAM**
- **หายเมื่อ reload** (ถ้าไม่ save)

**ดู:**

```
Switch# show running-config
หรือ
Switch# sh run
```

**แก้ไข:**

```
# เข้า configuration mode
Switch# configure terminal
Switch(config)# [คำสั่ง config]
Switch(config)# exit

# การเปลี่ยนแปลงเกิดกับ running-config ทันที
# แต่ยังไม่ได้บันทึกใน startup-config
```

**Reload:**

```
Switch# reload
System configuration has been modified. Save? [yes/no]: no
Proceed with reload? [confirm] [Enter]

# ถ้าไม่ save → เปลี่ยนแปลงหาย
```

#### Configuration Register

**คำจำกัดความ:**

- ค่า 16-bit ใน NVRAM
- ควบคุม**พฤติกรรมการ boot**

**ค่าที่ใช้บ่อย:**

```
0x2102  = Default (boot normal, load startup-config)
0x2142  = Ignore startup-config (password recovery)
```

**ดู:**

```
Switch# show version
...
Configuration register is 0x2102
```

**เปลี่ยน:**

```
Switch(config)# config-register 0x2142
Switch(config)# exit
Switch# reload
```

---

## 7.6 Switch Management (การจัดการ Switch)

### Initial Configuration (การตั้งค่าเริ่มต้น)

#### Setup Mode

**เข้า Setup Mode:**

- เมื่อ**ไม่มี startup-config**
- Boot ครั้งแรก (out of box)

**ข้อความ:**

```
--- System Configuration Dialog ---

Would you like to enter the initial configuration dialog? [yes/no]: no

Press RETURN to get started!
```

**แนะนำ:**

- พิมพ์ **no**
- ตั้งค่าเองผ่าน CLI (มีความยืดหยุ่นกว่า)

#### Basic Configuration Steps

**1. เข้า Privileged EXEC Mode:**

```
Switch> enable
Switch#
```

**2. เข้า Global Configuration Mode:**

```
Switch# configure terminal
Switch(config)#
```

**3. ตั้งชื่อ Switch:**

```
Switch(config)# hostname SW1
SW1(config)#
```

**4. ป้องกัน Domain Lookup:**

```
SW1(config)# no ip domain-lookup
```

- ป้องกัน Switch พยายาม resolve typo เป็น hostname

**5. ตั้ง Password:**

**Console Password:**

```
SW1(config)# line console 0
SW1(config-line)# password cisco
SW1(config-line)# login
SW1(config-line)# exit
```

**VTY (Telnet/SSH) Password:**

```
SW1(config)# line vty 0 15
SW1(config-line)# password cisco
SW1(config-line)# login
SW1(config-line)# exit
```

**Enable Password:**

```
SW1(config)# enable secret class
```

**6. เข้ารหัส Passwords:**

```
SW1(config)# service password-encryption
```

- เข้ารหัส passwords ใน config (Type 7 - weak)
- Enable secret ใช้ MD5 (Type 5 - strong) อยู่แล้ว

**7. Banner:**

```
SW1(config)# banner motd #
***********************************************
*  Authorized Access Only!                   *
*  Violators will be prosecuted!            *
***********************************************
#
```

**8. ตั้งค่า Management IP:**

```
SW1(config)# interface vlan 1
SW1(config-if)# ip address 192.168.1.2 255.255.255.0
SW1(config-if)# no shutdown
SW1(config-if)# exit

SW1(config)# ip default-gateway 192.168.1.1
```

**9. บันทึก Configuration:**

```
SW1(config)# exit
SW1# copy running-config startup-config
หรือ
SW1# write memory
```

---

### Remote Management Access

#### Telnet

**คำจำกัดความ:**

- Remote access แบบ **text-based**
- **ไม่เข้ารหัส** (insecure)
- Port **23**

**Enable Telnet:**

```
SW1(config)# line vty 0 15
SW1(config-line)# password cisco
SW1(config-line)# login
SW1(config-line)# transport input telnet
SW1(config-line)# exit
```

**Connect:**

```
PC> telnet 192.168.1.2
```

**ข้อเสีย:**

- ❌ **ไม่ปลอดภัย** - password/data ถูกส่งแบบ plaintext
- ❌ ไม่แนะนำใช้ใน production

#### SSH (Secure Shell)

**คำจำกัดความ:**

- Remote access แบบ **encrypted**
- **ปลอดภัย**
- Port **22**

**Enable SSH:**

**1. ตั้ง hostname และ domain:**

```
SW1(config)# hostname SW1
SW1(config)# ip domain-name example.com
```

**2. สร้าง RSA keys:**

```
SW1(config)# crypto key generate rsa

How many bits in the modulus [512]: 2048

% Generating 2048 bit RSA keys, keys will be non-exportable...
[OK] (key generation may take a minute)
```

**3. สร้าง local user:**

```
SW1(config)# username admin privilege 15 secret cisco123
```

**4. Configure VTY lines:**

```
SW1(config)# line vty 0 15
SW1(config-line)# login local
SW1(config-line)# transport input ssh
SW1(config-line)# exit
```

**5. ตั้งค่า SSH version (แนะนำ):**

```
SW1(config)# ip ssh version 2
SW1(config)# ip ssh time-out 60
SW1(config)# ip ssh authentication-retries 3
```

**Verify:**

```
SW1# show ip ssh
SW1# show ssh
```

**Connect:**

```
PC> ssh -l admin 192.168.1.2
Password: [cisco123]

SW1>
```

**ข้อดี:**

- ✅ **เข้ารหัส** - password และ data ปลอดภัย
- ✅ **Authentication** - ใช้ username/password
- ✅ **แนะนำสำหรับ production**

---

### Port Configuration

#### Configure Interface

**เข้า Interface Configuration Mode:**

```
SW1(config)# interface fastethernet 0/1
SW1(config-if)#

หรือ short version:
SW1(config)# int fa0/1
```

**คำสั่งพื้นฐาน:**

**1. Description:**

```
SW1(config-if)# description Link to PC1
```

**2. Speed:**

```
SW1(config-if)# speed 100
หรือ
SW1(config-if)# speed auto
```

**3. Duplex:**

```
SW1(config-if)# duplex full
หรือ
SW1(config-if)# duplex auto
```

**4. Enable/Disable:**

```
# Disable (shutdown)
SW1(config-if)# shutdown

# Enable (no shutdown)
SW1(config-if)# no shutdown
```

**5. MDIX (Auto-MDIX):**

```
SW1(config-if)# mdix auto
```

- อนุญาตให้ใช้ straight-through cable แทน crossover

**ตัวอย่างเต็ม:**

```
SW1(config)# interface fa0/1
SW1(config-if)# description Link to PC1
SW1(config-if)# speed 100
SW1(config-if)# duplex full
SW1(config-if)# no shutdown
SW1(config-if)# exit
```

#### Configure Range of Interfaces

**Configure หลาย interfaces พร้อมกัน:**

```
SW1(config)# interface range fastethernet 0/1 - 24
SW1(config-if-range)# shutdown
SW1(config-if-range)# exit

หรือ

SW1(config)# interface range fa0/1 - 12
SW1(config-if-range)# description Access Ports
SW1(config-if-range)# switchport mode access
SW1(config-if-range)# exit
```

**Multiple Ranges:**

```
SW1(config)# interface range fa0/1-5, fa0/7-10, fa0/15-20
SW1(config-if-range)# [commands]
```

---

### Verification Commands (คำสั่งตรวจสอบ)

#### Show Version

**ข้อมูลที่แสดง:**

- IOS version
- System uptime
- Hardware (CPU, Memory)
- Configuration register
- IOS image filename

```
SW1# show version

Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE, RELEASE SOFTWARE (fc1)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2012 by Cisco Systems, Inc.

ROM: Bootstrap program is C2960 boot loader
BOOTLDR: C2960 Boot Loader (C2960-HBOOT-M) Version 12.2(53r)SEY3, RELEASE SOFTWARE (fc1)

SW1 uptime is 1 hour, 23 minutes
System returned to ROM by power-on
System image file is "flash:c2960-lanbasek9-mz.150-2.SE.bin"

cisco WS-C2960-24TT-L (PowerPC405) processor with 65536K bytes of memory.
Processor board ID FOC1234X5YZ
Last reset from power-on
24 FastEthernet interfaces
2 Gigabit Ethernet interfaces

64K bytes of flash-simulated non-volatile configuration memory.
Base ethernet MAC Address       : 00:1A:2B:3C:4D:5E
Model number                    : WS-C2960-24TT-L
System serial number            : FOC1234X5YZ
Configuration register is 0x2102
```

#### Show Interfaces

**แสดงข้อมูลทุก interfaces:**

```
SW1# show interfaces
```

**แสดง specific interface:**

```
SW1# show interfaces fastethernet 0/1
```

**ข้อมูลที่แสดง:**

- Status (up/down)
- Speed, Duplex
- MAC address
- IP address (L3 interface)
- Errors, Collisions
- Traffic statistics

**ตัวอย่าง:**

```
SW1# show interfaces fa0/1

FastEthernet0/1 is up, line protocol is up (connected)
  Hardware is Fast Ethernet, address is 001a.2b3c.4d5e (bia 001a.2b3c.4d5e)
  Description: Link to PC1
  MTU 1500 bytes, BW 100000 Kbit/sec, DLY 100 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive set (10 sec)
  Full-duplex, 100Mb/s, media type is 10/100BaseTX
  input flow-control is off, output flow-control is unsupported
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:00, output 00:00:01, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     1234 packets input, 567890 bytes, 0 no buffer
     Received 100 broadcasts (50 multicasts)
     0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 50 multicast, 0 pause input
     0 input packets with dribble condition detected
     2468 packets output, 987654 bytes, 0 underruns
     0 output errors, 0 collisions, 1 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier, 0 pause output
     0 output buffer failures, 0 output buffers swapped out
```

**Interface Status:**

```
Status Line             Meaning
------------------------------------------------------------------------
up, line protocol up    Interface is working (connected, good cable)
up, line protocol down  Layer 1 OK, Layer 2 problem (encapsulation, etc.)
down, line protocol down Interface is disabled or cable problem
administratively down   Interface is shutdown (disabled by admin)
```

#### Show Interface Status

**แสดงสรุป all interfaces:**

```
SW1# show interfaces status

Port      Name               Status       Vlan       Duplex  Speed Type
Fa0/1     Link to PC1        connected    1          full    100   10/100BaseTX
Fa0/2                        notconnect   1          auto    auto  10/100BaseTX
Fa0/3                        disabled     1          auto    auto  10/100BaseTX
Fa0/4                        connected    10         full    1000  10/100BaseTX
...
```

#### Show MAC Address Table

```
SW1# show mac address-table
SW1# show mac address-table dynamic
SW1# show mac address-table interface fa0/1
```

#### Show Running/Startup Config

```
SW1# show running-config
SW1# show startup-config
```

#### Show VLAN

```
SW1# show vlan brief
SW1# show vlan
```

#### Show IP Interface Brief

```
SW1# show ip interface brief

Interface              IP-Address      OK? Method Status                Protocol
Vlan1                  192.168.1.2     YES manual up                    up
FastEthernet0/1        unassigned      YES unset  up                    up
FastEthernet0/2        unassigned      YES unset  down                  down
...
```

---

### Troubleshooting Commands

#### Ping

**Test connectivity:**

```
SW1# ping 192.168.1.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/2/4 ms
```

**Symbols:**

```
!  = Success (reply received)
.  = Timeout (no reply)
U  = Destination unreachable
C  = Congestion experienced
I  = User interrupted test
?  = Unknown packet type
&  = Packet lifetime exceeded
```

#### Traceroute

**Track path to destination:**

```
SW1# traceroute 8.8.8.8
```

#### Show Controllers

```
SW1# show controllers
```

#### Debug (ระวัง - ใช้ CPU สูง)

```
SW1# debug [options]

# Stop debugging
SW1# no debug all
หรือ
SW1# undebug all
```

---

## Summary (สรุป)

Module 7 นี้เราได้เรียนรู้:

1. ✅ **Ethernet Frame Structure** - Preamble, MAC addresses, Type, Data, FCS
2. ✅ **Frame Size** - Minimum 64 bytes, Maximum 1518 bytes, Jumbo frames
3. ✅ **Frame Types** - Ethernet II, IEEE 802.3, 802.1Q (VLAN tagging)
4. ✅ **MAC Address Table** - Learning, Forwarding, Filtering, Aging
5. ✅ **Forwarding Methods** - Store-and-Forward, Cut-Through, Fragment-Free
6. ✅ **Collision Domain** - แยกโดย Switch port, Full-duplex = no collision
7. ✅ **Broadcast Domain** - แยกโดย Router หรือ VLAN
8. ✅ **Switch Boot Process** - POST, Boot Loader, IOS, Configuration
9. ✅ **Memory Types** - ROM, Flash, NVRAM, RAM
10. ✅ **Configuration Files** - startup-config (NVRAM), running-config (RAM)
11. ✅ **Initial Configuration** - Hostname, Passwords, Management IP, SSH
12. ✅ **Verification Commands** - show version, show interfaces, show mac address-table

**สิ่งสำคัญที่ต้องจำ:**

- Ethernet = ใช้มากที่สุดใน LAN
- Ethernet II frame = ใช้มากที่สุดในปัจจุบัน
- MAC address table = Switch เรียนรู้จาก Source MAC, forward ตาม Destination MAC
- Store-and-Forward = Default, reliable, error checking
- Collision domain = แยกโดย Switch (1 per port), Full-duplex = no collision
- Broadcast domain = แยกโดย Router/VLAN
- startup-config (NVRAM) = saved, running-config (RAM) = active
- SSH > Telnet (encrypted vs plaintext)

**Next Module:** Module 8 - Network Layer

---

**[ไฟล์ Module 7 สมบูรณ์แล้ว!]**