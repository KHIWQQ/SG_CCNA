# CCNA Course 1 - Module 15: Application Layer

## ชั้นแอปพลิเคชัน

---

## 15.1 Application Layer (ภาพรวม)

### Application Layer Overview

**คำจำกัดความ:**

- **ชั้นที่ 7** ของ OSI Model
- ชั้น**บนสุด** ที่ใกล้ user ที่สุด
- Interface ระหว่าง **network** และ **applications**
- ให้บริการเครือข่ายกับ **user applications**

**PDU (Protocol Data Unit):** **Data**

---

### OSI Model Review

```
Layer 7: Application    ← เราอยู่ที่นี่
Layer 6: Presentation
Layer 5: Session
Layer 4: Transport
Layer 3: Network
Layer 2: Data Link
Layer 1: Physical
```

**TCP/IP Model:**

```
Application Layer (OSI 7,6,5) ← Application Layer
Transport Layer (OSI 4)
Internet Layer (OSI 3)
Network Access (OSI 2,1)
```

---

### Application Layer ไม่ใช่ Application!

**สิ่งสำคัญที่ต้องเข้าใจ:**

❌ **Application Layer ≠ Application Program**

**Application Layer = Protocols และ Services**

```
User Application (เช่น Chrome, Outlook)
         |
         | ใช้
         ↓
Application Layer Protocols (HTTP, SMTP, DNS)
         |
         | เรียกใช้
         ↓
Transport Layer (TCP/UDP)
```

**ตัวอย่าง:**

```
Application: Google Chrome
↓ ใช้
Application Layer Protocol: HTTP/HTTPS
↓ ใช้
Transport: TCP port 80/443
```

---

### Application Layer Functions (หน้าที่)

#### 1. Network Services to Applications

**คำจำกัดความ:**

- ให้บริการเครือข่ายแก่ applications
- แปลง user requests → network operations

**ตัวอย่าง:**

```
User: พิมพ์ "google.com" ใน browser
↓
Application Layer:
  1. DNS: แปลง "google.com" → IP address
  2. HTTP: ขอหน้าเว็บจาก server
  3. แสดงผลกลับให้ user
```

#### 2. User Interface

**คำจำกัดความ:**

- Interface สำหรับ user ติดต่อกับ network
- แสดง data ในรูปแบบที่ user เข้าใจ

**ตัวอย่าง:**

- Web browser แสดง HTML pages
- Email client แสดง emails
- FTP client แสดง file lists

#### 3. Data Formatting

**คำจำกัดความ:**

- จัดรูปแบบข้อมูลให้พร้อมส่ง
- Encoding, Encryption, Compression

**ตัวอย่าง:**

```
Raw data → JPEG compression → Encrypted → Send
```

---

### Application Layer Protocols

**โปรโตคอลหลักๆ:**

```
Protocol  Port   TCP/UDP  Purpose
------------------------------------------------------------------------
HTTP      80     TCP      Web (Hypertext Transfer)
HTTPS     443    TCP      Secure Web (HTTP + TLS/SSL)
FTP       20,21  TCP      File Transfer
TFTP      69     UDP      Trivial File Transfer
SMTP      25     TCP      Email Sending
POP3      110    TCP      Email Receiving
IMAP      143    TCP      Email Receiving (advanced)
DNS       53     TCP/UDP  Domain Name Resolution
DHCP      67,68  UDP      IP Address Assignment
Telnet    23     TCP      Remote Terminal (insecure)
SSH       22     TCP      Secure Remote Terminal
SNMP      161    UDP      Network Management
NTP       123    UDP      Time Synchronization
------------------------------------------------------------------------
```

---

### Client-Server Model

**คำจำกัดความ:**

- **Client** = ขอบริการ (requests)
- **Server** = ให้บริการ (responds)

**การทำงาน:**

#### Client (ผู้ขอ)

- **เริ่มต้น** communication
- ส่ง **requests**
- รอรับ **responses**
- มักเป็น **end user devices**

#### Server (ผู้ให้)

- **รอรับ** requests
- **ประมวลผล** requests
- ส่ง **responses** กลับ
- **Always on** (พร้อมให้บริการตลอด)

**ตัวอย่าง:**

```
Web Browsing:

Client (Your PC)              Server (google.com)
    |                                |
    |------ HTTP GET Request ------->|
    |     "ขอหน้าเว็บหน่อย"           |
    |                                |
    |<----- HTTP Response -----------|
    |     "นี่ไงหน้าเว็บ (HTML)"       |
    |                                |

Client: Web Browser (Chrome)
Server: Web Server (Apache/Nginx)
Protocol: HTTP
Port: 80
```

---

### Peer-to-Peer (P2P) Model

**คำจำกัดความ:**

- แต่ละ device เป็นทั้ง **client และ server**
- ไม่มี dedicated server
- Resources แบ่งปันกันโดยตรง

**ข้อดี:**

- ✅ ไม่ต้องการ dedicated server (ประหยัด)
- ✅ Scalable (peers เพิ่ม = resources เพิ่ม)
- ✅ Decentralized (ไม่มี single point of failure)

**ข้อเสีย:**

- ❌ Performance ไม่สม่ำเสมอ (peers on/off)
- ❌ Security ยาก (ไม่มี central control)
- ❌ จัดการยาก

**ตัวอย่าง:**

