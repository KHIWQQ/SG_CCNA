# CCNA Course 1 - Module 16: Network Security Fundamentals

## พื้นฐานความปลอดภัยเครือข่าย

---

## 16.1 Security Threats and Vulnerabilities (ภัยคุกคามและช่องโหว่)

### Network Security Overview

**คำจำกัดความ:**

- **Network Security** = การปกป้องเครือข่ายและข้อมูลจากภัยคุกคาม
- ป้องกัน **Unauthorized Access, Misuse, Modification, Destruction**

**เป้าหมายของ Network Security:**

- **Confidentiality** (ความลับ) - เฉพาะคนที่ได้รับอนุญาตเท่านั้นเข้าถึงได้
- **Integrity** (ความถูกต้อง) - ข้อมูลไม่ถูกแก้ไขโดยไม่ได้รับอนุญาต
- **Availability** (ความพร้อมใช้งาน) - ระบบพร้อมใช้งานเมื่อต้องการ

**CIA Triad:**

```
         Confidentiality
              /\
             /  \
            /    \
           /      \
          /        \
         /          \
        /   Network  \
       /   Security   \
      /________________\
 Integrity          Availability
```

---

### Types of Security Threats (ประเภทภัยคุกคาม)

#### 1. Information Theft (การขโมยข้อมูล)

**คำจำกัดความ:**

- ขโมย **confidential information**
- Credit cards, passwords, personal data

**ตัวอย่าง:**

```
Attacker ดักจับ:
  - Login credentials
  - Credit card numbers
  - Personal information
  - Company secrets
  - Customer database

Methods:
  - Packet sniffing
  - Man-in-the-Middle attacks
  - Database breaches
```

---

#### 2. Data Loss and Manipulation (การสูญหายและแก้ไขข้อมูล)

**คำจำกัดความ:**

- ทำลายหรือแก้ไข data
- ทำให้ข้อมูล**ไม่น่าเชื่อถือ**

**ตัวอย่าง:**

```
Attacker:
  - ลบ files
  - แก้ไข financial records
  - เปลี่ยนข้อมูลในฐานข้อมูล
  - Ransomware (เข้ารหัส files)

ผลกระทบ:
  ❌ ข้อมูลสูญหาย
  ❌ ข้อมูลไม่ถูกต้อง
  ❌ ไม่สามารถ trust data
```

---

#### 3. Identity Theft (การขโมยตัวตน)

**คำจำกัดความ:**

- ขโมยข้อมูลส่วนตัว
- แอบอ้างเป็นบุคคลอื่น

**ตัวอย่าง:**

```
Attacker ขโมย:
  - Social Security Numbers
  - Passport information
  - Bank account details
  - Driver's license

ใช้เพื่อ:
  - เปิด credit cards
  - ขอสินเชื่อ
  - กระทำผิดกฎหมาย
```

---

#### 4. Disruption of Service (การหยุดชะงักของบริการ)

**คำจำกัดความ:**

- ทำให้เครือข่ายหรือบริการ**ใช้งานไม่ได้**
- **DoS/DDoS attacks**

**ตัวอย่าง:**

```
Attack types:
  - DoS (Denial of Service)
  - DDoS (Distributed DoS)
  - Network flooding
  - Resource exhaustion

ผลกระทบ:
  ❌ Website down
  ❌ Services ไม่สามารถใช้งาน
  ❌ ธุรกิจขาดทุน
```

---

### Types of Vulnerabilities (ช่องโหว่)

#### 1. Technology Vulnerabilities

**คำจำกัดความ:**

- ช่องโหว่ใน **hardware/software**

**ประเภท:**

**a) Hardware Vulnerabilities**

```
ตัวอย่าง:
  - Outdated firmware
  - Hardware bugs
  - Physical access (USB ports)
  - Weak default configurations

Example:
  Router ใช้ default password (admin/admin)
  → Attacker login ได้ง่าย
```

**b) Software Vulnerabilities**

```
ตัวอย่าง:
  - Unpatched operating systems
  - Application bugs
  - Zero-day vulnerabilities
  - Backdoors

Example:
  Windows XP ไม่อัปเดต
  → มี vulnerabilities เยอะ
  → WannaCry ransomware
```

**c) Network Vulnerabilities**

```
ตัวอย่าง:
  - Unencrypted data transmission
  - Weak wireless security (WEP)
  - Open ports/services
  - Network misconfigurations

Example:
  HTTP แทน HTTPS
  → Attacker sniff passwords
```

---

#### 2. Configuration Vulnerabilities

**คำจำกัดความ:**

- ตั้งค่าผิดพลาดหรือไม่ปลอดภัย

**ตัวอย่าง:**

```
Misconfigurations:
  ❌ Default passwords ไม่เปลี่ยน
  ❌ Unnecessary services เปิดอยู่
  ❌ Firewall rules ผิดพลาด
  ❌ Permissions มากเกินไป

Example:
  Database server:
    - ไม่มี password
    - เปิดให้ access จากภายนอก
    → Attacker เข้าถึงได้ทันที
```

---

#### 3. Policy Vulnerabilities

**คำจำกัดความ:**

- ไม่มีหรือมี **security policies ที่อ่อนแอ**

**ตัวอย่าง:**

```
Weak policies:
  ❌ ไม่มี password policy (weak passwords ได้)
  ❌ ไม่มี access control policy
  ❌ ไม่มี data backup policy
  ❌ ไม่มี incident response plan

Example:
  Company ไม่มี policy:
    - Employees ใช้ password "123456"
    - ไม่มี backup → Ransomware = สูญหายหมด
    - ไม่รู้ว่าถูก breach แล้ว
```

---

### Types of Malware (มัลแวร์)

**คำจำกัดความ:**

- **Malicious Software** (software ที่เป็นอันตราย)
- ออกแบบมาเพื่อ **ทำลาย, ขโมย, หรือควบคุม** systems

---

#### 1. Virus (ไวรัส)

**คำจำกัดความ:**

- **Attaches** ตัวเองเข้ากับ **host files**
- แพร่เมื่อ user **เปิดไฟล์ที่ติดเชื้อ**
- ต้องการ **user action** เพื่อแพร่

**การทำงาน:**

```
1. Virus ฝังตัวใน file (e.g., document.exe)
2. User เปิดไฟล์
3. Virus execute และทำลาย
4. Virus copy ตัวเองไปยัง files อื่น
5. User แชร์ไฟล์ → แพร่ต่อ
```

**ตัวอย่าง:**

