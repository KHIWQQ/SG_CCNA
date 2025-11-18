# CCNA Course 3 - Module 7: WAN Concepts

## แนวคิดเกี่ยวกับ Wide Area Network

---

## 7.1 Purpose of WANs (วัตถุประสงค์ของ WAN)

### What is a WAN?

**คำจำกัดความ:**

- **Wide Area Network (WAN)** = เครือข่ายที่เชื่อมต่อ LANs ข้าม geographical distances
- ครอบคลุมพื้นที่กว้าง (cities, countries, continents)
- ใช้ services จาก **service providers**

**เปรียบเทียบ LAN vs WAN:**

```
Feature              LAN                          WAN
------------------------------------------------------------------------
Coverage             Small area (building)        Large area (cities/countries)
Ownership            Organization owns            Leased from providers
Speed                High (100Mbps-100Gbps)       Lower (56Kbps-100Gbps)
Technology           Ethernet, Wi-Fi              Various WAN technologies
Cost                 Lower                        Higher
Media                Copper, Fiber                Fiber, Wireless, Satellite
Management           Internal IT                  Service provider
------------------------------------------------------------------------
```

---

### Why WANs Are Needed

**1. Connect Remote Sites**

```
Company locations:
  - Headquarters: Bangkok
  - Branch offices: Chiang Mai, Phuket, Khon Kaen
  - Data center: USA

→ Need WAN to connect all sites
```

**2. Enable Remote Access**

```
- Employees work from home
- Mobile workers
- Telecommuting
- Remote IT support

→ Need WAN connection (VPN over Internet)
```

**3. Access Cloud Services**

```
- Cloud applications (Office 365, Salesforce)
- Cloud storage (Dropbox, Google Drive)
- Cloud infrastructure (AWS, Azure)

→ Need Internet/WAN connectivity
```

**4. Business Continuity**

```
- Backup sites
- Disaster recovery
- Redundant connections

→ Need reliable WAN links
```

---

### WAN Characteristics

**1. Operates over Large Geographical Areas**

```
- City-to-city
- Country-to-country
- Continent-to-continent
- Global coverage
```

**2. Uses Service Providers**

```
Common providers:
  - ISPs (Internet Service Providers)
  - Telcos (Telephone companies)
  - Cable companies
  - Satellite providers
  - Cellular networks
```

**3. Uses Serial Connections**

```
- Point-to-point links
- Packet-switched networks
- Circuit-switched networks
```

**4. Slower Than LANs**

```
Typical speeds:
  - Legacy: 56 Kbps (dial-up)
  - T1: 1.544 Mbps
  - E1: 2.048 Mbps
  - T3: 44.736 Mbps
  - OC-3: 155 Mbps
  - OC-12: 622 Mbps
  - Ethernet WAN: 10Mbps - 100Gbps
```

---

## 7.2 WAN Operations

### WAN Devices

#### Customer Premises Equipment (CPE)

**คำจำกัดความ:**

- อุปกรณ์ฝั่ง customer
- Customer owns/leases

**ประเภท:**

**Router:**

```
- เชื่อม LAN กับ WAN
- WAN interface (Serial, Ethernet)
- Routing, Security (Firewall, VPN)

ตัวอย่าง:
  - Cisco ISR (Integrated Services Router)
  - Branch routers
```

**Modem:**

```
- Modulator/Demodulator
- แปลง digital ↔ analog signals
- ใช้กับ telephone lines, cable

ประเภท:
  - DSL modem
  - Cable modem
  - Dial-up modem (legacy)
```

**CSU/DSU (Channel Service Unit/Data Service Unit):**

```
- ใช้กับ digital leased lines (T1/E1)
- CSU: เชื่อมต่อ line, protect
- DSU: แปลง signals

หมายเหตุ: Modern routers มี built-in CSU/DSU
```

#### Provider Equipment

**Demarcation Point (Demarc):**

```
- จุดแบ่งความรับผิดชอบ
- Customer side ↔ Provider side
- ปกติอยู่ที่ customer premises

Responsibility:
  - Customer: CPE → Demarc
  - Provider: Demarc → Provider network
```

**Local Loop (Last Mile):**

```
- สายเชื่อมต่อจาก customer ไป provider
- ระยะทาง: premises → Central Office (CO)
- เรียกอีกชื่อว่า "Last Mile"

Media types:
  - Copper (twisted pair)
  - Coaxial cable
  - Fiber optic
  - Wireless
```

**Central Office (CO):**

```
- สำนักงานกลางของ service provider
- มี switching equipment
- เชื่อมต่อ local loops เข้า provider backbone
```

