# CCNA Course 1 - Module 17: Build a Small Network

## สร้างเครือข่ายขนาดเล็ก

---

## 17.1 Devices in a Small Network (อุปกรณ์ในเครือข่ายขนาดเล็ก)

### Small Network Overview

**คำจำกัดความ:**

- เครือข่ายที่ให้บริการ **small office หรือ home office (SOHO)**
- มักมี users **น้อยกว่า 50-200 คน**
- **Simple topology** แต่ต้อง **reliable และ secure**

**ลักษณะของ Small Network:**

```
✅ จำนวน users น้อย
✅ Single location หรือ 2-3 locations ใกล้กัน
✅ Limited budget
✅ ต้องการ reliability และ security
✅ Easy to manage
✅ Scalable (ขยายได้ในอนาคต)
```

---

### Typical Small Network Devices

#### 1. Router

**หน้าที่:**

```
✅ เชื่อมต่อ network หลายๆ network
✅ Route traffic ระหว่าง networks
✅ Connect to Internet (ISP)
✅ NAT/PAT (แปลง private ↔ public IPs)
✅ DHCP server
✅ Basic firewall
```

**ตัวอย่างในบ้าน/SOHO:**

```
ISP (Internet) ← → Router ← → Internal Network
                    |
                    ├→ Wi-Fi AP (integrated)
                    ├→ Switch
                    └→ Devices
```

**ฟีเจอร์สำคัญ:**

- WAN port (เชื่อม ISP)
- LAN ports (เชื่อม internal devices)
- Wireless capability (integrated AP)
- Security features (firewall, VPN)

---

#### 2. Switch

**หน้าที่:**

```
✅ เชื่อมต่อ devices ใน LAN
✅ Forward frames ตาม MAC addresses
✅ Create VLANs (แบ่ง networks)
✅ Power over Ethernet (PoE) - จ่ายไฟให้ devices
```

**ประเภท:**

**Unmanaged Switch:**

```
คำจำกัดความ: Plug-and-play, ไม่มี configuration

ข้อดี:
  ✅ ง่าย (เสียบใช้เลย)
  ✅ ราคาถูก
  ✅ ไม่ต้อง management

ข้อเสีย:
  ❌ ไม่มี VLANs
  ❌ ไม่มี QoS
  ❌ ไม่มี security features
  ❌ ไม่มี monitoring

ใช้สำหรับ:
  - Home networks
  - Very small offices
  - เมื่อไม่ต้องการ advanced features
```

**Managed Switch:**

```
คำจำกัดความ: มี configuration และ management

ข้อดี:
  ✅ VLANs
  ✅ QoS (Quality of Service)
  ✅ Port security
  ✅ Link aggregation
  ✅ SNMP monitoring
  ✅ Port mirroring (troubleshooting)

ข้อเสีย:
  ❌ ราคาแพงกว่า
  ❌ ต้องการ configuration
  ❌ ต้องการความรู้

ใช้สำหรับ:
  - Business networks
  - เมื่อต้องการ VLANs/security
  - Networks ที่ต้อง monitoring
```

**PoE Switch:**

```
คำจำกัดความ: จ่ายไฟผ่าน Ethernet cable

ใช้สำหรับ:
  - IP phones
  - Wireless Access Points
  - IP cameras
  - IoT devices

Standards:
  - PoE (802.3af): 15.4W per port
  - PoE+ (802.3at): 25.5W per port
  - PoE++ (802.3bt): 60-100W per port

ข้อดี:
  ✅ ไม่ต้องเดิน power cables
  ✅ Flexibility ในการติดตั้ง
  ✅ Centralized power management
```

---

#### 3. Wireless Access Point (WAP/AP)

**หน้าที่:**

```
✅ ให้บริการ wireless connectivity
✅ Bridge ระหว่าง wireless clients และ wired network
✅ Multiple SSIDs (Guest network, Employee network)
```

**ประเภท:**

**Standalone AP:**

```
Independent device
Configuration แยก per AP

ข้อดี:
  ✅ Simple
  ✅ ราคาถูก

ข้อเสีย:
  ❌ ยากจัดการเมื่อมีหลาย APs
  ❌ ไม่มี centralized management
  ❌ Roaming ไม่ smooth

ใช้สำหรับ:
  - 1-3 APs
  - Small networks
```

**Controller-Based AP (Lightweight AP):**

```
Managed โดย Wireless LAN Controller (WLC)
APs = "dumb" (รับคำสั่งจาก controller)

ข้อดี:
  ✅ Centralized management
  ✅ Consistent configuration
  ✅ Seamless roaming
  ✅ Easy scaling

ข้อเสีย:
  ❌ ต้องมี controller (ค่าใช้จ่าย)
  ❌ Complex setup

ใช้สำหรับ:
  - 10+ APs
  - Enterprise networks
  - เมื่อต้องการ roaming
```

**Features:**

- WPA3/WPA2 encryption
- Multiple SSIDs
- Guest network isolation
- Band steering (2.4GHz ↔ 5GHz)
- Load balancing

---

#### 4. Firewall

**หน้าที่:**

```
✅ Filter traffic (allow/deny)
✅ ป้องกัน unauthorized access
✅ Inspect packets
✅ Block malicious traffic
✅ VPN termination
```

**ประเภท:**

**SOHO Router (Integrated Firewall):**

```
Firewall built-in router

Features:
  - Basic packet filtering
  - NAT (hiding internal IPs)
  - Port forwarding
  - Simple rules

เหมาะสำหรับ:
  - Home/small office
  - Basic protection
```

**Dedicated Firewall:**

```
Hardware firewall device

Features:
  - Advanced filtering
  - IPS (Intrusion Prevention)
  - Deep packet inspection
  - VPN
  - Detailed logging
  - High throughput

เหมาะสำหรับ:
  - Business networks
  - เมื่อต้องการ advanced security
  - High traffic volumes

Examples: 
  - Cisco ASA
  - Palo Alto
  - Fortinet FortiGate
```

---

#### 5. Server (Optional)

**ประเภท:**

**File Server:**

```
เก็บและแชร์ไฟล์

ใช้สำหรับ:
  - Centralized file storage
  - Backup
  - Collaboration

Protocols:
  - SMB/CIFS (Windows)
  - NFS (Linux/Unix)
```

**Print Server:**

```
จัดการ printers

ใช้สำหรับ:
  - แชร์ printers
  - Print queue management
  - Centralized printing
```

**DHCP Server:**