- **ILOVEYOU** virus (2000)
- **Melissa** virus (1999)

**ผลกระทบ:**

- ลบ files
- ทำให้ system ช้า
- ขโมยข้อมูล

---

#### 2. Worm (เวิร์ม)

**คำจำกัดความ:**

- **Self-replicating** (แพร่ตัวเองอัตโนมัติ)
- **ไม่ต้องการ user action**
- แพร่ผ่าน network

**ความแตกต่างจาก Virus:**

```
Virus:
  - ต้อง attach กับ file
  - ต้องการ user action
  - แพร่ช้ากว่า

Worm:
  - Independent (ไม่ต้อง attach)
  - แพร่อัตโนมัติ
  - แพร่เร็วมาก
```

**การทำงาน:**

```
1. Worm scan network หา vulnerable systems
2. Exploit vulnerability เพื่อเข้าไป
3. Install ตัวเองบน system
4. Scan หา targets ใหม่
5. แพร่ต่อ (exponential growth)
```

**ตัวอย่าง:**

- **WannaCry** (2017) - Ransomware worm
- **Conficker** (2008)
- **Code Red** (2001)

**ผลกระทบ:**

- เครือข่ายช้า (traffic เยอะ)
- System resources หมด
- แพร่เร็วมาก

---

#### 3. Trojan Horse (โทรจัน)

**คำจำกัดความ:**

- **ปลอมตัว**เป็น legitimate software
- User **ติดตั้งด้วยความสมัครใจ**
- ไม่ self-replicate (ไม่แพร่เอง)

**การทำงาน:**

```
1. Trojan ปลอมเป็น useful program
   Example: "Free Antivirus", "Game Crack"
2. User download และ install
3. Trojan ทำงานลับๆ ในพื้นหลัง
   - เปิด backdoor
   - ขโมยข้อมูล
   - download malware เพิ่ม
```

**ประเภท Trojans:**

```
Remote Access Trojan (RAT):
  - ให้ attacker ควบคุม system จากระยะไกล

Banking Trojan:
  - ขโมย banking credentials

Downloader Trojan:
  - Download malware เพิ่มเติม

Ransomware Trojan:
  - เข้ารหัส files และขู่เรียกค่าไถ่
```

**ตัวอย่าง:**

- **Zeus** - Banking Trojan
- **Emotet** - Downloader Trojan

---

#### 4. Spyware (สปายแวร์)

**คำจำกัดความ:**

- **Monitor และขโมยข้อมูล** โดยไม่ได้รับอนุญาต
- รวบรวม **sensitive information**

**ข้อมูลที่เก็บ:**

```
- Keystrokes (keylogger)
- Browsing history
- Login credentials
- Credit card numbers
- Screenshots
- Files
```

**การทำงาน:**

```
1. Spyware ติดตั้งโดยไม่รู้ตัว (bundle กับ software)
2. ทำงานพื้นหลังโดยซ่อนตัว
3. เก็บข้อมูล user
4. ส่งข้อมูลกลับไปยัง attacker
```

**ตัวอย่าง:**

- **Keyloggers** - บันทึก keystrokes
- **Adware** - แสดง ads, track browsing
- **Stalkerware** - track location, messages

---

#### 5. Adware (แอดแวร์)

**คำจำกัดความ:**

- แสดง **โฆษณาที่ไม่พึงประสงค์**
- Track browsing habits
- มักมากับ **free software**

**ผลกระทบ:**

```
❌ Pop-up ads รบกวน
❌ Browser redirects
❌ System ช้า
❌ Privacy concerns (track behavior)
```

**ตัวอย่าง:**

```
Download free software
→ Adware ติดมาด้วย (bundled)
→ Pop-ups everywhere
→ Browser homepage เปลี่ยน
→ Search engine เปลี่ยน
```

---

#### 6. Ransomware (แรนซัมแวร์)

**คำจำกัดความ:**

- **เข้ารหัส files** ของ victim
- ขู่เรียก**ค่าไถ่** (ransom) เพื่อ decrypt

**การทำงาน:**

```
1. Ransomware ติดตั้ง (email, exploit, download)
2. Encrypt files โดยใช้ strong encryption
3. แสดง ransom note:
   "จ่าย $500 ใน Bitcoin ใน 72 ชม."
   "ไม่จ่าย = ลบ decryption key"
4. Victim ตัดสินใจ:
   - จ่าย (ไม่รับประกันได้ key คืน)
   - ไม่จ่าย (files หายไปตลอดกาล)
```

**ตัวอย่าง:**

- **WannaCry** (2017) - แพร่ผ่าน SMB vulnerability
- **Petya/NotPetya** (2017)
- **Ryuk** (2018+)

**ป้องกัน:**

```
✅ Backup ข้อมูลสม่ำเสมอ (offline backup)
✅ อัปเดต software/OS
✅ ระวัง email attachments
✅ Antivirus/Anti-malware
✅ User education
```

---

#### 7. Rootkit (รูทคิต)

**คำจำกัดความ:**

- **ซ่อนตัว**และ**ยากต่อการตรวจจับ**
- Modify OS เพื่อซ่อน malware
- มี **privileged access** (root/admin)

**การทำงาน:**

```
1. Attacker exploit vulnerability → gain admin access
2. Install rootkit
3. Rootkit modify OS:
   - ซ่อน files/processes
   - ซ่อน network connections
   - ซ่อนตัวเองจาก antivirus
4. Attacker มี persistent access
```

**ประเภท:**

```
User-mode Rootkit:
  - ทำงานใน user space
  - ตรวจจับง่ายกว่า

Kernel-mode Rootkit:
  - ทำงานใน kernel space
  - ควบคุม OS เต็มรูปแบบ
  - ตรวจจับยากมาก

Bootkit:
  - ติดตั้งใน boot sector
  - โหลดก่อน OS
  - ตรวจจับยากที่สุด
```

**ผลกระทบ:**

```
❌ ยากมากต่อการตรวจจับและลบ
❌ Attacker มี full control
❌ มักต้อง reformat เพื่อแก้
```

---

### Malware Comparison Summary

```
Malware Type    Self-Replicate  User Action  Hiding      Purpose
------------------------------------------------------------------------
Virus           Yes             Required     No          Damage/Spread
Worm            Yes             Not Required No          Damage/Spread
Trojan          No              Required     Disguise    Backdoor/Theft
Spyware         No              N/A          Yes         Steal Data
Adware          No              N/A          No          Show Ads
Ransomware      No              May/May not  No          Extortion
Rootkit         No              N/A          Yes         Persistent Access
------------------------------------------------------------------------
```