```
P2P File Sharing (BitTorrent):

Peer A ────┐
           ├──→ File pieces
Peer B ────┤
           ├──→ Exchange
Peer C ────┤
           └──→ Distributed

แต่ละ peer:
  - Download from others (client)
  - Upload to others (server)
```

**Use Cases:**

- File sharing (BitTorrent)
- Cryptocurrency (Bitcoin)
- Video calls (Skype)
- Gaming (some online games)

---

## 15.2 Web and Email Services

### HTTP and HTTPS

#### HTTP (Hypertext Transfer Protocol)

**คำจำกัดความ:**

- โปรโตคอลสำหรับ **web communication**
- ส่ง **web pages, images, videos**
- **Port 80, TCP**

**คุณสมบัติ:**

- **Stateless** - แต่ละ request เป็นอิสระ (ไม่จำ previous requests)
- **Text-based** - commands เป็น text
- **Request-Response** model

**HTTP Request Methods:**

```
GET     - ขอข้อมูล (read)
POST    - ส่งข้อมูล (create)
PUT     - แก้ไขข้อมูล (update)
DELETE  - ลบข้อมูล (delete)
HEAD    - ขอ header เท่านั้น (ไม่เอา body)
```

**ตัวอย่าง HTTP Request:**

```http
GET /index.html HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0
Accept: text/html
Connection: keep-alive

```

**ตัวอย่าง HTTP Response:**

```http
HTTP/1.1 200 OK
Date: Tue, 18 Nov 2025 10:00:00 GMT
Server: Apache/2.4
Content-Type: text/html
Content-Length: 1234

<!DOCTYPE html>
<html>
<body>
<h1>Hello World!</h1>
</body>
</html>
```

**HTTP Status Codes:**

```
1xx - Information
  100 Continue

2xx - Success
  200 OK
  201 Created
  204 No Content

3xx - Redirection
  301 Moved Permanently
  302 Found (Temporary Redirect)
  304 Not Modified

4xx - Client Error
  400 Bad Request
  401 Unauthorized
  403 Forbidden
  404 Not Found
  429 Too Many Requests

5xx - Server Error
  500 Internal Server Error
  502 Bad Gateway
  503 Service Unavailable
  504 Gateway Timeout
```

---

#### HTTPS (HTTP Secure)

**คำจำกัดความ:**

- HTTP + **TLS/SSL** encryption
- **Secure** web communication
- **Port 443, TCP**

**คุณสมบัติ:**

- ✅ **Encryption** - ข้อมูล encrypted (ไม่สามารถอ่านได้)
- ✅ **Authentication** - ยืนยันตัวตน server (SSL Certificate)
- ✅ **Integrity** - ข้อมูลไม่ถูกแก้ไขระหว่างทาง

**HTTPS vs HTTP:**

```
HTTP (Port 80):
  Client <--- Plain Text ---> Server
         ❌ Anyone can read

HTTPS (Port 443):
  Client <--- Encrypted ---> Server
         ✅ Only Client & Server can decrypt
```

**SSL/TLS Handshake:**

```
1. Client → Server: ClientHello (supported ciphers)
2. Server → Client: ServerHello (chosen cipher)
3. Server → Client: Certificate (public key)
4. Client: Verify certificate
5. Client → Server: Encrypted session key
6. Encrypted communication established
```

**SSL Certificate:**

- ออกโดย **Certificate Authority (CA)**
- ยืนยันว่า server เป็นของจริง
- มี **public key** สำหรับ encryption

**ตัวอย่าง:**

```
Visit: https://www.google.com

1. Browser ขอ SSL certificate
2. Server ส่ง certificate (signed by CA)
3. Browser verify certificate:
   - Issued by trusted CA? ✓
   - Domain match? ✓
   - Not expired? ✓
4. Browser แสดง 🔒 (padlock icon)
5. Encrypted communication
```

---

### Email Protocols

#### SMTP (Simple Mail Transfer Protocol)

**คำจำกัดความ:**

- โปรโตคอลสำหรับ**ส่ง email**
- **Port 25, TCP** (หรือ 587 สำหรับ submission)
- **Push protocol** (client → server)

**การทำงาน:**

**Sending Email:**

```
Step 1: User เขียน email
Step 2: Email client → SMTP server (ผู้ส่ง)
Step 3: SMTP server (ผู้ส่ง) → SMTP server (ผู้รับ)
Step 4: Email stored in mailbox (ผู้รับ)
```

**ตัวอย่าง:**

```
alice@company.com → bob@example.com

1. Alice เขียน email ใน Outlook
2. Outlook (SMTP client) → company.com SMTP server
3. company.com SMTP → example.com SMTP server
4. example.com SMTP → Bob's mailbox
5. Bob ใช้ POP3/IMAP ดึง email
```

**SMTP Commands:**

```
HELO   - ทักทาย server
MAIL   - ระบุผู้ส่ง
RCPT   - ระบุผู้รับ
DATA   - ส่งเนื้อหา email
QUIT   - จบ session
```

**ตัวอย่าง SMTP Session:**

```
Client: HELO client.example.com
Server: 250 Hello client.example.com

Client: MAIL FROM:<alice@company.com>
Server: 250 OK

Client: RCPT TO:<bob@example.com>
Server: 250 OK

Client: DATA
Server: 354 Start mail input

Client: Subject: Hello
Client: 
Client: Hi Bob!
Client: .
Server: 250 OK

Client: QUIT
Server: 221 Bye
```

---

#### POP3 (Post Office Protocol 3)

**คำจำกัดความ:**

