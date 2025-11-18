# CCNA Course 1 - Module 1: Networking Today

## เครือข่ายในปัจจุบัน

---

## 1.1 Networks Affect Our Lives (เครือข่ายส่งผลต่อชีวิตเรา)

### Network (เครือข่าย)

**คำจำกัดความ:**

- ระบบที่เชื่อมต่ออุปกรณ์ต่างๆ เข้าด้วยกันเพื่อแชร์ข้อมูลและทรัพยากร
- ช่วยให้เราสามารถสื่อสาร ทำงาน และเข้าถึงข้อมูลได้ทุกที่ทุกเวลา

### Types of Networks (ประเภทของเครือข่าย)

#### LAN (Local Area Network)

- **ความหมาย:** เครือข่ายท้องถิ่น
- **ขอบเขต:** ครอบคลุมพื้นที่จำกัด
- **ตัวอย่าง:** บ้าน สำนักงาน โรงเรียน อาคารเดียว
- **ลักษณะ:**
    - ความเร็วสูง (1 Gbps - 10 Gbps)
    - Latency ต่ำ
    - เป็นเจ้าของและดูแลเอง
    - ต้นทุนต่ำ

#### WAN (Wide Area Network)

- **ความหมาย:** เครือข่ายพื้นที่กว้าง
- **ขอบเขต:** เชื่อมต่อ LAN หลายๆ แห่ง ครอบคลุมระยะไกล
- **ตัวอย่าง:** ระหว่างเมือง ระหว่างประเทศ Internet
- **ลักษณะ:**
    - ครอบคลุมพื้นที่กว้าง
    - มักเช่าบริการจาก Service Provider (ISP)
    - ความเร็วต่ำกว่า LAN
    - Latency สูงกว่า
    - ต้นทุนสูง

#### MAN (Metropolitan Area Network)

- **ความหมาย:** เครือข่ายขนาดกลาง
- **ขอบเขต:** ครอบคลุมในเมืองหรือจังหวัด
- **ตัวอย่าง:** เครือข่ายเชื่อมสาขาในเมืองเดียวกัน
- **ลักษณะ:**
    - ขนาดระหว่าง LAN และ WAN
    - มักใช้ Fiber Optic

#### WLAN (Wireless LAN)

- **ความหมาย:** เครือข่าย LAN แบบไร้สาย
- **เทคโนโลยี:** Wi-Fi (IEEE 802.11)
- **ตัวอย่าง:** Wi-Fi ที่บ้าน ออฟฟิศ ร้านกาแฟ
- **ลักษณะ:**
    - ไม่ต้องใช้สาย
    - ยืดหยุ่น เคลื่อนย้ายได้
    - ความปลอดภัยน้อยกว่า Wired LAN

#### PAN (Personal Area Network)

- **ความหมาย:** เครือข่ายส่วนบุคคล
- **ขอบเขต:** รอบตัวบุคคล (ไม่กี่เมตร)
- **เทคโนโลยี:** Bluetooth, USB, NFC
- **ตัวอย่าง:**
    - มือถือเชื่อมต่อกับหูฟัง Bluetooth
    - Smartwatch เชื่อมต่อกับมือถือ
    - เมาส์และคีย์บอร์ดไร้สาย

### Network Trends (แนวโน้มเครือข่าย)

#### BYOD (Bring Your Own Device)

- นำอุปกรณ์ส่วนตัวมาใช้ทำงาน
- ความท้าทาย: Security, Compatibility, Support

#### Online Collaboration

- ทำงานร่วมกันออนไลน์
- เครื่องมือ: Video conferencing, Shared documents, Cloud storage

#### Video Communication

- การสื่อสารด้วยวิดีโอ
- ตัวอย่าง: Zoom, Microsoft Teams, Google Meet

#### Cloud Computing

- ใช้ทรัพยากรผ่าน Internet
- ประเภท:
    - **Public Cloud** - เช่น AWS, Google Cloud, Azure
    - **Private Cloud** - สำหรับองค์กรเฉพาะ
    - **Hybrid Cloud** - ผสมระหว่าง Public และ Private

---

## 1.2 Network Components (ส่วนประกอบของเครือข่าย)

### End Devices (อุปกรณ์ปลายทาง)

**คำจำกัดความ:**

- อุปกรณ์ที่เป็นจุดเริ่มต้นหรือจุดสิ้นสุดของข้อมูล
- เรียกอีกชื่อว่า **Hosts**

