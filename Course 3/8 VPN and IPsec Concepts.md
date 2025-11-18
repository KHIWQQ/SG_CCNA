# CCNA Course 3 - Module 8: VPN and IPsec Concepts

## แนวคิดเกี่ยว VPN และ IPsec

---

## 8.1 VPN Technology (เทคโนโลยี VPN)

### What is a VPN?

**คำจำกัดความ:**

- **Virtual Private Network (VPN)** = เครือข่ายส่วนตัวเสมือน
- สร้าง secure tunnel ผ่าน public network (Internet)
- เข้ารหัส traffic
- ปกป้อง confidentiality, integrity, authentication

**เปรียบเทียบ:**

```
Without VPN:
[Site A] ─── Internet (unencrypted) ─── [Site B]
           ❌ Anyone can intercept
           ❌ Data readable
           ❌ No privacy

With VPN:
[Site A] ═══ Encrypted Tunnel ═══ [Site B]
           ✅ Encrypted
           ✅ Private
           ✅ Secure
```

---

### VPN Benefits

**1. Cost Savings:**

```
Traditional WAN:
  - Dedicated leased lines
  - Expensive (per site)
  - Example: $1000/month per T1 line

VPN over Internet:
  - Use existing Internet connections
  - Much cheaper
  - Example: $100/month broadband
  
Savings: 90%+ for small/medium businesses
```

**2. Security:**

```
✅ Encryption (data confidentiality)
✅ Authentication (verify identity)
✅ Data integrity (detect tampering)
✅ Private communication over public network
```

**3. Scalability:**

```
✅ Easy to add new sites
✅ No need for dedicated circuits
✅ Internet widely available
```

**4. Compatibility:**

```
✅ Works over any IP network
✅ Multiple WAN technologies (DSL, Cable, Fiber, 4G/5G)
✅ Standards-based (IPsec)
```

**5. Flexibility:**

```
✅ Remote access (work from anywhere)
✅ BYOD (Bring Your Own Device)
✅ Mobile workforce
```

---

### VPN Types Overview

**2 ประเภทหลัก:**

```
1. Site-to-Site VPN
   - Router-to-router
   - Connects sites/networks
   - Always-on
   - Transparent to users

2. Remote Access VPN
   - Client-to-gateway
   - Individual users
   - On-demand
   - Client software required
```

---

## 8.2 Types of VPNs

### Site-to-Site VPN

**คำจำกัดความ:**

- เชื่อมต่อ networks/sites
- Router-to-router connection
- Permanent VPN tunnel
- Users ไม่รู้ว่ามี VPN (transparent)

**Topology:**

```
[LAN A] ─── [Router A] ╍╍╍ VPN Tunnel ╍╍╍ [Router B] ─── [LAN B]
                           (Internet)

Users on LAN A ↔ Users on LAN B
(ผ่าน encrypted tunnel)
```

**Use Cases:**

```
- เชื่อม branch offices กับ headquarters
- เชื่อม business partners
- Disaster recovery sites
- Cloud connectivity (to AWS, Azure)
```

**Characteristics:**

```
✅ Always-on connection
✅ Transparent to users
✅ High throughput
✅ Connects networks (not individual users)
❌ Requires static public IPs (usually)
❌ More complex setup
```

**Example:**

```
Headquarters (Bangkok) ↔ Branch Office (Chiang Mai)

HQ Router: 203.0.113.1
Branch Router: 198.51.100.1

VPN Tunnel: 203.0.113.1 ↔ 198.51.100.1
Traffic: 192.168.1.0/24 (HQ) ↔ 192.168.2.0/24 (Branch)
```

---

### Remote Access VPN

**คำจำกัดความ:**

- Individual users เชื่อมต่อกับ corporate network
- Client-to-gateway connection
- On-demand connection
- Requires VPN client software

**Topology:**

```
[User Laptop] ╍╍╍ VPN Connection ╍╍╍ [VPN Gateway] ─── [Corporate LAN]
 (Home/Hotel)     (Internet)         (Office)

User ได้ IP จาก corporate network
Access resources เหมือนอยู่ที่ office
```

**Use Cases:**

```
- Remote workers (work from home)
- Traveling employees
- Contractors/vendors
- BYOD scenarios
```

**Characteristics:**

```
✅ Flexible (connect from anywhere)
✅ On-demand (เมื่อต้องการ)
✅ User authentication
✅ Per-user policies
❌ Requires client software
❌ Lower throughput (client device limitations)
❌ More overhead (per connection)
```

**Client Types:**

**Full Tunnel:**