```
จัดสรร IP addresses

มักอยู่ใน:
  - Router (SOHO)
  - Dedicated server (enterprise)
```

**DNS Server:**

```
Internal DNS resolution

ใช้สำหรับ:
  - Resolve internal hostnames
  - Cache DNS queries
  - Forward to external DNS
```

**Email Server (Optional):**

```
Internal email system

Protocols:
  - SMTP (sending)
  - POP3/IMAP (receiving)

หรือใช้ Cloud (Microsoft 365, Gmail)
```

---

#### 6. End Devices

**ประเภท:**

- Desktop computers
- Laptops
- Smartphones/Tablets
- IP phones (VoIP)
- Printers
- IoT devices (cameras, sensors)

---

### Small Network Topology Example

```
                    Internet (ISP)
                         |
                    [Modem/ONT]
                         |
                    [Router/FW]
                     (Gateway)
                    192.168.1.1
                         |
              +----------+----------+
              |                     |
         [Switch 24-port]      [Wireless AP]
          (PoE capable)          (Employee)
         192.168.1.2           192.168.1.3
              |                     |
    +---------+---------+           |
    |         |         |           |
[Desktop] [Laptop] [IP Phone]  [Wireless]
   .10       .11      .12        [Devices]
                                   
         [Guest AP]
        192.168.2.1
        (Isolated)
            |
        [Guests]

IP Scheme:
  - 192.168.1.0/24: Internal network
  - 192.168.2.0/24: Guest network
  - 192.168.1.1: Default gateway
  - 192.168.1.100-200: DHCP pool
```

---

## 17.2 Small Network Applications and Protocols

### Common Applications

#### 1. Email

**Protocols:**

- **SMTP** (Port 25/587) - Sending
- **POP3** (Port 110/995) - Receiving (download)
- **IMAP** (Port 143/993) - Receiving (sync)

**Setup Options:**

**a) Cloud Email (แนะนำสำหรับ Small Network):**

```
Services:
  - Microsoft 365 (Outlook)
  - Google Workspace (Gmail)
  - Others (Zoho, ProtonMail)

ข้อดี:
  ✅ No server maintenance
  ✅ High availability
  ✅ Automatic backups
  ✅ Spam filtering
  ✅ Large storage
  ✅ Mobile access

ข้อเสีย:
  ❌ Monthly cost
  ❌ Internet dependency
  ❌ Data อยู่กับ provider
```

**b) On-Premises Email Server:**

```
Software:
  - Microsoft Exchange
  - Zimbra
  - Others

ข้อดี:
  ✅ Full control
  ✅ Data อยู่ใน organization

ข้อเสีย:
  ❌ Server ต้อง maintain
  ❌ High initial cost
  ❌ ต้องการ expertise
  ❌ Backup = responsibility ของตัวเอง
```

---

#### 2. Web Services

**Internal Web Server:**

```
ใช้สำหรับ:
  - Company intranet
  - Internal documentation
  - Web applications
  - Employee portal

Software:
  - Apache
  - Nginx
  - IIS (Windows)

Configuration:
  - HTTP/HTTPS (Port 80/443)
  - SSL/TLS certificates
  - Access control
```

**External Web Server (ถ้ามี website):**

```
Options:

a) Hosted On-Premises:
   Pros:
     ✅ Full control
   Cons:
     ❌ ต้องมี static public IP
     ❌ Bandwidth ต้องเพียงพอ
     ❌ DDoS protection ยาก
     ❌ Maintenance

b) Cloud Hosting (แนะนำ):
   Services:
     - AWS, Azure, Google Cloud
     - Shared hosting (Bluehost, etc.)
   
   Pros:
     ✅ High availability
     ✅ Scalable
     ✅ DDoS protection
     ✅ Backup
   
   Cons:
     ❌ Ongoing cost
```

---

#### 3. File Sharing

**Network Attached Storage (NAS):**

```
คำจำกัดความ: Dedicated file storage device

ใช้สำหรับ:
  - Centralized file storage
  - Backup
  - Media streaming
  - Collaboration

Features:
  - RAID (data redundancy)
  - Snapshot (point-in-time backup)
  - User/Group permissions
  - SMB, NFS, FTP protocols

Popular brands:
  - Synology
  - QNAP
  - Western Digital My Cloud

ข้อดี:
  ✅ Easy to manage
  ✅ Cost-effective
  ✅ Reliable
  ✅ Good for small networks

ข้อเสีย:
  ❌ Single point of failure (ถ้าไม่มี redundancy)
  ❌ Performance จำกัด
```

**File Server:**

```
Dedicated server สำหรับ files

OS:
  - Windows Server
  - Linux (Samba)
  - FreeNAS/TrueNAS

ข้อดี:
  ✅ High performance
  ✅ Scalable
  ✅ Advanced features (Active Directory integration)

ข้อเสีย:
  ❌ ราคาสูงกว่า NAS
  ❌ ต้องการ IT expertise
```

**Cloud Storage:**

```
Services:
  - Dropbox Business
  - Google Drive
  - Microsoft OneDrive/SharePoint
  - Box

ข้อดี:
  ✅ Access anywhere
  ✅ Automatic sync
  ✅ Collaboration features
  ✅ Version control
  ✅ No local maintenance

ข้อเสีย:
  ❌ Monthly cost per user
  ❌ Internet dependency
  ❌ Storage limits
```

---

#### 4. Voice Services (VoIP)

**คำจำกัดความ:**

- **Voice over IP** - โทรศัพท์ผ่าน IP network

**Components:**

**IP Phones:**

```
Hardware phones ที่ต่อกับ network

Features:
  - HD voice quality
  - Display screen
  - Multiple lines
  - Programmable buttons
  - PoE powered

หรือ Softphones (software บน PC/mobile)
```

**IP PBX (Private Branch Exchange):**

```
คำจำกัดความ: Phone system สำหรับ organization

Options:

a) On-Premises IP PBX:
   Software:
     - Cisco Unified Communications Manager
     - Asterisk (open source)
     - 3CX
   
   ข้อดี:
     ✅ Full control
     ✅ No per-user fees
   
   ข้อเสีย:
     ❌ High initial cost
     ❌ Maintenance

b) Cloud PBX/UCaaS (แนะนำสำหรับ small networks):
   Services:
     - RingCentral
     - 8x8
     - Microsoft Teams Phone
   
   ข้อดี:
     ✅ No hardware needed
     ✅ Easy scaling
     ✅ Feature-rich
     ✅ Remote work ready
   
   ข้อเสิย:
     ❌ Monthly per-user cost
     ❌ Internet quality critical
```

