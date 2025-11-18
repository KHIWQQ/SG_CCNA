# CCNA Course 1 - Module 4: Physical Layer

## ชั้นกายภาพ

---

## 4.1 Purpose of the Physical Layer (วัตถุประสงค์ของชั้นกายภาพ)

### Physical Layer (Layer 1)

**คำจำกัดความ:**

- ชั้นล่างสุดของ OSI Model
- รับผิดชอบการส่งสัญญาณทางกายภาพ
- ทำงานกับ **Bits** (0 และ 1)
- เป็นตัวเชื่อมระหว่าง software และ hardware

### หน้าที่หลัก:

#### 1. Physical Components (ส่วนประกอบทางกายภาพ)

**อุปกรณ์:**

- **NIC (Network Interface Card)** - การ์ดเครือข่าย
- **Ports and Interfaces** - พอร์ตและอินเทอร์เฟซ
- **Cables** - สายเคเบิล (UTP, Fiber, Coaxial)
- **Connectors** - หัวต่อ (RJ-45, SC, LC, ST)
- **Transceivers** - ตัวแปลงสัญญาณ (SFP, SFP+)

#### 2. Encoding (การเข้ารหัส)

**คำจำกัดความ:**

- แปลง frame (Layer 2) เป็นสัญญาณที่ส่งได้
- แปลง bits เป็นรูปแบบทางไฟฟ้า แสง หรือคลื่นวิทยุ

**ประเภทของ Encoding:**

**Manchester Encoding:**

```
Bit 1: High-to-Low transition (ขาลงตอนกลาง bit)
Bit 0: Low-to-High transition (ขาขึ้นตอนกลาง bit)
```

- ใช้ใน 10 Mbps Ethernet (10BASE-T)
- Self-clocking (มี clock signal ในตัว)
- ง่ายต่อการ synchronize

**4B/5B Encoding:**

- แปลง 4 bits → 5 bits
- ใช้ใน 100BASE-TX (Fast Ethernet)
- ป้องกัน long strings ของ 0s หรือ 1s
- รักษา clock synchronization

**8B/10B Encoding:**

- แปลง 8 bits → 10 bits
- ใช้ใน Gigabit Ethernet และ Fiber Channel
- DC balance (จำนวน 1s และ 0s สมดุล)
- Error detection

#### 3. Signaling (การส่งสัญญาณ)

**Copper Cable Signaling (สัญญาณไฟฟ้า):**

- ใช้ **Voltage changes** แทน 0 และ 1
- ตัวอย่าง: +5V = 1, -5V = 0
- แรงดันไฟฟ้าเป็นตัวแทนข้อมูล

**Fiber Optic Signaling (สัญญาณแสง):**

- ใช้ **Pulses of light**
- แสงมี (On) = 1
- แสงไม่มี (Off) = 0
- ใช้ LED หรือ Laser

**Wireless Signaling (สัญญาณคลื่นวิทยุ):**

- ใช้ **Radio frequency patterns**
- **Amplitude Modulation (AM)** - เปลี่ยนความสูงของคลื่น
- **Frequency Modulation (FM)** - เปลี่ยนความถี่
- **Phase Modulation** - เปลี่ยนเฟส

### Physical Layer Standards (มาตรฐาน)

**องค์กรที่กำหนดมาตรฐาน:**

**ISO (International Organization for Standardization)**

- มาตรฐานสากล
- OSI Model

**TIA/EIA (Telecommunications Industry Association / Electronic Industries Association)**

- มาตรฐานสาย UTP
- TIA/EIA-568A, 568B
- มาตรฐาน cabling infrastructure

**ITU-T (International Telecommunication Union - Telecommunication Standardization Sector)**

- มาตรฐาน telecommunications
- DSL, ISDN

**IEEE (Institute of Electrical and Electronics Engineers)**

- 802.3 - Ethernet
- 802.11 - Wireless LAN (Wi-Fi)
- 802.15 - Wireless PAN (Bluetooth)

**ANSI (American National Standards Institute)**

- มาตรฐานอเมริกัน
- FDDI (Fiber Distributed Data Interface)

**FCC (Federal Communications Commission)**

- ควบคุมคลื่นความถี่ในสหรัฐอเมริกา
- กำหนด wireless frequency allocation

---

## 4.2 Physical Layer Characteristics (ลักษณะของชั้นกายภาพ)

### Three Functional Areas:

#### 1. Physical Components (ส่วนประกอบทางกายภาพ)

**NIC (Network Interface Card):**

- ติดตั้งในคอมพิวเตอร์หรือเซิร์ฟเวอร์
- มี MAC address ฝังอยู่
- ประเภท:
    - **Wired NIC** - RJ-45 port
    - **Wireless NIC** - Antenna
    - **Fiber NIC** - LC/SC port

**Cables:**

- **Copper** - UTP, STP, Coaxial
- **Fiber Optic** - SMF, MMF
- **Wireless** - ไม่มีสาย

**Connectors:**

- **RJ-45** - UTP cable
- **RJ-11** - Telephone
- **SC, LC, ST** - Fiber optic
- **BNC** - Coaxial (ล้าสมัย)

**Transceivers:**

- **SFP (Small Form-factor Pluggable)** - 1 Gbps
- **SFP+** - 10 Gbps
- **QSFP** - 40 Gbps
- **QSFP28** - 100 Gbps

**Hubs:**

- Layer 1 device
- Broadcast ไปทุก ports
- Half-duplex
- ล้าสมัย (ไม่ใช้แล้ว)

**Repeaters:**

- ขยายสัญญาณ
- ยืดระยะทาง
- Layer 1 device

**Modems:**