```
- All traffic ผ่าน VPN
- Internet traffic ก็ผ่าน corporate network

Example:
  User browsing google.com
  → VPN → Corporate → Internet → Google
  (slower, but more secure)
```

**Split Tunnel:**

```
- Corporate traffic ผ่าน VPN
- Internet traffic ออกโดยตรง (local Internet)

Example:
  User accessing file server → VPN → Corporate
  User browsing google.com → Local Internet → Google
  (faster, but less secure)
```

---

### Clientless SSL VPN (WebVPN)

**คำจำกัดความ:**

- Browser-based VPN
- No client software required
- Uses SSL/TLS (HTTPS)
- Access via web browser

**How it Works:**

```
1. User opens browser
2. Navigate to VPN URL (https://vpn.company.com)
3. Login with credentials
4. Portal page with applications
5. Click application → Access through browser
```

**Characteristics:**

```
✅ No client software needed
✅ Works on any device (PC, tablet, phone)
✅ Easy for users
✅ Works through firewalls (port 443)
❌ Application-level only (not full network)
❌ Limited protocols (HTTP, HTTPS, SSH, RDP)
❌ Less flexible
```

**Use Cases:**

```
- Access web applications
- Email (webmail)
- File shares (web interface)
- RDP/SSH (via web portal)
- Guest access
- BYOD (unmanaged devices)
```

---

### VPN Comparison

```
Feature          Site-to-Site      Remote Access     Clientless SSL
------------------------------------------------------------------------
Purpose          Connect sites     Individual users  Web apps access
Connection       Router-router     Client-gateway    Browser-gateway
Always-on        Yes               No (on-demand)    No
Client SW        No                Yes               No (browser)
Transparency     Full              Full tunnel       Application-level
Throughput       High              Medium            Lower
Complexity       Higher            Medium            Lower
Use Case         Branch offices    Remote workers    Web apps, BYOD
Authentication   Pre-shared key,   Username/pass,    Username/pass,
                 Certificates      Certificates      2FA
------------------------------------------------------------------------
```

---

## 8.3 IPsec (IP Security)

### IPsec Overview

**คำจำกัดความ:**

- Framework สำหรับ secure IP communications
- Layer 3 protocol (Network Layer)
- Standard (IETF)
- Widely supported

**IPsec Services:**

```
1. Confidentiality (Encryption)
   - ข้อมูลเข้ารหัส
   - อ่านไม่ได้ถ้าไม่มี key

2. Data Integrity
   - ตรวจสอบว่าข้อมูลไม่ถูกแก้ไข
   - Hashing (MD5, SHA)

3. Authentication
   - ยืนยันตัวตน peers
   - Pre-shared keys, Certificates

4. Anti-replay Protection
   - ป้องกัน replay attacks
   - Sequence numbers
```

---

### IPsec Protocols

#### AH (Authentication Header)

**คำจำกัดความ:**

- Authentication + Integrity
- **ไม่มี Encryption**
- IP Protocol number: **51**

**Use Case:**

```
- ต้องการ authentication และ integrity
- ไม่ต้องการ encryption (ความเร็วสำคัญ)
- หรือ encryption ทำชั้นอื่น
```

**AH Header:**

```
┌─────────────────────────────────────┐
│        Original IP Header           │
├─────────────────────────────────────┤
│          AH Header                  │ ← Inserted
│  (Authentication, Integrity)        │
├─────────────────────────────────────┤
│          TCP/UDP Header             │
├─────────────────────────────────────┤
│            Data                     │
└─────────────────────────────────────┘

❌ Data ไม่เข้ารหัส (readable)
✅ Integrity protected
```

**Limitations:**

```
❌ No encryption
❌ Problems with NAT (modifies IP header)
❌ Less commonly used
```

---

#### ESP (Encapsulating Security Payload)

**คำจำกัดความ:**

- Authentication + Integrity + **Encryption**
- IP Protocol number: **50**
- Most commonly used

**Use Case:**

```
- ต้องการ full protection
- Confidentiality + Integrity + Authentication
- Standard for VPNs
```

**ESP Packet:**

```
┌─────────────────────────────────────┐
│        Original IP Header           │
├─────────────────────────────────────┤
│          ESP Header                 │ ← Inserted
├═════════════════════════════════════┤
│          TCP/UDP Header             │ ┐
├─────────────────────────────────────┤ │
│            Data                     │ │ Encrypted
├─────────────────────────────────────┤ │
│          ESP Trailer                │ ┘
├─────────────────────────────────────┤
│      ESP Authentication             │
└─────────────────────────────────────┘

✅ Data encrypted (confidentiality)
✅ Integrity protected
✅ Authenticated
```