**Protocols:**

- **SIP** (Session Initiation Protocol) - Signaling
- **RTP** (Real-time Transport Protocol) - Voice data

**QoS Requirements:**

```
Voice = real-time application

ต้องการ:
  ✅ Low latency (< 150ms)
  ✅ Low jitter (< 30ms)
  ✅ Low packet loss (< 1%)
  ✅ Sufficient bandwidth (~100 Kbps per call)

Configuration:
  - QoS policies on switches/routers
  - VLAN แยกสำหรับ voice
  - Priority queuing
```

---

#### 5. Video Conferencing

**Solutions:**

```
Popular platforms:
  - Zoom
  - Microsoft Teams
  - Google Meet
  - Cisco Webex
  - Skype for Business

Requirements:
  - High-speed Internet
  - Webcams
  - Microphones/Speakers
  - (Optional) Dedicated video conferencing equipment
```

**Network Requirements:**

```
Bandwidth per user:
  - HD video (720p): 1-2 Mbps
  - Full HD (1080p): 2-4 Mbps
  - Screen sharing: +0.5-1 Mbps

QoS:
  - Similar to VoIP (low latency/jitter)
  - Priority traffic
```

---

### Network Services Configuration

#### DHCP Configuration Example (Cisco Router)

```cisco
Router(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.50
Router(config)# ip dhcp excluded-address 192.168.1.251 192.168.1.254

Router(config)# ip dhcp pool LAN-POOL
Router(dhcp-config)# network 192.168.1.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.1.1
Router(dhcp-config)# dns-server 8.8.8.8 8.8.4.4
Router(dhcp-config)# domain-name company.local
Router(dhcp-config)# lease 7
Router(dhcp-config)# exit

! Guest Network Pool
Router(config)# ip dhcp pool GUEST-POOL
Router(dhcp-config)# network 192.168.2.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.2.1
Router(dhcp-config)# dns-server 1.1.1.1
Router(dhcp-config)# lease 0 1
Router(dhcp-config)# exit

คำอธิบาย:
  - excluded-address: IPs ที่ไม่จัดสรร (reserved for servers, printers)
  - network: ช่วง network
  - default-router: Gateway
  - dns-server: DNS servers
  - domain-name: Domain suffix
  - lease 7: 7 days (Guest = 1 hour = 0 days 1 hour)
```

---

#### NAT/PAT Configuration (Cisco Router)

```cisco
! Inside interface (LAN)
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# ip nat inside
Router(config-if)# no shutdown
Router(config-if)# exit

! Outside interface (WAN/Internet)
Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip address dhcp
Router(config-if)# ip nat outside
Router(config-if)# no shutdown
Router(config-if)# exit

! PAT (NAT Overload) - Many to One
Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255
Router(config)# ip nat inside source list 1 interface GigabitEthernet0/1 overload

คำอธิบาย:
  - ip nat inside: interface ฝั่ง internal
  - ip nat outside: interface ฝั่ง Internet
  - access-list 1: กำหนด internal network ที่อนุญาต NAT
  - overload: PAT (ใช้ port numbers แยก connections)

! Static NAT (สำหรับ web server)
Router(config)# ip nat inside source static 192.168.1.10 203.0.113.5
  → แปลง 192.168.1.10 (internal server) ↔ 203.0.113.5 (public IP)

! Port Forwarding
Router(config)# ip nat inside source static tcp 192.168.1.10 80 203.0.113.5 8080
  → Port 8080 บน public IP → Port 80 บน internal server
```

---

#### Basic Firewall (ACL) Configuration

```cisco
! Standard ACL - Block specific source
Router(config)# access-list 10 deny 192.168.5.0 0.0.0.255
Router(config)# access-list 10 permit any

Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip access-group 10 in
Router(config-if)# exit

! Extended ACL - Block specific traffic
Router(config)# ip access-list extended BLOCK-TRAFFIC
Router(config-ext-nacl)# deny tcp any any eq telnet
Router(config-ext-nacl)# deny tcp any any eq 445
Router(config-ext-nacl)# permit ip any any
Router(config-ext-nacl)# exit

Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip access-group BLOCK-TRAFFIC in
Router(config-if)# exit

คำอธิบาย:
  - Standard ACL: filter ตาม source IP only
  - Extended ACL: filter ตาม source/dest IP, port, protocol
  - eq telnet: port 23
  - eq 445: SMB (Windows file sharing)
  - permit ip any any: อนุญาตที่เหลือ (implicit deny ถ้าไม่ใส่)
```

---

## 17.3 Scale to Larger Networks (ขยายเครือข่ายที่ใหญ่ขึ้น)

### Growth Challenges

**ปัญหาเมื่อ network เติบโต:**

#### 1. Performance Degradation

```
ปัญหา:
  - มี devices มากขึ้น
  - Traffic มากขึ้น
  - Collisions/broadcasts เพิ่มขึ้น
  - Bandwidth bottlenecks

Solutions:
  ✅ Switch แทน hubs
  ✅ Segment networks (VLANs)
  ✅ Upgrade links (1G → 10G)
  ✅ Add switches (hierarchical design)
```

#### 2. Management Complexity

```
ปัญหา:
  - Device จำนวนมาก
  - Configuration inconsistency
  - Troubleshooting ยาก
  - การเปลี่ยนแปลง = risk

Solutions:
  ✅ Centralized management (network management system)
  ✅ Standardized configurations
  ✅ Documentation
  ✅ Automation (Ansible, Python scripts)
  ✅ Monitoring tools (SNMP, Syslog)
```

#### 3. Security Concerns

```
ปัญหา:
  - Attack surface เพิ่ม
  - ยากควบคุม access
  - ยากตรวจจับ intrusions

Solutions:
  ✅ Defense in Depth
  ✅ Network segmentation
  ✅ IDS/IPS
  ✅ Centralized authentication (RADIUS, 802.1X)
  ✅ Security policies และ enforcement
```

#### 4. Availability Requirements

```
ปัญหา:
  - Single point of failure
  - Downtime = business impact มากขึ้น

Solutions:
  ✅ Redundancy (dual ISPs, dual routers)
  ✅ High availability protocols (HSRP, VRRP)
  ✅ Backup power (UPS, generators)
  ✅ Monitoring และ alerting
```

---

### Design Considerations

#### 1. Hierarchical Network Design

**คำจำกัดความ:**

- แบ่ง network เป็น **3 layers**: Access, Distribution, Core

