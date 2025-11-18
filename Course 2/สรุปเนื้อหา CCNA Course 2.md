# สรุปเนื้อหา CCNA Course 2
## Switching, Routing, and Wireless Essentials (SRWE) v7.02

---

## Module 1: Basic Device Configuration

### การตั้งค่าเบื้องต้นของอุปกรณ์

- **Switch Boot Sequence** - กระบวนการบูตของ Switch
- **LED Indicators** - การอ่านสัญญาณไฟ LED
- **System Crash Recovery** - การกู้คืนระบบเมื่อเกิดปัญหา
- **Switch Management Access (SVI)** - การตั้งค่าการเข้าถึงจัดการ Switch
- **Switch Port Configuration** - การตั้งค่าพอร์ตของ Switch
- **Duplex Communication** - การสื่อสารแบบ Full/Half Duplex
- **Auto-MDIX** - การตรวจจับสายแบบอัตโนมัติ
- **SSH และ Telnet Configuration** - การตั้งค่าการเข้าถึงระยะไกล
- **Basic Router Configuration** - การตั้งค่าเราเตอร์เบื้องต้น

**ทักษะสำคัญ:**

- ตั้งค่า Initial Configuration บน Switch และ Router
- Configure SSH สำหรับ Secure Remote Access
- Verify และ Troubleshoot Port Configuration

---

## Module 2: Switching Concepts

### แนวคิดการทำงานของ Switch

- **Switch Operation** - หลักการทำงานของ Switch
- **Frame Forwarding Methods** - วิธีการส่งต่อเฟรม
    - Store-and-Forward
    - Cut-Through
    - Fragment-Free
- **MAC Address Table** - ตารางเก็บ MAC Address
- **Collision Domains** - โดเมนการชน
- **Broadcast Domains** - โดเมนการกระจายสัญญาณ
- **Switch Port Settings** - การตั้งค่าพอร์ต Switch

**ทักษะสำคัญ:**

- เข้าใจกระบวนการ Frame Forwarding
- วิเคราะห์ MAC Address Table
- แยกแยะ Collision และ Broadcast Domains

---

## Module 3: VLANs

### Virtual Local Area Networks

- **VLAN Concepts** - แนวคิด VLAN และประโยชน์
- **VLAN Types** - ประเภทของ VLAN
    - Data VLAN
    - Voice VLAN
    - Management VLAN
    - Native VLAN
- **VLAN Configuration** - การตั้งค่า VLAN
- **VLAN Trunking** - การเชื่อมต่อ Trunk
- **802.1Q Protocol** - โปรโตคอลสำหรับ VLAN Tagging
- **Dynamic Trunking Protocol (DTP)** - การเจรจา Trunk อัตโนมัติ

**ทักษะสำคัญ:**

- สร้างและกำหนด VLAN
- Configure Trunk Links
- Troubleshoot VLAN Issues

---

## Module 4: Inter-VLAN Routing

### การกำหนดเส้นทางระหว่าง VLAN

- **Inter-VLAN Routing Concepts** - แนวคิดการ Routing ระหว่าง VLAN
- **Router-on-a-Stick** - การใช้ Router กำหนดเส้นทาง
- **Subinterface Configuration** - การตั้งค่า Subinterface
- **Layer 3 Switch Inter-VLAN Routing** - การใช้ Layer 3 Switch
- **Switch Virtual Interface (SVI)** - การตั้งค่า SVI
- **Troubleshooting Inter-VLAN Routing** - การแก้ไขปัญหา

**ทักษะสำคัญ:**

- Configure Router-on-a-Stick
- Implement Layer 3 Switching
- Verify Inter-VLAN Connectivity

---

## Module 5: STP Concepts

### Spanning Tree Protocol

- **Purpose of Spanning Tree** - วัตถุประสงค์ของ STP
- **STP Operations** - การทำงานของ STP
- **Layer 2 Loop Prevention** - การป้องกัน Loop ที่ Layer 2
- **PVST+ (Per-VLAN Spanning Tree Plus)** - STP แบบแยกตาม VLAN
- **Rapid PVST+** - STP แบบเร็ว
- **Port States และ Port Roles**
    - Root Port, Designated Port, Alternate Port
    - Blocking, Listening, Learning, Forwarding
