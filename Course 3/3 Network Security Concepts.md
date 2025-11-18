# CCNA Course 3 - Module 3: Network Security Concepts

## แนวคิดด้านความปลอดภัยของเครือข่าย

---

## 3.1 Current State of Cybersecurity (สถานการณ์ความปลอดภัยไปเบอร์ปัจจุบัน)

### Overview of Cybersecurity

**คำจำกัดความ:**

- **Cybersecurity** = การป้องกันระบบ, เครือข่าย, โปรแกรม, และข้อมูลจากการโจมตีทางดิจิทัล
- **Information Security (InfoSec)** = การป้องกันข้อมูลจากการเข้าถึง, ใช้งาน, เปิดเผย, ทำลาย, หรือเปลี่ยนแปลงโดยไม่ได้รับอนุญาต

**ความสำคัญ:**

```
✅ ปกป้องข้อมูลส่วนบุคคล
✅ ปกป้องทรัพย์สินทางปัญญา
✅ รักษาความน่าเชื่อถือของธุรกิจ
✅ ป้องกันความเสียหายทางการเงิน
✅ ปฏิบัติตามกฎหมายและข้อบังคับ
```

---

### Security Threats (ภัยคุกคาม)

**ประเภทของ Threats:**

#### 1. Malware (มัลแวร์)

**คำจำกัดความ:**

- **Malicious Software** = ซอฟต์แวร์ที่ออกแบบมาเพื่อทำลาย, รบกวน, หรือขโมยข้อมูล

**ประเภท:**

**Virus (ไวรัส):**

```
- ติดไฟล์หรือโปรแกรม
- แพร่กระจายเมื่อ user เปิดไฟล์
- สามารถทำลายข้อมูล, ลบไฟล์

ตัวอย่าง:
  - Melissa Virus (1999)
  - ILOVEYOU Virus (2000)
```

**Worm (เวิร์ม):**

```
- แพร่กระจายอัตโนมัติผ่าน network
- ไม่ต้องอาศัย user เปิดไฟล์
- ใช้ทรัพยากร network/system

ตัวอย่าง:
  - WannaCry (2017) - Ransomware worm
  - SQL Slammer (2003)
  - Conficker (2008)
```

**Trojan Horse (โทรจัน):**

```
- ปลอมตัวเป็นโปรแกรมปกติ
- สร้าง backdoor เข้าระบบ
- ขโมยข้อมูล, ดาวน์โหลด malware อื่น

ตัวอย่าง:
  - Zeus Trojan (ขโมยข้อมูลธนาคาร)
  - Emotet
```

**Ransomware (แรนซัมแวร์):**

```
- เข้ารหัสไฟล์ของเหยื่อ
- เรียกค่าไถ่เพื่อ decrypt
- อาจขู่เผยแพร่ข้อมูล

ตัวอย่าง:
  - WannaCry (2017)
  - Petya/NotPetya (2017)
  - Ryuk
  - LockBit
```

**Spyware (สปายแวร์):**

```
- ติดตาม monitor กิจกรรม user
- ขโมยข้อมูลส่วนตัว
- keylogger, screen capture

ตัวอย่าง:
  - Keyloggers
  - Browser hijackers
```

**Adware (แอดแวร์):**

```
- แสดงโฆษณาที่ไม่ต้องการ
- redirect browser
- ติดตามพฤติกรรม
```

**Rootkit (รูทคิท):**

```
- ซ่อนตัวใน OS ระดับลึก
- ควบคุมระบบโดยไม่ให้รู้ตัว
- ยากต่อการตรวจจับ
```

#### 2. Social Engineering (การหลอกลวงทางสังคม)

**คำจำกัดความ:**

- การหลอกลวงให้ user เปิดเผยข้อมูลหรือทำสิ่งที่เป็นอันตราย
- โจมตีจิตวิทยามากกว่าเทคนิค

**ประเภท:**

**Phishing (ฟิชชิ่ง):**

```
- ส่ง email หลอกลวง
- ปลอมเป็นบริษัทหรือบุคคลที่น่าเชื่อถือ
- ขอข้อมูลส่วนตัว, รหัสผ่าน

ตัวอย่าง:
  Email: "Your account has been suspended. Click here to verify."
  → Link ไป fake website
```

**Spear Phishing:**

```
- Phishing แบบเจาะจง target
- ใช้ข้อมูลส่วนตัวทำให้น่าเชื่อถือ
- มักกำหนดเป้าเป็น executives, VIP

ตัวอย่าง:
  Email ถึง CEO: "From: CFO - Urgent wire transfer needed"
```

**Whaling:**

```
- Spear phishing กำหนดเป้าเป็น executives ระดับสูง
- มูลค่าความเสียหายสูง
```

**Vishing (Voice Phishing):**

```
- Phishing ผ่านโทรศัพท์
- ปลอมเป็น bank, government, IT support

ตัวอย่าง:
  โทรศัพท์: "This is Microsoft Support. Your computer has virus."
```