**Layers:**

**Access Layer (ชั้นล่าง):**

```
หน้าที่:
  - เชื่อมต่อ end devices
  - Port security
  - VLANs
  - PoE

Devices:
  - Access switches
  - Wireless APs

Features:
  - High port density
  - PoE capability
  - Basic security (port security, DHCP snooping)
```

**Distribution Layer (ชั้นกลาง):**

```
หน้าที่:
  - Aggregate access layer
  - Routing between VLANs
  - Policy enforcement
  - QoS
  - Security filtering (ACLs)

Devices:
  - Layer 3 switches (multilayer switches)
  - Routers

Features:
  - High performance
  - Redundancy
  - Routing protocols
  - Advanced security
```

**Core Layer (ชั้นบน):**

```
หน้าที่:
  - High-speed backbone
  - Connect distribution layers
  - Minimal policy (เพื่อความเร็ว)

Devices:
  - High-end Layer 3 switches
  - Core routers

Features:
  - Very high speed (10G, 40G, 100G)
  - Redundancy (dual core)
  - Minimal latency
  - High availability
```

**Diagram:**

```
                    [Core Layer]
                   /            \
              [Distribution] [Distribution]
              /    |    \    /    |    \
        [Access] [Access] [Access] [Access]
          |||      |||      |||      |||
        Devices  Devices  Devices  Devices
```

**ข้อดีของ Hierarchical Design:**

```
✅ Scalability - ขยายง่าย (เพิ่ม access switches)
✅ Redundancy - แต่ละ layer มี redundant paths
✅ Performance - แต่ละ layer optimize สำหรับหน้าที่ของมัน
✅ Manageable - ชัดเจนว่า device แต่ละตัวทำอะไร
✅ Troubleshooting ง่าย - แยก layer ได้
```

---

#### 2. Redundancy

**Single Point of Failure (SPOF):**

```
ปัญหา: อุปกรณ์ตัวเดียวพัง → ทั้ง network down

Example:
  Internet ← Single Router ← Switch ← Devices
              (SPOF here!)
  
  ถ้า router พัง → ไม่มี Internet
```

**Solutions:**

**Redundant ISPs:**

```
Setup:
       ISP 1 ←──┐
                ├─ Router
       ISP 2 ←──┘

ข้อดี:
  ✅ ถ้า ISP 1 down → ใช้ ISP 2
  ✅ Load balancing (ใช้ทั้ง 2 พร้อมกัน)

Configuration:
  - Static routes กับ metric ต่างกัน
  - BGP (Advanced)
```

**Redundant Routers (HSRP/VRRP):**

```
Setup:
              Internet
                 |
        ┌────────┴────────┐
    Router1 (Active)   Router2 (Standby)
        └────────┬────────┘
                 |
            [Switch Layer]

HSRP (Hot Standby Router Protocol):
  - Cisco proprietary
  - Virtual IP (VIP) = default gateway
  - Active router ให้บริการ
  - Standby router รอ (monitor active)
  - ถ้า active down → standby เป็น active

Configuration:
  Router1(config-if)# standby 1 ip 192.168.1.1
  Router1(config-if)# standby 1 priority 110
  Router1(config-if)# standby 1 preempt

  Router2(config-if)# standby 1 ip 192.168.1.1
  Router2(config-if)# standby 1 priority 100

  Default gateway ของ PCs = 192.168.1.1 (VIP)
```

**Redundant Switches:**

```
Setup: Dual switches ในแต่ละ layer

   Distribution1 ═══ Distribution2
        ║   ║           ║   ║
        ║   ╠═══════════╣   ║
        ║   ║           ║   ║
    Access1 ═══════ Access2

Links:
  - Dual uplinks (access → distribution)
  - Cross-connect (switch ↔ switch)

Protocols:
  - STP (Spanning Tree Protocol) - ป้องกัน loops
  - EtherChannel (Link Aggregation) - bundle links เป็น 1
```

---

#### 3. Scalability

**คำจำกัดความ:**

- ความสามารถ**ขยาย network** โดยไม่ redesign

**Strategies:**

**Modular Design:**

```
ออกแบบเป็น modules

Example:
  - Building A: Access switches → Distribution
  - Building B: Access switches → Distribution
  - Core connects distributions

ขยาย:
  - เพิ่ม Building C → เพิ่ม access + distribution
  - ไม่กระทบ Building A, B
```

**IP Addressing Plan:**

```
วางแผน IP addresses ล่วงหน้า

Example:
  Office 1: 10.1.0.0/16 (65,534 hosts)
    VLAN 10: 10.1.10.0/24 (254 hosts)
    VLAN 20: 10.1.20.0/24
    VLAN 30: 10.1.30.0/24
    ... room to grow

  Office 2: 10.2.0.0/16
  Office 3: 10.3.0.0/16

ข้อดี:
  ✅ ชัดเจน (site ไหนใช้ subnet ไหน)
  ✅ เพิ่ม VLANs ได้ (มี space)
  ✅ Summarization ง่าย (routing)
```

**Wireless Scalability:**

```
Controller-Based APs:
  - เพิ่ม APs → License เพิ่ม (ถ้าจำเป็น)
  - Configuration automatic (push from controller)
  - Roaming seamless

Cloud-Managed APs:
  - Cisco Meraki, Ubiquiti UniFi
  - จัดการผ่าน cloud portal
  - เหมาะสำหรับ multi-site
```

---

#### 4. Network Documentation

**คำจำกัดความ:**

- บันทึก **ทุกอย่าง** เกี่ยวกับ network

**ประเภท:**

**Network Topology Diagrams:**

```
Physical topology:
  - แสดง devices และ cable connections จริง
  - Cable types, lengths
  - Port numbers

Logical topology:
  - แสดง IP addressing
  - VLANs
  - Subnets
  - Routing

Tools: Microsoft Visio, Draw.io, Lucidchart
```

**IP Address Plan:**

```
Spreadsheet หรือ IPAM (IP Address Management) tool

บันทึก:
  - Subnet
  - VLAN ID
  - Purpose
  - IP range
  - Gateway
  - DNS
  - DHCP pool

Example:
  Subnet           VLAN  Purpose      Gateway       DHCP Pool
  ------------------------------------------------------------------------
  10.1.10.0/24     10    Employees    10.1.10.1     10.1.10.100-200
  10.1.20.0/24     20    Guests       10.1.20.1     10.1.20.100-200
  10.1.30.0/24     30    Servers      10.1.30.1     Static only
  10.1.40.0/24     40    VoIP         10.1.40.1     10.1.40.100-250
```

