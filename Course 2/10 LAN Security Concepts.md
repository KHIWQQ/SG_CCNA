# CCNA 2: Module 10 - LAN Security Concepts

## แนวคิดการรักษาความปลอดภัย LAN

---

## สารบัญ

1. [วัตถุประสงค์ของ Module](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#%E0%B8%A7%E0%B8%B1%E0%B8%95%E0%B8%96%E0%B8%B8%E0%B8%9B%E0%B8%A3%E0%B8%B0%E0%B8%AA%E0%B8%87%E0%B8%84%E0%B9%8C%E0%B8%82%E0%B8%AD%E0%B8%87-module)
2. [Endpoint Security](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#1-endpoint-security)
3. [Access Control (AAA และ 802.1X)](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#2-access-control-aaa-%E0%B9%81%E0%B8%A5%E0%B8%B0-8021x)
4. [Layer 2 Security Threats](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#3-layer-2-security-threats)
5. [MAC Address Table Attack](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#4-mac-address-table-attack)
6. [LAN Attacks](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#5-lan-attacks)
7. [สรุป](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#%E0%B8%AA%E0%B8%A3%E0%B8%B8%E0%B8%9B)

---

## วัตถุประสงค์ของ Module

เมื่อจบ Module นี้ คุณจะสามารถ:

- ✅ อธิบาย endpoint security และวิธีป้องกัน
- ✅ เข้าใจการทำงานของ AAA (Authentication, Authorization, Accounting)
- ✅ อธิบายการทำงานของ 802.1X
- ✅ ระบุช่องโหว่ใน Layer 2
- ✅ อธิบาย MAC address table attacks
- ✅ เข้าใจการโจมตี LAN ประเภทต่างๆ
- ✅ Configure security features เพื่อป้องกัน

---

## 1. Endpoint Security

### การรักษาความปลอดภัยอุปกรณ์ปลายทาง

### 1.1 Network Attacks Today

**การโจมตีเครือข่ายสมัยใหม่:**

**1. DDoS (Distributed Denial of Service)**

```
คืออะไร:
- การโจมตีแบบประสานจากหลายอุปกรณ์
- อุปกรณ์ที่โดนควบคุมเรียกว่า "Zombies" หรือ "Bots"
- จุดประสงค์: ทำให้เว็บไซต์หรือบริการล่ม

ตัวอย่าง:
Internet
    ↓
[Target Website]
    ↑
[Zombie 1] ----\
[Zombie 2] -----→ Flood Traffic
[Zombie 3] ----/
...
[Zombie N]

ผลกระทบ:
❌ Website ช้าหรือล่ม
❌ Service unavailable
❌ Business disruption
❌ Financial loss
```

**2. Data Breach (การรั่วไหลข้อมูล)**

```
คืออะไร:
- การโจมตีเพื่อขโมยข้อมูลความลับ
- เจาะ servers หรือ hosts
- เข้าถึงข้อมูลส่วนบุคคล, การเงิน, ทรัพย์สินทางปัญญา

ตัวอย่างข้อมูลที่ถูกขโมย:
- Credit card numbers
- Personal information (SSN, addresses)
- Medical records
- Business secrets
- Login credentials

ผลกระทบ:
❌ Loss of customer trust
❌ Legal penalties (GDPR, PDPA)
❌ Financial loss
❌ Reputation damage
```

**3. Malware (มัลแวร์)**

```
คืออะไร:
- Malicious Software
- โปรแกรมที่ทำร้ายระบบ
- แพร่กระจายผ่าน email, downloads, USB

ประเภท Malware:
1. Virus - ติดไฟล์และแพร่กระจาย
2. Worm - แพร่กระจายเองอัตโนมัติ
3. Trojan - ปลอมตัวเป็นโปรแกรมปกติ
4. Ransomware - เข้ารหัสไฟล์เรียกค่าไถ่
5. Spyware - ดักจับข้อมูล
6. Rootkit - ซ่อนตัวในระบบ
7. Adware - โฆษณาที่ไม่พึงประสงค์

ผลกระทบ:
❌ System slowdown
❌ Data loss/theft
❌ Privacy violation
❌ System damage
```

### 1.2 Network Security Devices

**อุปกรณ์รักษาความปลอดภัยเครือข่าย:**

**1. VPN-Enabled Router**

```
หน้าที่:
- สร้าง secure connection สำหรับ remote users
- เข้ารหัสข้อมูลผ่าน public network (Internet)
- เข้าถึง enterprise network อย่างปลอดภัย

Technology:
- IPsec VPN
- SSL VPN
- Site-to-Site VPN
- Remote Access VPN

[Remote User] --[Internet]-- [VPN Router] --[Enterprise Network]
                   (Encrypted Tunnel)
```

**2. NGFW (Next-Generation Firewall)**

```
คุณสมบัติ:
- Stateful packet inspection
- Application awareness and control
- Intrusion Prevention System (IPS)
- Advanced Malware Protection (AMP)
- URL filtering
- Deep packet inspection

Traditional Firewall vs NGFW:
Traditional:
- Layer 3-4 filtering
- IP/Port based rules
- Stateful inspection

NGFW:
- All of above PLUS
- Layer 7 (Application) awareness
- User/Identity awareness
- Malware detection
- SSL inspection
```

**3. IPS (Intrusion Prevention System)**

```
หน้าที่:
- ตรวจจับและป้องกันการโจมตี
- วิเคราะห์ traffic แบบ real-time
- Block malicious traffic อัตโนมัติ

IDS vs IPS:
IDS (Detection): ตรวจจับ + Alert
IPS (Prevention): ตรวจจับ + Block

Placement:
Internet --> Firewall --> IPS --> Internal Network
```

**4. ESA (Email Security Appliance)**

```
หน้าที่:
- กรองและป้องกัน email threats
- Block spam และ phishing
- Encrypt outgoing emails
- Data Loss Prevention (DLP)

สถิติ:
- 85% ของ email ทั้งหมดเป็น spam
- 95% ของการโจมตีมาจาก spear phishing

Protection:
✅ Anti-spam
✅ Anti-malware
✅ Phishing protection
✅ Email encryption
✅ DLP
```

**5. WSA (Web Security Appliance)**

```
หน้าที่:
- ควบคุมการเข้าถึง web
- กรอง URLs
- Block malicious websites
- Monitor web usage

Features:
- URL filtering
- Malware scanning
- Application visibility
- Bandwidth management
- User authentication
- Reporting and analytics

Use Cases:
- Block social media
- Limit streaming
- Prevent malware downloads
- Enforce acceptable use policy
```

### 1.3 Endpoint Threats

**Endpoints (อุปกรณ์ปลายทาง):**

```
อุปกรณ์ที่เชื่อมต่อกับ network:
- Laptops/Desktops
- Servers
- IP Phones
- Printers
- Mobile devices
- BYOD (Bring Your Own Device)
- IoT devices
```

**ภัยคุกคามที่ Endpoints:**

**1. Malware-related Attacks**

```
แหล่งที่มา:
- Email attachments
- Malicious websites
- Downloads
- USB drives
- Network shares

การป้องกัน:
✅ Antivirus/Anti-malware
✅ Host-based Firewall
✅ HIPS (Host-based IPS)
✅ Application whitelisting
✅ Regular updates
✅ User education
```

**2. Phishing และ Spear Phishing**

```
Phishing:
- Email หลอกลวงทั่วไป
- ส่งให้หลายคนพร้อมกัน
- ปลอมตัวเป็น banks, services

Spear Phishing:
- Target คนๆ เดียว
- Personalized messages
- เป้าหมายมักเป็น executives, managers
- อันตรายกว่า phishing ธรรมดา

ตัวอย่าง:
"Dear CEO, urgent wire transfer needed..."
"IT Security: Your password expires today..."
"HR: Review your benefits package..."

การป้องกัน:
✅ Security awareness training
✅ Email filtering
✅ Two-factor authentication
✅ Verify requests through other channels
```

**3. Zero-Day Attacks**

```
คืออะไร:
- การโจมตีที่ใช้ vulnerability ที่ยังไม่มี patch
- "Zero-day" = วันที่ 0 ที่ vendor รู้เรื่อง
- ยากต่อการป้องกัน

Timeline:
Day 0: Vulnerability discovered by attacker
Day X: Vendor discovers vulnerability  
Day Y: Patch released
Day Z: Users apply patch

การป้องกัน:
✅ Behavioral analysis
✅ Sandboxing
✅ Network segmentation
✅ Principle of least privilege
```

### 1.4 Endpoint Protection

**Host-based Security Solutions:**

**1. Antivirus/Anti-malware**

```
หน้าที่:
- Detect และ remove malware
- Real-time scanning
- Scheduled scans
- Quarantine infected files

Detection Methods:
- Signature-based (รู้จัก malware)
- Heuristic-based (พฤติกรรมผิดปกติ)
- Behavior-based (ดู actions)
- Cloud-based (ตรวจสอบกับ cloud database)
```

**2. Host-based Firewall**

```
หน้าที่:
- กรอง inbound/outbound traffic
- Block unauthorized connections
- Application control

Configuration:
- Allow/Deny rules
- Port-based filtering
- Application-based filtering
- Zone-based filtering
```

**3. HIPS (Host-based Intrusion Prevention System)**

```
หน้าที่:
- ตรวจจับและป้องกันการโจมตี
- Monitor system activities
- Block suspicious behaviors
- Complement antivirus

Features:
- Buffer overflow protection
- Registry protection
- File system protection
- Network traffic analysis
```

**Advanced Endpoint Security:**

**Cisco AMP (Advanced Malware Protection)**

```
คุณสมบัติ:
- Continuous monitoring
- Retrospective security
- File trajectory
- Outbreak control
- Sandboxing integration

Workflow:
1. File appears on endpoint
2. Check local cache
3. Check cloud database
4. Submit to sandbox if unknown
5. Get verdict
6. Block or allow
7. Continue monitoring
```

**EDR (Endpoint Detection and Response)**

```
คุณสมบัติ:
- Threat hunting
- Forensics
- Incident response
- Behavioral analysis
- Timeline reconstruction

Use Cases:
- Investigate incidents
- Root cause analysis
- Threat intelligence
- Compliance reporting
```

### 1.5 Network Admission Control

**NAC (Network Admission Control):**

**Purpose:**

```
ควบคุมว่าอุปกรณ์ไหนสามารถเข้า network ได้
ตรวจสอบ security posture ก่อนอนุญาต
Enforce security policies
```

**NAC Process:**

```
Step 1: Device connects to network
        ↓
Step 2: NAC agent checks security posture
        - OS updates current?
        - Antivirus installed and updated?
        - Firewall enabled?
        - Encryption enabled?
        ↓
Step 3: NAC policy evaluation
        ↓
Step 4: Decision
        - Compliant → Full access
        - Non-compliant → Quarantine/Remediation VLAN
        - Unknown → Guest access/Deny
```

**NAC Components:**

```
1. NAC Server
   - Policy database
   - Decision engine
   - Reporting

2. NAC Agent
   - Installed on endpoints
   - Reports security posture
   - Can be:
     * Permanent (installed software)
     * Temporal (web-based)
     * Agentless (scan from network)

3. Network Access Devices
   - Switches
   - Wireless controllers
   - VPN concentrators
   - Enforce NAC decisions
```

**Cisco ISE (Identity Services Engine):**

```
Next-generation NAC solution

Features:
✅ Policy-based access control
✅ Guest management
✅ BYOD support
✅ Device profiling
✅ Threat containment
✅ Integration with other security tools

Use Cases:
- Corporate network access
- Guest WiFi
- BYOD programs
- IoT device management
- Compliance enforcement
```

---

## 2. Access Control (AAA และ 802.1X)

### การควบคุมการเข้าถึง

### 2.1 AAA Overview

**AAA = Authentication, Authorization, Accounting**

**AAA Framework:**

```
┌─────────────────┐
│ Authentication  │ ← "Who are you?"
│                 │   ตรวจสอบตัวตน
└────────┬────────┘
         ↓
┌─────────────────┐
│ Authorization   │ ← "What can you do?"
│                 │   กำหนดสิทธิ์
└────────┬────────┘
         ↓
┌─────────────────┐
│ Accounting      │ ← "What did you do?"
│                 │   บันทึกการใช้งาน
└─────────────────┘
```

### 2.2 Authentication (การยืนยันตัวตน)

**วิธีการ Authentication:**

**1. Something You Know (ความรู้)**

```
Examples:
- Username/Password
- PIN
- Security questions

Pros:
✅ Easy to implement
✅ Familiar to users
✅ No special hardware

Cons:
❌ Can be stolen/guessed
❌ Forgotten
❌ Shared
❌ Weak passwords
```

**2. Something You Have (การครอบครอง)**

```
Examples:
- Smart card
- Token (RSA SecurID)
- Mobile phone (SMS code)
- USB security key

Pros:
✅ Harder to steal
✅ Physical possession required

Cons:
❌ Can be lost
❌ Cost of deployment
❌ Hardware failures
```

**3. Something You Are (ลักษณะทางชีวภาพ)**

```
Examples:
- Fingerprint
- Face recognition
- Iris scan
- Voice recognition
- DNA

Pros:
✅ Unique to individual
✅ Cannot be lost or forgotten
✅ Difficult to fake

Cons:
❌ Expensive
❌ Privacy concerns
❌ False positives/negatives
```

**Multi-Factor Authentication (MFA):**

```
ใช้ 2 หรือ 3 factors รวมกัน

Two-Factor Authentication (2FA):
Password (know) + SMS code (have)
Password (know) + Fingerprint (are)

Three-Factor Authentication:
Password + Smart card + Fingerprint

Benefits:
✅ Much more secure
✅ Harder to compromise
✅ Recommended best practice
```

### 2.3 Authorization (การให้สิทธิ์)

**Authorization คืออะไร:**

```
กำหนดว่า user ที่ authenticate แล้วสามารถทำอะไรได้บ้าง

Examples:
- Network access (VLANs, subnets)
- File access (read, write, execute)
- Application access
- Resource quotas
- Time restrictions
```

**Authorization Methods:**

**1. Local Database**

```
Router/Switch Configuration:
username admin privilege 15 secret Cisco123
username user1 privilege 1 secret User123

Privilege Levels:
- 0: User EXEC (limited)
- 1: User EXEC 
- 15: Privileged EXEC (enable)
- 2-14: Custom levels

Pros:
✅ Simple
✅ No external server needed

Cons:
❌ Not scalable
❌ Configuration per device
❌ Limited features
```

**2. Server-based (TACACS+/RADIUS)**

```
Centralized authorization
Fine-grained control
Command authorization
Scalable

Example Authorization:
User: network_admin
- Can: configure interfaces
- Can: configure VLANs
- Cannot: reload system
- Cannot: erase startup-config
```

**Authorization Policies:**

```
Role-Based Access Control (RBAC):
- Define roles
- Assign permissions to roles
- Assign users to roles

Example Roles:
┌──────────────┬─────────────────────────┐
│ Role         │ Permissions             │
├──────────────┼─────────────────────────┤
│ Admin        │ Full access             │
│ Network Eng  │ Configure network only  │
│ Help Desk    │ Show commands only      │
│ Guest        │ Internet access only    │
└──────────────┴─────────────────────────┘
```

### 2.4 Accounting (การบันทึก)

**Accounting คืออะไร:**

```
เก็บ log ว่าใครทำอะไร เมื่อไหร่

Information Collected:
- Username
- Start/Stop time
- Commands executed
- Bytes transferred
- Services used
- Connection duration
```

**Accounting Use Cases:**

**1. Security Auditing**

```
ตรวจสอบว่าใครทำอะไร
Identify suspicious activities
Forensics investigations

Example Questions:
- Who configured this interface?
- What commands did user X run?
- When was this change made?
```

**2. Billing**

```
คำนวณค่าใช้จ่ายตามการใช้งาน
ISP billing
Resource usage tracking

Example:
- Bandwidth used
- Connection duration
- Data transferred
```

**3. Trend Analysis**

```
วิเคราะห์รูปแบบการใช้งาน
Capacity planning
Performance optimization

Example:
- Peak usage times
- Most used services
- Resource consumption patterns
```

**4. Compliance**

```
ตอบสนอง regulatory requirements
Audit trails
Reporting

Standards:
- SOX (Sarbanes-Oxley)
- HIPAA (Healthcare)
- PCI DSS (Payment cards)
- GDPR (Privacy)
```

**Accounting Logs:**

```
Example Log Entry:
Timestamp: 2025-11-18 14:30:15
User: admin
Source: 192.168.1.100
Action: configure terminal
Command: interface GigabitEthernet0/1
Command: ip address 10.1.1.1 255.255.255.0
Command: no shutdown
Status: Success
Session Duration: 5 minutes
```

### 2.5 AAA Implementation

**AAA Protocols:**

**1. RADIUS (Remote Authentication Dial-In User Service)**

```
RFC 2865

Characteristics:
- Uses UDP (ports 1812/1813 or 1645/1646)
- Combines authentication and authorization
- Standard protocol (open)
- Widely supported
- Encrypts only password

Packet Types:
- Access-Request
- Access-Accept
- Access-Reject
- Access-Challenge
- Accounting-Request
- Accounting-Response

Common Uses:
- Network access (WiFi, VPN)
- ISP authentication
- Basic device access
```

**2. TACACS+ (Terminal Access Controller Access-Control System Plus)**

```
Cisco proprietary (but open documentation)

Characteristics:
- Uses TCP (port 49)
- Separates AAA functions
- More granular control
- Encrypts entire packet
- Better for device administration

Features:
- Command authorization
- Per-command accounting
- More detailed logging
- Failover support

Common Uses:
- Network device administration
- Cisco equipment
- Fine-grained control needed
```

**RADIUS vs TACACS+:**

```
┌─────────────────┬────────────┬────────────┐
│ Feature         │ RADIUS     │ TACACS+    │
├─────────────────┼────────────┼────────────┤
│ Transport       │ UDP        │ TCP        │
│ Port            │ 1812/1813  │ 49         │
│ Encryption      │ Password   │ All packet │
│ AAA Separation  │ Combined   │ Separate   │
│ Standard        │ Open       │ Cisco      │
│ Multiprotocol   │ No         │ Yes        │
│ Command Auth    │ No         │ Yes        │
│ Best For        │ Network    │ Device     │
│                 │ Access     │ Admin      │
└─────────────────┴────────────┴────────────┘
```

**AAA Configuration Methods:**

**1. Local AAA**

```cisco
! Enable AAA
Router(config)# aaa new-model

! Authentication
Router(config)# aaa authentication login default local
Router(config)# aaa authentication enable default enable

! Authorization
Router(config)# aaa authorization exec default local
Router(config)# aaa authorization commands 15 default local

! Accounting
Router(config)# aaa accounting exec default start-stop local
Router(config)# aaa accounting commands 15 default start-stop local

! Create local users
Router(config)# username admin privilege 15 secret AdminPass123
Router(config)# username user1 privilege 1 secret UserPass123

! Apply to VTY lines
Router(config)# line vty 0 4
Router(config-line)# login authentication default
Router(config-line)# authorization exec default
```

**2. Server-Based AAA (RADIUS)**

```cisco
! Enable AAA
Router(config)# aaa new-model

! Define RADIUS server
Router(config)# radius server MY-RADIUS
Router(config-radius-server)# address ipv4 10.1.1.100 auth-port 1812 acct-port 1813
Router(config-radius-server)# key MySecretKey123

! Authentication
Router(config)# aaa authentication login default group radius local
! Try RADIUS first, fall back to local

! Authorization
Router(config)# aaa authorization exec default group radius local
Router(config)# aaa authorization network default group radius

! Accounting
Router(config)# aaa accounting exec default start-stop group radius
Router(config)# aaa accounting network default start-stop group radius
```

**3. Server-Based AAA (TACACS+)**

```cisco
! Enable AAA
Router(config)# aaa new-model

! Define TACACS+ server
Router(config)# tacacs server MY-TACACS
Router(config-server-tacacs)# address ipv4 10.1.1.200
Router(config-server-tacacs)# key MyTacacsKey456

! Authentication
Router(config)# aaa authentication login default group tacacs+ local

! Authorization
Router(config)# aaa authorization exec default group tacacs+ local
Router(config)# aaa authorization commands 15 default group tacacs+ local

! Accounting
Router(config)# aaa accounting exec default start-stop group tacacs+
Router(config)# aaa accounting commands 15 default start-stop group tacacs+
```

### 2.6 802.1X Port-Based Authentication

**802.1X Overview:**

```
IEEE Standard สำหรับ network access control
Port-based authentication
ใช้ร่วมกับ RADIUS/TACACS+
Authenticate ก่อนอนุญาตให้เข้า network
```

**802.1X Components:**

**1. Supplicant (Client)**

```
คือ:
- อุปกรณ์ที่ต้องการเข้า network
- มี 802.1X client software

Examples:
- Windows (built-in)
- macOS (built-in)
- Linux (wpa_supplicant)
- Mobile devices

Function:
- Request network access
- Provide credentials
- Respond to authentication
```

**2. Authenticator (Switch/AP)**

```
คือ:
- Network device (Switch หรือ Wireless AP)
- ตรงกลางระหว่าง client และ authentication server

Function:
- Relay messages
- Control port state (authorized/unauthorized)
- Enforce authentication

States:
- Unauthorized: traffic ไม่ผ่าน (except EAP)
- Authorized: traffic ผ่านได้ปกติ
```

**3. Authentication Server (RADIUS)**

```
คือ:
- RADIUS server
- ตรวจสอบ credentials
- ตัดสินใจ allow/deny

Examples:
- Cisco ISE
- Microsoft NPS
- FreeRADIUS
- Aruba ClearPass

Function:
- Validate credentials
- Check authorization policies
- Return decision to authenticator
```

**802.1X Process:**

```
Step 1: Connection
Client ────────────> Switch
    (Physical connection)

Step 2: EAPoL Start
Client ───EAPoL───> Switch
    (Request authentication)

Step 3: Identity Request
Switch ───EAPoL───> Client
    (Ask for identity)

Step 4: Identity Response
Client ───EAPoL───> Switch ───RADIUS───> Server
    (Send username)

Step 5: Authentication Challenge
Server ───RADIUS───> Switch ───EAPoL───> Client
    (Challenge)

Step 6: Credentials
Client ───EAPoL───> Switch ───RADIUS───> Server
    (Password/Certificate)

Step 7: Access Decision
Server ───RADIUS───> Switch
    (Access-Accept or Access-Reject)

Step 8: Port State
Switch: Change port to authorized/unauthorized
    ↓
Client: Network access granted/denied
```

**EAP (Extensible Authentication Protocol):**

```
Framework สำหรับ authentication
หลาย EAP methods มีให้เลือก

Common EAP Methods:
```

**1. EAP-MD5**

```
- Simple password-based
- One-way authentication
- ไม่แนะนำ (insecure)
```

**2. EAP-TLS**

```
- Certificate-based
- Mutual authentication
- Most secure
- Client และ server ต้องมี certificates

Pros:
✅ Very secure
✅ No passwords to steal
✅ Mutual authentication

Cons:
❌ Complex deployment
❌ Certificate management
❌ Cost
```

**3. EAP-PEAP (Protected EAP)**

```
- Protected tunnel
- Server certificate only
- Username/password inside tunnel

Pros:
✅ Secure (tunnel protects credentials)
✅ Easier than EAP-TLS
✅ Widely supported

Cons:
❌ Client doesn't authenticate server strongly
```

**4. EAP-FAST**

```
- Cisco proprietary
- No certificates needed
- Uses PACs (Protected Access Credentials)

Pros:
✅ Easy deployment
✅ No certificates
✅ Good performance

Cons:
❌ Cisco specific
❌ Less common
```

**802.1X Configuration:**

**Switch Configuration:**

```cisco
! Enable AAA
Switch(config)# aaa new-model

! Configure RADIUS server
Switch(config)# radius server ISE-SERVER
Switch(config-radius-server)# address ipv4 10.1.1.100 auth-port 1812 acct-port 1813
Switch(config-radius-server)# key RadiusKey123

! AAA configuration
Switch(config)# aaa authentication dot1x default group radius
Switch(config)# aaa authorization network default group radius

! Enable 802.1X globally
Switch(config)# dot1x system-auth-control

! Configure interface
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# description 802.1X Enabled Port
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10

! Enable 802.1X on port
Switch(config-if)# authentication port-control auto
Switch(config-if)# dot1x pae authenticator
Switch(config-if)# spanning-tree portfast
```

**802.1X Port Control Modes:**

```cisco
! Mode 1: auto (default)
authentication port-control auto
- Port starts unauthorized
- Client must authenticate
- ถ้า authenticate สำเร็จ → authorized

! Mode 2: force-authorized
authentication port-control force-authorized
- Port always authorized
- No authentication required
- เหมือนไม่มี 802.1X

! Mode 3: force-unauthorized
authentication port-control force-unauthorized
- Port always unauthorized
- ไม่อนุญาตให้เข้าเลย
- ใช้ disable port
```

**802.1X Violation Modes:**

```cisco
! authentication violation {shutdown | restrict | protect | replace}

Mode 1: shutdown (default)
- Shutdown port
- Need manual intervention
- err-disabled

Mode 2: restrict
- Drop unauthorized traffic
- Log violations
- Send SNMP trap
- Port ยัง up

Mode 3: protect
- Drop unauthorized traffic
- ไม่ log, ไม่ trap
- Silent drop

Mode 4: replace
- Replace current session
- Allow new authentication
```

**802.1X with Multiple Devices:**

```cisco
! Enable multiple hosts per port
Switch(config-if)# authentication host-mode multi-auth
! หรือ
Switch(config-if)# authentication host-mode multi-domain

Host Modes:
1. single-host (default)
   - 1 device per port
   
2. multi-host
   - หลาย devices
   - 1 device authenticate → ทุก device ได้เข้า
   
3. multi-auth
   - หลาย devices
   - แต่ละ device ต้อง authenticate
   
4. multi-domain
   - 1 data device + 1 voice device (IP Phone)
```

**802.1X with MAB (MAC Authentication Bypass):**

```cisco
! Enable MAB as fallback
Switch(config-if)# authentication fallback MAB
Switch(config-if)# mab

Scenario:
1. Try 802.1X first
2. ถ้าไม่ได้ (no supplicant) → Try MAB
3. Authenticate ด้วย MAC address

Use Case:
- Printers (no 802.1X support)
- IP Cameras
- Legacy devices
```

**Verify 802.1X:**

```cisco
! Show 802.1X status
Switch# show dot1x all

! Show authentication sessions
Switch# show authentication sessions

Interface        MAC Address    Method  Domain  Status
Gi0/1            0011.2233.4455 dot1x   DATA    Auth
Gi0/2            0055.6677.8899 mab     DATA    Auth

! Show 802.1X statistics
Switch# show dot1x statistics
```

---

## 3. Layer 2 Security Threats

### ภัยคุกคามใน Layer 2

### 3.1 Layer 2 Vulnerabilities

**ทำไม Layer 2 เป็นจุดอ่อน:**

```
OSI Model Security:
Layer 7 - Application  } 
Layer 6 - Presentation }  Typically protected
Layer 5 - Session      }  (Firewall, IPS, etc.)
Layer 4 - Transport    }
Layer 3 - Network      }
────────────────────────────
Layer 2 - Data Link    }  Often overlooked!
Layer 1 - Physical     }  Weak link

Problem:
- Admins focus on Layer 3-7
- Layer 2 มักถูกมองว่าปลอดภัย (trusted)
- ถ้า Layer 2 ถูกโจมตี → Layer บนๆ ก็ไม่ปลอดภัย
```

**Layer 2 Attack Categories:**

```
1. MAC Table Attacks
   - MAC flooding
   - MAC spoofing

2. VLAN Attacks
   - VLAN hopping
   - VLAN double-tagging
   - Switch spoofing

3. DHCP Attacks
   - DHCP starvation
   - DHCP spoofing

4. ARP Attacks
   - ARP spoofing
   - ARP poisoning

5. Address Spoofing
   - IP spoofing
   - MAC spoofing

6. STP Attacks
   - STP manipulation
   - Root bridge hijacking
```

### 3.2 Switch Attack Mitigation Overview

**Security Features ที่ใช้ป้องกัน:**

```
┌──────────────────────┬────────────────────────┐
│ Security Feature     │ Protects Against       │
├──────────────────────┼────────────────────────┤
│ Port Security        │ MAC flooding           │
│                      │ MAC spoofing           │
│                      │ DHCP starvation        │
├──────────────────────┼────────────────────────┤
│ DHCP Snooping        │ DHCP spoofing          │
│                      │ DHCP starvation        │
├──────────────────────┼────────────────────────┤
│ DAI                  │ ARP spoofing           │
│ (Dynamic ARP Inspect)│ ARP poisoning          │
├──────────────────────┼────────────────────────┤
│ IPSG                 │ IP spoofing            │
│ (IP Source Guard)    │ MAC spoofing           │
├──────────────────────┼────────────────────────┤
│ BPDU Guard           │ STP attacks            │
│ Root Guard           │ Rogue switches         │
└──────────────────────┴────────────────────────┘
```

---

## 4. MAC Address Table Attack

### การโจมตี MAC Address Table

### 4.1 Switch Operation Review

**Normal Switch Operation:**

```
MAC Address Table (CAM Table):
┌─────────────┬──────────┬──────┐
│ MAC Address │ Port     │ VLAN │
├─────────────┼──────────┼──────┤
│ 0001.1111   │ Fa0/1    │ 1    │
│ 0002.2222   │ Fa0/2    │ 1    │
│ 0003.3333   │ Fa0/3    │ 1    │
└─────────────┴──────────┴──────┘

Process:
1. Frame arrives
2. Learn source MAC → Update table
3. Look up destination MAC
4. Forward out appropriate port

ถ้าไม่เจอ MAC → Flood to all ports (except incoming)
```

**MAC Table Capacity:**

```
Typical Switch Capacities:
- Small switch: 8,000 entries
- Enterprise switch: 16,000+ entries

Aging Time:
- Default: 300 seconds (5 minutes)
- ถ้าไม่มี traffic → entry ถูกลบ
```

### 4.2 MAC Address Table Flooding Attack

**MAC Flooding Attack:**

**วิธีการ:**

```
Attacker ส่ง frames จำนวนมหาศาล
แต่ละ frame มี source MAC ต่างกัน
Switch เรียนรู้ MAC ทั้งหมด
MAC table เต็ม!

Attack Tool:
- macof (part of dsniff)
- Yersinia
- Custom scripts

Example:
macof -i eth0 -n 10000
# ส่ง 10,000 frames ด้วย random MAC addresses
```

**ผลกระทบ:**

```
Step 1: MAC table เต็ม
┌─────────────┬──────────┬──────┐
│ MAC Address │ Port     │ VLAN │
├─────────────┼──────────┼──────┤
│ AAAA.AAAA   │ Fa0/10   │ 1    │
│ BBBB.BBBB   │ Fa0/10   │ 1    │
│ CCCC.CCCC   │ Fa0/10   │ 1    │
│ ...         │ ...      │ ...  │
│ FFFF.FFFF   │ Fa0/10   │ 1    │ ← FULL!
└─────────────┴──────────┴──────┘

Step 2: Switch ไม่สามารถเรียนรู้ MAC ใหม่

Step 3: Switch กลายเป็น Hub!
- Forward frames ออกทุก port
- Flood mode
- ไม่มี switching intelligence

Step 4: Attacker ดัก traffic ได้ทั้งหมด
- เห็น frames ของทุกคน
- Packet sniffing
- Steal sensitive data
```

**Attack Scenario:**

```
Normal Operation:
PC1 → Switch → PC2
(ส่งตรงไป PC2)

During Attack:
PC1 → Switch → [PC2, PC3, PC4, Attacker]
(ส่งไปทุก port)

Attacker:
- เห็น traffic ระหว่าง PC1-PC2
- Capture passwords
- Capture sensitive data
- Man-in-the-middle opportunities
```

### 4.3 MAC Address Table Attack Mitigation

**Port Security:**

**Configuration:**

```cisco
! Enable port security
Switch(config)# interface FastEthernet0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security

! Limit number of MAC addresses
Switch(config-if)# switchport port-security maximum 2

! Define secure MAC addresses
! Option 1: Static
Switch(config-if)# switchport port-security mac-address 0001.1111.1111

! Option 2: Sticky (learn dynamically)
Switch(config-if)# switchport port-security mac-address sticky

! Set violation action
Switch(config-if)# switchport port-security violation shutdown
```

**Port Security Parameters:**

**1. Maximum MAC Addresses:**

```cisco
switchport port-security maximum <1-8192>

Default: 1
Range: 1-8192

Examples:
maximum 1  → 1 device (typical desktop)
maximum 2  → Device + VM or IP Phone
maximum 3  → Multiple VMs
```

**2. Secure MAC Address Types:**

```cisco
Static MAC:
switchport port-security mac-address 0001.1111.1111
- Manually configured
- Saved in running-config
- Requires manual entry

Dynamic MAC:
- Learned automatically
- NOT saved in config
- Lost after reload

Sticky MAC:
switchport port-security mac-address sticky
- Learned automatically
- Saved in running-config
- Persistent across reloads
- Best of both worlds!
```

**3. Violation Modes:**

```cisco
switchport port-security violation {shutdown | restrict | protect}
```

**Shutdown (Default):**

```
Action:
- Port → err-disabled state
- Port LED ปิด
- SNMP trap
- Syslog message

Recovery:
Manual intervention required:
Switch(config)# interface Fa0/1
Switch(config-if)# shutdown
Switch(config-if)# no shutdown

Or enable auto-recovery:
Switch(config)# errdisable recovery cause psecure-violation
Switch(config)# errdisable recovery interval 300
```

**Restrict:**

```
Action:
- Drop packets from violating MAC
- Port ยัง up
- Increment violation counter
- SNMP trap
- Syslog message

Use Case:
- Want to monitor violations
- Don't want port down
- Allow legitimate traffic to continue
```

**Protect:**

```
Action:
- Drop packets from violating MAC
- Port ยัง up
- NO trap, NO log
- Silent drop

Use Case:
- Don't want logs
- Performance considerations
- Simple protection
```

**Port Security Aging:**

```cisco
! Enable aging
Switch(config-if)# switchport port-security aging time 10
Switch(config-if)# switchport port-security aging type inactivity

Aging Types:

1. absolute
   - Entry removed after X minutes
   - ไม่ว่าจะมี traffic หรือไม่

2. inactivity (recommended)
   - Entry removed after X minutes of NO traffic
   - ถ้ามี traffic → reset timer
```

**Complete Port Security Example:**

```cisco
! Interface configuration
Switch(config)# interface FastEthernet0/5
Switch(config-if)# description User Access Port
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10

! Enable port security
Switch(config-if)# switchport port-security

! Maximum 2 MAC addresses
Switch(config-if)# switchport port-security maximum 2

! Learn MACs and make them sticky
Switch(config-if)# switchport port-security mac-address sticky

! Violation = shutdown port
Switch(config-if)# switchport port-security violation shutdown

! Aging
Switch(config-if)# switchport port-security aging time 120
Switch(config-if)# switchport port-security aging type inactivity

! Optional: manually add MAC
Switch(config-if)# switchport port-security mac-address 0011.2233.4455
```

**Verify Port Security:**

```cisco
! Show port security status
Switch# show port-security

Secure Port  MaxSecureAddr  CurrentAddr  SecurityViolation  Security Action
                (Count)       (Count)          (Count)
---------------------------------------------------------------------------
Fa0/1              2              1                 0               Shutdown
Fa0/5              2              2                 3               Restrict

! Show specific interface
Switch# show port-security interface FastEthernet0/1

Port Security              : Enabled
Port Status                : Secure-up
Violation Mode             : Shutdown
Aging Time                 : 120 mins
Aging Type                 : Inactivity
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 2
Total MAC Addresses        : 1
Configured MAC Addresses   : 0
Sticky MAC Addresses       : 1
Last Source Address:Vlan   : 0011.2233.4455:10
Security Violation Count   : 0

! Show MAC addresses
Switch# show port-security address

Secure Mac Address Table
-------------------------------------------------------------------
Vlan    Mac Address       Type                Ports   Remaining Time
                                                          (mins)
----    -----------       ----                -----   --------------
  10    0011.2233.4455    SecureSticky        Fa0/1        -
-------------------------------------------------------------------
```

**Troubleshooting Port Security:**

```cisco
! Check err-disabled ports
Switch# show interfaces status err-disabled

Port      Name               Status       Reason
Fa0/1                        err-disabled psecure-violation

! Check violation counters
Switch# show port-security

! Check logs
Switch# show logging | include PSECURE

! Recovery
Switch(config)# interface Fa0/1
Switch(config-if)# shutdown
Switch(config-if)# no shutdown

! Or enable auto-recovery
Switch(config)# errdisable recovery cause psecure-violation
Switch(config)# errdisable recovery interval 600

! Clear violation counter
Switch# clear port-security all
```

---

## 5. LAN Attacks

### การโจมตี LAN อื่นๆ

### 5.1 VLAN Hopping Attacks

**VLAN Hopping คืออะไร:**

```
Attacker ส่ง traffic ไป VLAN อื่นโดยไม่ได้รับอนุญาต
"กระโดด" ข้าม VLAN boundaries
เข้าถึง resources ที่ไม่ควรเข้าถึง
```

**Attack Type 1: Switch Spoofing**

**วิธีการ:**

```
Attacker ปลอมตัวเป็น switch
ส่ง DTP packets
สร้าง trunk link กับ switch จริง
เข้าถึงได้ทุก VLAN

Attack Steps:
1. Connect to switch port (access mode)
2. Send DTP packets
3. Switch thinks attacker is a switch
4. Port becomes trunk
5. Attacker tags frames ไป VLAN ใดก็ได้
```

**Scenario:**

```
Before Attack:
[Switch] ─ Fa0/1 (Access, VLAN 10) ─ [PC]

Attack:
[Switch] ─ Fa0/1 (Access → Trunk!) ─ [Attacker]
                                     ↓
                                 Tags frames:
                                 VLAN 10, 20, 30...

After Attack:
Attacker can access all VLANs!
```

**Mitigation - Switch Spoofing:**

```cisco
! Disable DTP (Dynamic Trunking Protocol)
Switch(config-if)# switchport mode access
Switch(config-if)# switchport nonegotiate

! On trunk ports, set manually
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport nonegotiate

! Disable unused ports
Switch(config-if)# shutdown
```

**Attack Type 2: Double Tagging**

**วิธีการ:**

```
Attacker อยู่ใน native VLAN
ส่ง frame ที่มี 2 VLAN tags
Tag นอก = native VLAN
Tag ใน = target VLAN

Attack Process:
1. Attacker sends double-tagged frame
2. First switch strips outer tag (native VLAN)
3. Forward frame with inner tag
4. Next switch reads inner tag
5. Frame goes to target VLAN
```

**Scenario:**

```
Topology:
[SW1] ─trunk(native=10)─ [SW2]
  │                        │
Fa0/1 (VLAN 10)      Fa0/2 (VLAN 20)
  │                        │
[Attacker]               [Victim]

Attack:
1. Attacker in VLAN 10
2. Sends frame: [Outer:VLAN 10][Inner:VLAN 20][Data]
3. SW1 strips outer tag (native)
4. Forwards: [VLAN 20][Data]
5. SW2 sees VLAN 20 tag
6. Delivers to Victim in VLAN 20

One-way attack:
Attacker → Victim ✓
Victim → Attacker ✗
```

**Mitigation - Double Tagging:**

```cisco
! Don't use VLAN 1 as native
! Use unused VLAN as native
Switch(config-if)# switchport trunk native vlan 999

! Tag native VLAN (optional)
Switch(config)# vlan dot1q tag native

! Restrict allowed VLANs on trunk
Switch(config-if)# switchport trunk allowed vlan 10,20,30
```

**VLAN Attack Best Practices:**

```
✅ Disable DTP on all access ports
✅ Manually configure trunk ports
✅ Use unused VLAN as native
✅ Don't use VLAN 1
✅ Disable unused ports
✅ Use PVLAN (Private VLAN) where appropriate
```

### 5.2 DHCP Attacks

**DHCP Review:**

```
DHCP Process (DORA):
1. Discover - Client broadcast
2. Offer - Server offers IP
3. Request - Client requests IP
4. Acknowledge - Server confirms

[Client] ─→ Discover (broadcast) ─→ [Switch] ─→ [DHCP Server]
[Client] ←─ Offer ←─────────────── [Switch] ←─ [DHCP Server]
[Client] ─→ Request ─→ [Switch] ─→ [DHCP Server]
[Client] ←─ ACK ←─────────────── [Switch] ←─ [DHCP Server]
```

**Attack Type 1: DHCP Starvation**

**วิธีการ:**

```
Attacker ส่ง DHCP Discover messages จำนวนมาก
แต่ละ request ใช้ MAC address ต่างกัน
DHCP server allocate IPs ให้หมด
IP pool หมด!
Legitimate users ไม่ได้ IP
```

**Attack Process:**

```
Step 1: Attacker floods DHCP Discovers
[Attacker] ─→ DHCP Discover (MAC: AA:AA:AA:AA:AA:01)
[Attacker] ─→ DHCP Discover (MAC: AA:AA:AA:AA:AA:02)
[Attacker] ─→ DHCP Discover (MAC: AA:AA:AA:AA:AA:03)
... (thousands of requests)

Step 2: DHCP Server responds
[Server] ─→ Offer 192.168.1.10 to AA:AA:AA:AA:AA:01
[Server] ─→ Offer 192.168.1.11 to AA:AA:AA:AA:AA:02
[Server] ─→ Offer 192.168.1.12 to AA:AA:AA:AA:AA:03
...

Step 3: IP Pool exhausted
DHCP Pool: 192.168.1.10 - 192.168.1.254
All IPs allocated to fake MACs!

Step 4: Legitimate user tries
[PC] ─→ DHCP Discover
[Server] ─→ No IPs available!
[PC] ← DHCP NAK (or no response)
```

**ผลกระทบ:**

```
❌ DoS - Users can't get IPs
❌ Network unavailable
❌ Business disruption
```

**Attack Type 2: DHCP Spoofing**

**วิธีการ:**

```
Attacker ตั้ง rogue DHCP server
Responds เร็วกว่า legitimate server
Client รับ IP จาก rogue server
Attacker กำหนด malicious gateway/DNS
```

**Attack Scenario:**

```
Topology:
[Legitimate Server]     [Rogue Server]
      (slow)              (fast!)
        │                    │
        └────[Switch]────────┘
                │
             [Client]

Process:
1. Client broadcasts Discover
2. Both servers receive it
3. Rogue responds first! (closer/faster)
4. Client accepts rogue's offer

Rogue DHCP Response:
IP: 192.168.1.50
Mask: 255.255.255.0
Gateway: 192.168.1.100 ← Attacker!
DNS: 8.8.4.4 ← Malicious DNS!

Result:
Client thinks 192.168.1.100 is gateway
All traffic goes through attacker
Man-in-the-middle!
```

**ผลกระทบ:**

```
❌ Man-in-the-middle attack
❌ Traffic interception
❌ Credential theft
❌ DNS hijacking
❌ Phishing
```

### 5.3 DHCP Snooping

**DHCP Snooping คืออะไร:**

```
Security feature ที่ป้องกัน DHCP attacks
Switch ทำหน้าที่ "DHCP firewall"
กรอง DHCP messages
อนุญาตเฉพาะ trusted sources
```

**DHCP Snooping Concepts:**

**Trusted vs Untrusted Ports:**

```
Trusted Ports:
- Ports ที่เชื่อมต่อ legitimate DHCP servers
- Ports ที่เชื่อมต่อ switches/routers
- Allow DHCP server messages

Untrusted Ports:
- Ports ที่เชื่อมต่อ end devices
- Default = ALL ports
- Block DHCP server messages
```

**Port Trust Configuration:**

```
[DHCP Server] ─ Fa0/24 (Trusted) ─ [Switch] ─ Fa0/1 (Untrusted) ─ [PC]
                                      │
                                  Fa0/10 (Untrusted) ─ [Attacker]

Allowed:
✓ Server sends Offer/ACK via Fa0/24
✓ Clients send Discover/Request via Fa0/1, Fa0/10

Blocked:
✗ Attacker sends Offer/ACK via Fa0/10
```

**DHCP Snooping Operation:**

```
1. Validate DHCP messages
   - Check message type
   - Check source port (trusted/untrusted)
   - Validate packet format

2. Build DHCP Snooping Binding Table
   - MAC address
   - IP address
   - VLAN
   - Interface
   - Lease time

3. Use binding table for other features
   - Dynamic ARP Inspection (DAI)
   - IP Source Guard (IPSG)
```

**DHCP Snooping Binding Table:**

```
Example Table:
┌─────────────────┬──────────────┬──────┬───────┬─────────┐
│ MAC Address     │ IP Address   │ VLAN │ Port  │ Lease   │
├─────────────────┼──────────────┼──────┼───────┼─────────┤
│ 0011.2233.4455  │ 192.168.1.10 │ 10   │ Fa0/1 │ 86400   │
│ 0055.6677.8899  │ 192.168.1.11 │ 10   │ Fa0/2 │ 86400   │
│ 00AA.BBCC.DDEE  │ 192.168.1.12 │ 20   │ Fa0/5 │ 43200   │
└─────────────────┴──────────────┴──────┴───────┴─────────┘

Used by:
- DAI (verify ARP packets)
- IPSG (verify IP source)
```

**DHCP Snooping Configuration:**

```cisco
! Enable DHCP snooping globally
Switch(config)# ip dhcp snooping

! Enable for specific VLANs
Switch(config)# ip dhcp snooping vlan 10,20,30

! Configure trusted ports (uplinks, server ports)
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# description Uplink to DHCP Server
Switch(config-if)# ip dhcp snooping trust

! Rate limit on untrusted ports (optional but recommended)
Switch(config)# interface range FastEthernet0/1-24
Switch(config-if-range)# ip dhcp snooping limit rate 10
! 10 DHCP packets per second

! Enable Option 82 (optional)
Switch(config)# ip dhcp snooping information option

! Verify
Switch# show ip dhcp snooping
Switch DHCP snooping is enabled
DHCP snooping is configured on following VLANs:
10,20,30
Insertion of option 82 is enabled
Option 82 on untrusted port is not allowed
Verification of hwaddr field is enabled
Interface                  Trusted    Rate limit (pps)
------------------------   -------    ----------------
GigabitEthernet0/1         yes        unlimited
FastEthernet0/1-24         no         10
```

**DHCP Snooping Rate Limiting:**

```cisco
! Why rate limit?
- Prevent DHCP starvation attacks
- Limit DHCP packets per second
- Apply to untrusted ports

! Configuration
Switch(config-if)# ip dhcp snooping limit rate <1-2048>

! Example
Switch(config-if)# ip dhcp snooping limit rate 10
! Allow maximum 10 DHCP packets/second

! ถ้าเกิน rate limit
- Port enters err-disabled state
- Need recovery (manual or auto)
```

**Show DHCP Snooping Binding:**

```cisco
Switch# show ip dhcp snooping binding

MacAddress          IpAddress    Lease(sec)  Type           VLAN  Interface
------------------  -----------  ----------  -------------  ----  ----------------
00:11:22:33:44:55   192.168.1.10    86400    dhcp-snooping   10   FastEthernet0/1
00:55:66:77:88:99   192.168.1.11    86400    dhcp-snooping   10   FastEthernet0/2
00:AA:BB:CC:DD:EE   192.168.1.12    43200    dhcp-snooping   20   FastEthernet0/5
Total number of bindings: 3
```

**DHCP Snooping Best Practices:**

```
✅ Enable globally and per VLAN
✅ Trust only legitimate DHCP server ports
✅ Trust uplinks to other switches
✅ All access ports = untrusted
✅ Enable rate limiting on untrusted ports
✅ Monitor binding table
✅ Use with DAI and IPSG
```

### 5.4 ARP Attacks

**ARP Review:**

```
ARP (Address Resolution Protocol):
Purpose: Map IP addresses to MAC addresses

Process:
1. Host needs MAC for IP
2. Sends ARP Request (broadcast)
   "Who has 192.168.1.1? Tell 192.168.1.10"
3. Owner responds with ARP Reply (unicast)
   "192.168.1.1 is at 00:11:22:33:44:55"
4. Requester updates ARP cache

[PC1]                          [PC2]
IP: 192.168.1.10              IP: 192.168.1.1
MAC: AA:AA:AA:AA:AA:AA        MAC: BB:BB:BB:BB:BB:BB

PC1 ARP Cache:
192.168.1.1 → BB:BB:BB:BB:BB:BB
```

**ARP Spoofing/Poisoning Attack:**

**วิธีการ:**

```
Attacker ส่ง fake ARP replies
Claim เป็น gateway หรือ target host
Victims update ARP cache ด้วยข้อมูลผิด
Traffic ไปหา attacker แทน destination จริง
```

**Attack Scenario - Man-in-the-Middle:**

```
Normal Communication:
[PC] ←───────────────────────→ [Gateway]
     Direct communication

Attack:
1. Attacker sends fake ARPs
   To PC: "Gateway (192.168.1.1) is at Attacker-MAC"
   To Gateway: "PC (192.168.1.10) is at Attacker-MAC"

2. Victims update ARP cache
   PC ARP Cache: 192.168.1.1 → Attacker-MAC (WRONG!)
   Gateway ARP Cache: 192.168.1.10 → Attacker-MAC (WRONG!)

3. Traffic flow
   [PC] → [Attacker] → [Gateway]
        ↓
   Intercept, modify, forward

Result: Complete Man-in-the-Middle
```

**Attack Tools:**

```
- arpspoof (dsniff)
- Ettercap
- Cain & Abel
- Bettercap

Example:
arpspoof -i eth0 -t 192.168.1.10 192.168.1.1
# Spoof gateway to PC
arpspoof -i eth0 -t 192.168.1.1 192.168.1.10
# Spoof PC to gateway
```

**ผลกระทบ:**

```
❌ Traffic interception
❌ Session hijacking
❌ Credential theft
❌ Data modification
❌ DoS (drop packets)
```

### 5.5 Dynamic ARP Inspection (DAI)

**DAI คืออะไร:**

```
Security feature ป้องกัน ARP attacks
Validate ARP packets
ใช้ DHCP Snooping binding table
Block invalid ARP packets
```

**DAI Operation:**

```
1. ARP packet arrives at switch
2. DAI checks:
   - Source MAC in Ethernet header
   - Source MAC in ARP payload
   - Sender IP in ARP payload
   - Compare with DHCP snooping binding table

3. Decision:
   Valid ARP → Forward
   Invalid ARP → Drop + Log

Validation:
┌──────────────────────────────────┐
│ ARP Packet                       │
├──────────────────────────────────┤
│ Ethernet Header:                 │
│   Src MAC: AA:AA:AA:AA:AA:AA     │ ← Check 1
├──────────────────────────────────┤
│ ARP Payload:                     │
│   Sender MAC: AA:AA:AA:AA:AA:AA  │ ← Check 2 (match?)
│   Sender IP: 192.168.1.10        │ ← Check 3
│   Target IP: 192.168.1.1         │
└──────────────────────────────────┘
         ↓
DHCP Snooping Binding Table
MAC: AA:AA:AA:AA:AA:AA → IP: 192.168.1.10 ✓
```

**Trusted vs Untrusted Ports:**

```
DAI Trusted Ports:
- ARP packets NOT validated
- Typically uplinks, DHCP server ports
- Same as DHCP snooping trusted ports

DAI Untrusted Ports:
- All ARP packets validated
- Typically access ports
- Default = ALL ports untrusted
```

**DAI Configuration:**

```cisco
! Prerequisites: DHCP Snooping must be configured first
Switch(config)# ip dhcp snooping
Switch(config)# ip dhcp snooping vlan 10

! Enable DAI
Switch(config)# ip arp inspection vlan 10

! Configure trusted ports
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# description Uplink
Switch(config-if)# ip arp inspection trust

! Rate limiting (untrusted ports)
Switch(config)# interface FastEthernet0/5
Switch(config-if)# ip arp inspection limit rate 15
! 15 ARP packets per second
! ถ้าเกิน → err-disabled

! Validation options (optional)
Switch(config)# ip arp inspection validate src-mac
Switch(config)# ip arp inspection validate dst-mac
Switch(config)# ip arp inspection validate ip
! Or all at once:
Switch(config)# ip arp inspection validate src-mac dst-mac ip
```

**DAI Validation Checks:**

```cisco
ip arp inspection validate {src-mac | dst-mac | ip}

src-mac:
- Ethernet source MAC = ARP sender MAC

dst-mac:
- Ethernet destination MAC = ARP target MAC
- For ARP Replies only

ip:
- Check for invalid/unexpected IP addresses
- 0.0.0.0, 255.255.255.255, multicast

ตัวอย่าง:
Switch(config)# ip arp inspection validate src-mac dst-mac ip
```

**Show DAI Status:**

```cisco
! Show DAI configuration
Switch# show ip arp inspection

Source Mac Validation      : Enabled
Destination Mac Validation : Enabled
IP Address Validation      : Enabled

Vlan     Configuration    Operation   ACL Match          Static ACL
----     -------------    ---------   ---------          ----------
  10     Enabled          Active

Vlan     ACL Logging      DHCP Logging      Probe Logging
----     -----------      ------------      -------------
  10     Deny             Deny              Off

! Show DAI statistics
Switch# show ip arp inspection statistics

Vlan      Forwarded        Dropped     DHCP Drops    ACL Drops
----      ---------        -------     ----------    ---------
  10            100             15             15            0

! Show interfaces
Switch# show ip arp inspection interfaces

Interface        Trust State     Rate (pps)    Burst Interval
---------------  -----------     ----------    --------------
Gi0/1            Trusted         None          N/A
Fa0/1            Untrusted       15            1
Fa0/2            Untrusted       15            1
```

**Static ARP Entries (Alternative):**

```cisco
! ถ้าไม่ใช้ DHCP หรือมี static IP
! สร้าง static ARP bindings

Switch(config)# arp access-list ARP-PERMIT-LIST
Switch(config-arp-nacl)# permit ip host 192.168.1.10 mac host 0011.2233.4455
Switch(config-arp-nacl)# permit ip host 192.168.1.11 mac host 0055.6677.8899
Switch(config-arp-nacl)# exit

Switch(config)# ip arp inspection filter ARP-PERMIT-LIST vlan 10
```

### 5.6 IP Source Guard (IPSG)

**IPSG คืออะไร:**

```
ป้องกัน IP address spoofing
Validate source IP addresses
ใช้ DHCP snooping binding table
Block packets จาก invalid source IPs
```

**IPSG Operation:**

```
Without IPSG:
[Attacker] ─→ Packet (Src: 192.168.1.100) ─→ [Network]
(อาจจะไม่ใช่ IP จริงของ attacker)

With IPSG:
[Attacker] ─→ Packet (Src: 192.168.1.100) ─→ [Switch]
                                                  ↓
                                         Check binding table
                                         Attacker port → 192.168.1.10
                                         Packet claims → 192.168.1.100
                                         MISMATCH! → DROP

Only allows:
Packets with source IP = IP in binding table for that port
```

**IPSG Binding Sources:**

```
1. DHCP Snooping Binding Table
   - Dynamic bindings from DHCP
   - Most common source

2. Static IP Source Binding
   - Manually configured
   - For static IP devices

Binding Table:
┌──────┬─────────────────┬──────────────┬──────────┐
│ Port │ MAC Address     │ IP Address   │ VLAN     │
├──────┼─────────────────┼──────────────┼──────────┤
│ Fa0/1│ 0011.2233.4455  │ 192.168.1.10 │ 10       │
│ Fa0/2│ 0055.6677.8899  │ 192.168.1.11 │ 10       │
└──────┴─────────────────┴──────────────┴──────────┘
```

**IPSG Configuration:**

```cisco
! Prerequisites: DHCP Snooping
Switch(config)# ip dhcp snooping
Switch(config)# ip dhcp snooping vlan 10

! Enable IPSG on interface
Switch(config)# interface FastEthernet0/1
Switch(config-if)# ip verify source

! Or enable IPSG with MAC filtering
Switch(config-if)# ip verify source port-security

! Static binding (for static IP hosts)
Switch(config)# ip source binding 0011.2233.4455 vlan 10 192.168.1.50 interface FastEthernet0/5

! Verify
Switch# show ip verify source

Interface  Filter-type  Filter-mode  IP-address       Mac-address        Vlan
---------  -----------  -----------  ---------------  -----------------  ----
Fa0/1      ip           active       192.168.1.10                        10
Fa0/5      ip           active       192.168.1.50     0011.2233.4455     10
                       (static)

! Show bindings
Switch# show ip source binding
MacAddress          IpAddress        Lease(sec)  Type           VLAN  Interface
------------------  ---------------  ----------  -------------  ----  --------------------
00:11:22:33:44:55   192.168.1.10     infinite    static          10   FastEthernet0/1
00:55:66:77:88:99   192.168.1.11       86400     dhcp-snooping   10   FastEthernet0/2
```

**IPSG Filtering Types:**

```
1. IP Filtering (default)
   ip verify source
   - Validates source IP only

2. IP and MAC Filtering
   ip verify source port-security
   - Validates both IP and MAC
   - Requires port security enabled
   - More secure
```

### 5.7 STP Attacks

**STP Review:**

```
Spanning Tree Protocol:
- Prevents Layer 2 loops
- Elect root bridge
- Block redundant paths
- Topology changes

Root Bridge:
- Bridge with lowest bridge ID
- Bridge ID = Priority + MAC address
- Default priority: 32768
```

**STP Manipulation Attack:**

**วิธีการ:**

```
Attacker ส่ง fake BPDU (Bridge Protocol Data Unit)
Claim มี priority ต่ำมาก (0)
Become root bridge
Control spanning tree topology
```

**Attack Process:**

```
Normal Topology:
[Core Switch] ← Root Bridge (Priority 0)
      │
  [Dist Switch]
      │
  [Access Switch] ─ [Users]

Attack:
1. Attacker connects
2. Sends BPDUs with Priority 0
3. Becomes root bridge

New Topology:
[Core Switch]
      │
  [Dist Switch]
      │
  [Access Switch] ─ [Attacker] ← New Root!
                         │
                    All traffic flows through here!

Result:
✓ Attacker sees all traffic
✓ Can cause topology loops
✓ DoS opportunities
```

**ผลกระทบ:**

```
❌ Man-in-the-middle
❌ Traffic interception
❌ Network instability
❌ Topology loops
❌ Broadcast storms
```

**STP Attack Mitigation:**

**1. BPDU Guard**

```cisco
Purpose:
- Protect access ports
- ถ้าได้รับ BPDU → shutdown port immediately
- Prevents rogue switches

Configuration:
! Global (affects all PortFast ports)
Switch(config)# spanning-tree portfast bpduguard default

! Per-interface
Switch(config-if)# spanning-tree bpduguard enable

! ถ้าได้รับ BPDU
- Port → err-disabled
- Syslog message

Recovery:
Switch(config-if)# shutdown
Switch(config-if)# no shutdown

Or auto-recovery:
Switch(config)# errdisable recovery cause bpduguard
Switch(config)# errdisable recovery interval 300
```

**2. Root Guard**

```cisco
Purpose:
- Protect topology
- Prevent unauthorized root bridge
- ใช้บน ports ที่ไม่ควรเป็น root port

Configuration:
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# spanning-tree guard root

ถ้าได้รับ superior BPDU (better than current root):
- Port → root-inconsistent state
- Port blocked
- Syslog message

When superior BPDU stops:
- Port recovers automatically
```

**3. Loop Guard**

```cisco
Purpose:
- Prevent loops from unidirectional link failures
- ป้องกัน alternate/root ports กลายเป็น designated

Configuration:
! Global
Switch(config)# spanning-tree loopguard default

! Per-interface
Switch(config-if)# spanning-tree guard loop
```

**Best Practices:**

```
Access Ports (End Devices):
✅ Enable PortFast
✅ Enable BPDU Guard
✅ Enable Port Security

Distribution/Uplink Ports:
✅ Enable Root Guard
✅ Manual trunk configuration
✅ Appropriate STP priority

Core/Root Bridge:
✅ Set lowest priority manually
✅ Consistent configuration
```

**Complete Access Port Security Configuration:**

```cisco
! Access port configuration
Switch(config)# interface FastEthernet0/5
Switch(config-if)# description User Access Port

! Basic configuration
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10

! Disable DTP
Switch(config-if)# switchport nonegotiate

! Port Security
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximum 2
Switch(config-if)# switchport port-security mac-address sticky
Switch(config-if)# switchport port-security violation restrict

! DHCP Snooping rate limit
Switch(config-if)# ip dhcp snooping limit rate 15

! DAI rate limit
Switch(config-if)# ip arp inspection limit rate 15

! IP Source Guard
Switch(config-if)# ip verify source port-security

! STP
Switch(config-if)# spanning-tree portfast
Switch(config-if)# spanning-tree bpduguard enable

! Enable
Switch(config-if)# no shutdown
```

---

## สรุป

### สิ่งที่ได้เรียนรู้ใน Module 10:

✅ **Endpoint Security:**

- Network attacks (DDoS, Data Breach, Malware)
- Security devices (NGFW, IPS, ESA, WSA)
- Endpoint protection (Antivirus, HIPS, Firewall)
- NAC (Network Admission Control)

✅ **Access Control:**

- AAA framework (Authentication, Authorization, Accounting)
- RADIUS vs TACACS+
- Local vs Server-based AAA
- 802.1X port-based authentication
- EAP methods

✅ **Layer 2 Threats:**

- Why Layer 2 is vulnerable
- Attack categories
- Security features overview

✅ **MAC Address Table Attacks:**

- MAC flooding attacks
- Port Security configuration
- Violation modes
- Sticky MAC addresses

✅ **LAN Attacks:**

- VLAN hopping (switch spoofing, double-tagging)
- DHCP attacks (starvation, spoofing)
- DHCP Snooping
- ARP spoofing/poisoning
- Dynamic ARP Inspection (DAI)
- IP Source Guard (IPSG)
- STP attacks
- BPDU Guard, Root Guard

---

## คำสั่งสำคัญสรุป

### AAA Configuration:

```cisco
! Enable AAA
aaa new-model

! RADIUS
radius server <name>
 address ipv4 <ip> auth-port 1812 acct-port 1813
 key <secret>
aaa authentication login default group radius local
aaa authorization exec default group radius local
aaa accounting exec default start-stop group radius

! TACACS+
tacacs server <name>
 address ipv4 <ip>
 key <secret>
aaa authentication login default group tacacs+ local
aaa authorization commands 15 default group tacacs+ local
aaa accounting commands 15 default start-stop group tacacs+
```

### 802.1X Configuration:

```cisco
! Enable 802.1X
dot1x system-auth-control
aaa authentication dot1x default group radius

! Interface
interface <interface>
 authentication port-control auto
 dot1x pae authenticator
 authentication host-mode multi-auth
 mab
```

### Port Security:

```cisco
interface <interface>
 switchport mode access
 switchport port-security
 switchport port-security maximum <number>
 switchport port-security mac-address sticky
 switchport port-security violation {shutdown|restrict|protect}
 switchport port-security aging time <minutes>
 switchport port-security aging type inactivity
```

### DHCP Snooping:

```cisco
! Global
ip dhcp snooping
ip dhcp snooping vlan <vlan-list>

! Trusted port
interface <interface>
 ip dhcp snooping trust

! Rate limit
interface <interface>
 ip dhcp snooping limit rate <rate>
```

### DAI (Dynamic ARP Inspection):

```cisco
! Enable
ip arp inspection vlan <vlan-list>
ip arp inspection validate src-mac dst-mac ip

! Trusted port
interface <interface>
 ip arp inspection trust

! Rate limit
interface <interface>
 ip arp inspection limit rate <rate>
```

### IPSG (IP Source Guard):

```cisco
! Enable on interface
interface <interface>
 ip verify source
 ! Or with MAC filtering
 ip verify source port-security

! Static binding
ip source binding <mac> vlan <vlan> <ip> interface <interface>
```

### STP Protection:

```cisco
! BPDU Guard
spanning-tree portfast bpduguard default
! Or per-interface
interface <interface>
 spanning-tree bpduguard enable

! Root Guard
interface <interface>
 spanning-tree guard root

! Loop Guard
spanning-tree loopguard default
! Or per-interface
interface <interface>
 spanning-tree guard loop
```

### Verification:

```cisco
show port-security [interface <interface>]
show port-security address
show ip dhcp snooping
show ip dhcp snooping binding
show ip arp inspection
show ip arp inspection statistics
show ip verify source
show ip source binding
show spanning-tree inconsistentports
```

---

## Best Practices

### Layer 2 Security:

```
✅ Enable port security on all access ports
✅ Use DHCP snooping + DAI + IPSG together
✅ Disable DTP on access ports
✅ Use unused VLAN as native on trunks
✅ Enable BPDU Guard on access ports
✅ Implement 802.1X where possible
✅ Regular security audits
✅ Monitor logs and alerts
```

### Access Port Template:

```
✅ switchport mode access
✅ switchport nonegotiate
✅ Port security enabled
✅ DHCP snooping rate limit
✅ DAI rate limit
✅ IP Source Guard
✅ PortFast + BPDU Guard
✅ Assigned to appropriate VLAN
```

### AAA Best Practices:

```
✅ Use server-based AAA (RADIUS/TACACS+)
✅ Enable accounting for audit trail
✅ Use strong secrets/keys
✅ Configure fallback to local
✅ Implement least privilege
✅ Regular password changes
✅ Use 802.1X for network access
```

---

## Common Issues

**Problem 1: Port Security Violation**

```
Symptom: Port goes err-disabled
Cause: More MACs than maximum, or wrong MAC

Solution:
1. Check violation reason
   show port-security interface <int>
2. Verify maximum setting
3. Check sticky MACs
4. Recover port:
   shutdown / no shutdown
```

**Problem 2: DHCP Clients Not Getting IPs**

```
Symptom: No DHCP after enabling snooping
Cause: Server port not trusted

Solution:
interface <server-port>
 ip dhcp snooping trust
```

**Problem 3: DAI Dropping Valid ARPs**

```
Symptom: Connectivity issues after enabling DAI
Cause: No DHCP snooping bindings, or static IPs

Solution:
- Ensure DHCP snooping enabled first
- Or create static ARP ACLs
- Or trust the port (temporary)
```

**Problem 4: 802.1X Not Working**

```
Symptom: Clients can't authenticate
Cause: RADIUS unreachable, wrong config

Troubleshooting:
debug dot1x all
show authentication sessions
test aaa group radius <user> <pass> new-code
```

---

## Lab Activities

### Lab 10.1: Port Security

**วัตถุประสงค์:**

- Configure port security
- Test violation modes
- Verify operation

### Lab 10.2: DHCP Snooping

**วัตถุประสงค์:**

- Enable DHCP snooping
- Configure trusted ports
- View binding table

### Lab 10.3: DAI and IPSG

**วัตถุประสงค์:**

- Configure DAI
- Enable IPSG
- Test protection

### Lab 10.4: 802.1X Authentication

**วัตถุประสงค์:**

- Configure 802.1X
- Setup RADIUS server
- Test authentication

---

## คำถามทบทวน

1. AAA ย่อมาจากอะไร? แต่ละส่วนทำอะไร?
2. ต่างระหว่าง RADIUS และ TACACS+ อย่างไร?
3. 802.1X มี components อะไรบ้าง?
4. Port Security violation modes มีอะไรบ้าง?
5. DHCP Snooping ทำงานอย่างไร?
6. DAI ใช้ข้อมูลจากไหนในการ validate ARP?
7. VLAN hopping มีกี่แบบ? แต่ละแบบทำงานอย่างไร?
8. IP Source Guard ป้องกันอะไร?
9. BPDU Guard และ Root Guard ต่างกันอย่างไร?
10. Layer 2 security features ควรใช้ร่วมกันอย่างไร?

---

## เฉลยคำถาม

1. **AAA:** Authentication (ยืนยันตัวตน), Authorization (ให้สิทธิ์), Accounting (บันทึกการใช้งาน)
2. **RADIUS vs TACACS+:** RADIUS ใช้ UDP, รวม AA; TACACS+ ใช้ TCP, แยก AAA, encrypt ทั้ง packet
3. **802.1X:** Supplicant (client), Authenticator (switch/AP), Authentication Server (RADIUS)
4. **Violation Modes:** Shutdown (port err-disabled), Restrict (drop + log), Protect (drop only)
5. **DHCP Snooping:** กรอง DHCP messages, trust/untrust ports, build binding table
6. **DAI:** ใช้ DHCP Snooping binding table เพื่อ validate source MAC/IP ใน ARP packets
7. **VLAN Hopping:** 1) Switch spoofing (DTP), 2) Double-tagging (native VLAN)
8. **IPSG:** ป้องกัน IP spoofing โดยตรวจสอบ source IP กับ binding table
9. **BPDU Guard:** shutdown port เมื่อได้รับ BPDU; Root Guard: block port เมื่อได้รับ superior BPDU
10. **Combined Use:** Port Security + DHCP Snooping + DAI + IPSG ร่วมกันให้ comprehensive protection

---

**หมายเหตุ:** LAN Security เป็นพื้นฐานสำคัญของ network security การเข้าใจและ implement features เหล่านี้อย่างถูกต้องจะช่วยป้องกันภัยคุกคามส่วนใหญ่ใน Layer 2

---

**เอกสารจัดทำโดย:** Claude (Anthropic AI)  
**Module:** CCNA 2 Module 10 - LAN Security Concepts  
**Version:** v7.02 SRWE  
**วันที่อัพเดท:** พฤศจิกายน 2025