---

### Attack Types (ประเภทการโจมตี)

#### 1. Reconnaissance Attacks (การสำรวจ)

**คำจำกัดความ:**

- **รวบรวมข้อมูล**เกี่ยวกับ target
- เตรียมการก่อน attack จริง

**วิธีการ:**

**a) Information Gathering:**

```
- Google hacking (search engine queries)
- Social media reconnaissance
- WHOIS lookups (domain information)
- DNS queries
- Company website analysis
```

**b) Scanning:**

```
- Port scanning (nmap)
  Example: nmap -p- 192.168.1.0/24
  → หา open ports

- Network scanning
  → หา live hosts

- Vulnerability scanning
  → หา vulnerabilities
```

**c) Social Engineering:**

```
- Phishing emails
- Phone calls (pretexting)
- Physical intrusion attempts
```

**เป้าหมาย:**

```
รู้ว่า:
  - เครือข่ายมี structure อย่างไร
  - มี services/ports อะไรเปิดบ้าง
  - OS และ software versions
  - Employees (targets สำหรับ social engineering)
  - Vulnerabilities ที่ exploit ได้
```

---

#### 2. Access Attacks (การเข้าถึง)

**คำจำกัดความ:**

- พยายาม**เข้าถึง systems หรือข้อมูล** โดยไม่ได้รับอนุญาต

**ประเภท:**

**a) Password Attacks:**

**Brute Force:**

```
คำจำกัดความ: ลองรหัสผ่านทุกความเป็นไปได้

Example:
Password = 4 digits
Try: 0000, 0001, 0002, ..., 9999

Tools: Hydra, John the Ripper

ป้องกัน:
  - Account lockout (lock หลังพยายามผิดหลายครั้ง)
  - Strong passwords
  - Rate limiting
```

**Dictionary Attack:**

```
คำจำกัดความ: ลองรหัสผ่านจาก wordlist (dictionary)

Example:
Wordlist: common passwords
  - password
  - 123456
  - admin
  - letmein

เร็วกว่า brute force (ลองเฉพาะคำที่น่าจะใช้)

ป้องกัน:
  - ห้ามใช้ common passwords
  - Password complexity requirements
```

**Rainbow Table:**

```
คำจำกัดความ: ใช้ precomputed hashes เพื่อ crack passwords

Concept:
  Password → Hash (MD5, SHA1)
  Rainbow table = table ของ (password, hash) pairs

Attack:
  1. ได้ hash จาก database leak
  2. Look up ใน rainbow table
  3. หา password ที่ตรง

ป้องกัน:
  - Salt passwords (เพิ่ม random data ก่อน hash)
  - ใช้ strong hashing algorithms (bcrypt, Argon2)
```

**Password Spraying:**

```
คำจำกัดความ: ลอง common password กับหลาย accounts

Example:
Accounts: user1, user2, user3, ..., user100
Password: "Spring2025!" (common pattern)

Try: 
  - user1:Spring2025!
  - user2:Spring2025!
  - ...

ข้อดี (สำหรับ attacker):
  - หลีกเลี่ยง account lockout (ไม่ลองซ้ำ account เดิม)

ป้องกัน:
  - Detect patterns (ลอง password เดิมหลาย accounts)
  - Multi-factor authentication (MFA)
```

---

**b) Spoofing:**

**คำจำกัดความ:**

- **ปลอม identity** เพื่อหลอก systems หรือ users

**ประเภท:**

**IP Spoofing:**

```
Attacker ปลอม source IP address

Example:
Real packet: Source=192.168.1.100
Spoofed packet: Source=192.168.1.1 (trusted IP)

ใช้เพื่อ:
  - Bypass IP-based access control
  - DoS attacks (flood ด้วย spoofed IPs)
  - Hide identity

ป้องกัน:
  - Ingress/Egress filtering
  - Authentication (ไม่ trust แค่ IP)
```

**MAC Spoofing:**

```
Attacker เปลี่ยน MAC address

Example:
Real MAC: 00:11:22:33:44:55
Spoofed MAC: AA:BB:CC:DD:EE:FF (authorized device)

ใช้เพื่อ:
  - Bypass MAC filtering
  - Impersonate authorized device

ป้องกัน:
  - Port security
  - 802.1X authentication
```

**Email Spoofing:**

```
Attacker ปลอม sender email address

Example:
Fake email:
  From: ceo@company.com
  Subject: Urgent! Transfer $50,000

ใช้เพื่อ:
  - Phishing
  - Business Email Compromise (BEC)

ป้องกัน:
  - SPF, DKIM, DMARC
  - Email authentication
  - User training
```

---

**c) Trust Exploitation:**

**Man-in-the-Middle (MitM):**

```
คำจำกัดความ: Attacker อยู่ระหว่าง client และ server

การทำงาน:
  Client → Attacker → Server
  Client ← Attacker ← Server

Attacker:
  - Intercept traffic
  - ดู/แก้ไข data
  - Impersonate ทั้งสองฝั่ง

Example:
  Client คิดว่าพูดกับ server
  Server คิดว่าพูดกับ client
  แท้จริง: ทั้งคู่พูดกับ attacker

ใช้เพื่อ:
  - ขโมย credentials
  - Session hijacking
  - แก้ไข transactions

ป้องกัน:
  - Encryption (HTTPS, VPN)
  - Certificate validation
  - Mutual authentication
```

---

#### 3. Denial of Service (DoS) Attacks

**คำจำกัดความ:**

- ทำให้ service/network **ใช้งานไม่ได้**
- **Overwhelm resources**

**ประเภท:**

**a) DoS (Denial of Service):**

```
คำจำกัดความ: Attack จาก single source

Methods:
  - Flood traffic (overwhelm bandwidth)
  - Exhaust resources (CPU, memory)
  - Crash application/OS

Example:
  Ping Flood:
    Attacker → flood ICMP echo requests
    → Target overwhelmed
```

**b) DDoS (Distributed DoS):**

```
คำจำกัดความ: Attack จาก multiple sources (botnet)

การทำงาน:
  1. Attacker สร้าง botnet (zombie computers)
  2. สั่ง botnet attack target พร้อมกัน
  3. Target overwhelmed (traffic จากทุกทิศทาง)

Example:
  100,000 bots → Target
  Traffic: 1 Tbps (terabit/second)
  → Target down ทันที

ป้องกันยาก:
  ❌ Traffic จาก legitimate IPs (ยาก block)
  ❌ Volume มหาศาล
```