**Device Inventory:**

```
รายการ devices ทั้งหมด

บันทึก:
  - Device name
  - Type/Model
  - Serial number
  - Location
  - IP address
  - Management IP
  - Firmware version
  - Purchase date
  - Warranty expiration

Example:
  Name        Type      Model        Location   IP            Firmware
  ------------------------------------------------------------------------
  SW-FL1-01   Switch    Cisco 2960X  Floor 1    10.1.1.10     15.2(7)E
  RTR-MAIN    Router    Cisco 4331   Rack A1    10.1.1.1      16.9.5
```

**Configuration Backups:**

```
เก็บ configurations ของทุก devices

Schedule:
  - ก่อนทำการเปลี่ยนแปลง
  - หลังทำการเปลี่ยนแปลง
  - Weekly/Monthly automatic backups

Storage:
  - Version control (Git)
  - Off-site backup
  - Encrypted

Tools:
  - TFTP server
  - Kiwi CatTools
  - Ansible
  - Oxidized (open source)
```

**Change Log:**

```
บันทึกการเปลี่ยนแปลงทั้งหมด

บันทึก:
  - Date/Time
  - Who made the change
  - What was changed
  - Why (reason)
  - Rollback plan

Example:
  Date       Who        Change                    Reason
  ------------------------------------------------------------------------
  2025-11-18 John       Add VLAN 50               New department
  2025-11-17 Sarah      Update ACL on FW          Block malicious IP
  2025-11-15 John       Upgrade SW-FL1-01 FW      Security patch
```

**Standard Operating Procedures (SOPs):**

```
ขั้นตอนการทำงานมาตรฐาน

ตัวอย่าง SOPs:
  - Adding a new user
  - Configuring a new VLAN
  - Backup procedures
  - Disaster recovery
  - Troubleshooting common issues
  - Change management process

Format:
  1. Purpose
  2. Prerequisites
  3. Step-by-step instructions
  4. Expected results
  5. Rollback procedure
  6. Contacts
```

---

#### 5. Monitoring and Management

**Network Monitoring:**

**SNMP (Simple Network Management Protocol):**

```
Protocol สำหรับ monitor/manage devices

Components:
  - SNMP Manager (monitoring station)
  - SNMP Agent (on devices)
  - MIB (Management Information Base) - database of variables

Configuration:
  Router(config)# snmp-server community public RO
  Router(config)# snmp-server host 10.1.1.100 version 2c public

Tools:
  - PRTG Network Monitor
  - Nagios
  - Zabbix
  - SolarWinds

Monitors:
  - Interface status (up/down)
  - Bandwidth utilization
  - CPU/Memory usage
  - Errors/discards
  - Temperature
```

**Syslog:**

```
Centralized logging

Configuration:
  Router(config)# logging host 10.1.1.100
  Router(config)# logging trap informational

Syslog Server:
  - Collects logs จากทุก devices
  - Search/Filter
  - Alerting

ใช้สำหรับ:
  - Troubleshooting
  - Security monitoring
  - Compliance/Auditing

Tools:
  - Splunk
  - ELK Stack (Elasticsearch, Logstash, Kibana)
  - Graylog
```

**NetFlow:**

```
Traffic analysis

ข้อมูลที่เก็บ:
  - Source/Destination IPs
  - Source/Destination Ports
  - Protocol
  - Bytes/Packets

ใช้สำหรับ:
  - Bandwidth analysis (ใครใช้มากที่สุด)
  - Capacity planning
  - Security (ตรวจจับ anomalies)

Configuration:
  Router(config)# ip flow-export destination 10.1.1.100 2055
  Router(config-if)# ip flow ingress

Tools:
  - SolarWinds NetFlow Traffic Analyzer
  - PRTG
  - ntopng
```

---

### Best Practices Summary

```
Design:
  ✅ Hierarchical design (Access, Distribution, Core)
  ✅ Redundancy (no single points of failure)
  ✅ Scalability (plan for growth)
  ✅ Modularity (easy to expand)

Security:
  ✅ Defense in Depth (multiple layers)
  ✅ Segmentation (VLANs, subnets)
  ✅ Access control (ACLs, 802.1X)
  ✅ Monitoring (IDS/IPS, logging)

Management:
  ✅ Documentation (keep updated!)
  ✅ Standardization (consistent configs)
  ✅ Automation (reduce human error)
  ✅ Monitoring (SNMP, Syslog, NetFlow)
  ✅ Change management (controlled, documented)

Operations:
  ✅ Backups (configurations, data)
  ✅ Updates (firmware, patches)
  ✅ Testing (before production deployment)
  ✅ Training (staff knowledge)
```

---

## 17.4 Verify Connectivity (ตรวจสอบการเชื่อมต่อ)

### Troubleshooting Methodology

**Step-by-step approach:**

#### 1. Identify the Problem

```
Gather information:
  - What is the symptom?
  - Who is affected? (single user, department, entire network)
  - When did it start?
  - Any recent changes?
  - Can you reproduce it?

Questions to ask:
  - "ไม่สามารถเข้า Internet" → ทั้งหมดหรือบาง sites?
  - "Network ช้า" → ช้าตลอดหรือบางช่วงเวลา?
  - "ไม่สามารถ connect to server" → Server IP เท่าไหร่?
```

#### 2. Establish a Theory

```
สมมติฐานที่เป็นไปได้:

Example: "Cannot access Internet"

Theories:
  ❓ PC configuration ผิด (IP, Gateway, DNS)
  ❓ Cable unplugged
  ❓ Switch port down
  ❓ Router/Gateway down
  ❓ ISP outage
  ❓ Firewall blocking
  ❓ DNS issue

เรียงตาม probability (มากไปน้อย)
```

#### 3. Test the Theory

```
ทดสอบสมมติฐานทีละข้อ

Example tests:
  ✓ Check PC IP configuration: ipconfig
  ✓ Check cable: Link light on NIC
  ✓ Ping default gateway: ping 192.168.1.1
  ✓ Ping external IP: ping 8.8.8.8
  ✓ Ping domain: ping google.com
  ✓ Check DNS: nslookup google.com
```

#### 4. Establish a Plan of Action

```
สร้างแผนแก้ไข

Example:
  Problem: DNS not working
  
  Plan:
    1. Verify DNS server IPs
    2. Test DNS servers (ping 8.8.8.8)
    3. Change DNS servers (to 1.1.1.1)
    4. Flush DNS cache: ipconfig /flushdns
    5. Test again: nslookup google.com
```