- MOdulator-DEModulator
- แปลงสัญญาณ digital ↔ analog
- ประเภท:
    - **DSL modem** - ผ่านสายโทรศัพท์
    - **Cable modem** - ผ่านสาย Coaxial
    - **Fiber modem (ONT)** - Optical Network Terminal

#### 2. Encoding Techniques (เทคนิคการเข้ารหัส)

**Manchester Encoding:**

- ใช้ใน 10BASE-T Ethernet
- แต่ละ bit มี transition ตรงกลาง
- Bit 1: High→Low
- Bit 0: Low→High
- ข้อดี: Self-clocking, ง่ายต่อการ implement
- ข้อเสีย: ต้องการ bandwidth 2 เท่า

**4B/5B Encoding:**

- ใช้ใน 100BASE-TX, 100BASE-FX
- แปลง 4 data bits → 5 code bits
- ป้องกัน long runs ของ 0s
- Efficiency: 80% (4/5)
- รักษา clock synchronization

**8B/10B Encoding:**

- ใช้ใน Gigabit Ethernet, Fiber Channel, SATA
- แปลง 8 data bits → 10 code bits
- DC balance (จำนวน 1s และ 0s เท่ากัน)
- Efficiency: 80% (8/10)
- Error detection capability
- Running disparity control

**PAM5 (Pulse Amplitude Modulation - 5 levels):**

- ใช้ใน 1000BASE-T (Gigabit Ethernet over copper)
- 5 voltage levels: -2, -1, 0, +1, +2
- ส่ง 4 bits พร้อมกัน 4 คู่สาย
- Complex แต่ efficient

#### 3. Signaling Methods (วิธีการส่งสัญญาณ)

**Copper Signaling:**

- **Baseband** - สัญญาณดิจิทัลโดยตรง
- **Voltage levels** แทน 0 และ 1
- ตัวอย่าง:
    - 10BASE-T: Manchester encoding
    - 100BASE-TX: MLT-3 (Multi-Level Transmit)
    - 1000BASE-T: PAM5

**Fiber Optic Signaling:**

- **Light pulses** (พัลส์แสง)
- Light ON = 1
- Light OFF = 0
- แหล่งกำเนิดแสง:
    - **LED** - Multi-Mode Fiber
    - **Laser** - Single-Mode Fiber

**Wireless Signaling:**

- **Radio Frequency (RF)** patterns
- **Modulation techniques:**
    - **ASK (Amplitude Shift Keying)** - เปลี่ยนแอมพลิจูด
    - **FSK (Frequency Shift Keying)** - เปลี่ยนความถี่
    - **PSK (Phase Shift Keying)** - เปลี่ยนเฟส
    - **QAM (Quadrature Amplitude Modulation)** - เปลี่ยนทั้งแอมพลิจูดและเฟส

### Bandwidth และ Throughput

#### Bandwidth (แบนด์วิดท์)

**คำจำกัดความ:**

- ความจุสูงสุดของสื่อ (Maximum capacity)
- ข้อมูลมากที่สุดที่ส่งได้ในหนึ่งหน่วยเวลา
- วัดเป็น **bits per second (bps)**

**หน่วยวัด:**

- **bps** - bits per second
- **Kbps** - Kilobits per second (1,000 bps)
- **Mbps** - Megabits per second (1,000,000 bps)
- **Gbps** - Gigabits per second (1,000,000,000 bps)
- **Tbps** - Terabits per second (1,000,000,000,000 bps)

**หมายเหตุ:**

- ใช้ **lowercase "b"** = bits
- ใช้ **uppercase "B"** = bytes (8 bits)
- 100 Mbps = 12.5 MBps

**ปัจจัยที่มีผลต่อ Bandwidth:**

1. **ประเภทของสื่อ**
    
    - Copper: 10 Mbps - 10 Gbps
    - Fiber: 100 Mbps - 400 Gbps+
    - Wireless: 54 Mbps - 9.6 Gbps
2. **ระยะทาง**
    
    - ยิ่งไกล bandwidth ที่ได้จริงยิ่งลดลง
    - เพราะ attenuation และ signal degradation
3. **EMI/RFI**
    
    - สัญญาณรบกวนลด effective bandwidth
4. **Physical properties**
    
    - Frequency ที่ใช้
    - วัสดุของสื่อ

#### Throughput (ทรูพุต)

**คำจำกัดความ:**

- ปริมาณข้อมูลจริงที่ส่งได้ (Actual data transfer rate)
- **มักน้อยกว่า Bandwidth เสมอ**
- วัดเป็น bps เหมือน bandwidth

**ปัจจัยที่มีผลต่อ Throughput:**

1. **ปริมาณ Traffic**
    
    - เครือข่ายแออัด → throughput ลดลง
    - Collision, retransmission
2. **ประเภทของ Traffic**
    
    - Real-time traffic (VoIP, Video) มี priority
    - Bulk data transfer
3. **Latency**
    
    - ความหน่วง (delay)
    - Round-trip time (RTT)
4. **จำนวน Network Devices**
    
    - ยิ่งผ่าน device มาก ยิ่งช้า
    - Processing delay
5. **Server/Client Specifications**
    
    - CPU, RAM, Disk speed
    - NIC speed
6. **Overhead**
    
    - Protocol headers (TCP/IP, Ethernet)
    - ข้อมูลที่ไม่ใช่ user data

**ตัวอย่าง:**

```
Scenario: 100 Mbps Ethernet connection

Bandwidth:    100 Mbps (ทฤษฎี)
Throughput:   85 Mbps (จริง - มี traffic อื่นๆ)
                       (ประมาณ 85% ของ bandwidth)
```

#### Goodput (กู๊ดพุต)

**คำจำกัดความ:**

- ข้อมูลที่ใช้งานได้จริง (Usable data)
- Throughput ลบ Protocol overhead
- **น้อยที่สุดใน 3 ตัว**
- เป็นข้อมูลที่ application ใช้จริง