**Advantages:**

```
✅ Encryption + Authentication + Integrity
✅ Works with NAT (NAT-T)
✅ Widely supported
✅ Recommended
```

---

#### AH vs ESP Comparison

```
Feature              AH                  ESP
------------------------------------------------------------------------
Authentication       Yes                 Yes
Integrity            Yes                 Yes
Encryption           No                  Yes
IP Protocol          51                  50
NAT Compatibility    No                  Yes (with NAT-T)
Common Usage         Rare                Very Common
Protection           IP header + data    Data only
Overhead             Lower               Higher
Recommendation       Use ESP             ✅ Recommended
------------------------------------------------------------------------
```

**Recommendation: Use ESP**

```
เหตุผล:
✅ Provides encryption (AH ไม่มี)
✅ Works with NAT
✅ Industry standard
✅ More secure
```

---

### IPsec Modes

#### Transport Mode

**คำจำกัดความ:**

- เข้ารหัส **payload** เท่านั้น
- Original IP header ไม่เข้ารหัส
- ใช้สำหรับ **host-to-host** communication

**Packet Structure:**

```
Before IPsec:
┌───────────┬───────────┬──────────┐
│ IP Header │ TCP/UDP   │   Data   │
└───────────┴───────────┴──────────┘

After IPsec Transport Mode (ESP):
┌───────────┬────────┬═══════════════════════┬────────┐
│ IP Header │  ESP   │ TCP/UDP │    Data     │  ESP   │
│(Original) │ Header │         │             │ Trailer│
└───────────┴────────┴═══════════════════════┴────────┘
              ↑        └────── Encrypted ─────┘

Original IP Header เห็นได้ (ไม่เข้ารหัส)
Payload encrypted
```

**Characteristics:**

```
✅ Lower overhead (smaller packet)
✅ Original IPs visible
❌ No protection for IP header
❌ Not suitable for site-to-site VPN
```

**Use Cases:**

```
- Host-to-host (PC to Server)
- Same IP domain
- End-to-end encryption
- Example: Encrypted Telnet between two servers
```

---

#### Tunnel Mode

**คำจำกัดความ:**

- เข้ารหัส **entire IP packet**
- เพิ่ม **new IP header**
- ใช้สำหรับ **site-to-site** VPN

**Packet Structure:**

```
Before IPsec:
┌────────────┬───────────┬──────────┐
│ IP Header  │ TCP/UDP   │   Data   │
│(Original)  │           │          │
└────────────┴───────────┴──────────┘

After IPsec Tunnel Mode (ESP):
┌─────────┬────────┬═══════════════════════════════════┬────────┐
│New IP   │  ESP   │Original│ TCP/UDP │    Data       │  ESP   │
│ Header  │ Header │IP Hdr  │         │               │ Trailer│
└─────────┴────────┴═══════════════════════════════════┴────────┘
   ↑                └──────── Entire packet encrypted ──────┘
Gateway IPs

New IP Header: Gateway-to-Gateway (public IPs)
Original IP Header: Host-to-Host (private IPs) ← Encrypted
```

**Characteristics:**

```
✅ Full packet protection
✅ Hides internal topology
✅ Suitable for site-to-site VPN
❌ Higher overhead (larger packet)
```

**Use Cases:**

```
- Site-to-site VPN
- Remote access VPN
- Gateway-to-gateway
- Standard for VPNs
```

**Example:**

```
Site-to-Site VPN:

Original packet:
  Src: 192.168.1.10 (PC at Site A)
  Dst: 192.168.2.20 (Server at Site B)

After IPsec Tunnel Mode:
  New IP Header:
    Src: 203.0.113.1 (Router A public IP)
    Dst: 198.51.100.1 (Router B public IP)
  
  Encrypted (inside):
    Original Src: 192.168.1.10
    Original Dst: 192.168.2.20
    TCP/UDP + Data
```

---

#### Transport vs Tunnel Mode Comparison

```
Feature          Transport Mode         Tunnel Mode
------------------------------------------------------------------------
Encrypts         Payload only           Entire IP packet
IP Header        Original (visible)     New + Original (encrypted)
Overhead         Lower                  Higher
Use Case         Host-to-host           Site-to-site, Remote access
Protection       Data only              Full packet
Common           Less common            Very common (VPNs)
Hides IPs        No                     Yes (internal IPs)
Recommendation   Host-to-host only      ✅ VPNs
------------------------------------------------------------------------
```

**Recommendation: Tunnel Mode for VPNs**