#### 5. Implement the Solution

```
ทำตามแผน

Important:
  ⚠️ Make one change at a time
  ⚠️ Document what you do
  ⚠️ Have a rollback plan
```

#### 6. Verify Full System Functionality

```
ทดสอบว่าแก้ไขสำเร็จ

Tests:
  ✓ Original problem fixed?
  ✓ No new problems created?
  ✓ All functions working?

Example:
  - Browse to multiple websites ✓
  - Check email ✓
  - Access internal server ✓
```

#### 7. Document Findings

```
บันทึกทุกอย่าง

Document:
  - Problem description
  - Troubleshooting steps
  - Root cause
  - Solution
  - Date/Time/Who

ประโยชน์:
  ✅ Reference สำหรับอนาคต
  ✅ Knowledge base
  ✅ Training material
```

---

### Troubleshooting Tools and Commands

#### 1. ipconfig (Windows) / ifconfig (Linux)

**ipconfig (Windows):**

```cmd
ipconfig
  แสดง IP configuration พื้นฐาน

ipconfig /all
  แสดงทุกอย่าง (MAC, DHCP server, DNS, etc.)

ipconfig /release
  ปล่อย DHCP lease

ipconfig /renew
  ขอ DHCP lease ใหม่

ipconfig /flushdns
  ล้าง DNS cache

ipconfig /displaydns
  แสดง DNS cache

Output:
  Ethernet adapter Local Area Connection:
    Connection-specific DNS Suffix: company.local
    IPv4 Address: 192.168.1.100
    Subnet Mask: 255.255.255.0
    Default Gateway: 192.168.1.1
```

**ifconfig (Linux):**

```bash
ifconfig
  แสดง network interfaces

ifconfig eth0
  แสดง interface เฉพาะ

ifconfig eth0 up
  เปิด interface

ifconfig eth0 down
  ปิด interface

Output:
  eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.100  netmask 255.255.255.0  broadcast 192.168.1.255
        ether 00:0c:29:3e:4f:5a  txqueuelen 1000  (Ethernet)
```

---

#### 2. ping

**คำจำกัดความ:**

- ทดสอบ **reachability** และ **round-trip time**
- ใช้ **ICMP Echo Request/Reply**

**Syntax:**

```bash
ping <destination>

Options:
  -t (Windows): ping ต่อเนื่อง (จนกว่า Ctrl+C)
  -c <count> (Linux): จำนวนครั้ง
  -n <count> (Windows): จำนวนครั้ง
  -l <size> (Windows): ขนาด packet
  -s <size> (Linux): ขนาด packet
```

**Examples:**

```cmd
# Ping default gateway
ping 192.168.1.1

# Ping Google DNS
ping 8.8.8.8

# Ping domain
ping google.com

# Ping with large packet
ping -l 1500 192.168.1.1

# Continuous ping
ping -t 192.168.1.1
```

**Output:**

```
Pinging 192.168.1.1 with 32 bytes of data:
Reply from 192.168.1.1: bytes=32 time=1ms TTL=64
Reply from 192.168.1.1: bytes=32 time=1ms TTL=64
Reply from 192.168.1.1: bytes=32 time<1ms TTL=64
Reply from 192.168.1.1: bytes=32 time=1ms TTL=64

Ping statistics for 192.168.1.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 1ms, Average = 0ms
```

**Troubleshooting with ping:**

```
Scenario: Cannot access Internet

Test 1: Ping localhost
  ping 127.0.0.1
  Purpose: ทดสอบ TCP/IP stack
  
  ✓ Success → TCP/IP stack OK
  ✗ Fail → TCP/IP problem (reinstall)

Test 2: Ping own IP
  ping 192.168.1.100 (own IP)
  Purpose: ทดสอบ NIC
  
  ✓ Success → NIC OK
  ✗ Fail → NIC problem

Test 3: Ping default gateway
  ping 192.168.1.1
  Purpose: ทดสอบ local network connectivity
  
  ✓ Success → Local network OK
  ✗ Fail → Check cable, switch, gateway

Test 4: Ping remote IP (public)
  ping 8.8.8.8
  Purpose: ทดสอบ Internet connectivity
  
  ✓ Success → Internet OK, likely DNS problem
  ✗ Fail → Routing/ISP problem

Test 5: Ping domain name
  ping google.com
  Purpose: ทดสอบ DNS resolution
  
  ✓ Success → Everything OK!
  ✗ Fail → DNS problem
```

---

#### 3. traceroute (tracert in Windows)

**คำจำกัดความ:**

- แสดง **path** ที่ packets เดินทาง
- แสดงแต่ละ **hop** (router) ระหว่างทาง
- วัด **latency** ของแต่ละ hop

**Syntax:**

```bash
tracert <destination>   (Windows)
traceroute <destination> (Linux/Mac)
```

**Example:**

```cmd
tracert google.com

Output:
Tracing route to google.com [142.250.185.46]
over a maximum of 30 hops:

  1    <1 ms    <1 ms    <1 ms  192.168.1.1 (Home router)
  2     5 ms     5 ms     5 ms  10.0.0.1 (ISP router)
  3    10 ms    10 ms    10 ms  172.16.1.1
  4    15 ms    15 ms    15 ms  203.0.113.5
  5    20 ms    20 ms    20 ms  142.250.185.46 (google.com)

Trace complete.
```

**Interpretation:**

```
Hop 1: Home router (very fast, < 1ms)
Hop 2: ISP router (5ms)
Hop 3-5: Internet routers (increasing latency)

ถ้าเห็น:
  * * * Request timed out.
  → Router ไม่ respond to ICMP (ไม่จำเป็นว่าจะมีปัญหา)
  
  High latency at specific hop:
  → ปัญหาที่ hop นั้น

  Packets reach certain hop then stop:
  → ปัญหาที่ hop ถัดไป (routing, firewall)
```

**Use Cases:**

```
✓ หา bottleneck (hop ไหนช้า)
✓ หา routing problems
✓ Verify path (เดินทางตามที่คาดไหม)
✓ Diagnose latency issues
```

---

#### 4. nslookup

**คำจำกัดความ:**

- Query **DNS servers**
- แปลง domain name → IP
- Troubleshoot DNS problems

**Syntax:**

```bash
nslookup <domain>
nslookup <domain> <dns-server>
```

**Examples:**