**DDoS Attack Types:**

**SYN Flood:**

```
Exploit TCP 3-way handshake

Normal:
  Client → SYN → Server
  Client ← SYN-ACK ← Server
  Client → ACK → Server

Attack:
  Attacker → SYN (spoofed IPs) → Server
  Server → SYN-ACK → (no response)
  Server รอ ACK (ไม่มา)
  → Server's connection table full
  → Legitimate users ไม่สามารถ connect

ป้องกัน:
  - SYN cookies
  - Increase connection table size
  - Timeout ลดลง
```

**HTTP Flood:**

```
Flood HTTP requests

Attack:
  Botnet → HTTP GET requests → Web server
  Volume: thousands/millions per second
  → Server resources exhausted

Example:
  GET /?random=12345 (แต่ละ request ต่างกัน)
  → ไม่สามารถ cache
  → Server ต้อง process ทุกอัน

ป้องกัน:
  - Rate limiting
  - Challenge-Response (CAPTCHA)
  - CDN (Content Delivery Network)
```

**DNS Amplification:**

```
Exploit DNS servers เพื่อ amplify attack

การทำงาน:
  1. Attacker → DNS query (spoofed source = target IP)
  2. DNS server → large response → Target
  3. Amplification: 1 byte request → 100 bytes response

Example:
  Query: 60 bytes → Response: 3000 bytes
  Amplification factor: 50x

ป้องกัน:
  - DNS server configuration (disable recursion)
  - Rate limiting
  - Response Rate Limiting (RRL)
```

---

#### 4. Social Engineering Attacks

**คำจำกัดความ:**

- หลอกลวง **humans** แทนที่จะ hack systems
- Exploit **human psychology**

**ประเภท:**

**a) Phishing:**

```
คำจำกัดความ: Fake emails/websites เพื่อขโมยข้อมูล

Example:
  Email:
    From: security@paypal.com (fake)
    Subject: Your account has been suspended
    Link: http://paypa1.com/login (fake site)

  Victim:
    - คลิก link
    - ใส่ username/password
    → Attacker ได้ credentials

ป้องกัน:
  - ตรวจสอบ sender email
  - ดู URL ให้ดี
  - ไม่คลิก links ใน emails
  - Multi-factor authentication
```

**b) Spear Phishing:**

```
คำจำกัดความ: Phishing แบบ targeted (เจาะจง victim)

Example:
  Target: CFO ของ company
  Email:
    From: CEO@company.com (fake)
    Subject: Urgent wire transfer needed
    "Transfer $100,000 to this account immediately"

  CFO:
    - เชื่อว่าเป็น CEO จริง
    - โอนเงิน
    → เงินหาย

ป้องกัน:
  - Verify requests (โทรศัพท์หา sender)
  - Email authentication (SPF, DKIM)
  - Employee training
```

**c) Whaling:**

```
คำจำกัดความ: Spear phishing ที่ target = executives (CEO, CFO)

ทำไมเรียก "Whaling":
  - Target = "big fish" (ผู้บริหารระดับสูง)

Example:
  Email → CEO:
    "You have a legal subpoena"
    Attachment: subpoena.pdf.exe (malware)

  CEO:
    - เปิด attachment
    → Malware execute
    → Attacker เข้าถึง sensitive data
```

**d) Pretexting:**

```
คำจำกัดความ: สร้าง scenario (pretext) เพื่อหลอกข้อมูล

Example:
  Attacker โทร help desk:
    "สวัสดีครับ ผม John จาก IT
     ต้องการ reset password ของ CEO
     มี ticket ID: 12345 (fake)"

  Help desk:
    - เชื่อว่าเป็น IT จริง
    - Reset password
    → Attacker login เป็น CEO

ป้องกัน:
  - Verification procedures
  - Callback protocols
  - Never trust caller ID
```

**e) Baiting:**

```
คำจำกัดความ: ใช้ "bait" (เหยื่อล่อ) เพื่อหลอก victim

Example:
  Physical:
    USB drive ทิ้งไว้ในที่จอดรถ
    Label: "Employee Salaries 2025"
    
  Victim:
    - เก็บ USB ไป
    - เสียบเข้าคอมพิวเตอร์
    - เปิด files
    → Malware execute

  Digital:
    "Free Movie Downloads!"
    → Download = Malware

ป้องกัน:
  - ไม่เสียบ unknown USB devices
  - ไม่ download จาก untrusted sources
  - USB port disabling
```

**f) Tailgating (Piggybacking):**

```
คำจำกัดความ: ตามเข้าไปหลัง authorized person

Example:
  Scenario:
    Employee กำลังเปิดประตูด้วย badge
    Attacker เดินตามเข้ามาทันที
    
  Employee:
    - ไม่กล้าปิดประตู (ไม่สุภาพ)
    - คิดว่า attacker = employee คนอื่น
    
  Attacker:
    - เข้าไปใน secure area
    - เข้าถึง servers, documents

ป้องกัน:
  - Security guards
  - Mantraps (ประตู 2 ชั้น)
  - Badge readers (ต้อง badge ทุกคน)
  - Employee training (ท้าทายคนแปลกหน้า)
```

**g) Shoulder Surfing:**

```
คำจำกัดความ: มองดูข้อมูลทางหน้าจอหรือ keyboard

Example:
  Location: Coffee shop, airport, office

  Victim พิมพ์ password
  Attacker มองจากข้างหลัง (shoulder)
  → Attacker เห็น password

ป้องกัน:
  - Privacy screens
  - ระวังตัวในที่สาธารณะ
  - ไม่พิมพ์ sensitive info ในที่คนเยอะ
```

---

## 16.2 Security Best Practices (แนวทางปฏิบัติที่ดี)

### Defense in Depth (ป้องกันหลายชั้น)

**คำจำกัดความ:**

- ใช้**หลายชั้นของการป้องกัน**
- ถ้าชั้นหนึ่งถูก breach → ยังมีชั้นอื่นปกป้อง

**ตัวอย่าง:**

```
Layer 1: Physical Security
  - Locks, guards, cameras

Layer 2: Network Perimeter
  - Firewall, IPS/IDS

Layer 3: Network Segmentation
  - VLANs, subnets

Layer 4: Endpoint Protection
  - Antivirus, host firewall

Layer 5: Application Security
  - Input validation, authentication

Layer 6: Data Security
  - Encryption, access control

Layer 7: User Education
  - Security awareness training
```

**Concept:**