- **PortFast** - การเปิดพอร์ตเร็ว
- **BPDU Guard** - การป้องกัน BPDU

**ทักษะสำคัญ:**

- เข้าใจกลไก STP Operation
- Configure PVST+ และ Rapid PVST+
- Implement PortFast และ BPDU Guard

---

## Module 6: EtherChannel

### Link Aggregation

- **EtherChannel Concepts** - แนวคิดการรวม Link
- **Link Aggregation Benefits** - ประโยชน์ของการรวมลิงก์
- **EtherChannel Configuration** - การตั้งค่า EtherChannel
- **LACP (Link Aggregation Control Protocol)** - โปรโตคอล IEEE Standard
- **PAgP (Port Aggregation Protocol)** - โปรโตคอลของ Cisco
- **EtherChannel Modes**
    - On, Desirable, Auto (PAgP)
    - Active, Passive (LACP)
- **Troubleshooting EtherChannel** - การแก้ไขปัญหา

**ทักษะสำคัญ:**

- Configure EtherChannel ด้วย LACP และ PAgP
- Verify EtherChannel Operation
- Load Balancing Configuration

---

## Module 7: DHCPv4

### Dynamic Host Configuration Protocol for IPv4

- **DHCP Operation** - การทำงานของ DHCP
- **DHCP Message Types** - ประเภทข้อความ DHCP
    - DHCPDISCOVER
    - DHCPOFFER
    - DHCPREQUEST
    - DHCPACK
- **DHCPv4 Server Configuration** - การตั้งค่า DHCP Server
- **DHCP Relay Agent** - ตัวกลางส่งต่อ DHCP
- **DHCPv4 Client Configuration** - การตั้งค่าไคลเอนต์
- **Troubleshooting DHCPv4** - การแก้ไขปัญหา

**ทักษะสำคัญ:**

- Configure DHCP Server บน Router
- Implement DHCP Relay
- Verify และ Troubleshoot DHCP Operation

---

## Module 8: SLAAC and DHCPv6

### IPv6 Address Configuration

- **SLAAC (Stateless Address Autoconfiguration)** - การกำหนด IP อัตโนมัติ
- **ICMPv6 Router Solicitation และ Router Advertisement**
- **DHCPv6 Operations** - การทำงานของ DHCPv6
- **Stateless DHCPv6** - DHCP แบบไม่เก็บสถานะ
- **Stateful DHCPv6** - DHCP แบบเก็บสถานะ
- **DHCPv6 Server Configuration** - การตั้งค่า DHCPv6 Server
- **RA Messages และ M/O Flags**

**ทักษะสำคัญ:**

- Configure SLAAC
- Implement Stateless และ Stateful DHCPv6
- Verify IPv6 Address Assignment

---

## Module 9: FHRP Concepts

### First Hop Redundancy Protocols

- **Default Gateway Redundancy** - ความซ้ำซ้อนของเกตเวย์
- **FHRP Operations** - การทำงานของโปรโตคอล FHRP
- **HSRP (Hot Standby Router Protocol)** - โปรโตคอลของ Cisco
    - Active และ Standby Router
    - Virtual IP Address
    - Priority และ Preemption
- **VRRP (Virtual Router Redundancy Protocol)** - IEEE Standard
- **GLBP (Gateway Load Balancing Protocol)** - Load Balancing Gateway

**ทักษะสำคัญ:**

- Configure HSRP
- Understand VRRP และ GLBP
- Implement Gateway Redundancy

---

## Module 10: LAN Security Concepts

### แนวคิดความปลอดภัยในเครือข่าย LAN

- **Network Security Threats** - ภัยคุกคามความปลอดภัย
- **Layer 2 Security Attacks**
    - MAC Address Table Flooding
    - DHCP Attacks (Starvation, Spoofing)
    - ARP Spoofing
    - Address Spoofing Attacks
- **VLAN Attacks**
    - VLAN Hopping
    - VLAN Double-Tagging
- **STP Attacks** - การโจมตี Spanning Tree
- **Network Security Best Practices** - แนวทางปฏิบัติที่ดี

**ทักษะสำคัญ:**

- ระบุภัยคุกคาม Layer 2
- เข้าใจวิธีการโจมตี
- วางแผนป้องกันความปลอดภัย