---

## 8.4 IPsec Framework

### IPsec Components

IPsec ประกอบด้วย 4 ส่วนหลัก:

```
1. IKE (Internet Key Exchange)
   - Key management
   - Negotiate parameters
   - Establish Security Associations (SAs)

2. Security Protocols
   - AH (Authentication Header)
   - ESP (Encapsulating Security Payload)

3. Encryption Algorithms
   - DES, 3DES, AES
   - Encrypt data

4. Hashing Algorithms
   - MD5, SHA-1, SHA-256
   - Data integrity
```

---

### IKE (Internet Key Exchange)

**คำจำกัดความ:**

- Protocol สำหรับ establish IPsec connection
- Negotiate security parameters
- Generate encryption keys
- Two phases: IKE Phase 1 และ IKE Phase 2

---

#### IKE Phase 1

**Purpose:**

```
- Establish secure channel (ISAKMP SA)
- Negotiate:
  • Encryption algorithm
  • Hashing algorithm
  • Authentication method
  • Diffie-Hellman group
- Protect IKE Phase 2 negotiation
```

**Modes:**

**Main Mode:**

```
- 6 messages
- More secure (identities protected)
- Slower
- Used with pre-shared keys or certificates
```

**Aggressive Mode:**

```
- 3 messages
- Faster
- Less secure (identities exposed)
- Used when Main Mode not possible
```

**Phase 1 Process:**

```
1. Peers negotiate policy
   - Encryption: DES, 3DES, AES
   - Hashing: MD5, SHA-1, SHA-256
   - Authentication: Pre-shared key or Certificates
   - DH group: 1, 2, 5, 14, etc.

2. Diffie-Hellman key exchange
   - Generate shared secret
   - Without sending it over network

3. Authentication
   - Verify peer identity
   - Using pre-shared key or certificates

Result: ISAKMP SA (IKE SA)
- Bidirectional secure channel
- Used for Phase 2 negotiation
```

---

#### IKE Phase 2

**Purpose:**

```
- Negotiate IPsec SA
- Parameters for actual data encryption
- Create IPsec tunnel
```

**Mode:**

```
Quick Mode (only mode):
- 3 messages
- Negotiates through Phase 1 tunnel
```

**Phase 2 Process:**

```
1. Negotiate IPsec policy
   - Security protocol: ESP or AH
   - Encryption: DES, 3DES, AES
   - Hashing: MD5, SHA-1, SHA-256
   - Mode: Transport or Tunnel
   - Lifetime

2. Create IPsec SAs
   - One per direction (inbound/outbound)
   - Unidirectional

Result: IPsec SAs
- Two SAs (bidirectional communication)
- Used to encrypt actual data
```

---

#### IKE Phase Comparison

```
Feature          Phase 1 (ISAKMP)      Phase 2 (IPsec)
------------------------------------------------------------------------
Purpose          Establish IKE SA      Establish IPsec SAs
Protection       IKE peers             User data
Mode             Main or Aggressive    Quick Mode
Messages         6 (Main) / 3 (Agg)    3
SA Type          ISAKMP SA (1)         IPsec SAs (2)
Direction        Bidirectional         Unidirectional (2 SAs)
Lifetime         Longer (hours/days)   Shorter (minutes/hours)
Priority         Security              Performance
------------------------------------------------------------------------
```

**IKE Summary:**

```
Phase 1: "Let's agree how to talk securely"
  → Creates secure channel (ISAKMP SA)

Phase 2: "Let's agree how to encrypt data"
  → Creates IPsec tunnels (IPsec SAs)

Then: Actual data transmission (encrypted)
```

---

### Security Associations (SAs)

**คำจำกัดความ:**

- Agreement ระหว่าง peers
- เก็บ parameters สำหรับ IPsec connection
- One-way (unidirectional)

**SA Components:**

```
- Security Parameter Index (SPI)
  • Unique identifier

- Destination IP address
  • Peer IP

- Security Protocol
  • AH or ESP

- Encryption algorithm
  • DES, 3DES, AES

- Hashing algorithm
  • MD5, SHA-1, SHA-256

- Lifetime
  • Time or data limit
```

**SA Types:**

```
ISAKMP SA (IKE Phase 1):
  - One per peer pair
  - Bidirectional
  - Protects IKE traffic

IPsec SA (IKE Phase 2):
  - Two per connection (inbound/outbound)
  - Unidirectional
  - Protects user data
```

**Example:**