**Smishing (SMS Phishing):**

```
- Phishing ผ่าน SMS/Text message

ตัวอย่าง:
  SMS: "Your package is waiting. Click to reschedule delivery."
```

**Pretexting:**

```
- สร้างเหตุการณ์หลอกลวง
- ขอข้อมูลโดยอ้างว่ามีเหตุผลจำเป็น

ตัวอย่าง:
  โทร: "I'm from IT. Need your password to fix your account."
```

**Baiting:**

```
- ล่อด้วยของฟรีหรือรางวัล
- USB drive ทิ้งไว้ในที่สาธารณะ
```

**Tailgating/Piggybacking:**

```
- ตามเข้าประตูที่ต้องใช้ access card
- อ้างว่าลืม badge
```

**Shoulder Surfing:**

```
- มองข้อมูลจากข้างหลัง
- ดู password, PIN ที่กำลังพิมพ์
```

#### 3. Network Attacks (การโจมตีเครือข่าย)

**Denial of Service (DoS):**

```
คำจำกัดความ:
- ส่ง traffic ท่วมเพื่อทำให้ service ไม่สามารถใช้งานได้

ประเภท:
  - SYN Flood: ส่ง SYN packets จำนวนมาก
  - Ping Flood: ส่ง ICMP packets ท่วม
  - UDP Flood: ส่ง UDP packets ท่วม
  - HTTP Flood: ส่ง HTTP requests จำนวนมาก
```

**Distributed Denial of Service (DDoS):**

```
คำจำกัดความ:
- DoS จากหลายแหล่งพร้อมกัน
- ใช้ botnet (zombie computers)
- ยากต่อการป้องกัน

ตัวอย่าง:
  - Mirai Botnet (2016) - IoT devices
  - GitHub DDoS (2018) - 1.35 Tbps
```

**Man-in-the-Middle (MitM):**

```
คำจำกัดความ:
- ผู้โจมตีอยู่ระหว่าง sender และ receiver
- ดักจับ, อ่าน, แก้ไขข้อมูล

ประเภท:
  - ARP Spoofing/Poisoning
  - DNS Spoofing
  - Session Hijacking
  - SSL Stripping

ตัวอย่าง:
  User → [Attacker] → Bank
  Attacker ดูทุกอย่างที่ user ส่ง
```

**Packet Sniffing:**

```
คำจำกัดความ:
- ดักจับ network traffic
- อ่านข้อมูลที่ไม่เข้ารหัส
- ใช้ tools เช่น Wireshark

ข้อมูลที่ดักได้:
  - Passwords (unencrypted)
  - Emails
  - Files transferred
```

**IP Spoofing:**

```
คำจำกัดความ:
- ปลอม source IP address
- หลบหลีกการตรวจจับ
- ใช้ใน DDoS attacks
```

**DNS Attacks:**

```
DNS Spoofing/Cache Poisoning:
  - ใส่ข้อมูล DNS ปลอมใน cache
  - redirect user ไป malicious site

DNS Tunneling:
  - ใช้ DNS queries ส่งข้อมูล
  - bypass firewall
```

**Zero-Day Attack:**

```
คำจำกัดความ:
- โจมตีช่องโหว่ที่ยังไม่มี patch
- vendor ยังไม่รู้หรือยังไม่แก้
- อันตรายสูงมาก
```

#### 4. Application Attacks

**SQL Injection:**

```
คำจำกัดความ:
- ใส่ SQL code ใน input field
- execute คำสั่ง SQL บน database
- อ่าน, แก้ไข, ลบข้อมูล

ตัวอย่าง:
  Username: admin' OR '1'='1
  → bypass authentication
```

**Cross-Site Scripting (XSS):**

```
คำจำกัดความ:
- ใส่ malicious script ใน webpage
- execute ใน browser ของ victims
- ขโมย cookies, session tokens

ประเภท:
  - Stored XSS: script เก็บบน server
  - Reflected XSS: script ใน URL
  - DOM-based XSS: script manipulate DOM
```

**Cross-Site Request Forgery (CSRF):**

```
คำจำกัดความ:
- บังคับ user ทำ action โดยไม่รู้ตัว
- ใช้ authenticated session

ตัวอย่าง:
  <img src="http://bank.com/transfer?to=attacker&amount=1000">
  → ถ้า user login bank อยู่ จะโอนเงินโดยไม่รู้ตัว
```

**Buffer Overflow:**

```
คำจำกัดความ:
- ใส่ข้อมูลเกิน buffer capacity
- overwrite memory
- execute malicious code
```

#### 5. Password Attacks

**Brute Force:**

```
คำจำกัดความ:
- ลองรหัสผ่านทุกชุดที่เป็นไปได้
- ช้า แต่รับประกันได้ผล (ถ้ามีเวลา)

ตัวอย่าง:
  4-digit PIN: 0000, 0001, 0002, ..., 9999
  10,000 possibilities
```