**Toll Network:**

```
- Provider's WAN backbone
- เชื่อมต่อ COs ต่างๆ
- High-speed links (fiber optic)
- Long-haul communications
```

---

### WAN Connection Types

#### Dedicated (Leased Line)

**คำจำกัดความ:**

- Point-to-point permanent connection
- เช่าจาก service provider
- Bandwidth dedicated (ไม่แชร์)
- Always available

**Characteristics:**

```
✅ Guaranteed bandwidth
✅ Predictable performance
✅ Secure (dedicated line)
✅ High reliability
❌ Expensive
❌ Fixed bandwidth (ไม่ flexible)
❌ Long installation time
```

**Technologies:**

```
- T1/E1
- T3/E3
- SONET/SDH
- Ethernet WAN (Metro Ethernet)
```

**Use Cases:**

```
- Mission-critical applications
- Consistent traffic patterns
- Predictable bandwidth requirements
- Large enterprises
```

**Example:**

```
[HQ Bangkok] ---- T1 Leased Line ---- [Branch Chiang Mai]
              (1.544 Mbps dedicated)
```

#### Circuit-Switched

**คำจำกัดความ:**

- Dial-on-demand connections
- สร้าง dedicated circuit เมื่อต้องการ
- Disconnect เมื่อไม่ใช้
- เสียค่าใช้จ่ายตามเวลาใช้

**Characteristics:**

```
✅ Pay per use
✅ Flexible (dial when needed)
✅ Good for sporadic traffic
❌ Setup time (dial delay)
❌ Variable bandwidth
❌ Not suitable for continuous traffic
```

**Technologies:**

```
- PSTN (Public Switched Telephone Network)
- ISDN (Integrated Services Digital Network)
```

**Use Cases:**

```
- Backup connections
- Low-volume sites
- Legacy systems
- Disaster recovery (ปัจจุบันใช้น้อย)
```

**Example:**

```
[Router] ---dial--> [ISDN] ---establish circuit--> [Remote Router]
         ← connect when needed, disconnect after done
```

#### Packet-Switched

**คำจำกัดความ:**

- แบ่งข้อมูลเป็น packets
- Packets share provider network
- Virtual circuits (logical paths)
- Bandwidth shared

**Characteristics:**

```
✅ Cost-effective (แชร์ bandwidth)
✅ Flexible (multiple destinations)
✅ Scalable
❌ Variable latency
❌ Bandwidth not guaranteed
❌ Complex to configure
```

**Technologies:**

```
- Frame Relay (legacy)
- ATM (Asynchronous Transfer Mode) (legacy)
- X.25 (very old, obsolete)
- MPLS (Modern, widely used)
```

**Virtual Circuits:**

**PVC (Permanent Virtual Circuit):**

```
- Always connected (like leased line)
- Pre-configured path
- No call setup
- Common for Frame Relay
```

**SVC (Switched Virtual Circuit):**

```
- Established on-demand
- Call setup required
- Disconnected after use
- Less common
```

**Example:**

```
Frame Relay Network:

[Site A] ────┐
[Site B] ────┼──── [Frame Relay Cloud] ──── Multiple PVCs
[Site C] ────┘

PVCs:
  - Site A ↔ Site B
  - Site A ↔ Site C
  - Site B ↔ Site C
```

#### Internet-Based

**คำจำกัดความ:**

- ใช้ public Internet เป็น transport
- VPN เข้ารหัสข้อมูล
- Cost-effective
- Available everywhere

**Characteristics:**

```
✅ Low cost
✅ Easy deployment
✅ Available worldwide
✅ Flexible bandwidth
❌ Security concerns (ใช้ VPN แก้)
❌ Variable performance
❌ No QoS guarantee
```

**Technologies:**

```
- Broadband (DSL, Cable, Fiber)
- Wireless (4G, 5G, LTE)
- Satellite
- VPN over Internet
```

**Use Cases:**

```
- Small businesses
- Remote workers
- Backup connections
- Non-critical traffic
- Cost-sensitive deployments
```

**Example:**

```
[Branch] --DSL--> [Internet] --VPN--> [Headquarters]
         Encrypted tunnel over public Internet
```

---

## 7.3 Traditional WAN Connectivity

### Leased Line

**คำจำกัดความ:**

- **Dedicated** point-to-point connection
- Pre-established path
- Available 24/7
- เรียกอีกชื่อว่า **point-to-point link**, **serial link**

#### T-Carrier (North America)

**T1:**

```
Bandwidth: 1.544 Mbps
Channels: 24 DS0 channels (64 Kbps each)
Use: Common leased line
Cost: Moderate
```