- โปรโตคอลสำหรับ**รับ email**
- **Port 110, TCP**
- **Download-and-delete** model

**คุณสมบัติ:**

- **Download** emails จาก server → client
- **Delete** from server (default)
- **Offline access** (emails อยู่ใน client)
- **Simple** protocol

**การทำงาน:**

```
1. Client connect to POP3 server
2. Authenticate (username + password)
3. Download all emails
4. Delete emails from server (optional)
5. Disconnect
```

**ข้อดี:**

- ✅ Simple
- ✅ Offline access
- ✅ Server storage ไม่เต็ม (ลบแล้ว)

**ข้อเสีย:**

- ❌ Email อยู่ใน device เดียว (sync ไม่ได้)
- ❌ เปลี่ยน device = เห็น email ไม่ครบ
- ❌ ไม่มี folder structure on server

**POP3 Commands:**

```
USER   - Username
PASS   - Password
STAT   - Status (จำนวน emails)
LIST   - รายการ emails
RETR   - ดึง email
DELE   - ลบ email
QUIT   - ออก
```

---

#### IMAP (Internet Message Access Protocol)

**คำจำกัดความ:**

- โปรโตคอลสำหรับ**รับ email** (advanced)
- **Port 143, TCP** (993 สำหรับ IMAPS)
- **Keep-on-server** model

**คุณสมบัติ:**

- **Synchronization** - emails อยู่บน server
- **Multiple devices** - access จาก device ไหนก็ได้
- **Folder management** - จัดการ folders on server
- **Partial download** - ดึงเฉพาะส่วนที่ต้องการ
- **Online and offline** modes

**การทำงาน:**

```
1. Client connect to IMAP server
2. Authenticate
3. View email list (headers only)
4. Select email → download เฉพาะ email นั้น
5. การเปลี่ยนแปลง (read, delete, move) sync กับ server
6. Disconnect (emails ยังอยู่บน server)
```

**ข้อดี:**

- ✅ **Sync หลาย devices**
- ✅ **Server-side folders**
- ✅ **Partial download** (เร็ว, ประหยัด bandwidth)
- ✅ **Backup on server**

**ข้อเสีย:**

- ❌ ใช้ server storage มาก
- ❌ Complex กว่า POP3
- ❌ ต้อง online เพื่อ access (บางครั้ง)

**IMAP Commands:**

```
LOGIN     - Login
SELECT    - เลือก mailbox/folder
FETCH     - ดึง email
STORE     - แก้ไข flags (read, deleted)
SEARCH    - ค้นหา emails
CREATE    - สร้าง folder
DELETE    - ลบ folder
LOGOUT    - ออก
```

---

### POP3 vs IMAP Comparison

```
Feature              POP3                    IMAP
------------------------------------------------------------------------
Email Location       Client (downloaded)     Server (synced)
Multiple Devices     ❌ ไม่ sync              ✅ Sync ทุก devices
Offline Access       ✅ Full                 ✅ Partial (cached)
Server Storage       ✅ ว่าง (ลบแล้ว)        ❌ เต็ม (เก็บไว้)
Folder Management    ❌ Client-side only     ✅ Server-side
Download             ✅ Full email           ✅ Selective
Bandwidth            ❌ ดาวน์โหลดทั้งหมด      ✅ ดาวน์โหลดตามต้องการ
Complexity           ✅ Simple               ❌ Complex
Use Case             Single device           Multiple devices
Port                 110 (143 secure)        143 (993 secure)
------------------------------------------------------------------------
```

**เมื่อไหร่ใช้ POP3:**

- ✅ ใช้ device เดียว
- ✅ ต้องการ offline access
- ✅ Server storage จำกัด
- ✅ ต้องการความเรียบง่าย

**เมื่อไหร่ใช้ IMAP:**

- ✅ ใช้หลาย devices (PC, phone, tablet)
- ✅ ต้องการ sync
- ✅ ต้องการจัดการ folders on server
- ✅ Server storage เพียงพอ

---

### Email Flow Example

**Scenario:** Alice (alice@company.com) ส่ง email → Bob (bob@example.com)

```
Step 1: Alice เขียน email
   Email Client (Outlook)

Step 2: SMTP (Alice → company.com)
   Outlook --SMTP--> company.com Mail Server
   Port: 587 (or 25)

Step 3: SMTP (company.com → example.com)
   company.com Mail Server --SMTP--> example.com Mail Server
   Port: 25

Step 4: Email stored
   example.com Mail Server → Bob's Mailbox

Step 5: Bob ดึง email (IMAP)
   Bob's iPhone --IMAP--> example.com Mail Server
   Port: 993 (IMAPS)

Step 6: Bob อ่าน email
   iPhone Mail App แสดงผล
```

**Protocols ที่ใช้:**

```
Alice → company.com:  SMTP (587)
company.com → example.com:  SMTP (25)
example.com → Bob:  IMAP (993)
```

---

## 15.3 IP Addressing Services

### DNS (Domain Name System)

**คำจำกัดความ:**

- แปลง **Domain Names** → **IP Addresses**
- "Phone book of the Internet"
- **Port 53, UDP (TCP สำหรับ zone transfers)**

**ทำไมต้องมี DNS:**

```
Human-friendly:  www.google.com  ← จำง่าย
Computer-friendly:  142.250.185.46  ← ยากจำ

DNS แปลง:  www.google.com → 142.250.185.46
```

---

#### DNS Hierarchy (ลำดับชั้น)