**Dictionary Attack:**

```
คำจำกัดความ:
- ลองรหัสผ่านจาก list (dictionary)
- คำทั่วไป, รหัสที่ใช้บ่อย
- เร็วกว่า brute force

Dictionary file:
  - password
  - 123456
  - qwerty
  - admin
```

**Rainbow Table:**

```
คำจำกัดความ:
- Pre-computed hash table
- เปรียบเทียบ hash กับ table
- เร็วมาก (trade space for time)

Protection: ใช้ salt กับ password hash
```

**Password Spraying:**

```
คำจำกัดความ:
- ใช้รหัสผ่านทั่วไปกับหลาย accounts
- หลีกเลี่ยง account lockout

ตัวอย่าง:
  ลอง "Password123" กับ user1, user2, user3, ...
  (แทนที่จะลองหลายรหัสกับ user1)
```

**Credential Stuffing:**

```
คำจำกัดความ:
- ใช้ username/password ที่รั่วไหลจาก breach อื่น
- user มักใช้รหัสเดียวกันหลายเว็บ
```

---

### Security Vulnerabilities (ช่องโหว่)

**ประเภท Vulnerabilities:**

#### 1. Software Vulnerabilities

```
- Buffer overflow
- Unpatched software
- Zero-day vulnerabilities
- Insecure defaults
- Backdoors
```

#### 2. Hardware Vulnerabilities

```
- Firmware vulnerabilities
- Hardware backdoors
- Physical access
- Side-channel attacks
```

#### 3. Configuration Vulnerabilities

```
- Weak passwords
- Default credentials
- Open ports/services
- Misconfigured firewalls
- Unnecessary services running
```

#### 4. Policy Vulnerabilities

```
- No security policy
- Weak password policy
- No user training
- Lack of access control
```

---

### Security Best Practices

**Defense in Depth (ป้องกันหลายชั้น):**

```
Physical Security:
  - Locked server rooms
  - Security guards
  - CCTV

Perimeter Security:
  - Firewalls
  - IPS/IDS
  - VPN

Network Security:
  - Network segmentation
  - ACLs
  - Port security

Endpoint Security:
  - Antivirus
  - Host firewall
  - Patch management

Application Security:
  - Input validation
  - Secure coding
  - Regular updates

Data Security:
  - Encryption
  - Backup
  - Access control

User Education:
  - Security awareness training
  - Phishing simulations
  - Security policies
```

---

## 3.2 Security Threats and Vulnerabilities

### Types of Attackers

**ประเภทของผู้โจมตี:**

#### 1. Script Kiddies

```
- ไม่มีความรู้เชิงลึก
- ใช้ tools/scripts ที่คนอื่นสร้าง
- โจมตีแบบสุ่ม
- อันตรายน้อย แต่จำนวนมาก
```

#### 2. Hacktivists

```
- โจมตีเพื่อส่งข้อความทางการเมือง/สังคม
- defacement websites
- DDoS attacks
- leak ข้อมูล

ตัวอย่าง: Anonymous, LulzSec
```

#### 3. Cybercriminals

```
- โจมตีเพื่อผลประโยชน์ทางการเงิน
- ขโมยข้อมูล credit card
- ransomware
- banking trojans
- organized crime groups
```

#### 4. State-Sponsored Attackers

```
- รับเงินจากรัฐบาล
- espionage (จารกรรม)
- sabotage
- ทรัพยากรมาก, sophisticated
- APT (Advanced Persistent Threats)
```

#### 5. Insider Threats

```
- พนักงาน, อดีตพนักงาน
- มี access ถูกต้อง
- ขโมยข้อมูล, ทำลาย, sabotage
- ยากต่อการป้องกัน

ประเภท:
  - Malicious insider (ตั้งใจ)
  - Negligent insider (ประมาท)
  - Compromised insider (ถูกควบคุม)
```

---

### Common Attack Vectors

**ช่องทางการโจมตี:**

#### 1. Email

```
- Phishing emails
- Malware attachments
- Malicious links
- Business Email Compromise (BEC)
```

#### 2. Web

```
- Drive-by downloads
- Malicious websites
- Compromised legitimate sites
- Watering hole attacks
```

#### 3. Removable Media

```
- Infected USB drives
- External hard drives
- Optical media (CD/DVD)
```

#### 4. Cloud Services

```
- Compromised accounts
- Misconfigured cloud storage
- API vulnerabilities
```

#### 5. Supply Chain

```
- Compromised software updates
- Infected hardware
- Third-party services
```

#### 6. Physical Access

```
- Stolen devices
- Unauthorized physical access
- Tailgating
```

---

### Indicators of Compromise (IoC)

**สัญญาณที่บ่งบอกว่าถูกโจมตี:**