```
Attacker ต้อง breach ทุกชั้น
→ ยากมาก, ใช้เวลานาน
→ มีโอกาสตรวจจับสูง
```

---

### Security Devices and Tools

#### 1. Firewall

**คำจำกัดความ:**

- กรองและควบคุม **network traffic**
- อนุญาต/ปฏิเสธ traffic ตาม **rules**

**ตำแหน่ง:**

```
Internet ← → Firewall ← → Internal Network

Firewall = "gate keeper"
  - ตรวจสอบ traffic ทุก packet
  - อนุญาตเฉพาะที่ปลอดภัย
```

**Types:**

**Packet Filtering Firewall:**

```
ตรวจสอบ packet headers:
  - Source/Destination IP
  - Source/Destination Port
  - Protocol

Rules:
  ALLOW TCP from any to 192.168.1.10 port 80
  DENY TCP from any to any port 23 (Telnet)

ข้อจำกัด:
  - ไม่ดู packet content
  - ไม่เข้าใจ application layer
```

**Stateful Firewall:**

```
ตรวจสอบ connection state:
  - Track TCP connections
  - เข้าใจ sessions

Example:
  Outbound: 192.168.1.10:50001 → 8.8.8.8:80
  → Firewall จำ connection
  
  Inbound: 8.8.8.8:80 → 192.168.1.10:50001
  → Firewall รู้ว่าเป็น response → อนุญาต

ดีกว่า packet filtering:
  - เข้าใจ context
  - ป้องกัน spoofing ดีกว่า
```

**Application Layer Firewall:**

```
ตรวจสอบ application layer:
  - HTTP requests
  - FTP commands
  - SQL queries

Example:
  Block HTTP request ที่มี:
    - SQL injection attempts
    - Malicious scripts

ดีที่สุด:
  - เข้าใจ application protocols
  - Deep packet inspection
```

---

#### 2. IDS/IPS (Intrusion Detection/Prevention System)

**IDS (Intrusion Detection System):**

```
คำจำกัดความ: ตรวจจับ attacks (detect only)

การทำงาน:
  1. Monitor network traffic
  2. ตรวจจับ suspicious patterns
  3. Alert administrator
  4. ไม่ block (passive)

ข้อดี:
  ✅ ไม่รบกวน traffic
  ✅ ไม่มี false positive ที่ block

ข้อเสีย:
  ❌ ไม่ block attacks (ต้องมี human response)
```

**IPS (Intrusion Prevention System):**

```
คำจำกัดความ: ตรวจจับและป้องกัน attacks (detect & prevent)

การทำงาน:
  1. Monitor network traffic
  2. ตรวจจับ attacks
  3. Block attacks อัตโนมัติ
  4. Alert administrator

ข้อดี:
  ✅ Block attacks ทันที
  ✅ ไม่ต้องรอ human

ข้อเสีย:
  ❌ False positives อาจ block legitimate traffic
  ❌ ต้อง inline (latency เพิ่มขึ้น)
```

**Detection Methods:**

**Signature-Based:**

```
ใช้ database ของ known attack patterns

Example:
  Attack signature: SQL injection pattern
    " OR 1=1 --"
  
  ถ้าเจอ pattern → Alert/Block

ข้อดี:
  ✅ Accurate สำหรับ known attacks
  ✅ False positives น้อย

ข้อเสีย:
  ❌ ไม่ตรวจจับ zero-day attacks (ยังไม่รู้จัก)
  ❌ ต้องอัปเดต signatures สม่ำเสมอ
```

**Anomaly-Based:**

```
เรียนรู้ "normal behavior" → ตรวจจับสิ่งผิดปกติ

Example:
  Normal: User A login 9am-5pm จาก office
  Anomaly: User A login 3am จาก China
  → Alert

ข้อดี:
  ✅ ตรวจจับ zero-day attacks
  ✅ ตรวจจับ unknown threats

ข้อเสีย:
  ❌ False positives สูง
  ❌ ต้องการ training period
```

---

#### 3. VPN (Virtual Private Network)

**คำจำกัดความ:**

- สร้าง **encrypted tunnel** ผ่าน public network
- ทำให้ traffic **secure และ private**

**Use Cases:**

**Remote Access VPN:**

```
Scenario: Employee work from home

Employee (Home) → Internet → VPN Server → Company Network

การทำงาน:
  1. Employee connect to VPN server
  2. Authentication (username/password)
  3. Encrypted tunnel สร้างขึ้น
  4. Traffic encrypted ผ่าน Internet
  5. VPN server decrypt → forward to company network

ข้อดี:
  ✅ Secure remote access
  ✅ ป้องกัน eavesdropping
  ✅ Access internal resources
```

**Site-to-Site VPN:**

```
Scenario: เชื่อม 2 offices

Office A → Internet → Office B

การทำงาน:
  1. Router ทั้ง 2 ฝั่ง setup VPN tunnel
  2. Traffic ระหว่าง offices encrypted
  3. Networks แต่ละฝั่งเป็น 1 เดียว

ข้อดี:
  ✅ Secure connection ระหว่าง sites
  ✅ ไม่ต้องเช่า dedicated line (ประหยัด)
```

**VPN Protocols:**

```
IPsec (IP Security):
  - Encrypt IP packets
  - ใช้สำหรับ site-to-site VPN
  - Secure, complex

SSL/TLS VPN:
  - ใช้ HTTPS (port 443)
  - ใช้สำหรับ remote access
  - ง่าย (ผ่าน web browser)

OpenVPN:
  - Open source
  - Flexible
  - Cross-platform
```

---

#### 4. Antivirus/Anti-malware

**คำจำกัดความ:**

- ตรวจจับและลบ **malware**
- ป้องกัน infections

**การทำงาน:**

**1. Signature-Based Detection:**

```
ใช้ database ของ known malware signatures

Process:
  1. Scan files
  2. เปรียบเทียบกับ signatures
  3. ถ้าตรง → Malware detected → Quarantine/Remove

ข้อจำกัด:
  - ต้องอัปเดต signatures
  - ไม่จับ zero-day malware
```

**2. Heuristic Analysis:**

```
วิเคราะห์ behavior ของ programs

Example:
  Program พยายาม:
    - Modify system files
    - Disable antivirus
    - Encrypt files rapidly (ransomware behavior)
  → Suspicious → Block

ข้อดี:
  - ตรวจจับ unknown malware
  - ไม่ต้องมี signature

ข้อเสีย:
  - False positives สูงกว่า
```

**3. Sandboxing:**