**โครงสร้างแบบ hierarchical:**

```
                          Root (.)
                             |
        ┌────────────────────┼────────────────────┐
       com                  org                  net
        |                    |                    |
    ┌───┴───┐            ┌───┴───┐           ┌───┴───┐
  google  amazon        wikipedia           cloudflare
    |                                            |
    www                                          www
```

**ระดับของ DNS:**

#### 1. Root Level (.)

- **Root servers** (13 clusters ทั่วโลก)
- รู้จัก **TLD servers** ทั้งหมด

#### 2. Top-Level Domain (TLD)

```
Generic TLDs (gTLDs):
  .com  - Commercial
  .org  - Organization
  .net  - Network
  .edu  - Education
  .gov  - Government
  .mil  - Military

Country Code TLDs (ccTLDs):
  .th   - Thailand
  .us   - United States
  .uk   - United Kingdom
  .jp   - Japan
```

#### 3. Second-Level Domain

```
google.com
amazon.com
wikipedia.org

← จดทะเบียนโดย organizations/individuals
```

#### 4. Subdomain

```
www.google.com
mail.google.com
drive.google.com

← สร้างโดย domain owner
```

---

#### Fully Qualified Domain Name (FQDN)

**คำจำกัดความ:**

- Domain name **ที่สมบูรณ์**
- ระบุ location แบบเฉพาะเจาะจง

**รูปแบบ:**

```
[subdomain].[domain].[TLD].[root]

ตัวอย่าง:
www.google.com.  ← FQDN (มี . ท้าย = root)
www.google.com   ← ปกติเราละ . ท้าย

mail.google.com.
drive.google.com.
docs.google.com.
```

---

#### DNS Resolution Process

**คำจำกัดความ:**

- กระบวนการแปลง domain name → IP address

**แบบละเอียด:**

**Scenario:** Computer ต้องการ IP ของ www.google.com

```
Step 1: Check Local Cache
   Computer: มี cache ของ www.google.com ไหม?
   ถ้ามี → ใช้เลย, จบ
   ถ้าไม่มี → ถาม DNS Resolver

Step 2: DNS Resolver (ISP)
   Computer → DNS Resolver: "www.google.com = IP ไหน?"
   Resolver: ตรวจ cache
   ถ้ามี → ตอบกลับ, จบ
   ถ้าไม่มี → ถาม Root Server

Step 3: Root Server
   Resolver → Root Server: "www.google.com อยู่ไหน?"
   Root Server: "ฉันไม่รู้ IP, แต่ .com อยู่ที่ TLD Server นี้"
   Root → Resolver: TLD Server IP (a.gtld-servers.net)

Step 4: TLD Server (.com)
   Resolver → TLD Server: "www.google.com อยู่ไหน?"
   TLD Server: "google.com อยู่ที่ Authoritative Server นี้"
   TLD → Resolver: Authoritative Server IP (ns1.google.com)

Step 5: Authoritative Server (google.com)
   Resolver → Authoritative Server: "www.google.com อยู่ไหน?"
   Authoritative: "www.google.com = 142.250.185.46"
   Authoritative → Resolver: 142.250.185.46

Step 6: Return to Client
   Resolver → Computer: "www.google.com = 142.250.185.46"

Step 7: Computer connects
   Computer → 142.250.185.46 (Google Server)
```

**แบบง่าย (Recursive Query):**

```
Computer → DNS Resolver: "www.google.com?"
         (Resolver ทำ Steps 3-5 เอง)
Computer ← DNS Resolver: "142.250.185.46"
```

---

#### DNS Record Types

**คำจำกัดความ:**

- DNS records เก็บข้อมูลต่างๆ ใน DNS database

**ประเภท Records:**

#### A Record (Address)

```
คำจำกัดความ: แปลง hostname → IPv4 address

ตัวอย่าง:
www.example.com.  IN  A  93.184.216.34
```

#### AAAA Record (IPv6 Address)

```
คำจำกัดความ: แปลง hostname → IPv6 address

ตัวอย่าง:
www.example.com.  IN  AAAA  2606:2800:220:1:248:1893:25c8:1946
```

#### CNAME Record (Canonical Name)

```
คำจำกัดความ: Alias (ชื่อเล่น) → ชื่อจริง

ตัวอย่าง:
mail.example.com.  IN  CNAME  mailserver.example.com.
www.example.com.   IN  CNAME  example.com.

เมื่อ query mail.example.com:
  → resolve CNAME → mailserver.example.com
  → resolve A → 93.184.216.34
```

#### MX Record (Mail Exchange)

```
คำจำกัดความ: Mail server สำหรับ domain

ตัวอย่าง:
example.com.  IN  MX  10  mail1.example.com.
example.com.  IN  MX  20  mail2.example.com.

Priority: 10 < 20 (เลขต่ำ = priority สูง)
```

#### NS Record (Name Server)

```
คำจำกัดความ: Authoritative DNS servers สำหรับ domain

ตัวอย่าง:
example.com.  IN  NS  ns1.example.com.
example.com.  IN  NS  ns2.example.com.
```

#### PTR Record (Pointer)

```
คำจำกัดความ: Reverse DNS (IP → hostname)

ตัวอย่าง:
34.216.184.93.in-addr.arpa.  IN  PTR  www.example.com.

ใช้สำหรับ: verify email servers, security checks
```

#### SOA Record (Start of Authority)