**สูตร:**

```
Goodput = Throughput - Overhead

Overhead includes:
- TCP/IP headers
- Ethernet headers/trailers
- Acknowledgments
- Retransmissions
```

**ตัวอย่างเปรียบเทียบ:**

```
100 Mbps Connection:

Bandwidth:   100 Mbps  (ความจุสูงสุด)
              ↓
Throughput:  80 Mbps   (ข้อมูลจริงที่ส่ง รวม headers)
              ↓
Goodput:     65 Mbps   (ข้อมูลที่ใช้งานได้ ไม่รวม overhead)

User experience: Download 65 Mbps
```

**เปอร์เซ็นต์โดยทั่วไป:**

- Goodput ≈ 70-80% ของ Bandwidth
- ขึ้นกับ protocol และ network conditions

---

## 4.3 Copper Cabling (สายทองแดง)

### ข้อดีของ Copper Cable:

- ✅ **ราคาถูก** - เมื่อเทียบกับ fiber
- ✅ **ติดตั้งง่าย** - ไม่ต้องการทักษะพิเศษมาก
- ✅ **ใช้งานทั่วไป** - มีมาตรฐาน
- ✅ **ซ่อมแซมง่าย** - หาช่างได้ง่าย
- ✅ **อุปกรณ์หาง่าย** - มีขายทั่วไป

### ข้อเสียของ Copper Cable:

- ❌ **ระยะทางจำกัด** - สูงสุด 100 เมตร
- ❌ **รับผลกระทบจาก EMI/RFI** - สัญญาณรบกวน
- ❌ **Attenuation สูง** - สัญญาณลดลงเร็ว
- ❌ **ความเร็วจำกัด** - เทียบกับ fiber
- ❌ **ความปลอดภัย** - ดักฟังได้ง่ายกว่า fiber

### Types of Copper Cable

---

## 1. UTP (Unshielded Twisted Pair)

### ลักษณะทั่วไป:

- **ไม่มี** ฉนวนป้องกันสัญญาณรบกวน
- มี **4 คู่สาย** (8 เส้น) บิดเป็นเกลียว
- สายแต่ละคู่มีสีต่างกัน:
    - คู่ 1: Blue/White-Blue
    - คู่ 2: Orange/White-Orange
    - คู่ 3: Green/White-Green
    - คู่ 4: Brown/White-Brown
- ใช้งานทั่วไปที่สุดใน LAN
- ราคาถูก
- **ระยะทางสูงสุด: 100 เมตร**

### การบิดสาย (Twisting):

**วัตถุประสงค์:**

1. **ลด Crosstalk** - สัญญาณรบกวนระหว่างสายคู่
2. **ลด EMI/RFI** - สัญญาณรบกวนจากภายนอก
3. **ปรับปรุง Signal Quality**

**หลักการทำงาน:**

- สายแต่ละคู่มีจำนวนรอบบิดต่างกัน
- ยิ่งบิดมาก ยิ่งลด interference ได้ดี
- สายคู่ที่อยู่ใกล้กันมีรอบบิดต่างกันเพื่อลด crosstalk

**ตัวอย่างจำนวนรอบบิด (ต่อเมตร):**

- Cat 5: 3-4 twists/inch
- Cat 6: 4-5 twists/inch
- Cat 6a: 5-6 twists/inch

### UTP Categories (Cat)

---

#### Cat 5 (ล้าสมัย)

**Specifications:**

- **Bandwidth:** 100 MHz
- **Maximum Speed:** 100 Mbps
- **Maximum Distance:** 100 meters
- **Pairs Used:** 2 pairs (4 wires) for 100 Mbps
- **Applications:**
    - 10BASE-T (10 Mbps Ethernet)
    - 100BASE-TX (Fast Ethernet)

**สถานะ:**

- **ล้าสมัย** - ไม่แนะนำใช้แล้ว
- ถูกแทนที่ด้วย Cat 5e
- อาจยังพบในอาคารเก่า

---

#### Cat 5e (Category 5 Enhanced)

**Specifications:**

- **Bandwidth:** 100 MHz
- **Maximum Speed:** 1 Gbps (1000 Mbps)
- **Maximum Distance:** 100 meters
- **Pairs Used:** 4 pairs (8 wires) for Gigabit
- **Applications:**
    - 10BASE-T (10 Mbps)
    - 100BASE-TX (100 Mbps)
    - 1000BASE-T (1 Gbps) ✓

**ข้อดีเหนือ Cat 5:**

- ลด **Crosstalk** ได้ดีกว่า
- ลด **Delay skew**
- รองรับ Gigabit Ethernet
- ใช้ 4 คู่สายทั้งหมด

**สถานะ:**

- **ใช้งานทั่วไปที่สุด**
- Standard สำหรับ home/office
- ราคาถูก คุ้มค่า

---

#### Cat 6

**Specifications:**

- **Bandwidth:** 250 MHz
- **Maximum Speed:**
    - 1 Gbps: 100 meters
    - 10 Gbps: 55 meters (สั้น!)
- **Applications:**
    - 1000BASE-T (1 Gbps)
    - 10GBASE-T (10 Gbps) - ระยะสั้น

**ลักษณะพิเศษ:**

- มี **Separator** (เยื่อพลาสติก) แบ่งคู่สาย
- Tighter twist specifications
- Thicker wire gauge
- ลด crosstalk ได้ดีกว่า Cat 5e

**สถานะ:**

- **แนะนำสำหรับ new installations**
- Future-proof สำหรับ 10 Gigabit (ระยะสั้น)
- ราคาสูงกว่า Cat 5e เล็กน้อย

---

#### Cat 6a (Category 6 Augmented)

**Specifications:**