```cmd
# Basic lookup
nslookup google.com

Output:
Server:  dns.google
Address:  8.8.8.8

Non-authoritative answer:
Name:    google.com
Addresses:  142.250.185.46
            2607:f8b0:4004:c07::71

# Query specific DNS server
nslookup google.com 1.1.1.1

# Query specific record type
nslookup -type=MX google.com
  → MX records (mail servers)

nslookup -type=NS google.com
  → NS records (name servers)

nslookup -type=A google.com
  → A records (IPv4 addresses)

# Reverse lookup (IP → name)
nslookup 142.250.185.46
```

**Troubleshooting DNS:**

```
Scenario: Cannot access www.company.com

Test 1: nslookup www.company.com
  ✓ Returns IP → DNS working
  ✗ No response / Error → DNS problem

  Errors:
    "DNS request timed out"
      → Cannot reach DNS server
      → Check DNS server IPs
      → Check firewall (port 53)
    
    "Server failed" / "SERVFAIL"
      → DNS server cannot resolve
      → Domain ไม่มีจริง
      → DNS server configuration ผิด

Test 2: nslookup www.company.com 8.8.8.8
  Purpose: ทดสอบกับ public DNS
  
  ✓ Works with 8.8.8.8 → Internal DNS problem
  ✗ Doesn't work → Domain problem / Internet problem

Test 3: nslookup 142.250.185.46
  Purpose: Reverse lookup
  
  ✓ Returns google.com → DNS working both ways
```

---

#### 5. netstat

**คำจำกัดความ:**

- แสดง **network connections**
- แสดง **listening ports**
- แสดง **routing table**
- แสดง **network statistics**

**Common Options:**

```cmd
netstat
  แสดง active connections

netstat -a
  แสดง all connections และ listening ports

netstat -n
  แสดง addresses เป็นตัวเลข (ไม่ resolve names)

netstat -r
  แสดง routing table

netstat -s
  แสดง statistics

netstat -ano (Windows)
  -a: all
  -n: numeric
  -o: show process ID (PID)

netstat -tulpn (Linux)
  -t: TCP
  -u: UDP
  -l: listening
  -p: process name
  -n: numeric
```

**Examples:**

```cmd
# แสดง active connections
netstat -an

Output:
Proto  Local Address          Foreign Address        State
TCP    192.168.1.100:50001    142.250.185.46:443     ESTABLISHED
TCP    192.168.1.100:50002    8.8.8.8:53             TIME_WAIT
TCP    0.0.0.0:445            0.0.0.0:0              LISTENING
UDP    192.168.1.100:68       *:*

# แสดง routing table
netstat -r

Output:
Active Routes:
Network Destination    Netmask          Gateway       Interface
0.0.0.0                0.0.0.0          192.168.1.1   192.168.1.100
192.168.1.0            255.255.255.0    On-link       192.168.1.100

# หา process ที่ใช้ port
netstat -ano | findstr :80

Output:
TCP    0.0.0.0:80             0.0.0.0:0              LISTENING       1234
  → PID 1234 listening on port 80

Then check:
  tasklist | findstr 1234
  → See which program (e.g., Apache, IIS)
```

**Use Cases:**

```
✓ หา listening ports (security audit)
✓ หา active connections (ใครต่อกับใคร)
✓ Troubleshoot connectivity (connection established?)
✓ Verify routing table
✓ ตรวจสอบ malware (unknown connections)
```

---

#### 6. arp

**คำจำกัดความ:**

- แสดง **ARP cache** (IP ↔ MAC mappings)
- ใช้ troubleshoot Layer 2 connectivity

**Commands:**

```cmd
# แสดง ARP cache
arp -a

Output:
Interface: 192.168.1.100 --- 0x2
  Internet Address      Physical Address      Type
  192.168.1.1           aa-bb-cc-dd-ee-ff     dynamic
  192.168.1.50          11-22-33-44-55-66     dynamic
  192.168.1.255         ff-ff-ff-ff-ff-ff     static

# ลบ ARP entry
arp -d 192.168.1.50

# เพิ่ม static ARP entry
arp -s 192.168.1.50 11-22-33-44-55-66
```

**Troubleshooting:**

```
Problem: Can ping IP but cannot access device

Possible cause: ARP cache มี wrong MAC address

Solution:
  1. arp -a (ดู MAC address)
  2. เปรียบเทียบกับ MAC จริง (ดูที่ device)
  3. ถ้าไม่ตรง: arp -d <IP> (ลบ)
  4. ping <IP> (สร้าง ARP entry ใหม่)
  5. ทดสอบอีกครั้ง
```

---

### Common Network Problems and Solutions

#### Problem 1: No Network Connectivity

**Symptoms:**

- ไม่สามารถ access anything
- No IP address (169.254.x.x = APIPA)

**Troubleshooting:**

```
Step 1: Check physical
  ✓ Cable plugged in?
  ✓ Link light on NIC?
  ✓ Cable good? (try different cable)

Step 2: Check IP configuration
  ipconfig
  
  ถ้าเห็น 169.254.x.x (APIPA):
    → ไม่ได้ IP จาก DHCP
    → Check:
       - DHCP server running?
       - Cable to DHCP server OK?
       - Switch port configured correctly?

Step 3: Manual test
  Try static IP:
    IP: 192.168.1.100
    Mask: 255.255.255.0
    Gateway: 192.168.1.1
    DNS: 8.8.8.8
  
  ถ้าใช้ได้:
    → DHCP problem
  ถ้ายังไม่ได้:
    → Physical/Switch problem

Step 4: Ping default gateway
  ping 192.168.1.1
  
  ✓ Success → network OK, problem elsewhere
  ✗ Fail → local network problem
```

---

#### Problem 2: Cannot Access Internet

**Symptoms:**

- Local network OK
- Cannot access Internet

**Troubleshooting:**

```
Step 1: Ping gateway
  ping 192.168.1.1
  ✓ → Gateway reachable

Step 2: Ping public IP
  ping 8.8.8.8
  
  ✓ Success → Internet OK, likely DNS problem
  ✗ Fail → Router/ISP problem

If Step 2 fails:
  Check:
    - Router WAN interface up?
    - ISP connection OK?
    - Router has public IP? (check router config)
    - Default route configured?
      netstat -r (check 0.0.0.0 route)

Step 3: Check DNS
  ping google.com
  
  ✗ Fail but ping 8.8.8.8 works:
    → DNS problem
    
  nslookup google.com
  
  ถ้าไม่ resolve:
    → Change DNS servers (ipconfig or network settings)
    → Try 8.8.8.8, 1.1.1.1
    → ipconfig /flushdns (clear cache)
```