```
คำจำกัดความ: ข้อมูล administrative สำหรับ zone

ตัวอย่าง:
example.com.  IN  SOA  ns1.example.com. admin.example.com. (
    2025111801  ; Serial
    3600        ; Refresh (1 hour)
    600         ; Retry (10 minutes)
    86400       ; Expire (1 day)
    3600        ; Minimum TTL (1 hour)
)
```

#### TXT Record (Text)

```
คำจำกัดความ: ข้อมูล text ทั่วไป

ใช้สำหรับ:
  - SPF (Sender Policy Framework) สำหรับ email
  - DKIM (DomainKeys Identified Mail)
  - Domain verification

ตัวอย่าง:
example.com.  IN  TXT  "v=spf1 include:_spf.google.com ~all"
```

---

#### DNS Caching

**คำจำกัดความ:**

- เก็บ DNS responses ไว้ชั่วคราว
- ลด DNS queries, เร็วขึ้น

**ระดับของ Cache:**

#### 1. Browser Cache

```
Chrome, Firefox เก็บ cache เอง
ระยะเวลา: 60 seconds (โดยปกติ)
```

#### 2. Operating System Cache

```
Windows: DNS Client service
Linux: systemd-resolved, nscd

ดู cache:
  Windows: ipconfig /displaydns
  Linux: systemd-resolve --statistics
```

#### 3. DNS Resolver Cache

```
ISP DNS Resolver เก็บ cache
ระยะเวลา: ตาม TTL (Time To Live)
```

**TTL (Time To Live):**

```
คำจำกัดความ: ระยะเวลาที่ cache record ใช้ได้

ตัวอย่าง:
www.example.com.  300  IN  A  93.184.216.34
                  ^^^
                  TTL = 300 seconds (5 minutes)

หมายความ: Cache record นี้ได้ 5 นาที แล้วต้อง refresh
```

**ล้าง Cache:**

```
Windows: ipconfig /flushdns
Linux: systemd-resolve --flush-caches
      หรือ sudo service nscd restart
macOS: sudo dscacheutil -flushcache
```

---

#### nslookup Command

**คำจำกัดความ:**

- เครื่องมือ query DNS records

**ใช้งาน:**

**Query A record:**

```cmd
nslookup www.google.com

Output:
Server:  8.8.8.8
Address:  8.8.8.8

Non-authoritative answer:
Name:    www.google.com
Addresses:  142.250.185.46
```

**Query specific record type:**

```cmd
nslookup -type=MX google.com

Output:
google.com  MX preference = 10, mail exchanger = smtp.google.com
```

**Query specific DNS server:**

```cmd
nslookup www.google.com 1.1.1.1

(Query ไปที่ 1.1.1.1 - Cloudflare DNS)
```

**Record types:**

```
-type=A       IPv4 address
-type=AAAA    IPv6 address
-type=MX      Mail servers
-type=NS      Name servers
-type=CNAME   Canonical name
-type=TXT     Text records
-type=PTR     Reverse lookup
-type=SOA     Start of Authority
```

---

#### ipconfig /displaydns (Windows)

**ดู DNS cache:**

```cmd
ipconfig /displaydns

Output:
www.google.com
----------------------------------------
Record Name . . . . . : www.google.com
Record Type . . . . . : 1 (A)
Time To Live  . . . . : 299
Data Length . . . . . : 4
Section . . . . . . . : Answer
A (Host) Record . . . : 142.250.185.46
```

**ล้าง DNS cache:**

```cmd
ipconfig /flushdns

Output:
Successfully flushed the DNS Resolver Cache.
```

---

### DHCP (Dynamic Host Configuration Protocol)

**คำจำกัดความ:**

- จัดสรร **IP addresses** อัตโนมัติ
- **Port 67 (server), 68 (client), UDP**
- ให้บริการ: IP, Subnet Mask, Default Gateway, DNS

**ทำไมต้องมี DHCP:**

```
ไม่มี DHCP:
  ❌ Manual configuration (ตั้ง IP เอง)
  ❌ เสียเวลา, ผิดพลาดง่าย
  ❌ IP conflicts (IP ซ้ำกัน)

มี DHCP:
  ✅ Automatic configuration
  ✅ เร็ว, ไม่ผิดพลาด
  ✅ Centralized management
  ✅ ไม่มี IP conflicts
```

---

#### DHCP Components

#### 1. DHCP Server

- จัดสรร IP addresses
- จัดการ IP pool
- Track leases (IP ใช้โดยใคร, นานแค่ไหน)

#### 2. DHCP Client

- Request IP address จาก DHCP server
- ใช้ IP ที่ได้รับ

#### 3. DHCP Relay Agent

- Forward DHCP messages ข้าม subnets
- ใช้เมื่อ DHCP server อยู่คนละ subnet กับ client

---

#### DHCP Address Pool

**คำจำกัดความ:**

- ช่วง IP addresses ที่ DHCP server จัดสรรได้

**ตัวอย่าง:**

```
Network: 192.168.1.0/24

IP Pool: 192.168.1.100 - 192.168.1.200
  → DHCP จัดสรร IP จาก range นี้

Reserved (ไม่อยู่ใน pool):
  192.168.1.1       - Default Gateway (Router)
  192.168.1.2       - DNS Server
  192.168.1.10-50   - Static IPs (Servers, Printers)
  192.168.1.201-254 - Reserved for future
```

---

#### DHCP Lease

**คำจำกัดความ:**