```
Site-to-Site VPN:
  Router A ↔ Router B

SAs:
  1. ISAKMP SA (bidirectional) - Phase 1
  2. IPsec SA (A→B) - Phase 2 outbound
  3. IPsec SA (B→A) - Phase 2 inbound

Total: 3 SAs
```

---

### Diffie-Hellman (DH)

**คำจำกัดความ:**

- Algorithm สำหรับ key exchange
- ส่ง public values ผ่าน unsecured channel
- คำนวณ shared secret independently
- ไม่มีใครเห็น shared secret

**How it Works (Simplified):**

```
1. Router A และ B agree บน:
   - Prime number (p)
   - Generator (g)
   (Public values - ใครก็เห็นได้)

2. Router A:
   - สุ่ม private value (a)
   - คำนวณ public value: A = g^a mod p
   - ส่ง A ไป Router B

3. Router B:
   - สุ่ม private value (b)
   - คำนวณ public value: B = g^b mod p
   - ส่ง B ไป Router A

4. Router A:
   - คำนวณ shared secret: S = B^a mod p

5. Router B:
   - คำนวณ shared secret: S = A^b mod p

6. Both routers มี shared secret เหมือนกัน!
   - ไม่มีใครส่ง shared secret ผ่าน network
   - Attacker เห็นเฉพาะ p, g, A, B (insufficient)
```

**DH Groups:**

```
Group   Key Size    Security         Speed
------------------------------------------------------------------------
1       768-bit     Weak (obsolete)  Fast
2       1024-bit    Medium           Fast
5       1536-bit    Good             Medium
14      2048-bit    Strong           Slower
15      3072-bit    Very Strong      Slow
16      4096-bit    Strongest        Slowest
------------------------------------------------------------------------

Recommendation:
  - Group 14 (2048-bit) minimum
  - Group 15/16 for high security
  - Group 1/2 obsolete (insecure)
```

---

### IPsec Algorithms

#### Encryption Algorithms

**DES (Data Encryption Standard):**

```
Key size: 56-bit
Status: Obsolete (insecure)
Speed: Fast
Recommendation: ❌ Don't use
```

**3DES (Triple DES):**

```
Key size: 168-bit (3 × 56-bit)
Status: Legacy (being phased out)
Speed: Slow
Recommendation: ⚠️ Use only if AES unavailable
```

**AES (Advanced Encryption Standard):**

```
Key sizes:
  - AES-128: 128-bit key
  - AES-192: 192-bit key
  - AES-256: 256-bit key

Status: Current standard
Speed: Fast (hardware accelerated)
Security: Strong
Recommendation: ✅ Use AES-256 (or AES-128 minimum)
```

---

#### Hashing Algorithms

**Purpose:**

```
- Data integrity
- Detect tampering
- Create "fingerprint" (hash) of data
```

**MD5 (Message Digest 5):**

```
Hash size: 128-bit
Status: Obsolete (collisions found)
Recommendation: ❌ Don't use
```

**SHA-1 (Secure Hash Algorithm 1):**

```
Hash size: 160-bit
Status: Deprecated (collisions possible)
Recommendation: ⚠️ Being phased out
```

**SHA-2 Family:**

```
SHA-256: 256-bit hash
SHA-384: 384-bit hash
SHA-512: 512-bit hash

Status: Current standard
Security: Strong
Recommendation: ✅ Use SHA-256 (minimum) or SHA-512
```

---

#### HMAC (Hash-based Message Authentication Code)

**คำจำกัดความ:**

- Combines hash + secret key
- Provides authentication + integrity
- Used in IPsec

**How it Works:**

```
1. Message + Secret Key → Hash function → HMAC
2. Send Message + HMAC
3. Receiver calculates HMAC with same secret key
4. Compare HMACs
   - Match → Message authentic and intact
   - Different → Message tampered or fake
```

**HMAC Algorithms:**

```
HMAC-MD5: ❌ Obsolete
HMAC-SHA-1: ⚠️ Deprecated
HMAC-SHA-256: ✅ Recommended
HMAC-SHA-512: ✅ High security
```

---

### Authentication Methods

#### Pre-Shared Key (PSK)

**คำจำกัดความ:**

- Shared secret (password)
- Configured on both peers
- Simple
- Most common for site-to-site VPN

**Configuration:**

```
Router A: key = "MySecretPassword123"
Router B: key = "MySecretPassword123"

Must match exactly!
```

**Characteristics:**

```
✅ Simple to configure
✅ No PKI infrastructure needed
✅ Suitable for small deployments
❌ Doesn't scale (every peer = different key)
❌ Key management difficult
❌ If compromised, must change everywhere
```

**Best Practices:**