```
Network Indicators:
  - Unusual outbound traffic
  - Strange DNS requests
  - Connections to suspicious IPs
  - Unusual port activity

Host Indicators:
  - New user accounts
  - Changed file permissions
  - Unexpected processes
  - Modified registry keys (Windows)
  - High CPU/memory usage

Application Indicators:
  - Failed login attempts
  - Privilege escalation attempts
  - Disabled security tools
  - Encrypted or deleted files
```

---

## 3.3 Network Security Devices (อุปกรณ์รักษาความปลอดภัย)

### Firewall (ไฟร์วอลล์)

**คำจำกัดความ:**

- อุปกรณ์หรือซอฟต์แวร์ควบคุม traffic in/out ของ network
- กรอง traffic ตาม rules (policies)
- เป็นแนวป้องกันชั้นแรก

**ประเภท Firewalls:**

#### 1. Packet Filtering Firewall

**คำจำกัดความ:**

- ตรวจสอบ packet headers (Layer 3-4)
- กรองตาม: Source/Destination IP, Port, Protocol

**ตัวอย่าง:**

```
Rule: Allow TCP port 80 from any to 192.168.1.100
Rule: Deny TCP port 23 (Telnet) from any to any
Rule: Allow ICMP from 10.0.0.0/8 to 192.168.1.0/24
```

**ข้อดี:**

```
✅ เร็ว
✅ ราคาถูก
✅ ง่าย
```

**ข้อเสีย:**

```
❌ ไม่ตรวจสอบ content
❌ ไม่ track connections (stateless)
❌ ง่ายต่อการ bypass (IP spoofing)
```

#### 2. Stateful Firewall

**คำจำกัดความ:**

- ติดตาม state ของ connections
- จำ connection history
- ฉลาดกว่า packet filtering

**Connection States:**

```
- NEW: Connection ใหม่
- ESTABLISHED: Connection ที่สร้างแล้ว
- RELATED: Connection ที่เกี่ยวข้อง
- INVALID: Connection ผิดปกติ
```

**ตัวอย่าง:**

```
Rule: Allow NEW TCP port 80 to web server
      Allow ESTABLISHED,RELATED from web server

การทำงาน:
  1. Client ส่ง SYN → Web server (NEW) → Allow
  2. Server ส่ง SYN-ACK → Client (ESTABLISHED) → Allow (auto)
  3. Client ส่ง ACK → Server (ESTABLISHED) → Allow (auto)
  4. Data exchange (ESTABLISHED) → Allow (auto)
```

**ข้อดี:**

```
✅ ปลอดภัยกว่า packet filtering
✅ Track connections
✅ ป้องกัน spoofing ได้ดีกว่า
```

**ข้อเสีย:**

```
❌ ช้ากว่า packet filtering (track state)
❌ ไม่ตรวจสอบ application layer content
```

#### 3. Application Gateway (Proxy) Firewall

**คำจำกัดความ:**

- ทำหน้าที่เป็น proxy ระหว่าง client และ server
- ตรวจสอบ application layer (Layer 7)
- เข้าใจ protocols (HTTP, FTP, SMTP)

**การทำงาน:**

```
Client → Proxy Firewall → Server

Proxy รับ request จาก client
Proxy ตรวจสอบ content
Proxy ส่ง request ไป server (ในนามของ client)
Proxy รับ response จาก server
Proxy ตรวจสอบ content
Proxy ส่ง response ไป client
```

**ข้อดี:**

```
✅ ตรวจสอบ content ละเอียด
✅ Block malicious content
✅ Hide internal IP addresses
✅ Caching (เร็วขึ้น)
```

**ข้อเสีย:**

```
❌ ช้า (process application layer)
❌ Resource intensive
❌ ต้อง configure แต่ละ protocol
```

#### 4. Next-Generation Firewall (NGFW)

**คำจำกัดความ:**

- Stateful firewall + เพิ่ม features
- ตรวจสอบ application layer
- รวม IPS, antivirus, URL filtering

**Features:**

```
- Application awareness and control
- Integrated Intrusion Prevention (IPS)
- User identity awareness
- SSL/TLS inspection
- Advanced malware protection
- URL filtering
- Cloud-delivered threat intelligence
```

**ตัวอย่าง Products:**

```
- Cisco Firepower
- Palo Alto Networks
- Fortinet FortiGate
- Check Point
```

---

### Intrusion Prevention System (IPS)

**คำจำกัดความ:**

- ตรวจจับและ**ป้องกัน** attacks โดยอัตโนมัติ
- วิเคราะห์ traffic real-time
- Block/Drop malicious traffic

**การทำงาน:**

```
Traffic → IPS → Network

IPS ตรวจสอบ traffic:
  - ถ้าปกติ → Allow through
  - ถ้าเป็น attack → Block/Drop/Alert
```

**Detection Methods:**

#### 1. Signature-Based