- ระยะเวลาที่ client ใช้ IP address ได้
- หมดเวลา → ต้อง **renew** หรือ **release**

**กระบวนการ Lease:**

**1. Lease Time:**

```
Default: 24 hours (ขึ้นอยู่กับ configuration)

Client ได้ IP: 192.168.1.100
Lease: 24 hours

หลังจาก 12 hours (50%):
  → Client พยายาม renew (ขอต่ออายุ)

หลังจาก 21 hours (87.5%):
  → ถ้ายัง renew ไม่สำเร็จ → พยายามหา server ใหม่

หลังจาก 24 hours:
  → Lease หมดอายุ
  → Client ต้อง release IP และ request ใหม่
```

**2. Lease Renewal:**

```
Client → Server: DHCPREQUEST (ขอต่ออายุ IP เดิม)
Server → Client: DHCPACK (ตกลง, ต่ออายุให้)

หรือ

Server → Client: DHCPNAK (ปฏิเสธ, ขอใหม่เถอะ)
```

**3. Lease Release:**

```
Client ไม่ใช้แล้ว:
  Client → Server: DHCPRELEASE
  Server คืน IP ไปยัง pool
```

---

#### DHCP DORA Process

**คำจำกัดความ:**

- กระบวนการขอ IP address จาก DHCP
- **4 steps: Discover, Offer, Request, Acknowledge**

**การทำงานละเอียด:**

#### Step 1: DHCP Discover (Client → Broadcast)

```
Client (ไม่มี IP):
  Source: 0.0.0.0
  Destination: 255.255.255.255 (Broadcast)
  Message: "มี DHCP server ไหมจ๊ะ? ขอ IP หน่อย"

Broadcast ไปทั้ง subnet
DHCP servers ทุกตัวจะได้รับ
```

#### Step 2: DHCP Offer (Server → Broadcast/Unicast)

```
DHCP Server:
  Source: 192.168.1.1 (DHCP Server)
  Destination: 255.255.255.255 (Broadcast) หรือ Client MAC
  Message: "มีค่ะ! เอา IP 192.168.1.100 นี้ไปใช้ไหม?"
  
Offer ประกอบด้วย:
  - IP address: 192.168.1.100
  - Subnet mask: 255.255.255.0
  - Default gateway: 192.168.1.1
  - DNS server: 8.8.8.8
  - Lease time: 86400 seconds (24 hours)
```

#### Step 3: DHCP Request (Client → Broadcast)

```
Client:
  Source: 0.0.0.0 (ยังไม่ได้ใช้ IP)
  Destination: 255.255.255.255 (Broadcast)
  Message: "ขอ IP 192.168.1.100 จาก server 192.168.1.1 นะคะ"

Broadcast เพราะ:
  - บอก server อื่นว่าเลือก server นี้แล้ว
  - Server อื่น release IP ที่ offer ไว้
```

#### Step 4: DHCP Acknowledge (Server → Broadcast/Unicast)

```
DHCP Server:
  Source: 192.168.1.1
  Destination: 192.168.1.100 (Unicast) หรือ Broadcast
  Message: "ได้เลย! ใช้ IP 192.168.1.100 ได้แล้ว"

ACK ประกอบด้วย:
  - Confirmation ของ IP settings
  - Lease time เริ่มนับ
```

**ผลลัพธ์:**

- ✅ Client ได้ IP: 192.168.1.100
- ✅ Client configure network settings
- ✅ Client สามารถ communicate ได้

---

#### DHCP DORA Diagram

```
Client (No IP)                    DHCP Server (192.168.1.1)
      |                                    |
      |--- DHCP Discover (Broadcast) ---->|
      |    "มี DHCP server ไหม?"           |
      |                                    |
      |<--- DHCP Offer (Broadcast) -------|
      |    "เอา 192.168.1.100 ไหม?"        |
      |                                    |
      |--- DHCP Request (Broadcast) ------>|
      |    "ขอ 192.168.1.100"              |
      |                                    |
      |<--- DHCP Acknowledge (Unicast) ---|
      |    "ได้เลย!"                        |
      |                                    |
  [Client: 192.168.1.100]
      |
      |=== Ready to communicate ===========|
```

---

#### DHCP Relay Agent

**คำจำกัดความ:**

- Forward DHCP messages ระหว่าง subnets
- ใช้เมื่อ DHCP server อยู่คนละ subnet กับ client

**ทำไมต้องมี:**

```
DHCP Discover = Broadcast (255.255.255.255)
Router ไม่ forward broadcasts

ปัญหา:
  Client (Subnet A) --- Router --- DHCP Server (Subnet B)
          ❌ Broadcast ไม่ผ่าน Router

Solution:
  Router ทำหน้าที่เป็น DHCP Relay Agent
  → Forward DHCP messages เป็น Unicast
```

**การทำงาน:**

```
Subnet A                Router (Relay)         Subnet B
Client                                      DHCP Server
  |                         |                    |
  |-- Discover (Broadcast)-->|                   |
  |                         |                    |
  |                         |-- Discover (Unicast)-->|
  |                         |   (giaddr = Router IP) |
  |                         |                    |
  |                         |<-- Offer ---------|
  |<-- Offer -------------|                    |
  |                         |                    |
  |-- Request (Broadcast)-->|                   |
  |                         |-- Request (Unicast)-->|
  |                         |                    |
  |                         |<-- ACK -----------|
  |<-- ACK --------------|                    |
  |                         |                    |

giaddr (Gateway IP Address):
  - Router ใส่ IP ของตัวเองใน DHCP message
  - DHCP Server รู้ว่า client อยู่ subnet ไหน
  - Server จัดสรร IP จาก pool ที่ถูกต้อง
```