**T3:**

```
Bandwidth: 44.736 Mbps
Equivalent: 28 T1s
Use: High-bandwidth requirements
Cost: High
```

**Fractional T1:**

```
Bandwidth: Portion of T1 (e.g., 512 Kbps, 768 Kbps)
Use: Cost savings for lower bandwidth needs
```

#### E-Carrier (Europe/International)

**E1:**

```
Bandwidth: 2.048 Mbps
Channels: 32 channels (64 Kbps each, 30 for data)
Use: European standard
```

**E3:**

```
Bandwidth: 34.368 Mbps
Equivalent: 16 E1s
```

#### SONET/SDH

**SONET (Synchronous Optical Network) - North America:**

```
OC-1:   51.84 Mbps
OC-3:   155.52 Mbps
OC-12:  622.08 Mbps
OC-48:  2.488 Gbps
OC-192: 9.953 Gbps
OC-768: 39.813 Gbps
```

**SDH (Synchronous Digital Hierarchy) - International:**

```
STM-1:  155 Mbps (equivalent OC-3)
STM-4:  622 Mbps (equivalent OC-12)
STM-16: 2.5 Gbps (equivalent OC-48)
STM-64: 10 Gbps (equivalent OC-192)
```

**Use Cases:**

```
- Telecommunications backbone
- High-speed long-haul
- Service provider networks
- Metropolitan area networks
```

---

### HDLC (High-Level Data Link Control)

**คำจำกัดความ:**

- Layer 2 protocol สำหรับ serial links
- Default encapsulation บน Cisco routers
- Simple, efficient

**Characteristics:**

```
✅ Default บน Cisco
✅ Low overhead
✅ Reliable
✅ Point-to-point only
❌ Cisco HDLC proprietary (ไม่เข้ากับ vendors อื่น)
```

**HDLC Frame Format:**

```
+------+----------+--------+---------+------+
| Flag | Address  | Control|  Data   | FCS  |
| 8bit |  8bit    | 8bit   | Variable|16bit |
+------+----------+--------+---------+------+
```

**Configuration:**

```
! HDLC is default, no configuration needed
Router(config)# interface serial 0/0/0
Router(config-if)# encapsulation hdlc  (default)

! Verify
Router# show interfaces serial 0/0/0
Serial0/0/0 is up, line protocol is up
  Hardware is GT96K Serial
  Internet address is 10.1.1.1/30
  MTU 1500 bytes, BW 1544 Kbit/sec, DLY 20000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation HDLC, loopback not set
                ↑↑↑↑
```

---

### PPP (Point-to-Point Protocol)

**คำจำกัดความ:**

- Layer 2 protocol สำหรับ serial links
- Industry standard (RFC 1661)
- รองรับ multiple Layer 3 protocols

**Features:**

```
✅ Standard (inter-vendor compatibility)
✅ Authentication (PAP, CHAP)
✅ Multilink (combine multiple links)
✅ Compression
✅ Error detection
✅ Multi-protocol (IP, IPv6, IPX)
```

**PPP Components:**

#### 1. LCP (Link Control Protocol)

```
- Establish, configure, test link
- Negotiate options:
  * Authentication
  * Compression
  * Multilink
  * Callback
```

#### 2. NCP (Network Control Protocol)

```
- Configure Layer 3 protocols
- Different NCP for each protocol:
  * IPCP (IP Control Protocol)
  * IPV6CP (IPv6 CP)
```

#### 3. Authentication

**PAP (Password Authentication Protocol):**

```
- Plain text username/password
- 2-way handshake
- ❌ Insecure (no encryption)
- Legacy, not recommended

Process:
  1. Username/password sent in clear text
  2. Accept or reject
```

**CHAP (Challenge Handshake Authentication Protocol):**

```
- Encrypted password (MD5 hash)
- 3-way handshake
- ✅ Secure
- Recommended

Process:
  1. Server sends challenge
  2. Client responds with hash (challenge + password)
  3. Server verifies hash
  4. Periodic re-authentication
```

**PPP Configuration:**

```
! Basic PPP
Router(config)# interface serial 0/0/0
Router(config-if)# encapsulation ppp

! PPP with CHAP authentication
Router(config)# username RemoteRouter password cisco123
Router(config)# interface serial 0/0/0
Router(config-if)# encapsulation ppp
Router(config-if)# ppp authentication chap

! Verify
Router# show interfaces serial 0/0/0
  Encapsulation PPP, LCP Open
  Open: IPCP, CDPCP
```