**ตัวอย่าง:**

- **Computer** - Desktop, Laptop
- **Server** - Web server, Email server, File server
- **Mobile Devices** - Smartphone, Tablet
- **IP Phone** - โทรศัพท์ IP
- **Printer** - เครื่องพิมพ์เครือข่าย
- **IP Camera** - กล้องวงจรปิดแบบ IP
- **Smart TV**
- **IoT Devices** - Smart home devices

### Intermediary Devices (อุปกรณ์กลาง)

**คำจำกัดความ:**

- อุปกรณ์ที่ช่วยส่งต่อข้อมูลระหว่าง End Devices
- ไม่ได้เป็นจุดเริ่มต้นหรือจุดสิ้นสุดของข้อมูล

**ประเภทและหน้าที่:**

#### Switch

- **หน้าที่:** เชื่อมต่ออุปกรณ์ใน LAN เดียวกัน
- **Layer:** Layer 2 (Data Link)
- **การทำงาน:** ใช้ MAC address ในการส่งต่อข้อมูล
- **ลักษณะ:**
    - แต่ละ port เป็น collision domain แยกกัน
    - Full-duplex
    - ความเร็วสูง

#### Router

- **หน้าที่:** เชื่อมต่อระหว่างเครือข่ายต่างๆ และตัดสินใจเส้นทางของข้อมูล
- **Layer:** Layer 3 (Network)
- **การทำงาน:** ใช้ IP address ในการ routing
- **ลักษณะ:**
    - แบ่ง broadcast domains
    - Path selection
    - Packet forwarding

#### Wireless Access Point (AP)

- **หน้าที่:** ให้บริการ Wi-Fi แก่อุปกรณ์ไร้สาย
- **การทำงาน:** แปลงสัญญาณจาก wired เป็น wireless
- **มาตรฐาน:** IEEE 802.11 (Wi-Fi)

#### Firewall

- **หน้าที่:** ป้องกันความปลอดภัยของเครือข่าย
- **การทำงาน:** กรองและควบคุม traffic ที่เข้า-ออกเครือข่าย
- **ประเภท:**
    - Hardware firewall
    - Software firewall
    - Next-Generation Firewall (NGFW)

#### Modem

- **ความหมาย:** Modulator-Demodulator
- **หน้าที่:** แปลงสัญญาณระหว่างดิจิทัลและอนาล็อก
- **ประเภท:**
    - DSL modem
    - Cable modem
    - Fiber modem (ONT)

### Network Media (สื่อเครือข่าย)

**คำจำกัดความ:**

- สื่อกลางที่ใช้ในการส่งข้อมูลระหว่างอุปกรณ์

#### 1. Copper Cable (สายทองแดง)

**ประเภท:**

- **UTP (Unshielded Twisted Pair)**
    
    - ไม่มีฉนวนกันสัญญาณรบกวน
    - ใช้ทั่วไป ราคาถูก
    - ระยะทางสูงสุด 100 เมตร
    - Categories: Cat5e, Cat6, Cat6a, Cat7, Cat8
- **STP (Shielded Twisted Pair)**
    
    - มีฉนวนป้องกันสัญญาณรบกวน
    - ใช้ในสภาพแวดล้อมที่มี EMI/RFI สูง
    - ราคาแพงกว่า UTP
- **Coaxial Cable**
    
    - มี 2 ตัวนำ: แกนกลางและตัวนำรอบนอก
    - ใช้ใน: Cable TV, Cable Internet
    - ล้าสมัยสำหรับ LAN

**ข้อดี:**

- ราคาถูก
- ติดตั้งง่าย
- Bandwidth สูงพอสมควร

**ข้อเสีย:**

- รับผลกระทบจาก EMI/RFI
- ระยะทางจำกัด (100m)
- Attenuation สูง

#### 2. Fiber Optic Cable (สายใยแก้วนำแสง)

**การทำงาน:**

- ใช้แสงในการส่งข้อมูล
- แสงเดินทางผ่าน glass/plastic core

**ประเภท:**

- **SMF (Single-Mode Fiber)**
    
    - แกนเล็ก (8-10 microns)
    - ใช้แสง Laser
    - ระยะทางไกลมาก (10-100+ km)
    - ราคาแพง
    - ใช้ใน WAN, ISP backbone
    - สีเหลือง