```
Run suspicious programs ใน isolated environment

Process:
  1. Suspicious file → Sandbox
  2. Execute ใน sandbox
  3. Monitor behavior
  4. ถ้า malicious → Block

ข้อดี:
  - ปลอดภัย (ไม่กระทบ real system)
  - ตรวจจับ advanced malware
```

**Best Practices:**

```
✅ Enable real-time scanning
✅ อัปเดต definitions สม่ำเสมอ
✅ Schedule full scans
✅ Don't disable antivirus
✅ Use multiple layers (antivirus + firewall + ...)
```

---

### Password Security

#### Password Best Practices

**Strong Password Requirements:**

```
✅ Length: อย่างน้อย 12-16 characters
✅ Complexity:
   - Uppercase (A-Z)
   - Lowercase (a-z)
   - Numbers (0-9)
   - Special characters (!@#$%^&*)
✅ ไม่ใช้:
   - Dictionary words
   - Personal information (birthdays, names)
   - Common patterns (123456, qwerty)
   - Same password ทุก accounts

Example Strong Password:
  ❌ password123
  ❌ john1985
  ✅ K8$mP#9vL@2nQ!xR
  ✅ MyD0g!sC00l&Fluffy2025
```

---

#### Password Policies

**องค์กรควรมี:**

```
✅ Minimum length (12+ characters)
✅ Complexity requirements
✅ Password expiration (60-90 days)
✅ Password history (ไม่ให้ใช้ซ้ำ 5-10 passwords ล่าสุด)
✅ Account lockout:
   - Lock หลังพยายามผิด 3-5 ครั้ง
   - Lock duration: 15-30 minutes
✅ MFA requirement (Two-Factor Authentication)
```

---

#### Multi-Factor Authentication (MFA)

**คำจำกัดความ:**

- ใช้ **2 หรือมากกว่า factors** เพื่อ authenticate

**Authentication Factors:**

**1. Something You Know (รู้):**

```
- Password
- PIN
- Security questions
```

**2. Something You Have (มี):**

```
- Security token (RSA token)
- Smartphone (SMS, app)
- Smart card
- USB security key
```

**3. Something You Are (เป็น):**

```
- Fingerprint
- Face recognition
- Iris scan
- Voice recognition
```

**ตัวอย่าง MFA:**

```
Login process:
  1. ใส่ username/password (Something You Know)
  2. รับ SMS code บนโทรศัพท์ (Something You Have)
  3. ใส่ code
  → Login successful

ข้อดี:
  ✅ แม้ password ถูกขโมย → Attacker ยังต้องการ factor 2
  ✅ Security เพิ่มขึ้นมาก
```

**MFA Methods:**

```
SMS:
  ✅ ง่าย
  ❌ SIM swapping attacks

Authenticator Apps (Google Authenticator, Authy):
  ✅ ปลอดภัยกว่า SMS
  ✅ Offline (ไม่ต้องมี network)

Hardware Tokens (YubiKey):
  ✅ ปลอดภัยที่สุด
  ❌ ต้องซื้อ hardware

Biometric:
  ✅ สะดวก
  ❌ ไม่สามารถเปลี่ยนได้ (ถ้า compromised)
```

---

### Data Backup and Recovery

**คำจำกัดความ:**

- สำรอง**สำเนาข้อมูล** เพื่อป้องกันสูญหาย
- Recovery เมื่อเกิด disaster

**3-2-1 Backup Rule:**

```
3 = มี copies อย่างน้อย 3 ชุด
    (1 original + 2 backups)

2 = เก็บใน storage media 2 types
    (e.g., HDD + Cloud)

1 = เก็บ 1 copy offsite
    (ไม่อยู่ location เดียวกัน)

Example:
  Original: Production server
  Backup 1: External HDD (onsite)
  Backup 2: Cloud storage (offsite)
```

---

**Backup Types:**

**Full Backup:**

```
คำจำกัดความ: สำรอง everything

ข้อดี:
  ✅ Recovery ง่ายที่สุด (restore จาก backup เดียว)

ข้อเสีย:
  ❌ ใช้เวลานาน
  ❌ ใช้ storage มาก

ใช้เมื่อ:
  - Weekly/Monthly
```

**Incremental Backup:**

```
คำจำกัดความ: สำรอง changes ตั้งแต่ backup ครั้งล่าสุด

Example:
  Sunday: Full backup
  Monday: Incremental (changes since Sunday)
  Tuesday: Incremental (changes since Monday)
  Wednesday: Incremental (changes since Tuesday)

ข้อดี:
  ✅ เร็ว
  ✅ ใช้ storage น้อย

ข้อเสีย:
  ❌ Recovery ช้า (ต้อง restore full + ทุก incrementals)

ใช้เมื่อ:
  - Daily
```

**Differential Backup:**

```
คำจำกัดความ: สำรอง changes ตั้งแต่ full backup ครั้งล่าสุด

Example:
  Sunday: Full backup
  Monday: Differential (changes since Sunday)
  Tuesday: Differential (changes since Sunday)
  Wednesday: Differential (changes since Sunday)

ข้อดี:
  ✅ Recovery เร็วกว่า incremental (full + differential ล่าสุด)

ข้อเสีย:
  ❌ ช้ากว่า incremental
  ❌ ใช้ storage มากกว่า incremental

ใช้เมื่อ:
  - Daily
```

---

**Backup Best Practices:**

```
✅ Automate backups (schedule)
✅ Test restores สม่ำเสมอ (ทดสอบว่า restore ได้จริง)
✅ Encrypt backups (ป้องกันถูกขโมย)
✅ Offsite/Cloud backups (ป้องกัน fire, flood, theft)
✅ Version control (เก็บหลาย versions)
✅ Document backup procedures
✅ Monitor backup jobs (ให้แน่ใจว่าสำเร็จ)
```

---

### Security Updates and Patches

**คำจำกัดความ:**

- **Patches** = software updates ที่แก้ไข vulnerabilities

**ทำไมสำคัญ:**

```
Software มี bugs และ vulnerabilities
Attackers exploit vulnerabilities เหล่านี้
Vendors ออก patches เพื่อแก้ไข

ถ้าไม่อัปเดต:
  ❌ Vulnerabilities ยังอยู่
  ❌ Attackers exploit ได้
  ❌ System compromised

Example:
  WannaCry ransomware (2017)
  - Exploit SMB vulnerability (MS17-010)
  - Microsoft ออก patch แล้ว (March 2017)
  - Organizations ที่ไม่อัปเดต → ถูก attack (May 2017)
```