```
- เปรียบเทียบ traffic กับ database ของ attack signatures
- รู้จัก attacks ที่มีอยู่
- รวดเร็ว, แม่นยำ

ข้อเสีย:
  - ตรวจจับ zero-day attacks ไม่ได้
  - ต้อง update signatures
```

#### 2. Anomaly-Based

```
- เรียนรู้ traffic ปกติ (baseline)
- ตรวจจับสิ่งที่ผิดปกติ
- ตรวจจับ zero-day attacks ได้

ข้อเสีย:
  - False positives สูง
  - ช้ากว่า signature-based
```

#### 3. Policy-Based

```
- ตรวจสอบตาม security policies
- Block traffic ที่ผิด policy
```

**IPS Actions:**

```
- Alert: แจ้งเตือน
- Drop: ทิ้ง packet ที่เป็นอันตราย
- Reset: ส่ง TCP reset
- Block: block source IP (ชั่วคราวหรือถาวร)
```

**Placement:**

```
     Internet
        |
    [Firewall]
        |
      [IPS] ← Inline (traffic ผ่าน IPS)
        |
   Internal Network
```

---

### Intrusion Detection System (IDS)

**คำจำกัดความ:**

- ตรวจจับ attacks แต่**ไม่ป้องกัน**
- แจ้งเตือน administrators
- ไม่ block traffic

**IDS vs IPS:**

```
Feature         IDS                     IPS
------------------------------------------------------------------------
Detection       Yes                     Yes
Prevention      No                      Yes
Deployment      Out-of-band (monitor)   Inline (in path)
Action          Alert only              Block/Drop/Alert
Performance     No impact               May impact
False Positive  Acceptable              Critical
------------------------------------------------------------------------
```

**ประเภท IDS:**

#### 1. Network-Based IDS (NIDS)

```
- ติดตั้งบน network segment
- Monitor network traffic
- Placement: TAP หรือ SPAN port

     Internet
        |
    [Firewall]
        |
        +--- [SPAN Port] → [NIDS]
        |
   Internal Network
```

#### 2. Host-Based IDS (HIDS)

```
- ติดตั้งบน individual hosts/servers
- Monitor system logs, file integrity, processes
- ตรวจจับ local attacks

ตัวอย่าง: OSSEC, Tripwire
```

---

### Virtual Private Network (VPN)

**คำจำกัดความ:**

- สร้าง secure tunnel ผ่าน public network (Internet)
- เข้ารหัส traffic
- ปกป้อง confidentiality และ integrity

**ประเภท VPN:**

#### 1. Site-to-Site VPN

**คำจำกัดความ:**

- เชื่อม 2 sites (offices)
- Always-on connection
- Transparent ต่อ users

**Topology:**

```
[Office A] ←--[VPN Tunnel]--→ [Office B]
    |         (Internet)           |
 LAN A                           LAN B
```

**Use Case:**

```
- เชื่อม branch offices กับ headquarters
- เชื่อม offices หลายแห่ง
```

**Protocols:**

```
- IPsec (IP Security)
- GRE (Generic Routing Encapsulation)
- DMVPN (Dynamic Multipoint VPN)
```

#### 2. Remote Access VPN

**คำจำกัดความ:**

- Users เชื่อมต่อจากที่ไหนก็ได้
- On-demand connection
- Client software required

**Topology:**

```
[Remote User] --[VPN Client]--→ [VPN Server] → Corporate Network
                 (Internet)
```

**Use Case:**

```
- Remote workers
- Traveling employees
- Work from home
```

**Protocols:**

```
- SSL/TLS VPN (WebVPN)
- IPsec VPN
- OpenVPN
- WireGuard
```

**SSL VPN vs IPsec VPN:**

```
Feature          SSL VPN              IPsec VPN
------------------------------------------------------------------------
Client           Web browser          VPN client software
Ease of Use      Easy                 Complex
Access           Specific apps        Full network
Port             TCP 443              UDP 500, 4500
Firewall         Easy (HTTPS)         May be blocked
Security         Good                 Excellent
Performance      Lower                Higher
Use Case         Remote access        Site-to-site
------------------------------------------------------------------------
```

---

### Additional Security Devices

#### Content Filter

```
- กรอง web content
- Block malicious/inappropriate sites
- URL filtering
- Category-based blocking (gambling, adult, malware)

Placement: Proxy server หรือ NGFW
```

#### Email Security Gateway

```
- กรอง spam
- Block malware attachments
- Phishing protection
- DLP (Data Loss Prevention)

ตัวอย่าง: Cisco Email Security, Proofpoint
```

#### Web Security Gateway

```
- กรอง web traffic
- URL filtering
- Malware scanning
- SSL inspection

ตัวอย่าง: Cisco Umbrella, Zscaler
```

#### DDoS Mitigation Device