- **MMF (Multi-Mode Fiber)**
    
    - แกนใหญ่ (50 หรือ 62.5 microns)
    - ใช้แสง LED
    - ระยะทางสั้นกว่า (500-2000m)
    - ราคาถูกกว่า SMF
    - ใช้ใน LAN, Building backbone
    - สีส้มหรือฟ้า

**ข้อดี:**

- ความเร็วสูงมาก (10 Gbps - 100 Gbps+)
- ระยะทางไกล
- ไม่รับผลกระทบจาก EMI/RFI
- Bandwidth สูง
- ปลอดภัยกว่า (ดักฟังยาก)
- Attenuation ต่ำ

**ข้อเสีย:**

- ราคาแพง
- ติดตั้งยาก ต้องการทักษะพิเศษ
- อุปกรณ์แพง

#### 3. Wireless (ไร้สาย)

**เทคโนโลยี:**

- **Wi-Fi** - IEEE 802.11
- **Bluetooth** - ระยะสั้น
- **Cellular** - 4G, 5G

**Frequency Bands:**

- **2.4 GHz** - ระยะไกลกว่า แต่ช้ากว่า มีการรบกวนมาก
- **5 GHz** - เร็วกว่า แต่ระยะสั้นกว่า มีการรบกวนน้อย
- **6 GHz** - ใหม่ล่าสุด (Wi-Fi 6E)

**ข้อดี:**

- ไม่ต้องใช้สาย
- Mobility - เคลื่อนย้ายได้
- ติดตั้งง่าย
- ขยายได้ง่าย

**ข้อเสีย:**

- ความเร็วต่ำกว่า wired
- รับผลกระทบจากสิ่งกีดขวาง
- ความปลอดภัยน้อยกว่า
- มีการรบกวน (Interference)

---

## 1.3 Network Representations and Topologies (การแทนเครือข่ายและโครงสร้าง)

### Network Diagrams (แผนผังเครือข่าย)

#### Physical Topology (โครงสร้างทางกายภาพ)

- แสดงตำแหน่งจริงของอุปกรณ์และสายเคเบิล
- แสดงรายละเอียดทางกายภาพ
- ใช้สำหรับ: การติดตั้ง การบำรุงรักษา

#### Logical Topology (โครงสร้างทางตรรกะ)

- แสดงเส้นทางการไหลของข้อมูลในเครือข่าย
- แสดง IP address, VLANs, Routing
- ใช้สำหรับ: การวางแผน การแก้ปัญหา

### Common Network Icons (สัญลักษณ์ทั่วไป)

```
[Router]     = อุปกรณ์ router
[Switch]     = อุปกรณ์ switch
[PC]         = คอมพิวเตอร์
[Server]     = เซิร์ฟเวอร์
[Cloud]      = Internet/WAN
[Firewall]   = ไฟร์วอลล์
[AP]         = Access Point
```

### Topology Types (ประเภทของโครงสร้าง)

#### 1. Bus Topology (โครงสร้างแบบบัส)

**ลักษณะ:**

- อุปกรณ์ทั้งหมดเชื่อมต่อกับสายสัญญาณเส้นเดียว
- ใช้ Coaxial cable
- ล้าสมัย ไม่ใช้แล้ว

**ข้อดี:**

- ใช้สายน้อย
- ติดตั้งง่าย
- ราคาถูก

**ข้อเสีย:**

- ถ้าสายหลักเสีย เครือข่ายทั้งหมดเสีย
- ยากต่อการแก้ปัญหา
- Collision มาก
- จำกัดจำนวนอุปกรณ์

#### 2. Ring Topology (โครงสร้างแบบวงแหวน)

**ลักษณะ:**

- อุปกรณ์เชื่อมต่อเป็นวงกลม
- ข้อมูลเดินทางทางเดียว (หรือสองทาง)
- ใช้ Token passing

**ตัวอย่าง:**

- Token Ring (IEEE 802.5) - ล้าสมัย
- FDDI (Fiber Distributed Data Interface)

**ข้อดี:**

- Performance คงที่
- ไม่มี collision
- Fair access

**ข้อเสีย:**

- ถ้าอุปกรณ์หนึ่งเสีย เครือข่ายทั้งหมดเสีย (single ring)
- เพิ่มอุปกรณ์ยาก
- ล้าสมัย

#### 3. Star Topology (โครงสร้างแบบดาว) - ใช้มากที่สุด

**ลักษณะ:**

- อุปกรณ์ทั้งหมดเชื่อมต่อกับอุปกรณ์กลาง (Switch/Hub)
- เป็น topology มาตรฐานของ LAN ปัจจุบัน