**PPP vs HDLC:**

```
Feature              PPP                     HDLC (Cisco)
------------------------------------------------------------------------
Standard             Yes (RFC 1661)          Cisco proprietary
Authentication       Yes (PAP, CHAP)         No
Multilink            Yes                     No
Compression          Yes                     No
Error Detection      Yes                     Basic
Multi-protocol       Yes                     Yes
Overhead             Higher                  Lower
Use Case             Inter-vendor            Cisco-to-Cisco
------------------------------------------------------------------------
```

---

## 7.4 Modern WAN Connectivity

### Ethernet WAN (Metro Ethernet)

**คำจำกัดความ:**

- Ethernet เป็น WAN technology
- ใช้ Ethernet standards (802.3)
- จาก service provider
- เชื่อม sites ผ่าน provider's Ethernet network

**Characteristics:**

```
✅ Familiar technology (same as LAN)
✅ High bandwidth (10Mbps - 100Gbps)
✅ Simple configuration
✅ Cost-effective
✅ Scalable (easy bandwidth upgrades)
✅ Native Ethernet (no conversion)
```

**Ethernet WAN Services:**

#### E-Line (Ethernet Line)

```
- Point-to-point
- เหมือน leased line
- Dedicated bandwidth

Example:
[Site A] -------- E-Line -------- [Site B]
         (100 Mbps dedicated)
```

#### E-LAN (Ethernet LAN)

```
- Multipoint-to-multipoint
- All sites on same Ethernet segment
- Like being on same LAN

Example:
[Site A] ┐
[Site B] ├──── E-LAN Service
[Site C] ┘
(All sites same Layer 2 domain)
```

#### E-Tree (Ethernet Tree)

```
- Hub-and-spoke
- Root site (hub) talks to all leaf sites
- Leaf sites cannot talk directly

Example:
    [HQ - Root]
      /   \
   [B1]  [B2]
  (Leaf)(Leaf)
```

**Benefits:**

```
✅ High bandwidth
✅ Low latency
✅ Easy to manage
✅ Standard Ethernet interfaces
✅ No protocol conversion
✅ Support VLANs
```

---

### MPLS (Multiprotocol Label Switching)

**คำจำกัดความ:**

- Modern WAN technology
- Label-based forwarding (แทน IP routing)
- Service provider backbone
- Flexible, scalable, efficient

**How MPLS Works:**

```
1. Provider assigns labels to packets
2. Routers forward based on labels (not IP)
3. Fast switching (label lookup faster than IP lookup)
4. Remove label at destination

[CE Router] --- [PE Router] --- [MPLS Core] --- [PE Router] --- [CE Router]
 Customer       Provider         (Label            Provider       Customer
   Edge           Edge           Switching)          Edge           Edge
```

**MPLS Components:**

**LSR (Label Switch Router):**

```
- Core routers in MPLS network
- Forward based on labels
- High-speed switching
```

**LER (Label Edge Router):**

```
- Edge routers (PE routers)
- Add/Remove labels
- Interface with customer
```

**LSP (Label Switched Path):**

```
- Path through MPLS network
- Pre-determined route
- Multiple LSPs possible
```

**Characteristics:**

```
✅ Fast forwarding (label-based)
✅ Traffic engineering (control paths)
✅ QoS support (prioritize traffic)
✅ VPN support (MPLS VPN)
✅ Any-to-any connectivity
✅ Provider-managed
❌ Expensive (but less than multiple leased lines)
```

**MPLS VPN:**

```
- Layer 3 VPN over MPLS
- Isolate customer traffic
- Private routing tables (VRF)
- Secure, scalable

Example:
Company sites connected via MPLS VPN:
[Bangkok HQ] ┐
[Chiang Mai] ├──── MPLS VPN ──── Isolated from other customers
[Phuket]     ┘
```

**Benefits over Traditional WAN:**

```
vs Frame Relay:
  ✅ Higher bandwidth
  ✅ Better QoS
  ✅ Simpler configuration

vs Multiple Leased Lines:
  ✅ Lower cost
  ✅ Any-to-any connectivity
  ✅ Easier management
  ✅ Scalable
```

---

## 7.5 Internet Connectivity

### DSL (Digital Subscriber Line)

**คำจำกัดความ:**

- Broadband over telephone lines
- Always-on connection
- Asymmetric or symmetric speeds

**Types:**

**ADSL (Asymmetric DSL):**

```
Download: 1-8 Mbps (higher)
Upload: 64 Kbps - 1 Mbps (lower)
Distance: Up to 5.5 km from CO
Use: Home, small business
```