---

**Patch Management Best Practices:**

```
✅ Patch ทันที (especially critical patches)
✅ Test patches ก่อน deploy (ใน test environment)
✅ Prioritize critical systems
✅ Automate patching (ถ้าทำได้)
✅ Track patch status (รู้ว่า system ไหนยังไม่ patch)
✅ Have rollback plan (ถ้า patch มีปัญหา)
```

**Patch Schedule:**

```
Critical patches:
  → Deploy ภายใน 24-48 hours

High priority:
  → Deploy ภายใน 1 week

Medium/Low priority:
  → Deploy ตาม maintenance window
```

---

### Physical Security

**คำจำกัดความ:**

- ปกป้อง **physical access** to devices/facilities

**ทำไมสำคัญ:**

```
Physical access = full control

Attacker ที่เข้าถึง physical:
  - Steal devices (laptops, servers, hard drives)
  - Install malware (bootable USB)
  - Tamper with hardware
  - Access console
  - Bypass network security

Example:
  Attacker เข้า server room:
    → เสียบ USB → boot malware
    → Install rootkit
    → Full compromise
```

---

**Physical Security Measures:**

**1. Facility Security:**

```
✅ Perimeter security (fences, gates)
✅ Security guards
✅ Surveillance cameras
✅ Access control systems (badge readers)
✅ Visitor management (sign-in, escorts)
✅ Secure doors/windows
✅ Alarms
```

**2. Server Room Security:**

```
✅ Restricted access (authorized personnel only)
✅ Biometric access control
✅ Logging (who accessed when)
✅ Cameras
✅ Environmental controls:
   - Temperature/Humidity monitoring
   - Fire suppression
   - Water detection
✅ Equipment racks locked
```

**3. Device Security:**

```
✅ Laptop locks (Kensington lock)
✅ Disable USB ports (ป้องกัน malware)
✅ Encrypted hard drives (ป้องกันถูกขโมย)
✅ Asset tags และ inventory
✅ Secure disposal (ทำลาย hard drives)
```

**4. Access Control:**

```
Badge Systems:
  - RFID badges
  - Track entry/exit
  - Revoke access ทันที (เมื่อพนักงานออก)

Mantraps:
  - ประตู 2 ชั้น
  - ต้อง badge ทั้ง 2 ประตู
  - ป้องกัน tailgating
```

---

## 16.3 Wireless Security (ความปลอดภัยเครือข่ายไร้สาย)

### Wireless Security Challenges

**ปัญหาของ Wireless:**

```
Wired Network:
  ✅ Signal อยู่ใน cable (ยากดัก)
  ✅ Physical access ควบคุมได้

Wireless Network:
  ❌ Signal แพร่ออกไปทางอากาศ
  ❌ ใครก็ได้สามารถดักจับ (ในระยะ)
  ❌ ยากควบคุม physical access
```

---

### Wireless Encryption Standards

#### 1. WEP (Wired Equivalent Privacy)

**คำจำกัดความ:**

- Wireless encryption แบบเก่า
- **อย่าใช้!** (ไม่ปลอดภัย)

**ปัญหา:**

```
❌ Weak encryption (RC4, 64/128-bit)
❌ Crack ได้ใน 1-5 นาที (tools: Aircrack-ng)
❌ Initialization Vector (IV) ซ้ำ
❌ Outdated (1999)

Status: **DEPRECATED**
```

---

#### 2. WPA (Wi-Fi Protected Access)

**คำจำกัดความ:**

- แทน WEP (2003)
- ดีกว่า WEP แต่ยังมีปัญหา

**คุณสมบัติ:**

```
✅ TKIP (Temporal Key Integrity Protocol)
✅ Dynamic keys (เปลี่ยนบ่อย)
✅ Message Integrity Check (MIC)

ข้อจำกัด:
  ❌ TKIP ยังใช้ RC4 (ไม่แข็งแรงมาก)
  ❌ Vulnerable to some attacks

Status: Better than WEP, แต่ควรใช้ WPA2/WPA3
```

---

#### 3. WPA2 (Wi-Fi Protected Access 2)

**คำจำกัดความ:**

- Standard ปัจจุบัน (2004)
- **แนะนำให้ใช้** (minimum)

**คุณสมบัติ:**

```
✅ AES (Advanced Encryption Standard)
✅ CCMP (Counter Mode CBC-MAC Protocol)
✅ Strong encryption (128/256-bit)
✅ Enterprise mode (802.1X authentication)

Modes:
  WPA2-Personal (PSK):
    - Pre-Shared Key (password เดียวทุกคน)
    - เหมาะสำหรับ home/small office
  
  WPA2-Enterprise:
    - RADIUS server authentication
    - แต่ละ user มี credentials แยก
    - เหมาะสำหรับ organizations
```

**Vulnerabilities:**

```
❌ KRACK attack (2017)
   - Key Reinstallation Attack
   - Exploit 4-way handshake
   - แก้ไขด้วย patches แล้ว

❌ Dictionary attacks on weak PSKs
   - Attacker capture handshake
   - Brute force password offline
   - ป้องกัน: ใช้ strong password
```

---

#### 4. WPA3 (Wi-Fi Protected Access 3)

**คำจำกัดความ:**

- Latest standard (2018)
- **ดีที่สุด** (ใช้ถ้าทำได้)

**ปรับปรุง:**

```
✅ SAE (Simultaneous Authentication of Equals)
   - แทน PSK
   - ป้องกัน offline dictionary attacks

✅ Forward Secrecy
   - Session keys แยกจากกัน
   - Compromise 1 session ≠ ทุก sessions

✅ 192-bit encryption (WPA3-Enterprise)

✅ Easier configuration (Wi-Fi Easy Connect)

✅ ป้องกัน KRACK attacks

✅ Protected Management Frames (PMF)
   - ป้องกัน deauthentication attacks
```

**Modes:**

```
WPA3-Personal:
  - SAE (Dragonfly handshake)
  - Strong password ยังสำคัญ แต่ปลอดภัยกว่า

WPA3-Enterprise:
  - 192-bit encryption
  - RADIUS authentication
  - สูงสุดของความปลอดภัย
```

---

### Wireless Security Best Practices

**Configuration:**