---

#### Problem 3: Slow Network

**Symptoms:**

- Network works but very slow

**Troubleshooting:**

```
Step 1: Determine scope
  - All devices slow? → Network-wide problem
  - Single device slow? → Device problem

Step 2: Check bandwidth usage
  - Use monitoring tool (PRTG, Nagios)
  - Check for:
     - Bandwidth hog (large downloads)
     - Broadcast storm
     - Malware (botnet, crypto miner)

Step 3: Check for errors
  Router/Switch:
    show interfaces
    
  Look for:
    - CRC errors (bad cable)
    - Collisions (duplex mismatch)
    - Drops (congestion)

Step 4: Check duplex/speed
  Duplex mismatch:
    One side = Full, Other side = Half
    → Lots of collisions → slow
  
  Check:
    PC NIC settings
    Switch port: show interfaces status
    
  Fix: Set both sides to auto or both to fixed (full-duplex)

Step 5: Check for loops
  Spanning Tree disabled?
    → Broadcast storm → network down
  
  Check:
    Switch: show spanning-tree

Step 6: Test throughput
  Tools: iperf, speedtest.net
  
  Compare:
    - Actual vs Expected throughput
    - Different times (congestion?)
```

---

#### Problem 4: Intermittent Connectivity

**Symptoms:**

- Works sometimes, fails other times

**Troubleshooting:**

```
Step 1: Check physical
  - Loose cable?
  - Damaged cable? (intermittent connection)
  - Bad port? (try different port)

Step 2: Monitor over time
  ping -t 192.168.1.1
  
  Watch for:
    - Request timeout (complete loss)
    - High latency (> 100ms to gateway = problem)
    - Patterns (every X minutes)

Step 3: Check logs
  Router/Switch logs:
    - Interface up/down events?
    - Errors?
  
  Commands:
    show logging
    show interfaces

Step 4: Environmental
  - Electrical interference? (near motors, fluorescent lights)
  - Wireless interference? (neighboring APs on same channel)
  - Temperature? (equipment overheating)

Step 5: DHCP lease
  Short lease time?
    → Renewal fails intermittently?
  
  Check:
    ipconfig /all (see lease obtained/expires)
    Increase lease time if needed
```

---

#### Problem 5: Cannot Access Specific Server

**Symptoms:**

- Internet works
- Other servers work
- 1 specific server ไม่ได้

**Troubleshooting:**

```
Step 1: Can you reach the server IP?
  ping 192.168.1.50
  
  ✓ Success → Server reachable, likely application problem
  ✗ Fail → Network problem

If ping fails:
  tracert 192.168.1.50
  → See where packets stop
  
  Check:
    - Routing (can you route to that subnet?)
    - Firewall (blocking ICMP?)
    - Server firewall (Windows Firewall?)

Step 2: Can you access the service?
  telnet 192.168.1.50 80 (test HTTP)
  telnet 192.168.1.50 443 (test HTTPS)
  
  Connected:
    → Port open, service running
  Connection failed:
    → Port closed/filtered
    
  Check:
    - Service running? (check server)
    - Firewall rule? (check firewall)
    - Server listening on correct port?
      netstat -an (on server)

Step 3: DNS
  nslookup server.company.local
  
  ถ้า resolve เป็น wrong IP:
    → DNS record ผิด (update DNS)
  
  ถ้าไม่ resolve:
    → DNS record ไม่มี (add DNS record)

Step 4: Access Control
  ACL blocking?
  Check router/firewall ACLs
  
  Commands:
    show access-lists
    show ip access-lists
```

---

## Summary (สรุป Module 17)

Module 17 นี้เราได้เรียนรู้:

### 17.1 Devices in a Small Network

- **Router**: Gateway, NAT, DHCP, Firewall
- **Switch**: Layer 2 forwarding, VLANs, PoE
- **Wireless AP**: Wi-Fi connectivity, Standalone vs Controller-based
- **Firewall**: Packet filtering, Security
- **Servers**: File, Print, DHCP, DNS, Email

### 17.2 Small Network Applications

- **Email**: SMTP, POP3, IMAP, Cloud vs On-Premises
- **Web Services**: HTTP/HTTPS, Internal/External
- **File Sharing**: NAS, File Server, Cloud
- **VoIP**: IP phones, IP PBX, SIP, QoS
- **Video Conferencing**: Zoom, Teams, Bandwidth requirements

### 17.3 Scale to Larger Networks

- **Hierarchical Design**: Access, Distribution, Core layers
- **Redundancy**: Eliminate SPOFs, HSRP, Dual ISPs
- **Scalability**: Modular design, IP planning
- **Documentation**: Topology diagrams, IP plans, Configs, Change logs
- **Monitoring**: SNMP, Syslog, NetFlow

### 17.4 Verify Connectivity

- **Troubleshooting Methodology**: 7 steps
- **Tools**:
    - ipconfig/ifconfig - IP configuration
    - ping - Reachability testing
    - tracert/traceroute - Path tracing
    - nslookup - DNS queries
    - netstat - Connections, Routing table
    - arp - ARP cache
- **Common Problems**: No connectivity, No Internet, Slow network, Intermittent, Server access

---

**สิ่งสำคัญที่ต้องจำ:**

- **Small Network** = < 50-200 users, Simple yet reliable
- **Device Roles**: Router (routing), Switch (switching), AP (wireless), Firewall (security)
- **PoE**: จ่ายไฟผ่าน Ethernet (IP phones, APs, cameras)
- **Hierarchical Design**: Access → Distribution → Core
- **Redundancy** = ป้องกัน single points of failure
- **HSRP/VRRP**: Virtual gateway IP, Active/Standby routers
- **Troubleshooting**: Systematic approach (7 steps)
- **Ping Tests**: Localhost → Own IP → Gateway → Remote IP → Domain
- **tracert**: หา path และ bottlenecks
- **nslookup**: Troubleshoot DNS
- **Documentation**: ต้องมี, ต้อง update!

---

**[ไฟล์ Module 17 - Build a Small Network สมบูรณ์แล้ว!]**

**CCNA Course 1 - ALL MODULES COMPLETE! 🎉**

✅ Module 1-13: Networking Fundamentals  
✅ Module 14: Transport Layer (TCP/UDP)  
✅ Module 15: Application Layer  
✅ Module 16: Network Security  
✅ Module 17: Build a Small Network

คุณพร้อมสำหรับ CCNA Course 2 แล้ว! 🚀