---

## Module 11: Switch Security Configuration

### การตั้งค่าความปลอดภัยของ Switch

- **Port Security** - ความปลอดภัยระดับพอร์ต
    - Static MAC Addresses
    - Dynamic MAC Addresses
    - Sticky MAC Addresses
    - Violation Modes (Protect, Restrict, Shutdown)
- **DHCP Snooping** - การกรอง DHCP
    - Trusted และ Untrusted Ports
    - DHCP Binding Database
- **Dynamic ARP Inspection (DAI)** - การตรวจสอบ ARP
- **IP Source Guard** - การป้องกัน IP Spoofing
- **PortFast และ BPDU Guard Implementation**

**ทักษะสำคัญ:**

- Configure Port Security
- Implement DHCP Snooping
- Configure Dynamic ARP Inspection
- Apply Switch Security Best Practices

---

## Module 12: WLAN Concepts

### Wireless LAN Concepts

- **Wireless LAN Components** - ส่วนประกอบของ WLAN
    - Access Points (AP)
    - Wireless LAN Controllers (WLC)
    - Lightweight Access Points (LAP)
- **802.11 Standards** - มาตรฐาน WiFi
    - 802.11a/b/g/n/ac/ax
    - Frequency Bands (2.4 GHz, 5 GHz)
- **WLAN Topologies** - รูปแบบ Wireless Network
    - Ad Hoc Mode
    - Infrastructure Mode
- **Wireless Frame Structure** - โครงสร้างเฟรมไร้สาย
- **CAPWAP (Control and Provisioning of Wireless Access Points)** - โปรโตคอลควบคุม AP
- **Wireless Channels และ Channel Bonding**

**ทักษะสำคัญ:**

- เข้าใจ WLAN Architecture
- ศึกษามาตรฐาน 802.11
- เข้าใจ RF Concepts

---

## Module 13: WLAN Configuration

### การตั้งค่า Wireless LAN

- **WLC Management Access** - การเข้าถึง Wireless Controller
- **Remote Site WLAN Configuration** - การตั้งค่า WLAN ระยะไกล
- **Wireless Client Authentication Methods**
    - Open Authentication
    - WEP (Wired Equivalent Privacy) - เลิกใช้แล้ว
    - WPA (Wi-Fi Protected Access)
    - WPA2 (Personal และ Enterprise)
    - WPA3 - มาตรฐานล่าสุด
- **802.1X Authentication และ RADIUS**
- **SSID Configuration** - การตั้งค่าชื่อเครือข่าย
- **Wireless Security Configuration** - การตั้งค่าความปลอดภัย
- **Troubleshooting WLAN** - การแก้ไขปัญหา

**ทักษะสำคัญ:**

- Configure WLAN บน WLC
- Implement Wireless Security (WPA2/WPA3)
- Troubleshoot Wireless Connectivity

---

## Module 14: Routing Concepts

### แนวคิดการกำหนดเส้นทาง

- **Packet Forwarding Decisions** - การตัดสินใจส่งต่อแพ็กเก็ต
- **Path Determination** - การหาเส้นทาง
- **Routing Table** - ตารางเส้นทาง
    - Directly Connected Networks
    - Static Routes
    - Dynamic Routes
- **Administrative Distance (AD)** - ค่าความน่าเชื่อถือของเส้นทาง
- **Routing Metrics** - ค่าวัดในการเลือกเส้นทาง
- **Static vs Dynamic Routing** - การเปรียบเทียบ
- **Default Gateway** - เกตเวย์เริ่มต้น

**ทักษะสำคัญ:**

- อ่านและเข้าใจ Routing Table
- เปรียบเทียบ Static และ Dynamic Routing
- เข้าใจ Administrative Distance

---

## Module 15: IP Static Routing

### การกำหนดเส้นทางแบบคงที่

- **Static Route Configuration** - การตั้งค่า Static Route
- **Next-Hop และ Exit Interface Options**
- **Default Static Routes** - เส้นทางเริ่มต้นแบบคงที่
    - Quad-Zero Route (0.0.0.0/0)
- **Floating Static Routes** - Static Route สำรอง
    - การใช้ Administrative Distance