- **Bandwidth:** 500 MHz
- **Maximum Speed:** 10 Gbps
- **Maximum Distance:** 100 meters (เต็มระยะ!)
- **Applications:**
    - 10GBASE-T (10 Gbps) ✓ เต็มระยะ 100m

**ลักษณะพิเศษ:**

- **Shielding** (F/UTP หรือ U/FTP)
- Thicker และ heavier กว่า Cat 6
- ลด **Alien Crosstalk** (AXT) - รบกวนระหว่างสายหลายเส้น
- Tighter tolerances

**ข้อดี:**

- รองรับ 10 Gbps เต็มระยะ 100 เมตร
- Backwards compatible กับ Cat 6, 5e
- เหมาะสำหรับ data center

**ข้อเสีย:**

- ราคาแพง
- ติดตั้งยาก (สายหนา งอยาก)
- Termination ต้องการความระมัดระวัง

**สถานะ:**

- **Standard สำหรับ Enterprise และ Data Center**
- แนะนำสำหรับโครงการใหม่ที่ต้องการ 10G

---

#### Cat 7 (ไม่นิยม)

**Specifications:**

- **Bandwidth:** 600 MHz
- **Maximum Speed:** 10 Gbps
- **Maximum Distance:** 100 meters
- **Shielding:** S/FTP (Shielded Foiled Twisted Pair)

**ลักษณะพิเศษ:**

- Shielding รอบทุกคู่สาย
- Overall shield
- ต้องการ grounded installation

**ข้อเสีย:**

- **ไม่ใช้ RJ-45 connector มาตรฐาน**
- ต้องใช้ GG45 หรือ TERA connector
- ไม่เป็นมาตรฐาน TIA/EIA
- ราคาแพงมาก

**สถานะ:**

- **ใช้น้อยมาก ไม่นิยม**
- Replaced by Cat 6a และ Cat 8

---

#### Cat 8 (ใหม่ล่าสุด)

**Specifications:**

- **Bandwidth:** 2000 MHz (2 GHz)
- **Maximum Speed:** 40 Gbps
- **Maximum Distance:** **30 meters** (สั้นมาก!)
- **Applications:**
    - 40GBASE-T
    - Data Center short runs
    - Server to switch connections

**ลักษณะพิเศษ:**

- Shielded (F/FTP หรือ S/FTP)
- ใช้ RJ-45 connector (ใช้ได้กับอุปกรณ์เดิม)
- Very tight specifications
- สำหรับระยะสั้นเท่านั้น

**Two versions:**

- **Cat 8.1** - compatible with Cat 6a
- **Cat 8.2** - compatible with Cat 7

**สถานะ:**

- **ใหม่ล่าสุด**
- สำหรับ Data Center เท่านั้น
- ไม่เหมาะสำหรับ general LAN

---

### UTP Summary Table

|Category|Bandwidth|Max Speed|Distance|Shielding|Common Use|Status|
|---|---|---|---|---|---|---|
|**Cat 5**|100 MHz|100 Mbps|100 m|No|Fast Ethernet|Obsolete|
|**Cat 5e**|100 MHz|1 Gbps|100 m|No|Gigabit Ethernet|**Most Common**|
|**Cat 6**|250 MHz|10 Gbps|55 m|Optional|10G short runs|Recommended|
|**Cat 6a**|500 MHz|10 Gbps|100 m|Yes|10G full distance|**Enterprise**|
|**Cat 7**|600 MHz|10 Gbps|100 m|Yes|Specialized|Rare|
|**Cat 8**|2000 MHz|40 Gbps|30 m|Yes|Data Center|Latest|

**คำแนะนำการเลือกใช้:**

- **Home/Small Office:** Cat 5e (เพียงพอ)
- **New installations:** Cat 6 (future-proof)
- **Enterprise/10G needed:** Cat 6a
- **Data Center:** Cat 6a หรือ Cat 8

---

## 2. STP (Shielded Twisted Pair)

### ลักษณะทั่วไป:

- **มี** ฉนวนป้องกันสัญญาณรบกวน
- มี 4 คู่สาย เหมือน UTP
- เพิ่ม shielding layer

### ประเภทของ Shielding:

**Naming Convention: X/YTP**

- **X** = Overall shield (รอบสายทั้งหมด)
- **Y** = Individual pair shield (รอบแต่ละคู่)
- **TP** = Twisted Pair

**ตัวอักษร:**

- **U** = Unshielded (ไม่มีฉนวน)
- **F** = Foil shielding (ฉนวนฟอยล์)
- **S** = Braided shielding (ฉนวนถัก)

#### U/UTP (Unshielded/Unshielded Twisted Pair)

- ไม่มี shield เลย
- = UTP ปกติ

#### F/UTP (Foiled/Unshielded Twisted Pair)

- Foil shield รอบสายทั้งหมด
- ไม่มี shield รอบสายแต่ละคู่
- ป้องกัน EMI/RFI จากภายนอก

#### S/UTP (Screened/Unshielded Twisted Pair)

- Braided shield รอบสายทั้งหมด
- คล้าย F/UTP แต่ใช้ braided แทน foil

#### F/FTP (Foiled/Foiled Twisted Pair)

- Foil shield รอบสายแต่ละคู่
- Foil shield รอบสายทั้งหมด
- ป้องกันดีทั้งภายในและภายนอก

#### S/FTP (Screened/Foiled Twisted Pair)

- Foil shield รอบสายแต่ละคู่
- Braided shield รอบสายทั้งหมด
- **ป้องกันดีที่สุด**

### ข้อดี:

- ✅ ป้องกัน EMI/RFI ได้ดีกว่า UTP
- ✅ ลด crosstalk ได้ดีกว่า
- ✅ เหมาะกับสภาพแวดล้อมที่มีสัญญาณรบกวนสูง
- ✅ Signal quality ดีกว่า