```
- ตรวจจับและป้องกัน DDoS attacks
- Rate limiting
- Traffic scrubbing
- Cloud-based protection

ตัวอย่าง: Cloudflare, Akamai, Arbor Networks
```

---

## 3.4 Security Best Practices

### AAA (Authentication, Authorization, Accounting)

**คำจำกัดความ:**

- Framework สำหรับควบคุม access และ track กิจกรรม

#### Authentication (การพิสูจน์ตัวตน)

**คำจำกัดความ:**

- ยืนยันว่า user เป็นใครจริงๆ
- "Who are you?"

**Methods:**

**Something you know:**

```
- Password
- PIN
- Security questions
```

**Something you have:**

```
- Smart card
- Hardware token (RSA SecurID)
- Mobile phone (for SMS OTP)
- Security key (YubiKey)
```

**Something you are:**

```
- Fingerprint
- Iris scan
- Face recognition
- Voice recognition
```

**Somewhere you are:**

```
- Location-based (GPS)
- Network-based (IP range)
```

**Multi-Factor Authentication (MFA):**

```
ใช้ 2 หรือมากกว่า factors จากประเภทต่างกัน

ตัวอย่าง:
  Password (know) + SMS OTP (have)
  Password (know) + Fingerprint (are)

ข้อดี:
  ✅ ปลอดภัยกว่า single factor มาก
  ✅ ถ้า password รั่วไหล ยังมี factor อื่นป้องกัน
```

#### Authorization (การให้สิทธิ์)

**คำจำกัดความ:**

- กำหนดว่า user สามารถทำอะไรได้บ้าง
- "What can you do?"

**Methods:**

**Role-Based Access Control (RBAC):**

```
- กำหนดสิทธิ์ตาม roles
- User ได้รับ roles
- Roles มี permissions

ตัวอย่าง:
  Role: Administrator
    - Read, Write, Delete files
    - Manage users
    - Configure system

  Role: User
    - Read own files
    - Write own files
```

**Attribute-Based Access Control (ABAC):**

```
- กำหนดสิทธิ์ตาม attributes
- User attributes, Resource attributes, Environment attributes

ตัวอย่าง:
  Rule: Allow access if:
    - User department = Finance
    - Resource type = Financial report
    - Time = Business hours
```

**Principle of Least Privilege:**

```
- ให้สิทธิ์น้อยที่สุดที่จำเป็น
- ลด damage ถ้า account ถูก compromise
```

#### Accounting (การบันทึก)

**คำจำกัดความ:**

- บันทึกกิจกรรมของ users
- "What did you do?"

**Logs:**

```
- Login/Logout times
- Commands executed
- Files accessed
- Configuration changes
- Failures/Errors
```

**Use Cases:**

```
- Audit/Compliance
- Troubleshooting
- Forensics (ตรวจสอบหลังเกิดเหตุ)
- Billing
```

---

### AAA Protocols

#### RADIUS (Remote Authentication Dial-In User Service)

**คำจำกัดความ:**

- Client/Server protocol
- AAA สำหรับ network access
- RFC 2865

**การทำงาน:**

```
User → [NAS/Router] --RADIUS--> [RADIUS Server]
                                      |
                                  Database
                                (Users, Policies)

1. User login ที่ NAS (Network Access Server)
2. NAS ส่ง Access-Request → RADIUS Server
3. RADIUS Server ตรวจสอบ credentials
4. RADIUS Server ตอบ:
   - Access-Accept (ถ้าถูกต้อง) + Authorization info
   - Access-Reject (ถ้าผิด)
5. NAS allow/deny access
```

**Ports:**

```
- UDP 1812 (Authentication)
- UDP 1813 (Accounting)
- Old: UDP 1645, 1646
```

**Use Cases:**

```
- ISP authentication
- Wireless (WPA-Enterprise)
- VPN access
- Network device management
```

#### TACACS+ (Terminal Access Controller Access-Control System Plus)

**คำจำกัดความ:**

- Cisco proprietary protocol
- AAA สำหรับ network devices
- แยก Authentication, Authorization, Accounting

**RADIUS vs TACACS+:**

```
Feature              RADIUS           TACACS+
------------------------------------------------------------------------
Standard             Open             Cisco proprietary
Protocol             UDP              TCP
Port                 1812, 1813       49
Encryption           Password only    All packet
AAA Separation       Combined         Separate
Device Admin         Limited          Full
Flexibility          Lower            Higher
Use Case             End users        Network admins
------------------------------------------------------------------------
```

**การทำงาน:**

```
Admin → [Router] --TACACS+--> [TACACS+ Server]

1. Admin login router
2. Router ส่ง Auth request → TACACS+ Server
3. TACACS+ Server ตอบ (Accept/Reject)
4. Admin execute command
5. Router ส่ง Authorization request → TACACS+ Server
6. TACACS+ Server ตอบ (Allow/Deny)
7. Router บันทึก accounting log → TACACS+ Server
```