```
✅ Use strong keys (long, random)
✅ Change regularly
✅ Don't reuse across sites
✅ Secure storage
```

---

#### Digital Certificates (RSA)

**คำจำกัดความ:**

- Uses Public Key Infrastructure (PKI)
- Certificate Authority (CA) issues certificates
- Strong authentication
- Scalable

**How it Works:**

```
1. CA issues certificates to routers
2. Routers exchange certificates
3. Each router verifies peer's certificate with CA
4. If valid → Authenticated
```

**Characteristics:**

```
✅ Scalable (add routers easily)
✅ Strong security
✅ Centralized management (CA)
✅ Can revoke certificates (CRL)
❌ Complex setup (PKI required)
❌ CA infrastructure needed
❌ Certificate management overhead
```

**Use Cases:**

```
- Large deployments (many sites)
- Remote access VPN (many users)
- High security requirements
- When scalability important
```

---

#### Pre-Shared Key vs Certificates

```
Feature          PSK                   Certificates
------------------------------------------------------------------------
Complexity       Simple                Complex
Scalability      Poor                  Excellent
Setup Time       Fast                  Slow (PKI setup)
Security         Good                  Excellent
Management       Manual                Centralized (CA)
Revocation       Change key            CRL/OCSP
Use Case         Small/Medium          Large/Enterprise
Cost             Low                   Higher (CA)
Recommendation   Site-to-Site          Remote Access VPN
                 (few sites)           (many users)
------------------------------------------------------------------------
```

**Recommendation:**

```
Small deployment (< 10 sites):
  → Pre-Shared Keys

Large deployment (> 10 sites):
  → Digital Certificates

Remote Access VPN (many users):
  → Digital Certificates + Username/Password
```

---

## 8.5 IPsec Configuration Concepts

### IPsec Configuration Steps

**5-Step Process:**

```
1. Configure IKE Phase 1 (ISAKMP Policy)
   - Encryption, hash, authentication, DH group, lifetime

2. Configure IKE Phase 2 (IPsec Transform Set)
   - ESP/AH, encryption, hash

3. Define Interesting Traffic (Crypto ACL)
   - Traffic to encrypt

4. Create Crypto Map
   - Bind transform set, crypto ACL, peer

5. Apply Crypto Map to Interface
   - Outbound interface
```

---

### ISAKMP Policy (Phase 1)

**Purpose:**

```
- Define Phase 1 parameters
- Multiple policies (priority)
- Peers negotiate (highest to lowest priority)
```

**Parameters:**

```
Priority:       Number (lower = higher priority)
Encryption:     DES, 3DES, AES 128/192/256
Hash:           MD5, SHA-1, SHA-256, SHA-512
Authentication: Pre-shared key or RSA signatures
DH Group:       1, 2, 5, 14, 15, 16, etc.
Lifetime:       Seconds (default: 86400 = 24 hours)
```

**Configuration (Conceptual):**

```
Router(config)# crypto isakmp policy 10
Router(config-isakmp)# encryption aes 256
Router(config-isakmp)# hash sha256
Router(config-isakmp)# authentication pre-share
Router(config-isakmp)# group 14
Router(config-isakmp)# lifetime 86400
Router(config-isakmp)# exit

Router(config)# crypto isakmp key MySecretKey123 address 198.51.100.1
```

---

### Transform Set (Phase 2)

**Purpose:**

```
- Define Phase 2 parameters
- Encryption/hash for actual data
```

**Parameters:**

```
Security Protocol: ESP or AH
Encryption:        DES, 3DES, AES 128/192/256
Hash:              MD5, SHA-1, SHA-256, SHA-512
Mode:              Tunnel or Transport
```

**Configuration (Conceptual):**

```
Router(config)# crypto ipsec transform-set MYSET esp-aes 256 esp-sha256-hmac
Router(cfg-crypto-trans)# mode tunnel
Router(cfg-crypto-trans)# exit
```

---

### Crypto ACL

**Purpose:**

```
- Define "interesting traffic"
- Traffic to encrypt
- Trigger VPN
```

**Configuration (Conceptual):**

```
Router(config)# access-list 101 permit ip 192.168.1.0 0.0.0.255 192.168.2.0 0.0.0.255

Meaning:
  Traffic from 192.168.1.0/24 to 192.168.2.0/24 → Encrypt
```

**Important:**

```
⚠️ Crypto ACL ≠ Regular ACL
   - Don't use deny (unexpected behavior)
   - Use permit for traffic to encrypt
   - Traffic not matching ACL → Not encrypted (but allowed)
```

---

### Crypto Map