- **IPv4 Static Routes Configuration**
- **IPv6 Static Routes Configuration**
- **Static Host Routes** - เส้นทางไปยังโฮสต์เฉพาะ
- **Fully Specified Static Routes**

**ทักษะสำคัญ:**

- Configure IPv4 และ IPv6 Static Routes
- Implement Default Static Routes
- Configure Floating Static Routes
- Verify Static Route Configuration

---

## Module 16: Troubleshoot Static and Default Routes

### การแก้ไขปัญหาเส้นทางคงที่

- **Network Troubleshooting Methodology** - ระเบียบวิธีการแก้ไขปัญหา
- **Packet Processing with Static Routes** - การประมวลผลแพ็กเก็ต
- **Common Static Route Configuration Issues** - ปัญหาทั่วไป
    - Incorrect IP Address หรือ Subnet Mask
    - Incorrect Next-Hop Address
    - Missing Routes
- **Network Troubleshooting Commands**
    - `ping` - ทดสอบการเชื่อมต่อ
    - `traceroute` / `tracert` - ตรวจสอบเส้นทาง
    - `show ip route` - แสดงตารางเส้นทาง
    - `show ip interface brief` - แสดงสถานะอินเตอร์เฟซ
    - `show cdp neighbors` - แสดงอุปกรณ์เพื่อนบ้าน
- **Solving Connectivity Problems** - การแก้ปัญหาการเชื่อมต่อ
- **Network Changes** - การปรับเปลี่ยนเครือข่าย

**ทักษะสำคัญ:**

- Troubleshoot Static Route Configuration
- Use Troubleshooting Commands Effectively
- Solve Network Connectivity Issues
- Apply Systematic Troubleshooting Approach

---

## สรุปภาพรวม CCNA Course 2

### **เป้าหมายหลักของหลักสูตร:**

1. **Switching Technologies**
    
    - การตั้งค่าและจัดการ Switch
    - VLANs และ Inter-VLAN Routing
    - Spanning Tree Protocol
    - EtherChannel
2. **Routing Fundamentals**
    
    - Static Routing
    - Default Routes
    - IPv4 และ IPv6 Routing
3. **Network Services**
    
    - DHCP (IPv4 และ IPv6)
    - First Hop Redundancy (HSRP/VRRP/GLBP)
4. **Wireless Networking**
    
    - WLAN Concepts
    - Wireless Configuration
    - Wireless Security
5. **Network Security**
    
    - Layer 2 Security
    - Port Security
    - DHCP Snooping
    - Dynamic ARP Inspection
6. **Troubleshooting**
    
    - Systematic Approach
    - Common Issues Resolution
    - Verification Commands

---

### **ทักษะที่ได้รับหลังจบ Course 2:**

✅ Configuration และ Troubleshooting ของ Switch และ Router  
✅ ออกแบบและจัดการ VLAN  
✅ Implement Inter-VLAN Routing  
✅ Configure Layer 2 Security Features  
✅ Setup และ Configure Wireless Networks  
✅ Implement Network Redundancy (STP, EtherChannel, FHRP)  
✅ Configure Static Routing สำหรับ IPv4 และ IPv6  
✅ Apply Best Practices สำหรับความปลอดภัยเครือข่าย

---

### **การเตรียมตัวสอบ CCNA 200-301:**

Course 2 ครอบคลุมหัวข้อสำคัญในข้อสอบ CCNA รวมถึง:

- Network Fundamentals (Switching และ Routing)
- Network Access (VLANs, Wireless)
- IP Connectivity (Static Routes)
- IP Services (DHCP, FHRP)
- Security Fundamentals (Layer 2 Security)

---

**หมายเหตุ:** หลักสูตรนี้เป็น Course ที่ 2 จาก 3 Courses ในหลักสูตร CCNA v7 โดยต้องเรียน Course 1 (Introduction to Networks) ก่อน และตามด้วย Course 3 (Enterprise Networking, Security, and Automation) เพื่อเตรียมความพร้อมสำหรับการสอบ CCNA Certification

---

**เอกสารสรุปโดย:** Claude (Anthropic AI)  
**วันที่:** พฤศจิกายน 2025  
**เวอร์ชัน:** CCNA v7.02 SRWE