```
✅ ใช้ WPA3 (ถ้าได้) หรือ WPA2 (minimum)
✅ Strong passphrase (15+ characters)
✅ เปลี่ยน default SSID
   - อย่าเปิดเผยข้อมูล (e.g., "Linksys", "NETGEAR")
   - ใช้ชื่อที่ไม่เกี่ยวกับองค์กร

✅ ปิด SSID broadcast (optional, security by obscurity)
✅ เปลี่ยน default admin password
✅ Disable WPS (Wi-Fi Protected Setup)
   - มี PIN brute force vulnerability

✅ Enable MAC filtering (layer เพิ่มเติม)
   - อนุญาตเฉพาะ authorized MAC addresses
   - ข้อจำกัด: MAC spoofing ทำได้

✅ Reduce transmit power (ลด coverage area)
✅ Disable remote management
✅ Firmware updates
```

---

**Network Segmentation:**

```
แยก wireless network:

Guest Network:
  - SSID: "CompanyGuest"
  - Isolated จาก internal network
  - Internet access only

Employee Network:
  - SSID: "CompanyEmployee"
  - WPA3-Enterprise (802.1X)
  - Access to internal resources

IoT Network:
  - SSID: "CompanyIoT"
  - แยกจาก main networks
  - Restricted access
```

---

### Wireless Attacks

#### 1. Eavesdropping (Packet Sniffing)

**คำจำกัดความ:**

- ดักจับ wireless traffic

**การทำงาน:**

```
Attacker ใช้ wireless adapter (monitor mode)
Capture packets จากอากาศ
ถ้า unencrypted → อ่านได้ทันที
ถ้า encrypted (WEP/weak WPA) → crack ได้

Tools: Wireshark, Aircrack-ng

ป้องกัน:
  ✅ WPA2/WPA3 encryption
  ✅ HTTPS/VPN (encrypt application layer)
```

---

#### 2. Rogue Access Point

**คำจำกัดความ:**

- Unauthorized AP ที่ต่อเข้า network

**Scenarios:**

**a) Employee-Installed:**

```
Employee เสียบ wireless router โดยไม่ได้รับอนุญาต
  Reason: Wi-Fi signal ไม่ถึง
  
ปัญหา:
  ❌ Bypass security controls
  ❌ Unmanaged, unsecured
  ❌ Entry point สำหรับ attackers
```

**b) Attacker-Installed:**

```
Attacker เสียบ rogue AP เพื่อเข้าถึง network

ป้องกัน:
  ✅ Wireless IDS/IPS (ตรวจจับ unauthorized APs)
  ✅ Port security (ป้องกันต่อ unauthorized devices)
  ✅ Physical security
  ✅ Network Access Control (NAC)
```

---

#### 3. Evil Twin Attack

**คำจำกัดความ:**

- Attacker สร้าง fake AP ที่ดู legitimate

**การทำงาน:**

```
1. Attacker สร้าง AP:
   SSID: "Starbucks_WiFi" (เหมือนของจริง)
   Stronger signal กว่า

2. Victims connect ไปยัง fake AP

3. Attacker = Man-in-the-Middle:
   Victim ← Attacker ← Internet
   
4. Attacker:
   - ดู/แก้ไข traffic
   - Redirect to phishing pages
   - ขโมย credentials

ป้องกัน:
  ✅ ระวัง public Wi-Fi
  ✅ ตรวจสอบ certificate warnings
  ✅ ใช้ VPN
  ✅ HTTPS (อย่างน้อย)
```

---

#### 4. Deauthentication Attack

**คำจำกัดความ:**

- บังคับให้ clients disconnect จาก AP

**การทำงาน:**

```
1. Attacker ส่ง deauthentication frames (spoofed)
2. Clients disconnect
3. Uses:
   - DoS (ทำให้ใช้งานไม่ได้)
   - บังคับ reconnect (capture handshake สำหรับ crack)
   - บังคับ connect to evil twin

Tool: aireplay-ng

ป้องกัน:
  ✅ WPA3 (Protected Management Frames)
  ✅ Wireless IDS (ตรวจจับ deauth floods)
```

---

## Summary (สรุป Module 16)

Module 16 นี้เราได้เรียนรู้:

### 16.1 Security Threats and Vulnerabilities

- **CIA Triad**: Confidentiality, Integrity, Availability
- **Threats**: Information theft, Data loss, Identity theft, DoS
- **Vulnerabilities**: Technology, Configuration, Policy
- **Malware**: Virus, Worm, Trojan, Spyware, Adware, Ransomware, Rootkit
- **Attacks**: Reconnaissance, Access (password, spoofing, MitM), DoS/DDoS, Social Engineering

### 16.2 Security Best Practices

- **Defense in Depth**: หลายชั้นของการป้องกัน
- **Security Devices**: Firewall, IDS/IPS, VPN, Antivirus
- **Password Security**: Strong passwords, MFA
- **Backup**: 3-2-1 rule, Full/Incremental/Differential
- **Updates**: Patch management
- **Physical Security**: Access control, Surveillance

### 16.3 Wireless Security

- **Encryption**: WEP (อย่าใช้), WPA (เลิกใช้), WPA2 (minimum), WPA3 (ดีที่สุด)
- **Best Practices**: Strong passwords, Hide SSID, Disable WPS, Network segmentation
- **Attacks**: Eavesdropping, Rogue AP, Evil Twin, Deauthentication

---

**สิ่งสำคัญที่ต้องจำ:**

- **CIA Triad** = C (Confidentiality), I (Integrity), A (Availability)
- **Malware**: Virus (attach to files), Worm (self-replicate), Trojan (disguise)
- **Social Engineering**: Phishing, Spear Phishing, Pretexting, Baiting, Tailgating
- **DoS vs DDoS**: DoS = single source, DDoS = multiple sources (botnet)
- **Defense in Depth**: ไม่พึ่งพา single layer
- **Firewall types**: Packet filtering → Stateful → Application layer
- **IDS vs IPS**: IDS = detect only, IPS = detect + prevent
- **MFA**: Something you know + have + are
- **3-2-1 Backup**: 3 copies, 2 media types, 1 offsite
- **Wireless**: WPA3 > WPA2 > WPA > WEP (อย่าใช้)
- **WPA2 modes**: Personal (PSK) vs Enterprise (802.1X)

---

**[ไฟล์ Module 16 - Network Security Fundamentals สมบูรณ์แล้ว!]**

**CCNA Course 1 Progress:**

- ✅ Module 14: Transport Layer
- ✅ Module 15: Application Layer
- ✅ Module 16: Network Security Fundamentals

Module 16 เป็น module สุดท้ายของ CCNA Course 1! 🎉 ถ้าต้องการ review หรือทำแบบฝึกหัดเพิ่มเติม บอกได้เลยครับ!