**ข้อดี:**

- ถ้าสายเส้นหนึ่งเสีย อุปกรณ์เครื่องนั้นเท่านั้นที่เสีย
- เพิ่มอุปกรณ์ง่าย
- แก้ปัญหาง่าย
- Centralized management

**ข้อเสีย:**

- ถ้า Switch/Hub กลางเสีย เครือข่ายทั้งหมดเสีย
- ใช้สายมาก
- ต้นทุน Switch

#### 4. Extended Star Topology

**ลักษณะ:**

- Star หลายตัวเชื่อมต่อกัน
- Switch เชื่อมต่อกับ Switch อื่น
- ใช้ใน Enterprise networks

**ข้อดี:**

- ขยายได้ง่าย
- รองรับอุปกรณ์จำนวนมาก

#### 5. Mesh Topology (โครงสร้างแบบตาข่าย)

**ประเภท:**

**Full Mesh:**

- ทุกอุปกรณ์เชื่อมต่อกับทุกอุปกรณ์
- สูตร: n(n-1)/2 links (n = จำนวนอุปกรณ์)
- ตัวอย่าง: 5 อุปกรณ์ = 10 links

**Partial Mesh:**

- บางอุปกรณ์เชื่อมต่อกับหลายอุปกรณ์
- ไม่ใช่ทุกอุปกรณ์

**ข้อดี:**

- Redundancy สูง มีเส้นทางสำรอง
- Fault tolerance ดี
- Performance สูง
- ไม่มี Single Point of Failure

**ข้อเสีย:**

- ต้นทุนสูงมาก
- ซับซ้อน
- ยากต่อการจัดการ

**การใช้งาน:**

- Internet backbone
- WAN connections
- Critical networks

#### 6. Hybrid Topology (โครงสร้างแบบผสม)

**ลักษณะ:**

- ผสมผสาน topology หลายแบบ
- ตัวอย่าง: Star-Bus, Star-Ring

**ข้อดี:**

- ยืดหยุ่น
- Scalable
- เหมาะกับความต้องการที่แตกต่าง

---

## 1.4 Common Types of Networks (ประเภทของเครือข่ายทั่วไป)

### Intranet (อินทราเน็ต)

- เครือข่ายภายในองค์กร
- ใช้เฉพาะพนักงาน
- ใช้ TCP/IP protocols
- ตัวอย่าง: Internal website, File sharing

### Extranet (เอ็กซ์ทราเน็ต)

- ส่วนของ Intranet ที่เปิดให้บุคคลภายนอกเข้าถึง
- ต้องมีการ authentication
- ตัวอย่าง: Supplier portal, Customer portal, Partner access

### Internet (อินเทอร์เน็ต)

- เครือข่ายทั่วโลก
- Collection of interconnected networks
- ใช้ TCP/IP protocol suite
- ไม่มีเจ้าของคนเดียว

---

## Key Terminology (คำศัพท์สำคัญ)

|English|Thai|Meaning|
|---|---|---|
|Network|เครือข่าย|ระบบเชื่อมต่ออุปกรณ์|
|Host|โฮสต์|อุปกรณ์ปลายทาง|
|Client|ไคลเอนต์|ผู้ขอใช้บริการ|
|Server|เซิร์ฟเวอร์|ผู้ให้บริการ|
|Switch|สวิตช์|อุปกรณ์เชื่อม LAN|
|Router|เราเตอร์|อุปกรณ์เชื่อมเครือข่าย|
|Topology|โครงสร้าง|รูปแบบการเชื่อมต่อ|
|Bandwidth|แบนด์วิดท์|ความจุข้อมูล|
|Latency|เลเทนซี|ความหน่วง|
|Throughput|ทรูพุต|ข้อมูลจริงที่ส่งได้|

---

## Summary (สรุป)

Module 1 นี้เราได้เรียนรู้:

1. ✅ ความสำคัญของเครือข่ายในชีวิตประจำวัน
2. ✅ ประเภทของเครือข่าย (LAN, WAN, MAN, WLAN, PAN)
3. ✅ ส่วนประกอบของเครือข่าย (End Devices, Intermediary Devices, Network Media)
4. ✅ โครงสร้างเครือข่ายแบบต่างๆ (Physical และ Logical Topology)
5. ✅ การใช้งานเครือข่ายประเภทต่างๆ (Intranet, Extranet, Internet)

**Next Module:** Module 2 - Basic Switch and End Device Configuration