---

#### DHCP Configuration Example (Cisco Router)

```
Router(config)# ip dhcp pool LAN-POOL
Router(dhcp-config)# network 192.168.1.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.1.1
Router(dhcp-config)# dns-server 8.8.8.8 8.8.4.4
Router(dhcp-config)# lease 7
Router(dhcp-config)# exit

Router(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.50

คำอธิบาย:
  network: ช่วง network ที่จะจัดสรร
  default-router: Default gateway
  dns-server: DNS servers
  lease: ระยะเวลา lease (7 days)
  excluded-address: IP ที่ไม่จัดสรร (reserved)
```

---

### NAT (Network Address Translation)

**คำจำกัดความ:**

- แปลง **Private IP** ↔ **Public IP**
- ใช้ที่ **Router** (edge of network)
- อนุญาตให้หลาย devices แชร์ public IP เดียว

**ทำไมต้องมี NAT:**

```
ปัญหา: IPv4 addresses หมด!

Private network: 192.168.1.0/24 (256 hosts)
Public IPs available: 1 IP (ISP ให้มา 1 IP)

❌ ไม่สามารถให้ public IP ทุก device

✅ Solution: NAT
  - Devices ใช้ private IPs
  - Router แปลง private → public เวลาออก Internet
```

---

#### Private vs Public IP Addresses

**Private IP Addresses:**

```
RFC 1918 กำหนด:

Class A: 10.0.0.0 - 10.255.255.255       (10.0.0.0/8)
Class B: 172.16.0.0 - 172.31.255.255     (172.16.0.0/12)
Class C: 192.168.0.0 - 192.168.255.255   (192.168.0.0/16)

คุณสมบัติ:
  ❌ ไม่ routable บน Internet
  ✅ ใช้ภายใน private networks
  ✅ Free (ไม่ต้องจดทะเบียน)
  ✅ ซ้ำกันได้ (networks ต่างกัน)
```

**Public IP Addresses:**

```
ที่เหลือทั้งหมด (ยกเว้น reserved ranges)

คุณสมบัติ:
  ✅ Routable บน Internet
  ✅ Unique globally (ไม่ซ้ำกัน)
  ❌ ต้องจดทะเบียน (มีค่าใช้จ่าย)
  ❌ มีจำกัด (IPv4 หมดแล้ว)
```

---

#### Types of NAT

#### 1. Static NAT (One-to-One)

**คำจำกัดความ:**

- แปลง **1 private IP** ↔ **1 public IP** (ตายตัว)
- ใช้สำหรับ **servers** ที่ต้องการ access จากภายนอก

**ตัวอย่าง:**

```
Private Network:
  Web Server: 192.168.1.10

NAT Mapping (Static):
  192.168.1.10 ↔ 203.0.113.5

Internet User → 203.0.113.5
Router แปลง → 192.168.1.10 (Web Server)
```

**ข้อดี:**

- ✅ Server accessible จาก Internet
- ✅ Consistent mapping

**ข้อเสีย:**

- ❌ ใช้ public IP 1:1 (เปลือง)

---

#### 2. Dynamic NAT (Many-to-Many Pool)

**คำจำกัดความ:**

- แปลง **private IPs** → **pool of public IPs**
- Mapping เปลี่ยนแปลงได้ (ไม่ตายตัว)

**ตัวอย่าง:**

```
Private Network:
  PC1: 192.168.1.10
  PC2: 192.168.1.11
  PC3: 192.168.1.12

Public IP Pool:
  203.0.113.5
  203.0.113.6
  203.0.113.7

NAT Mappings (Dynamic):
  PC1 (192.168.1.10) → 203.0.113.5  (ขณะนี้)
  PC2 (192.168.1.11) → 203.0.113.6  (ขณะนี้)
  
ถ้า PC1 disconnect:
  → 203.0.113.5 ว่าง
  → PC3 connect → ใช้ 203.0.113.5
```

**ข้อดี:**

- ✅ หลาย devices แชร์ public IPs
- ✅ ประหยัด public IPs กว่า Static

**ข้อเสีย:**

- ❌ ยังต้องการหลาย public IPs
- ❌ ถ้า pool เต็ม → device ใหม่ connect ไม่ได้

---

#### 3. PAT / NAT Overload (Many-to-One)

**คำจำกัดความ:**

- แปลง **หลาย private IPs** → **1 public IP**
- ใช้ **port numbers** แยก connections
- **ใช้มากที่สุด** (home routers, offices)

**การทำงาน:**

```
NAT Table:
Inside (Private)          Outside (Public)
192.168.1.10:50001  ↔  203.0.113.5:50001
192.168.1.10:50002  ↔  203.0.113.5:50002
192.168.1.11:50001  ↔  203.0.113.5:50003
192.168.1.12:51000  ↔  203.0.113.5:51000

ทุก connection ใช้ public IP เดียวกัน (203.0.113.5)
แต่แยกด้วย port numbers ต่างกัน
```

**ตัวอย่าง:**