### ข้อเสีย:

- ❌ ราคาแพงกว่า UTP (30-50% แพงกว่า)
- ❌ ติดตั้งยากกว่า (สายหนัก งอยาก)
- ❌ **ต้องต่อ shield กับ ground** (สำคัญมาก!)
- ❌ Connector แพงกว่า (Shielded RJ-45)
- ❌ ถ้า ground ไม่ดี อาจแย่กว่า UTP

### การใช้งาน:

**สภาพแวดล้อมที่เหมาะสม:**

- โรงงานอุตสาหกรรม (มอเตอร์ เครื่องจักร)
- ห้อง Server ที่มี EMI สูง
- พื้นที่ใกล้อุปกรณ์ไฟฟ้าขนาดใหญ่
- Data Center
- Medical facilities (MRI, X-ray machines)
- ใกล้สนามบิน (Radar)

**การติดตั้ง:**

- Shield ต้องต่อ ground ที่ปลายทั้งสองข้าง
- Ground ต้องดี (Low impedance)
- ใช้ Shielded connectors และ patch panels

---

## 3. Coaxial Cable (สายโคแอกเชียล)

### โครงสร้าง:

```
[Outer Jacket] - ฉนวนนอกสุด (PVC/Plenum)
  [Braided Shield] - ตัวนำถักป้องกันสัญญาณรบกวน
    [Dielectric Insulator] - ฉนวนคั่นกลาง
      [Center Conductor] - ตัวนำแกนกลาง (Copper)
```

### ลักษณะ:

- มี **2 ตัวนำ**: แกนกลางและ shield
- Coaxial = อยู่บนแกนเดียวกัน
- Shield ป้องกัน EMI/RFI ได้ดี
- ใช้ **BNC connector**

### ประเภท:

#### RG-58 (Thinnet - 10BASE2)

**Specifications:**

- Impedance: 50 ohms
- Speed: 10 Mbps
- Distance: 185 meters
- Connector: BNC
- Topology: Bus

**สถานะ:** ล้าสมัย ไม่ใช้ใน LAN แล้ว

#### RG-8 (Thicknet - 10BASE5)

**Specifications:**

- Impedance: 50 ohms
- Speed: 10 Mbps
- Distance: 500 meters
- Connector: Vampire tap
- Topology: Bus

**สถานะ:** ล้าสมัยมาก

#### RG-59

**Specifications:**

- Impedance: 75 ohms
- ใช้สำหรับ: Analog video, CCTV
- ไม่ใช้สำหรับ data networking

#### RG-6

**Specifications:**

- Impedance: 75 ohms
- ใช้สำหรับ:
    - **Cable TV**
    - **Cable Internet (DOCSIS)**
    - **Satellite TV**
- Thicker กว่า RG-59
- Signal loss ต่ำกว่า

### การใช้งานปัจจุบัน:

**ยังใช้อยู่:**

- ✅ **Cable TV (CATV)**
- ✅ **Cable Internet** - Cable modem (DOCSIS 3.0, 3.1)
- ✅ **Analog CCTV**
- ✅ **Satellite TV**

**ไม่ใช้แล้ว:**

- ❌ **LAN** - ถูกแทนที่ด้วย UTP และ Fiber

### ข้อดี:

- ป้องกัน EMI/RFI ได้ดี
- ระยะทางไกลกว่า UTP (สมัยก่อน)
- Bandwidth สูง (สำหรับ cable internet)

### ข้อเสีย:

- ติดตั้งยาก งอยาก
- ราคาแพง
- Topology แบบ bus ไม่ flexible
- ถูกแทนที่ด้วย UTP ใน LAN

---

## 4.4 UTP Cable Termination (การต่อหัวสาย UTP)

### TIA/EIA-568 Standards

มาตรฐานสำหรับการต่อหัว RJ-45 connector บนสาย UTP

---

### T568A Standard

**Pin Assignment:**

```
Pin 1: White/Green   (Pair 3)
Pin 2: Green         (Pair 3)
Pin 3: White/Orange  (Pair 2)
Pin 4: Blue          (Pair 1)
Pin 5: White/Blue    (Pair 1)
Pin 6: Orange        (Pair 2)
Pin 7: White/Brown   (Pair 4)
Pin 8: Brown         (Pair 4)
```

**Wire Pairs:**

- **Pair 1:** Blue/White-Blue (Pins 4,5)
- **Pair 2:** Orange/White-Orange (Pins 3,6)
- **Pair 3:** Green/White-Green (Pins 1,2)
- **Pair 4:** Brown/White-Brown (Pins 7,8)

**การใช้งาน:**

- มาตรฐานเก่า
- ใช้ในยุโรปและบางประเทศ
- Government installations (สหรัฐฯ)

---

### T568B Standard (นิยมใช้มากกว่า)

**Pin Assignment:**

```
Pin 1: White/Orange  (Pair 2)
Pin 2: Orange        (Pair 2)
Pin 3: White/Green   (Pair 3)
Pin 4: Blue          (Pair 1)
Pin 5: White/Blue    (Pair 1)
Pin 6: Green         (Pair 3)
Pin 7: White/Brown   (Pair 4)
Pin 8: Brown         (Pair 4)
```

**Wire Pairs:**

- **Pair 1:** Blue/White-Blue (Pins 4,5) - เหมือน T568A
- **Pair 2:** Orange/White-Orange (Pins 1,2) - **สลับกับ T568A**
- **Pair 3:** Green/White-Green (Pins 3,6) - **สลับกับ T568A**
- **Pair 4:** Brown/White-Brown (Pins 7,8) - เหมือน T568A

**การใช้งาน:**

- **มาตรฐานที่นิยมใช้ที่สุด**
- ใช้ในสหรัฐอเมริกา เอเชีย
- Commercial installations