**ข้อดี TACACS+:**

```
✅ TCP (reliable)
✅ เข้ารหัสทั้ง packet
✅ แยก AAA (flexibility)
✅ Support command authorization
✅ Detailed accounting
```

---

### Password Security

**Password Best Practices:**

#### Strong Password Requirements

```
✅ ความยาวอย่างน้อย 12-16 characters
✅ ใช้ uppercase, lowercase, numbers, symbols
✅ ไม่ใช้คำใน dictionary
✅ ไม่ใช้ข้อมูลส่วนตัว (วันเกิด, ชื่อ)
✅ Unique สำหรับแต่ละ account

ตัวอย่างรหัสผ่านที่แข็งแรง:
  - T7$mK9#pL2@qN4!
  - Correct-Horse-Battery-Staple (passphrase)
```

#### Password Policies

```
- Minimum length: 12 characters
- Complexity requirements: Uppercase, lowercase, digit, symbol
- Password history: ห้ามใช้รหัสเดิม 5 ครั้งล่าสุด
- Maximum age: 90 days (เปลี่ยนรหัสผ่าน)
- Minimum age: 1 day (ป้องกันเปลี่ยนเร็วเกินไป)
- Account lockout: 5 failed attempts
- Lockout duration: 30 minutes
```

#### Password Storage

```
❌ ห้าม: Plain text
❌ ห้าม: Simple encryption (reversible)
✅ ใช้: Cryptographic hash + salt

ตัวอย่าง:
  Password: MyPassword123
  Salt: random_value_xyz
  Hash: bcrypt(MyPassword123 + random_value_xyz)
  Store: hash + salt (ไม่เก็บ plain password)

Popular algorithms:
  - bcrypt (recommended)
  - scrypt
  - Argon2
```

#### Password Managers

```
ข้อดี:
  ✅ สร้างรหัสผ่านที่แข็งแรง
  ✅ เก็บรหัสผ่านทุก accounts
  ✅ Auto-fill
  ✅ Sync across devices
  ✅ Secure notes

ตัวอย่าง:
  - 1Password
  - LastPass
  - Bitwarden
  - KeePass (offline)
```

---

### Access Control Lists (ACLs)

**คำจำกัดความ:**

- Rules กรอง traffic
- อนุญาตหรือปฏิเสธ traffic ตาม criteria
- ใช้บน routers, switches, firewalls

**จะครอบคลุมใน Module 4-5 โดยละเอียด**

---

### Network Segmentation

**คำจำกัดความ:**

- แบ่ง network เป็น segments/zones
- แยกตาม security level, function
- ควบคุม traffic ระหว่าง segments

**ข้อดี:**

```
✅ จำกัด lateral movement (ถ้าถูก compromise)
✅ ควบคุม access ได้ละเอียด
✅ ง่ายต่อการ monitor
✅ ปฏิบัติตามข้อบังคับ (compliance)
✅ Performance ดีขึ้น (broadcast domains เล็กลง)
```

**Methods:**

#### VLANs (Virtual LANs)

```
- แบ่ง network ด้วย VLANs
- Layer 2 segmentation

ตัวอย่าง:
  VLAN 10: Users
  VLAN 20: Servers
  VLAN 30: Guests
  VLAN 99: Management
```

#### Firewall Zones

```
- แบ่ง zones ด้วย firewalls
- ควบคุม traffic ระหว่าง zones

ตัวอย่าง:
  Outside Zone (Internet)
  DMZ (Public servers)
  Inside Zone (Internal network)
  Management Zone
```

#### Subnets

```
- แบ่ง IP address space
- Route ระหว่าง subnets ด้วย ACLs

ตัวอย่าง:
  10.1.10.0/24: Users
  10.1.20.0/24: Servers
  10.1.30.0/24: Printers
```

---

### Encryption

**คำจำกัดความ:**

- แปลงข้อมูลให้อ่านไม่ได้
- ป้องกัน confidentiality
- ต้องมี key ถึงจะ decrypt ได้

**ประเภท:**

#### Symmetric Encryption

```
- ใช้ key เดียวกันทั้ง encrypt และ decrypt
- เร็ว
- ปัญหา: key distribution

Algorithms:
  - AES (Advanced Encryption Standard) - แนะนำ
  - DES (Data Encryption Standard) - เก่า, ไม่ปลอดภัย
  - 3DES (Triple DES) - ช้า
```

#### Asymmetric Encryption

```
- ใช้ key คู่: Public key และ Private key
- Public key: เข้ารหัส (ใครก็ได้)
- Private key: ถอดรหัส (เจ้าของเท่านั้น)
- ช้ากว่า symmetric

Algorithms:
  - RSA
  - ECC (Elliptic Curve Cryptography)

Use cases:
  - Digital signatures
  - Key exchange
  - SSL/TLS
```

#### Hashing