```
PC1 (192.168.1.10) เข้า google.com:

1. PC1 → Router:
   Source: 192.168.1.10:50001
   Destination: 142.250.185.46:80

2. Router (NAT):
   - เปลี่ยน Source → 203.0.113.5:50001
   - บันทึกใน NAT table

3. Router → Internet:
   Source: 203.0.113.5:50001
   Destination: 142.250.185.46:80

4. Google → Router:
   Source: 142.250.185.46:80
   Destination: 203.0.113.5:50001

5. Router (NAT):
   - ดู NAT table: 203.0.113.5:50001 = 192.168.1.10:50001
   - เปลี่ยน Destination → 192.168.1.10:50001

6. Router → PC1:
   Source: 142.250.185.46:80
   Destination: 192.168.1.10:50001
```

**ข้อดี:**

- ✅ หลายร้อย/พัน devices ใช้ public IP เดียว
- ✅ **ประหยัด public IPs มากที่สุด**
- ✅ ใช้กันทั่วไป (home, office)

**ข้อเสีย:**

- ❌ ไม่สามารถ host servers (inbound connections ยาก)
- ❌ Port numbers หมด = ไม่สามารถ connect เพิ่ม

---

#### NAT Advantages and Disadvantages

**ข้อดี:**

- ✅ **ประหยัด public IPs** (IPv4 shortage)
- ✅ **Security** - Private IPs ซ่อน, ภายนอกไม่เห็น
- ✅ **Flexibility** - เปลี่ยน internal IPs ได้ (ภายนอกไม่เปลี่ยน)

**ข้อเสีย:**

- ❌ **End-to-end connectivity หาย** - direct connections ยาก
- ❌ **Complexity** - troubleshooting ยากขึ้น
- ❌ **Protocols ที่ embed IPs** - บาง protocols ใช้ไม่ได้ (ต้อง ALG)
- ❌ **Performance** - Router ต้อง process NAT (overhead)
- ❌ **Logging/Tracking** - ยากหา specific device (ภายนอกเห็นแค่ 1 IP)

---

#### Port Forwarding (NAT for Servers)

**คำจำกัดความ:**

- Forward **specific port** จาก public IP → private IP
- ใช้สำหรับ **host servers** เบื้องหลัง NAT

**ตัวอย่าง:**

```
Scenario:
  Internal Web Server: 192.168.1.10:80
  Public IP: 203.0.113.5

Port Forwarding Rule:
  203.0.113.5:8080 → 192.168.1.10:80

Internet User → 203.0.113.5:8080
Router forward → 192.168.1.10:80 (Web Server)

Web Server respond → 192.168.1.10:80
Router NAT → 203.0.113.5:8080 → Internet User
```

**Use Cases:**

- Web servers
- Game servers
- Remote desktop
- IP cameras
- FTP servers

---

## Summary (สรุป Module 15)

Module 15 นี้เราได้เรียนรู้:

1. ✅ **Application Layer** - Layer 7, interface กับ users
2. ✅ **Client-Server Model** - client requests, server responds
3. ✅ **P2P Model** - peers เป็นทั้ง client และ server
4. ✅ **HTTP/HTTPS**:
    - HTTP: Port 80, unencrypted
    - HTTPS: Port 443, encrypted (TLS/SSL)
5. ✅ **Email Protocols**:
    - SMTP: ส่ง email (Port 25)
    - POP3: รับ email, download-and-delete (Port 110)
    - IMAP: รับ email, sync multiple devices (Port 143)
6. ✅ **DNS**:
    - แปลง domain names → IP addresses
    - Port 53 (UDP/TCP)
    - Hierarchy: Root → TLD → Second-Level → Subdomain
    - DORA Process: Discover → Offer → Request → Acknowledge
    - Record types: A, AAAA, CNAME, MX, NS, PTR, SOA, TXT
7. ✅ **DHCP**:
    - จัดสรร IP addresses อัตโนมัติ
    - Port 67 (server), 68 (client), UDP
    - DORA Process: Discover → Offer → Request → Acknowledge
    - Lease time, renewal, release
8. ✅ **NAT**:
    - แปลง private IPs ↔ public IPs
    - Types: Static (1:1), Dynamic (pool), PAT/Overload (many:1)
    - Port Forwarding สำหรับ servers

**สิ่งสำคัญที่ต้องจำ:**

- Application Layer = Layer 7, protocols ที่ใกล้ user
- HTTP = Port 80, HTTPS = Port 443
- SMTP = ส่ง (Port 25), POP3/IMAP = รับ (110/143)
- DNS = Port 53, แปลง names → IPs
- DHCP = Port 67/68, จัดสรร IPs อัตโนมัติ
- NAT = แปลง private ↔ public IPs, ประหยัด IPs
- PAT (NAT Overload) = ใช้มากที่สุด, many devices → 1 public IP
- Port Forwarding = host servers เบื้องหลัง NAT

**ความแตกต่างสำคัญ:**

```
HTTP vs HTTPS:
  HTTP = ไม่ encrypted, HTTPS = encrypted

POP3 vs IMAP:
  POP3 = download-delete, IMAP = sync multiple devices

Static vs Dynamic vs PAT NAT:
  Static = 1:1, Dynamic = pool, PAT = many:1

DNS Cache vs DHCP Lease:
  DNS cache = temporary storage ของ DNS responses
  DHCP lease = ระยะเวลาใช้ IP address
```

---

**[ไฟล์ Module 15 - Application Layer สมบูรณ์แล้ว!]**

**Next:** หากต้องการเนื้อหาเพิ่มเติมหรือ clarification ในหัวข้อใดๆ ของ Module 15 บอกได้เลยครับ! 🎉