**Purpose:**

```
- Bind all pieces together
- Associate peer, transform set, crypto ACL
```

**Configuration (Conceptual):**

```
Router(config)# crypto map MYMAP 10 ipsec-isakmp
Router(config-crypto-map)# set peer 198.51.100.1
Router(config-crypto-map)# set transform-set MYSET
Router(config-crypto-map)# match address 101
Router(config-crypto-map)# exit

Router(config)# interface gigabitEthernet 0/1
Router(config-if)# crypto map MYMAP
```

---

### IPsec Configuration Example (Conceptual)

**Scenario:**

```
Site A:
  LAN: 192.168.1.0/24
  Router: 203.0.113.1

Site B:
  LAN: 192.168.2.0/24
  Router: 198.51.100.1

Goal: IPsec VPN between sites
```

**Router A Configuration:**

```
! Phase 1
crypto isakmp policy 10
 encryption aes 256
 hash sha256
 authentication pre-share
 group 14
 lifetime 86400
 exit

crypto isakmp key MySecretPassword123 address 198.51.100.1

! Phase 2
crypto ipsec transform-set MYSET esp-aes 256 esp-sha256-hmac
 mode tunnel
 exit

! Crypto ACL
access-list 101 permit ip 192.168.1.0 0.0.0.255 192.168.2.0 0.0.0.255

! Crypto Map
crypto map MYMAP 10 ipsec-isakmp
 set peer 198.51.100.1
 set transform-set MYSET
 match address 101
 exit

! Apply to interface
interface gigabitEthernet 0/1
 crypto map MYMAP
 exit
```

**Router B Configuration:**

```
! Mirror configuration with reversed IPs

crypto isakmp policy 10
 encryption aes 256
 hash sha256
 authentication pre-share
 group 14
 lifetime 86400

crypto isakmp key MySecretPassword123 address 203.0.113.1

crypto ipsec transform-set MYSET esp-aes 256 esp-sha256-hmac
 mode tunnel

access-list 101 permit ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255

crypto map MYMAP 10 ipsec-isakmp
 set peer 203.0.113.1
 set transform-set MYSET
 match address 101

interface gigabitEthernet 0/1
 crypto map MYMAP
```

---

### NAT Traversal (NAT-T)

**Problem:**

```
NAT modifies IP headers
IPsec authenticates IP headers
→ Conflict! IPsec fails
```

**Solution: NAT-T (NAT Traversal)**

```
- Encapsulate IPsec in UDP
- UDP port 4500
- NAT can translate UDP ports
- IPsec works through NAT
```

**When Needed:**

```
- VPN peer behind NAT
- Common for remote access VPN
- Home routers (NAT)
```

**Configuration:**

```
Router(config)# crypto isakmp nat-traversal 20
                                        ↑
                                 keepalive interval (seconds)
```

---

## 8.6 VPN Troubleshooting

### Common IPsec Issues

#### Issue 1: Phase 1 Failure

**Symptoms:**

```
- VPN not established
- "No ISAKMP SA" message
```

**Causes:**

```
1. Mismatched ISAKMP policies
   - Different encryption, hash, DH group

2. Incorrect pre-shared key
   - Typo, case-sensitive

3. Incorrect peer IP address

4. Firewall blocking UDP 500
```

**Troubleshooting:**

```
! Check ISAKMP SA
show crypto isakmp sa

! Debug
debug crypto isakmp

! Verify policy
show crypto isakmp policy
```

---

#### Issue 2: Phase 2 Failure

**Symptoms:**

```
- Phase 1 success
- Phase 2 fails
- "No IPsec SA" message
```

**Causes:**

```
1. Mismatched transform sets
   - Different encryption, hash

2. Mismatched crypto ACLs
   - Different networks defined

3. No interesting traffic
```

**Troubleshooting:**

```
! Check IPsec SAs
show crypto ipsec sa

! Debug
debug crypto ipsec

! Verify transform set
show crypto ipsec transform-set
```

---

#### Issue 3: Traffic Not Encrypted

**Symptoms:**

```
- VPN established
- Traffic not going through tunnel
```

**Causes:**

```
1. Crypto ACL doesn't match traffic
2. Routing issue
3. NAT before VPN (crypto map should be before NAT)
```

**Troubleshooting:**

```
! Check crypto ACL
show access-list 101

! Check routing
show ip route

! Verify NAT exemption
show ip nat translations
```

---

#### Issue 4: NAT Issues

**Symptoms:**

```
- Phase 1 or 2 fails
- Peer behind NAT
```

**Solutions:**