**SDSL (Symmetric DSL):**

```
Download: Same as upload
Upload: Same as download
Speed: 192 Kbps - 2.3 Mbps
Use: Business (need upload bandwidth)
```

**VDSL/VDSL2 (Very High-speed DSL):**

```
Speed: Up to 100 Mbps (short distance)
Distance: Up to 1.2 km
Use: High-speed access, IPTV
```

**Characteristics:**

```
✅ Always-on
✅ Use existing phone lines
✅ Affordable
✅ Wide availability
❌ Distance-dependent (speed decreases with distance)
❌ Asymmetric (ADSL)
❌ Slower than cable/fiber
```

**DSL Equipment:**

```
[Computer] --- [Router] --- [DSL Modem] --- [Telephone Line] --- [DSLAM] --- [ISP]
                                            (Existing copper)      (at CO)
```

---

### Cable

**คำจำกัดความ:**

- Broadband over cable TV lines
- Uses coaxial cable
- Shared medium

**DOCSIS (Data Over Cable Service Interface Specification):**

```
DOCSIS 3.0: Up to 1 Gbps download
DOCSIS 3.1: Up to 10 Gbps download
DOCSIS 4.0: Up to 10 Gbps symmetric
```

**Characteristics:**

```
✅ High speed
✅ Always-on
✅ Wide availability (urban/suburban)
✅ Affordable
❌ Shared medium (neighbors share bandwidth)
❌ Variable performance (peak hours slower)
❌ Security concerns (shared cable segment)
```

**Cable Network:**

```
[Computer] --- [Router] --- [Cable Modem] --- [Coax] --- [CMTS] --- [ISP]
                                             (Shared)   (Head-end)
                                               
Neighborhood shares same cable segment
→ More users = slower speeds
```

---

### Fiber (FTTH/FTTX)

**คำจำกัดความ:**

- Fiber optic to premises
- Highest speed available
- Future-proof

**Types:**

**FTTH (Fiber to the Home):**

```
- Fiber all the way to home
- Best performance
- Expensive to deploy
```

**FTTC (Fiber to the Curb):**

```
- Fiber to street cabinet
- Last 100m copper/coax
- Cheaper than FTTH
```

**FTTN (Fiber to the Node/Neighborhood):**

```
- Fiber to neighborhood node
- Last ~500m copper (VDSL)
```

**Characteristics:**

```
✅ Very high speed (100Mbps - 10Gbps)
✅ Symmetric (same upload/download)
✅ Low latency
✅ Reliable
✅ Future-proof
❌ Limited availability (mostly urban)
❌ Expensive (installation)
```

---

## Summary (สรุป)

Module 7 นี้เราได้เรียนรู้:

1. ✅ **WAN Purpose**:
    
    - เชื่อม LANs ข้าม large distances
    - Remote access, cloud services
    - Business continuity
2. ✅ **WAN Devices**:
    
    - CPE: Router, Modem, CSU/DSU
    - Provider: Demarc, Local Loop, CO, Toll Network
3. ✅ **Connection Types**:
    
    - Dedicated (Leased Line)
    - Circuit-Switched (ISDN)
    - Packet-Switched (Frame Relay, ATM, MPLS)
    - Internet-Based (VPN)
4. ✅ **Traditional WAN**:
    
    - T1/E1, T3/E3, SONET/SDH
    - HDLC (Cisco default, proprietary)
    - PPP (standard, authentication: PAP, CHAP)
5. ✅ **Modern WAN**:
    
    - **Ethernet WAN** (Metro Ethernet: E-Line, E-LAN, E-Tree)
    - **MPLS** (label switching, VPN, QoS)
6. ✅ **Internet Connectivity**:
    
    - **DSL** (ADSL, SDSL, VDSL)
    - **Cable** (DOCSIS, shared medium)
    - **Fiber** (FTTH, FTTC, FTTN)

**สิ่งสำคัญที่ต้องจำ:**

- WAN เชื่อม LANs ข้าม distances
- Service providers ให้บริการ WAN
- Leased Line = Dedicated, expensive, reliable
- Packet-Switched = Shared, cost-effective
- HDLC = Cisco default, proprietary
- PPP = Standard, authentication (CHAP recommended)
- MPLS = Modern, flexible, QoS, VPN
- Ethernet WAN = Familiar, high-speed
- DSL = Phone lines, distance-dependent
- Cable = Coax, shared medium
- Fiber = Fastest, symmetric

**Next Module:** Module 8 - VPN and IPsec Concepts

---

**[ไฟล์ Module 7 สมบูรณ์แล้ว!]**