---

### ความแตกต่างระหว่าง T568A และ T568B:

**สิ่งที่เหมือนกัน:**

- Pair 1 (Blue) - Pins 4,5
- Pair 4 (Brown) - Pins 7,8

**สิ่งที่ต่างกัน:**

- **T568A:** Green ที่ Pins 1,2 / Orange ที่ Pins 3,6
- **T568B:** Orange ที่ Pins 1,2 / Green ที่ Pins 3,6

**สรุป:** สลับตำแหน่ง Orange กับ Green (Pins 1,2,3,6 เท่านั้น)

**คำแนะนำ:**

- เลือกใช้มาตรฐานใดมาตรฐานหนึ่ง **สม่ำเสมอ**
- T568B นิยมใช้มากกว่า
- ไม่ควรผสมกันในองค์กรเดียว

---

### Cable Types (ประเภทของสาย)

## 1. Straight-Through Cable (สายตรง)

**การต่อหัว:**

- ปลายทั้งสองข้างใช้มาตรฐาน**เดียวกัน**
- **T568B ─────── T568B** (นิยม)
- หรือ T568A ─────── T568A

**Pin Mapping:**

```
End A          End B
Pin 1 ──────── Pin 1
Pin 2 ──────── Pin 2
Pin 3 ──────── Pin 3
Pin 4 ──────── Pin 4
Pin 5 ──────── Pin 5
Pin 6 ──────── Pin 6
Pin 7 ──────── Pin 7
Pin 8 ──────── Pin 8
```

**การใช้งาน - เชื่อม อุปกรณ์ต่างชนิด:**

- ✅ Router → Switch
- ✅ Switch → PC
- ✅ Switch → Server
- ✅ Switch → Printer
- ✅ Hub → PC
- ✅ Router → PC (ในบางกรณี)

**หลักจำ: DTE ↔ DCE**

```
DTE (Data Terminal Equipment):
- PC, Server, Printer, IP Phone

DCE (Data Communication Equipment):
- Switch, Hub, Router

เชื่อม DTE ↔ DCE ใช้ Straight-Through
```

---

## 2. Crossover Cable (สายไขว้)

**การต่อหัว:**

- ปลายละข้างใช้คนละมาตรฐาน
- **T568A ─────── T568B**

**Pin Mapping:**

```
End A (T568A)     End B (T568B)
Pin 1 (Grn/Wht) ─ Pin 3 (Grn/Wht)
Pin 2 (Green)   ─ Pin 6 (Green)
Pin 3 (Org/Wht) ─ Pin 1 (Org/Wht)
Pin 4 (Blue)    ─ Pin 4 (Blue)
Pin 5 (Blu/Wht) ─ Pin 5 (Blu/Wht)
Pin 6 (Orange)  ─ Pin 2 (Orange)
Pin 7 (Brn/Wht) ─ Pin 7 (Brn/Wht)
Pin 8 (Brown)   ─ Pin 8 (Brown)
```

**สายที่สลับ:**

- Pin 1 ↔ Pin 3
- Pin 2 ↔ Pin 6

**การใช้งาน - เชื่อม อุปกรณ์ชนิดเดียวกัน:**

- ✅ Switch → Switch
- ✅ Router → Router
- ✅ PC → PC
- ✅ Hub → Hub
- ✅ Hub → Switch

**หลักจำ: DTE ↔ DTE หรือ DCE ↔ DCE**

```
เชื่อม DTE ↔ DTE ใช้ Crossover
เชื่อม DCE ↔ DCE ใช้ Crossover
```

**หมายเหตุสำคัญ:**

> **ไม่จำเป็นในปัจจุบัน!**
> 
> อุปกรณ์รุ่นใหม่มี **Auto-MDIX** ตรวจจับและปรับอัตโนมัติ ใช้สายแบบไหนก็ได้

---

## 3. Rollover Cable (Console Cable / สายคอนโซล)

**การต่อหัว:**

- Pin ทุกตัวสลับกลับหมด
- Pin 1 ↔ Pin 8
- Pin 2 ↔ Pin 7
- และอื่นๆ

**Pin Mapping:**

```
End A          End B
Pin 1 ──────── Pin 8
Pin 2 ──────── Pin 7
Pin 3 ──────── Pin 6
Pin 4 ──────── Pin 5
Pin 5 ──────── Pin 4
Pin 6 ──────── Pin 3
Pin 7 ──────── Pin 2
Pin 8 ──────── Pin 1
```

**หรือคิดแบบนี้:**

- ถ้าวางสายสองข้างขนานกัน
- สีจะเรียงกลับกัน **พอดี**

**Connector:**

- ปลายหนึ่ง: RJ-45 (ไปอุปกรณ์)
- อีกปลาย: DB-9 (COM port) หรือ USB (ปลาย PC)

**สี:**

- มักเป็นสี **ฟ้าอ่อน** (Light Blue/Cyan)
- ทำให้แยกจากสาย Ethernet ทั่วไปได้ง่าย

**การใช้งาน:**

- ✅ เชื่อมต่อ **Console port** ของ Router/Switch
- ✅ PC/Laptop → Console port (RJ-45)
- ✅ **สำหรับการตั้งค่า configuration**
- ✅ Initial setup
- ✅ Password recovery
- ✅ Troubleshooting

**Terminal Settings:**

```
Baud rate: 9600
Data bits: 8
Parity: None
Stop bits: 1
Flow control: None
```

**Terminal Software:**

- Windows: PuTTY, Tera Term, SecureCRT
- macOS/Linux: screen, minicom, cu

---

## Auto-MDIX (Automatic Medium-Dependent Interface Crossover)

### ความหมาย:

- Automatic = อัตโนมัติ
- Medium-Dependent Interface = อินเทอร์เฟซที่ขึ้นกับสื่อ
- Crossover = การสลับสาย

### การทำงาน:

1. **Auto-negotiation** - เจรจาความเร็วและ duplex
2. **Polarity detection** - ตรวจจับขั้วของสัญญาณ
3. **Automatic adjustment** - ปรับ TX/RX อัตโนมัติ

### ข้อดี:

- ✅ **ไม่ต้องกังวล** ว่าจะใช้ Straight-through หรือ Crossover
- ✅ ใช้สายแบบไหนก็ทำงานได้
- ✅ ลดความผิดพลาดในการติดตั้ง
- ✅ ประหยัดเวลา
- ✅ ไม่ต้องมีสาย Crossover สำรอง

### อุปกรณ์ที่รองรับ:

- ✅ **Gigabit Ethernet ทุกตัว** (1000BASE-T)
- ✅ Router/Switch รุ่นใหม่ (2000+)
- ✅ Fast Ethernet รุ่นใหม่ (100BASE-TX)
- ❌ อุปกรณ์เก่า (10BASE-T) อาจไม่รองรับ

### Enable/Disable (Cisco):

```
Switch(config)# interface fastEthernet 0/1
Switch(config-if)# mdix auto          (Enable - Default)
Switch(config-if)# no mdix auto       (Disable)
Switch(config-if)# exit
```

**ตรวจสอบสถานะ:**

```
Switch# show controllers ethernet-controller fa0/1 phy | include MDIX
```

### สรุป:

> **ในปัจจุบัน:**
> 
> - Straight-through cable เพียงอย่างเดียวก็พอ
> - ไม่ต้องมี Crossover cable
> - อุปกรณ์ปรับเองอัตโนมัติ

---

## 4.5 Copper Cable Problems (ปัญหาของสายทองแดง)

### 1. EMI (Electromagnetic Interference)

**คำจำกัดความ:**

- สัญญาณรบกวนจากแม่เหล็กไฟฟ้า
- Electric current สร้างสนามแม่เหล็ก
- สนามแม่เหล็กรบกวนสัญญาณใน cable

**แหล่งที่มา:**

- **Motors** - มอเตอร์ไฟฟ้า
- **Transformers** - หม้อแปลงไฟฟ้า
- **Fluorescent lights** - หลอดฟลูออเรสเซนต์
- **Generators** - เครื่องกำเนิดไฟฟ้า
- **Power lines** - สายไฟแรงสูง
- **Elevators** - ลิฟต์
- **Air conditioners** - แอร์
- **Welding equipment** - เครื่องเชื่อม

**ผลกระทบ:**

- สัญญาณบิดเบือน (Signal distortion)
- Data corruption
- Packet loss
- Reduced speed

**วิธีแก้:**

1. ✅ **ใช้ STP แทน UTP** - มี shielding
2. ✅ **เพิ่มระยะห่าง** จากแหล่งรบกวน (≥6 นิ้ว)
3. ✅ **ใช้ Fiber optic** - ไม่รับผลจาก EMI
4. ✅ **Use conduit** - ท่อป้องกัน
5. ✅ **Proper cable routing** - วางสายห่างจากสายไฟ
6. ✅ **Category สูงกว่า** - Cat 6, 6a มีการป้องกันดีกว่า

### 2. RFI (Radio Frequency Interference)

**คำจำกัดความ:**

- สัญญาณรบกวนจากคลื่นวิทยุ
- Radio waves รบกวนสัญญาณใน cable

**แหล่งที่มา:**

- **Radio transmitters** - เครื่องส่งวิทยุ
- **Microwave ovens** - ไมโครเวฟ
- **Cell phones** - โทรศัพท์มือถือ
- **Wi-Fi** - ไวไฟ (2.4 GHz, 5 GHz)
- **Bluetooth devices** - อุปกรณ์บลูทูธ
- **Cordless phones** - โทรศัพท์ไร้สาย
- **Baby monitors** - เครื่องติดตามเด็ก
- **Wireless security cameras** - กล้องไร้สาย

**ผลกระทบ:**

- คล้าย EMI
- สัญญาณอ่อน
- Connection unstable

**วิธีแก้:**

1. ✅ **ใช้ shielded cable**
2. ✅ **Proper grounding**
3. ✅ **Cable management** - วางสายห่างจาก wireless devices
4. ✅ **Change wireless channels** - ถ้า Wi-Fi รบกวน
5. ✅ **ใช้ Fiber optic**

### 3. Crosstalk (การรบกวนข้าม)

**คำจำกัดความ:**

- สัญญาณรบกวนระหว่างสายคู่ในสายเดียวกัน
- สัญญาณจากสายคู่หนึ่งรั่วไปยังอีกคู่หนึ่ง
- เหมือนได้ยินเสียงคนคุยสายอื่นในโทรศัพท์

**ประเภท:**

#### NEXT (Near-End Crosstalk)

**คำจำกัดความ:**

- เกิดที่ปลายใกล้ (ปลายส่ง)
- Strong signal → Weak signal

**ลักษณะ:**

- วัดได้ง่าย
- มีผลมากกว่า FEXT
- สำคัญในการทดสอบสาย

**สูตร:**

```
NEXT (dB) = Power of disturbing signal (dB) 
          - Power of crosstalk signal (dB)

ยิ่งค่า NEXT สูง (dB มาก) ยิ่งดี
```

#### FEXT (Far-End Crosstalk)

**คำจำกัดความ:**

- เกิดที่ปลายไกล (ปลายรับ)
- สัญญาณอ่อนกว่า NEXT

**ลักษณะ:**

- วัดยากกว่า NEXT
- Attenuation ทำให้อ่อนลง
- สำคัญในสาย ยาวๆ

#### Alien Crosstalk (AXT)