```
1. Enable NAT-T
   crypto isakmp nat-traversal

2. Allow UDP 4500 through firewall

3. NAT exemption (don't NAT VPN traffic)
   access-list 110 deny ip 192.168.1.0 0.0.0.255 192.168.2.0 0.0.0.255
   access-list 110 permit ip 192.168.1.0 0.0.0.255 any
   ip nat inside source list 110 interface gi0/1 overload
```

---

### Verification Commands

```
! ISAKMP (Phase 1)
show crypto isakmp sa
show crypto isakmp policy

! IPsec (Phase 2)
show crypto ipsec sa
show crypto ipsec transform-set

! Crypto Map
show crypto map

! Statistics
show crypto engine connections active
show crypto session

! Debug (use carefully!)
debug crypto isakmp
debug crypto ipsec
```

---

### Troubleshooting Workflow

```
1. Verify connectivity (routing, reachability)
   ping <peer-ip>

2. Check Phase 1
   show crypto isakmp sa
   → Should show "QM_IDLE" (success)

3. Check Phase 2
   show crypto ipsec sa
   → Should show encaps/decaps counters

4. Generate interesting traffic
   ping <remote-network>

5. Verify encryption
   show crypto ipsec sa
   → Counters should increment

6. Debug (if needed)
   debug crypto isakmp
   debug crypto ipsec
```

---

## Summary (สรุป)

Module 8 นี้เราได้เรียนรู้:

1. ✅ **VPN Technology**:
    
    - Secure tunnel over public network
    - Benefits: Cost, Security, Scalability
    - Types: Site-to-Site, Remote Access
2. ✅ **Types of VPNs**:
    
    - **Site-to-Site**: Router-router, always-on
    - **Remote Access**: Client-gateway, on-demand
    - **Clientless SSL**: Browser-based, no client
3. ✅ **IPsec**:
    
    - Framework for secure IP
    - Services: Confidentiality, Integrity, Authentication, Anti-replay
    - Protocols: **AH** (auth only), **ESP** (auth + encrypt)
    - Modes: **Transport** (payload), **Tunnel** (entire packet)
4. ✅ **IPsec Framework**:
    
    - **IKE Phase 1**: Establish secure channel (ISAKMP SA)
    - **IKE Phase 2**: Negotiate IPsec SAs
    - **Diffie-Hellman**: Key exchange
    - **SAs**: Security Associations (parameters)
5. ✅ **Algorithms**:
    
    - Encryption: DES (obsolete), 3DES (legacy), **AES** ✅
    - Hash: MD5 (obsolete), SHA-1 (deprecated), **SHA-256/512** ✅
    - DH Groups: 1/2 (obsolete), **14** (good), 15/16 (strong)
6. ✅ **Authentication**:
    
    - **Pre-Shared Key**: Simple, small deployments
    - **Digital Certificates**: Scalable, large deployments
7. ✅ **Configuration Steps**:
    
    1. ISAKMP Policy (Phase 1)
    2. Transform Set (Phase 2)
    3. Crypto ACL (interesting traffic)
    4. Crypto Map (bind all)
    5. Apply to interface
8. ✅ **Troubleshooting**:
    
    - Phase 1 failure: Policy mismatch, wrong key
    - Phase 2 failure: Transform set mismatch
    - NAT issues: Use NAT-T

**สิ่งสำคัญที่ต้องจำ:**

- VPN = Secure tunnel over public network
- IPsec = Layer 3 security framework
- ESP = Recommended (encryption + auth)
- AH = Authentication only (rare)
- Tunnel Mode = Site-to-Site VPN (standard)
- Transport Mode = Host-to-host
- IKE Phase 1 = Protect negotiation
- IKE Phase 2 = Protect data
- AES-256 + SHA-256 = Current standard
- DH Group 14+ = Minimum
- Pre-shared key = Simple (small)
- Certificates = Scalable (large)
- NAT-T = NAT traversal (UDP 4500)

**Recommended Settings (2024):**

```
Encryption:    AES-256
Hash:          SHA-256 or SHA-512
DH Group:      14 (minimum), 16 (preferred)
Authentication: Pre-shared key (small) or Certificates (large)
Protocol:      ESP (not AH)
Mode:          Tunnel (VPNs)
```

**IPsec Ports:**

```
UDP 500:  IKE (Phase 1, Phase 2)
UDP 4500: NAT-T (IPsec through NAT)
IP 50:    ESP
IP 51:    AH
```

**Next Module:** Module 9 - QoS Concepts

---

**[ไฟล์ Module 8 สมบูรณ์แล้ว!]**