```
- One-way function
- ไม่สามารถ reverse กลับได้
- ใช้ตรวจสอบ integrity

Algorithms:
  - SHA-256, SHA-384, SHA-512 (Secure Hash Algorithm)
  - MD5 - เก่า, ไม่ปลอดภัย

Use cases:
  - Password storage
  - File integrity checking
  - Digital signatures
```

---

### Secure Protocols

**ใช้ secure protocols แทน insecure:**

```
Insecure         Secure          Description
------------------------------------------------------------------------
HTTP             HTTPS           Web (SSL/TLS encryption)
FTP              SFTP/FTPS       File transfer (SSH/SSL encryption)
Telnet           SSH             Remote access (encrypted)
SNMPv1/v2        SNMPv3          Network management (authentication)
TFTP             SFTP            Trivial file transfer
------------------------------------------------------------------------
```

---

### Security Monitoring and Logging

**Logging Best Practices:**

```
✅ Enable logging บนทุก devices
✅ Centralized logging (Syslog server)
✅ Log retention (เก็บนานพอ)
✅ Regular log review
✅ Automated alerting (SIEM)
✅ Protect log files (encryption, access control)
```

**What to Log:**

```
- Authentication attempts (success/failure)
- Authorization failures
- Configuration changes
- System errors
- Security events
- Network traffic (NetFlow)
```

**SIEM (Security Information and Event Management):**

```
- Collect logs จาก devices ทั้งหมด
- Correlate events
- Real-time alerting
- Compliance reporting

ตัวอย่าง: Splunk, IBM QRadar, ArcSight
```

---

### Incident Response

**Incident Response Plan:**

#### 1. Preparation

```
- สร้าง IR team
- กำหนด roles และ responsibilities
- เตรียม tools และ resources
- Training และ drills
```

#### 2. Identification

```
- Detect security incident
- Determine scope
- Classify severity
```

#### 3. Containment

```
- Short-term: แยก affected systems
- Long-term: patch vulnerabilities
- Prevent spreading
```

#### 4. Eradication

```
- ลบ malware
- Disable breached accounts
- Patch vulnerabilities
```

#### 5. Recovery

```
- Restore systems
- Verify integrity
- Monitor for residual threats
```

#### 6. Lessons Learned

```
- Post-incident review
- Document lessons
- Update policies และ procedures
- Improve defenses
```

---

## Summary (สรุป)

Module 3 นี้เราได้เรียนรู้:

1. ✅ **Current State of Cybersecurity**:
    
    - Security threats: Malware, Social Engineering, Network Attacks
    - Types of attackers: Script kiddies, Hacktivists, Cybercriminals, State-sponsored, Insiders
    - Attack vectors: Email, Web, Removable media, Cloud, Supply chain
2. ✅ **Security Devices**:
    
    - **Firewall**: Packet Filtering, Stateful, Application Gateway, NGFW
    - **IPS/IDS**: Signature-based, Anomaly-based, Policy-based
    - **VPN**: Site-to-Site, Remote Access, IPsec, SSL
3. ✅ **AAA**:
    
    - Authentication: MFA (Something you know/have/are)
    - Authorization: RBAC, ABAC, Least privilege
    - Accounting: Logging, Auditing
    - Protocols: RADIUS, TACACS+
4. ✅ **Security Best Practices**:
    
    - Strong passwords + Password policies
    - Encryption: Symmetric, Asymmetric, Hashing
    - Secure protocols (HTTPS, SSH, SFTP, SNMPv3)
    - Network segmentation (VLANs, Subnets, Zones)
    - Security monitoring and logging
    - Incident response

**สิ่งสำคัญที่ต้องจำ:**

- Defense in Depth: ป้องกันหลายชั้น
- Firewall types: Packet Filtering < Stateful < Application < NGFW
- IPS = Inline (block), IDS = Monitor (alert only)
- VPN: Site-to-Site (offices), Remote Access (users)
- AAA: Authentication (who?) → Authorization (what?) → Accounting (log)
- RADIUS vs TACACS+: RADIUS = users, TACACS+ = admins
- MFA = Security เพิ่มขึ้นอย่างมาก
- Password: Hash + Salt (never plain text)
- Use secure protocols: SSH > Telnet, HTTPS > HTTP, SNMPv3 > v1/v2
- Network segmentation = ลด attack surface
- Logging + Monitoring = ตรวจจับ incidents

**Security Mindset:**

```
- Assume breach (จะเกิดอยู่แล้ว)
- Zero Trust (ไม่เชื่อใครโดยอัตโนมัติ)
- Defense in Depth (ป้องกันหลายชั้น)
- Least Privilege (ให้สิทธิ์น้อยที่สุด)
- Security by Design (ออกแบบให้ปลอดภัยตั้งแต่ต้น)
```

**Next Module:** Module 4 - ACL Concepts

---

**[ไฟล์ Module 3 สมบูรณ์แล้ว!]**