**คำจำกัดความ:**

- รบกวนระหว่าง**สายหลายเส้น**
- สาย cable A รบกวน cable B
- สำคัญใน bundle ที่มีสายหลายเส้น

**วิธีแก้:**

- ใช้ Cat 6a หรือสูงกว่า (มีการป้องกัน AXT)
- Space cables apart
- Proper cable management

**สาเหตุของ Crosstalk:**

1. **ไม่บิดสายให้แน่น**
2. **เปิดเกลียวมากเกินไป** ตอนต่อหัว (>1.27 cm หรือ 0.5 นิ้ว)
3. **Cable category ต่ำ**
4. **สายเสียหาย** - งอมากเกินไป
5. **Connector ไม่ดี**
6. **ไม่ follow standards**

**วิธีแก้:**

1. ✅ **บิดสายให้แน่น** - ตามมาตรฐาน
2. ✅ **ต่อหัวให้ถูกต้อง** - เปิดเกลียวน้อยที่สุด (<1.27 cm)
3. ✅ **ใช้ cable category ที่สูงกว่า** - Cat 6, 6a
4. ✅ **ใช้ quality cable และ connectors**
5. ✅ **Test หลังติดตั้ง** - ใช้ cable tester
6. ✅ **ไม่งอสายมากเกินไป** - Bend radius ตามมาตรฐาน

### 4. Attenuation (การสูญเสียสัญญาณ)

**คำจำกัดความ:**

- สัญญาณลดลงตามระยะทาง
- ยิ่งส่งไกล สัญญาณยิ่งอ่อน
- วัดเป็น **dB (decibels)**

**สาเหตุ:**

1. **ความต้านทานของสาย** (Resistance)
2. **ระยะทาง** - ยิ่งไกล ยิ่งสูญเสีย
3. **ความถี่** - Frequency สูง สูญเสียมากกว่า
4. **อุณหภูมิ** - ร้อน → ต้านทานสูง
5. **คุณภาพสาย**

**สูตร:**

```
Attenuation (dB) = 10 × log₁₀(P_out / P_in)

P_in = Power เข้า
P_out = Power ออก
```

**ตัวอย่าง:**

```
Cat 5e @ 100 MHz:
- Attenuation: 24 dB/100m
- ที่ 100m สัญญาณเหลือประมาณ 0.4% ของต้นทาง

Cat 6 @ 250 MHz:
- Attenuation: 21.3 dB/100m
- ดีกว่า Cat 5e เล็กน้อย
```

**ผลกระทบ:**

- ระยะทางเกิน 100m → ไม่ทำงาน
- Signal weak
- Data errors
- Packet loss

**วิธีแก้:**

1. ✅ **จำกัดระยะทาง ≤100m**
2. ✅ **ใช้ Repeater** เพื่อขยายสัญญาณ
3. ✅ **ใช้ cable quality ที่ดี**
4. ✅ **ใช้ category สูงกว่า**
5. ✅ **ใช้ Fiber optic** สำหรับระยะไกล (Attenuation ต่ำมาก)
6. ✅ **Check connectors** - ต่อให้ดี
7. ✅ **Avoid sharp bends** - ไม่งอมาก

### 5. Noise (สัญญาณรบกวน)

**ประเภท:**

**Impulse Noise:**

- กะทันหัน สั้น
- แหล่งที่มา: ฟ้าผ่า, เครื่องจักร start/stop, arc welding

**White Noise (Thermal Noise):**

- สม่ำเสมอ
- จากการเคลื่อนที่ของ electron
- มีอยู่เสมอ

**Cross-talk:**

- จากสายคู่อื่น
- (อธิบายแล้วข้างบน)

**วิธีแก้:**

- Proper shielding
- Good quality cable
- Grounding
- Avoid noise sources
- Use differential signaling

---

## Cable Testing (การทดสอบสาย)

### Cable Tester Types:

**1. Continuity Tester**

- ทดสอบว่าสายต่อดีไหม
- มีสัญญาณผ่านหรือไม่
- ราคาถูก

**2. Wire Map Tester**

- ทดสอบการต่อหัว
- ตรวจสอบว่า pin ต่อถูกหรือไม่
- หา short, open, crossed wires

**3. Cable Certifier (ดีที่สุด)**

- ทดสอบครบทุกอย่าง
- Attenuation, NEXT, Length, etc.
- แพงมาก (หลักแสน-ล้าน)
- ใช้ใน professional installation

**Parameters ที่ทดสอบ:**

- Wire map
- Length
- Attenuation
- NEXT
- Return loss
- Delay skew
- Resistance

---

## สรุป Copper Cabling:

|Cable Type|Shield|Max Distance|Speed|Use Case|
|---|---|---|---|---|
|**UTP Cat 5e**|No|100 m|1 Gbps|Most common LAN|
|**UTP Cat 6**|Optional|55 m (10G)|10 Gbps|New installations|
|**UTP Cat 6a**|Yes|100 m|10 Gbps|Enterprise/DC|
|**STP**|Yes|100 m|Same as UTP|High EMI areas|
|**Coaxial**|Yes|185-500 m|10 Mbps|Cable TV/Internet|

**ปัญหาหลัก:**

1. EMI/RFI - ใช้ shielded cable หรือ fiber
2. Crosstalk - ต่อหัวให้ดี, ใช้ cat สูงกว่า
3. Attenuation - จำกัด 100m, ใช้ repeater หรือ fiber
4. Distance - max 100m สำหรับ UTP

**Next Section:** Fiber Optic Cabling (ไม่มีปัญหา EMI/RFI!)

---

(จะมีส่วนต่อไปเกี่ยวกับ Fiber Optic และ Wireless)

ผมกำลังสร้างต่อครับ กำลังจะถึงส่วน Fiber Optic...