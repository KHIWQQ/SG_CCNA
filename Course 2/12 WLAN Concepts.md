# CCNA 2: Module 12 - WLAN Concepts

## แนวคิด Wireless LAN

---

## สารบัญ

1. [วัตถุประสงค์ของ Module](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#%E0%B8%A7%E0%B8%B1%E0%B8%95%E0%B8%96%E0%B8%B8%E0%B8%9B%E0%B8%A3%E0%B8%B0%E0%B8%AA%E0%B8%87%E0%B8%84%E0%B9%8C%E0%B8%82%E0%B8%AD%E0%B8%87-module)
2. [Introduction to Wireless](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#1-introduction-to-wireless)
3. [Components of WLANs](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#2-components-of-wlans)
4. [WLAN Operation](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#3-wlan-operation)
5. [CAPWAP Operation](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#4-capwap-operation)
6. [Channel Management](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#5-channel-management)
7. [WLAN Threats](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#6-wlan-threats)
8. [Secure WLANs](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#7-secure-wlans)
9. [สรุป](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#%E0%B8%AA%E0%B8%A3%E0%B8%B8%E0%B8%9B)

---

## วัตถุประสงค์ของ Module

เมื่อจบ Module นี้ คุณจะสามารถ:

- ✅ อธิบาย WLAN technology และ standards
- ✅ อธิบาย components ของ WLAN infrastructure
- ✅ อธิบายการทำงานของ wireless technology ใน WLAN
- ✅ อธิบายวิธี WLC ใช้ CAPWAP จัดการ APs
- ✅ อธิบาย channel management ใน WLAN
- ✅ อธิบายภัยคุกคามต่อ WLANs
- ✅ อธิบาย WLAN security mechanisms

---

## 1. Introduction to Wireless

### บทนำเรื่อง Wireless

### 1.1 Benefits of Wireless

**ทำไมต้องใช้ Wireless:**

```
Advantages:
✅ Mobility - เคลื่อนที่ได้อิสระ
✅ Flexibility - ติดตั้งง่าย ไม่ต้องเดินสาย
✅ Cost-effective - ประหยัดค่าสายและแรงงาน
✅ Scalability - เพิ่ม users ได้ง่าย
✅ Productivity - ทำงานได้ทุกที่
✅ Guest access - ให้บริการแขกได้ง่าย

Disadvantages:
❌ Security risks - ความปลอดภัยน้อยกว่า wired
❌ Interference - สัญญาณรบกวนได้
❌ Speed - ช้ากว่า wired (โดยทั่วไป)
❌ Coverage - จำกัดด้วยระยะทาง
❌ Reliability - ผันแปรตามสภาพแวดล้อม
```

### 1.2 Types of Wireless Networks

**Wireless Network Types ตาม IEEE Standards:**

**1. WPAN (Wireless Personal Area Network)**

```
Coverage: 20-30 feet (6-9 meters)
Standard: IEEE 802.15
Frequency: 2.4 GHz
Applications: 
- Bluetooth devices
- Wireless headphones
- Wireless keyboards/mice
- Smartwatches
- IoT sensors (Zigbee)

Examples:
- Bluetooth: 1-3 Mbps, 10-30 feet
- Zigbee: 250 Kbps, low power, mesh network
```

**2. WLAN (Wireless Local Area Network)**

```
Coverage: Up to 300 feet (91 meters)
Standard: IEEE 802.11 (Wi-Fi)
Frequency: 2.4 GHz, 5 GHz, 6 GHz
Applications:
- Home networks
- Office networks
- Campus networks
- Hotspots

Speeds: 2 Mbps to 46 Gbps (ตาม standard)

Focus ของ Module นี้!
```

**3. WMAN (Wireless Metropolitan Area Network)**

```
Coverage: City or district (several miles)
Standard: IEEE 802.16 (WiMAX)
Frequency: Licensed frequencies
Applications:
- City-wide internet
- Connecting multiple buildings
- ISP backhaul
- Rural connectivity

Example: WiMAX
```

**4. WWAN (Wireless Wide Area Network)**

```
Coverage: National/Global
Standard: Cellular standards (3G, 4G, 5G)
Frequency: Licensed cellular frequencies
Applications:
- Mobile phones
- Tablets with cellular
- IoT devices
- Vehicle connectivity

Examples:
- 3G: HSPA - up to 42 Mbps
- 4G: LTE - up to 1 Gbps
- 5G: NR - up to 10+ Gbps
```

**Comparison:**

```
┌─────────┬──────────┬────────────┬─────────────┬──────────┐
│ Type    │ WPAN     │ WLAN       │ WMAN        │ WWAN     │
├─────────┼──────────┼────────────┼─────────────┼──────────┤
│ Range   │ 6-9m     │ ~91m       │ Several km  │ Global   │
│ Standard│ 802.15   │ 802.11     │ 802.16      │ Cellular │
│ Speed   │ Low      │ Medium-High│ Medium      │ Medium   │
│ Use     │ Personal │ LAN        │ City        │ Mobile   │
└─────────┴──────────┴────────────┴─────────────┴──────────┘
```

### 1.3 802.11 Standards

**IEEE 802.11 Standards Evolution:**

**802.11 (1997) - Original**

```
Frequency: 2.4 GHz
Speed: 1-2 Mbps
Technology: FHSS or DSSS
Status: Obsolete

Note: ใช้ไม่ได้แล้วในปัจจุบัน
```

**802.11b (1999) - Wi-Fi 1**

```
Frequency: 2.4 GHz
Speed: Up to 11 Mbps
Technology: HR-DSSS (High Rate Direct Sequence Spread Spectrum)
Range: Longer than 802.11a
Channels: 3 non-overlapping (1, 6, 11)

Advantages:
✅ Better wall penetration
✅ Longer range
✅ Lower cost

Disadvantages:
❌ Slower speed
❌ Interference (2.4 GHz band crowded)

Status: Obsolete
```

**802.11a (1999) - Wi-Fi 2**

```
Frequency: 5 GHz
Speed: Up to 54 Mbps
Technology: OFDM (Orthogonal Frequency Division Multiplexing)
Channels: More non-overlapping channels

Advantages:
✅ Faster speed
✅ Less interference (5 GHz less crowded)
✅ More channels available

Disadvantages:
❌ Shorter range
❌ Poor wall penetration
❌ Not compatible with 802.11b

Status: Legacy
```

**802.11g (2003) - Wi-Fi 3**

```
Frequency: 2.4 GHz
Speed: Up to 54 Mbps
Technology: OFDM
Backward compatible: Yes (with 802.11b)
Channels: 3 non-overlapping (1, 6, 11)

Advantages:
✅ Faster than 802.11b
✅ Better range than 802.11a
✅ Backward compatible

Disadvantages:
❌ Interference (2.4 GHz)
❌ Slowed by 802.11b clients

Status: Legacy
```

**802.11n (2009) - Wi-Fi 4**

```
Frequency: 2.4 GHz และ 5 GHz (Dual-band)
Speed: Up to 600 Mbps
Technology: 
- MIMO (Multiple Input Multiple Output)
- OFDM
- Channel bonding (20/40 MHz)

Channel Width:
- 20 MHz: 150 Mbps (per stream)
- 40 MHz: 300 Mbps (per stream)

Spatial Streams: Up to 4
Maximum: 4 streams × 150 Mbps = 600 Mbps

Advantages:
✅ Much faster
✅ MIMO technology
✅ Better range
✅ Both frequency bands
✅ Backward compatible

Status: Common (still widely used)
```

**802.11ac (2013) - Wi-Fi 5**

```
Frequency: 5 GHz only
Speed: 450 Mbps - 6.9 Gbps
Technology:
- MU-MIMO (Multi-User MIMO)
- OFDM
- Wider channels (80/160 MHz)
- 256-QAM modulation

Channel Width:
- 20 MHz
- 40 MHz
- 80 MHz (common)
- 160 MHz (optional)

Spatial Streams: Up to 8
Wave 1: Up to 3 streams (1.3 Gbps)
Wave 2: Up to 4 streams, MU-MIMO (3.5 Gbps)

Advantages:
✅ Very fast
✅ MU-MIMO (multiple users simultaneously)
✅ Less interference (5 GHz)
✅ Better for HD video, gaming

Status: Current (most common today)
```

**802.11ax (2019) - Wi-Fi 6 / Wi-Fi 6E**

```
Frequency: 
- Wi-Fi 6: 2.4 GHz และ 5 GHz
- Wi-Fi 6E: 2.4 GHz, 5 GHz, และ 6 GHz

Speed: Up to 9.6 Gbps

Technology:
- OFDMA (Orthogonal Frequency Division Multiple Access)
- MU-MIMO (uplink และ downlink)
- 1024-QAM modulation
- BSS Coloring (reduce interference)
- Target Wake Time (TWT) - power saving

Channel Width: 20, 40, 80, 160 MHz

Advantages:
✅ Higher efficiency
✅ Better in dense environments
✅ Lower latency
✅ Better battery life
✅ More simultaneous users
✅ Wi-Fi 6E: 6 GHz band (less congestion)

Status: Latest (rapidly deploying)
```

**802.11be (2024) - Wi-Fi 7**

```
Frequency: 2.4 GHz, 5 GHz, และ 6 GHz
Speed: Up to 46 Gbps

Technology:
- Multi-Link Operation (MLO)
- 320 MHz channels
- 4096-QAM modulation
- Multi-RU (Resource Unit)

Advantages:
✅ Extremely fast
✅ Ultra-low latency (<5 ms)
✅ Best for AR/VR, 8K video
✅ Maximum efficiency

Status: Emerging
```

**Standards Comparison Table:**

```
┌──────────┬──────────┬──────────────┬─────────────┬──────────┬────────────┐
│ Standard │ Wi-Fi    │ Year         │ Frequency   │ Max Speed│ Technology │
├──────────┼──────────┼──────────────┼─────────────┼──────────┼────────────┤
│ 802.11   │ -        │ 1997         │ 2.4 GHz     │ 2 Mbps   │ FHSS/DSSS  │
│ 802.11b  │ Wi-Fi 1  │ 1999         │ 2.4 GHz     │ 11 Mbps  │ DSSS       │
│ 802.11a  │ Wi-Fi 2  │ 1999         │ 5 GHz       │ 54 Mbps  │ OFDM       │
│ 802.11g  │ Wi-Fi 3  │ 2003         │ 2.4 GHz     │ 54 Mbps  │ OFDM       │
│ 802.11n  │ Wi-Fi 4  │ 2009         │ 2.4/5 GHz   │ 600 Mbps │ MIMO       │
│ 802.11ac │ Wi-Fi 5  │ 2013         │ 5 GHz       │ 6.9 Gbps │ MU-MIMO    │
│ 802.11ax │ Wi-Fi 6  │ 2019         │ 2.4/5/6 GHz │ 9.6 Gbps │ OFDMA      │
│ 802.11be │ Wi-Fi 7  │ 2024         │ 2.4/5/6 GHz │ 46 Gbps  │ MLO        │
└──────────┴──────────┴──────────────┴─────────────┴──────────┴────────────┘
```

### 1.4 Radio Frequencies

**Electromagnetic Spectrum:**

```
Radio Waves → Microwaves → Infrared → Visible → UV → X-Rays → Gamma

Wireless Networks ใช้:
- Radio Waves
- Microwaves
```

**Frequency Bands สำหรับ WLAN:**

**2.4 GHz Band (UHF - Ultra High Frequency)**

```
Range: 2.400 - 2.4835 GHz
Bandwidth: 83.5 MHz
Channels: 14 channels (varying by country)
Channel width: 20 MHz (standard), 40 MHz (802.11n+)

Standards:
- 802.11b
- 802.11g
- 802.11n (dual-band)
- 802.11ax (Wi-Fi 6)

Advantages:
✅ Better range
✅ Better wall penetration
✅ Works with all Wi-Fi devices
✅ Lower cost

Disadvantages:
❌ Crowded (Bluetooth, microwaves, cordless phones)
❌ Only 3 non-overlapping channels
❌ Slower speeds
❌ More interference
```

**5 GHz Band (SHF - Super High Frequency)**

```
Range: 5.150 - 5.825 GHz
Bandwidth: 675 MHz
Channels: Up to 24 non-overlapping channels (varies by country)
Channel width: 20, 40, 80, 160 MHz

Standards:
- 802.11a
- 802.11n (dual-band)
- 802.11ac
- 802.11ax (Wi-Fi 6)

Advantages:
✅ Less crowded
✅ More channels
✅ Faster speeds
✅ Less interference

Disadvantages:
❌ Shorter range
❌ Poor wall penetration
❌ Higher cost
❌ More power consumption
```

**6 GHz Band (NEW - Wi-Fi 6E)**

```
Range: 5.925 - 7.125 GHz
Bandwidth: 1200 MHz
Channels: Up to 59 channels

Standards:
- 802.11ax (Wi-Fi 6E)
- 802.11be (Wi-Fi 7)

Advantages:
✅ No legacy devices (clean spectrum)
✅ Many channels
✅ Very fast
✅ Low latency
✅ No interference

Disadvantages:
❌ New (limited device support)
❌ Shortest range
❌ Poorest penetration
```

**Frequency Comparison:**

```
Frequency    Range    Penetration    Speed    Interference    Channels
2.4 GHz      Longest  Best           Slower   High            3
5 GHz        Medium   Medium         Faster   Medium          24
6 GHz        Shortest Worst          Fastest  Low             59
```

### 1.5 Wireless Standards Organizations

**องค์กรที่กำหนด Wireless Standards:**

**1. ITU (International Telecommunication Union)**

```
Role:
- จัดสรร radio spectrum
- ประสานความถี่ระหว่างประเทศ
- กำหนด satellite orbits

Website: www.itu.int
```

**2. IEEE (Institute of Electrical and Electronics Engineers)**

```
Role:
- พัฒนา technical standards
- 802.11 WLAN standards
- กำหนดวิธี modulation
- ระบุ data rates, frequencies

Standards:
- 802.11 family (Wi-Fi)
- 802.15 (WPAN/Bluetooth)
- 802.16 (WiMAX)

Website: www.ieee.org
```

**3. Wi-Fi Alliance**

```
Role:
- ทดสอบและ certify อุปกรณ์
- รับรอง interoperability
- ส่งเสริม Wi-Fi adoption
- Wi-Fi trademarks

Certification:
- Wi-Fi CERTIFIED
- WPA3 certification
- Wi-Fi 6 certification

Website: www.wi-fi.org
```

**Relationship:**

```
IEEE creates standards → Wi-Fi Alliance certifies products

Example:
1. IEEE publishes 802.11ax standard
2. Vendors develop products
3. Wi-Fi Alliance tests products
4. Products get "Wi-Fi 6 CERTIFIED" badge
5. Consumers trust compatibility
```

---

## 2. Components of WLANs

### ส่วนประกอบของ WLAN

### 2.1 WLAN Devices

**อุปกรณ์หลักใน WLAN:**

**1. Wireless Clients (Stations)**

```
อุปกรณ์ที่เชื่อมต่อ WLAN:
- Laptops
- Smartphones
- Tablets
- Smart TVs
- IoT devices
- Gaming consoles

Requirements:
- Wireless NIC (Network Interface Card)
- Wi-Fi support
- Security support (WPA2/WPA3)

Types:
- Built-in wireless
- USB wireless adapters
- PCIe wireless cards
```

**2. Wireless Access Points (APs)**

```
หน้าที่:
- เชื่อมต่อ wireless clients กับ wired network
- Broadcast SSID
- Handle authentication
- Forward traffic

Types:

a) Autonomous AP:
- Standalone operation
- Individual configuration
- Full functionality in device
- Small deployments (SOHO)

b) Lightweight AP (LAP):
- Managed by WLC
- Minimal configuration
- Split MAC architecture
- Enterprise deployments

c) Home Router:
- Combined device
- Router + AP + Switch
- NAT, DHCP built-in
- SOHO use
```

**3. Wireless LAN Controller (WLC)**

```
หน้าที่:
- จัดการ multiple APs
- Centralized configuration
- Security policies
- RF management
- Mobility management

Benefits:
✅ Centralized management
✅ Consistent policies
✅ Easy scaling
✅ Advanced features
✅ Monitoring and troubleshooting

Use: Enterprise networks
```

**4. Wireless Router (SOHO)**

```
Combined functions:
- Router (Layer 3)
- Switch (4-port typical)
- Access Point (wireless)
- NAT/PAT
- DHCP server
- Basic firewall

Popular in:
- Homes
- Small offices
- Small businesses
```

### 2.2 Wireless Antennas

**Antenna Types:**

**1. Omnidirectional Antenna**

```
Pattern: 360° (donut shape)
Range: Equal in all directions
Use: Indoor APs, home routers

Typical Application:
- Ceiling-mounted APs
- General coverage
- Small office/home

Gain: 2-9 dBi typically

┌─────────┐
│   AP    │ ← Antenna
└─────────┘
    ↙ ↓ ↘
   ←  ·  →  360° coverage
    ↖ ↑ ↗
```

**2. Directional Antenna**

```
Pattern: Focused beam
Range: Longer in one direction
Use: Point-to-point links, outdoor

Types:
a) Yagi: Narrow beam, long range
b) Patch/Panel: Medium beam
c) Parabolic: Very narrow, very long

Use Cases:
- Building-to-building
- Outdoor coverage
- Specific area coverage

Gain: 10-24+ dBi

[AP] → ⟩⟩⟩⟩⟩⟩⟩ Focused beam →
```

**Antenna Gain (dBi):**

```
dBi = decibels relative to isotropic

Higher gain:
✅ Longer range
✅ Better signal
❌ Narrower coverage

Lower gain:
✅ Wider coverage
❌ Shorter range

Example:
2 dBi: Wide, short (omnidirectional)
9 dBi: Medium (omnidirectional)
24 dBi: Narrow, long (directional)
```

### 2.3 Wireless Infrastructure Modes

**WLAN Topology Modes:**

**1. Ad Hoc Mode (IBSS - Independent Basic Service Set)**

```
คืออะไร:
- Peer-to-peer connection
- No AP needed
- Direct device-to-device
- Temporary network

Configuration:
- All devices same SSID
- Same channel
- Same security

Topology:
[Laptop 1] ←→ [Laptop 2] ←→ [Laptop 3]
      \           |           /
       \          |          /
        All connected directly

Use Cases:
- File sharing between 2 devices
- Gaming (local multiplayer)
- Temporary connections
- Emergency networks

Limitations:
❌ Limited range
❌ Limited devices (scalability)
❌ No internet access (typically)
❌ Less secure
❌ Manual configuration
```

**2. Infrastructure Mode (BSS - Basic Service Set)**

```
คืออะไร:
- Centralized architecture
- Uses AP
- Most common mode
- Professional networks

Components:
- AP (Access Point)
- Wireless clients
- Wired network connection

Topology:
     [Internet]
         |
    [Router]
         |
      [AP] ← BSA (Basic Service Area)
     /  |  \
  [PC][Phone][Tablet]

BSS Elements:
- SSID: Network name
- BSSID: AP MAC address (unique identifier)
- BSA: Coverage area

Advantages:
✅ Scalable
✅ Centralized management
✅ Internet access
✅ Better security
✅ Better range (can add more APs)
```

**3. Extended Service Set (ESS)**

```
คืออะไร:
- Multiple APs
- Same SSID
- Same security settings
- Seamless roaming

Topology:
      [Wired Network]
      /      |      \
   [AP1]   [AP2]   [AP3]
   BSA1    BSA2    BSA3
     ↓       ↓       ↓
  Overlap Overlap Overlap
     ↓       ↓       ↓
  [Devices roam between APs]

ESS Elements:
- Common SSID: "Company_WiFi"
- Different BSSIDs (different AP MACs)
- Different channels (avoid interference)
- 10-15% overlap between APs

Advantages:
✅ Large coverage area
✅ Seamless roaming
✅ Load balancing
✅ Redundancy

Roaming:
User เดินจาก AP1 coverage → AP2 coverage
- Maintain connection
- Automatic handoff
- Transparent to user
```

**Mode Comparison:**

```
┌─────────────┬───────────┬──────────────┬────────────┐
│ Feature     │ Ad Hoc    │ BSS          │ ESS        │
├─────────────┼───────────┼──────────────┼────────────┤
│ AP Required │ No        │ Yes (1)      │ Yes (many) │
│ Topology    │ Peer      │ Star         │ Extended   │
│ Range       │ Limited   │ Medium       │ Large      │
│ Management  │ Manual    │ Centralized  │ Central    │
│ Roaming     │ No        │ No           │ Yes        │
│ Scalability │ Poor      │ Good         │ Excellent  │
│ Common Use  │ Rare      │ SOHO         │ Enterprise │
└─────────────┴───────────┴──────────────┴────────────┘
```

### 2.4 SSID and BSSID

**SSID (Service Set Identifier):**

```
คืออะไร:
- Wireless network name
- Human-readable
- Up to 32 characters
- Case-sensitive

Example: "CompanyWiFi", "Home_Network_5G"

Purpose:
- Identify network
- User selects SSID to connect
- Multiple APs can share same SSID (ESS)

Broadcasting:
- Beacon frames broadcast SSID
- Can be hidden (not recommended)
```

**BSSID (Basic Service Set Identifier):**

```
คืออะไร:
- Unique identifier per AP
- MAC address of AP radio
- 48-bit address (xx:xx:xx:xx:xx:xx)

Example: 00:1A:2B:3C:4D:5E

Purpose:
- Uniquely identify AP
- Distinguish APs in ESS
- Used in frame headers

Relationship:
ESS: Same SSID
BSS: Different BSSID (each AP)

Example:
SSID: "Office_WiFi" (same for all)
AP1 BSSID: 00:1A:2B:3C:4D:5E
AP2 BSSID: 00:1A:2B:3C:4D:5F
AP3 BSSID: 00:1A:2B:3C:4D:60
```

---

## 3. WLAN Operation

### การทำงานของ WLAN

### 3.1 802.11 Frame Structure

**Wireless Frame Format:**

```
┌────────────┬──────────┬──────────┬──────────┬──────────┬
│ Frame      │ Duration │ Address  │ Address  │ Address  │
│ Control    │          │ 1        │ 2        │ 3        │
│ 2 bytes    │ 2 bytes  │ 6 bytes  │ 6 bytes  │ 6 bytes  │
└────────────┴──────────┴──────────┴──────────┴──────────┴

┬──────────────┬──────────┬──────────┬──────────┬──────────┐
│ Sequence     │ Address  │ Payload  │ FCS      │          │
│ Control      │ 4        │ 0-2304   │ 4 bytes  │          │
│ 2 bytes      │ 6 bytes  │ bytes    │          │          │
┴──────────────┴──────────┴──────────┴──────────┴──────────┘

Fields:
1. Frame Control: Frame type, flags
2. Duration: Time to transmit
3. Address 1-4: MAC addresses (varies by frame type)
4. Sequence Control: Fragment and sequence numbers
5. Payload: Data
6. FCS: Frame Check Sequence (error detection)
```

**Address Fields Usage:**

```
Infrastructure Mode (to AP):
Address 1: Destination MAC (AP)
Address 2: Source MAC (Client)
Address 3: Ultimate destination (wired network)
Address 4: Not used

Infrastructure Mode (from AP):
Address 1: Destination MAC (Client)
Address 2: Source MAC (AP)
Address 3: Original source (wired network)
Address 4: Not used

Ad Hoc Mode:
Address 1: Destination
Address 2: Source
Address 3: BSSID
Address 4: Not used

WDS (Wireless Distribution System):
All 4 addresses used
```

**Frame Types:**

```
1. Management Frames:
- Beacon
- Probe Request/Response
- Authentication
- Association Request/Response
- Deauthentication
- Disassociation

2. Control Frames:
- RTS (Request to Send)
- CTS (Clear to Send)
- ACK (Acknowledgment)

3. Data Frames:
- Actual user data
- Can include QoS
```

### 3.2 CSMA/CA

**CSMA/CA (Carrier Sense Multiple Access with Collision Avoidance):**

```
Purpose:
- Manage access to wireless medium
- Avoid collisions (can't detect collisions in wireless)
- Fair access for all devices

Process:
1. Listen before transmit
2. Wait if busy
3. Random backoff if still busy
4. Transmit when clear
5. Wait for ACK
```

**CSMA/CA Process:**

```
Step 1: Listen to medium
├─ Idle? → Go to Step 2
└─ Busy? → Wait, then random backoff

Step 2: Wait DIFS (Distributed Inter-Frame Space)
- Short wait period

Step 3: Transmit frame

Step 4: Wait for ACK
├─ ACK received? → Success!
└─ No ACK? → Retransmit (up to limit)

Timeline:
Medium: [Busy]────────[DIFS]──[Frame]──[SIFS]──[ACK]
                ↑              ↑        ↑       ↑
              Listen         Transmit  Short  Confirm
                                       wait
```

**Collision Avoidance Mechanisms:**

**1. Inter-Frame Spaces (IFS)**

```
SIFS (Short IFS): 10 μs
- Highest priority
- ACK, CTS

PIFS (PCF IFS): 30 μs
- Medium priority
- PCF (Point Coordination Function)

DIFS (DCF IFS): 50 μs
- Normal priority
- Regular data frames

EIFS (Extended IFS): Longest
- Used after error

Priority: SIFS > PIFS > DIFS > EIFS
```

**2. RTS/CTS (Request to Send / Clear to Send)**

```
Purpose:
- Avoid hidden node problem
- Reserve medium
- Reduce collisions

Process:
1. Node A sends RTS to AP
2. AP responds with CTS (if clear)
3. All stations hear CTS
4. Wait for specified duration
5. Node A transmits data
6. AP sends ACK

Hidden Node Problem:
[Node A] ← → [AP] ← → [Node B]
         ↑           ↑
    Can't hear each other!
    Both might transmit simultaneously

Solution: RTS/CTS
- RTS/CTS heard by AP
- All stations wait
- Collision avoided

Cost: Overhead (reduces throughput)
Use: When needed (configurable threshold)
```

**3. Acknowledgments (ACK)**

```
Purpose:
- Confirm successful reception
- Trigger retransmission if lost

Process:
[Sender] ─[Data]→ [Receiver]
[Sender] ←[ACK]─ [Receiver]

No ACK:
- Assume collision/error
- Retransmit after random backoff
- Up to retry limit (typically 7)
```

### 3.3 Wireless Client and AP Association

**Association Process:**

```
3 Main Steps:
1. Discover AP
2. Authenticate
3. Associate
```

**Step 1: Discovery (Passive or Active)**

**Passive Scanning:**

```
Process:
1. AP broadcasts Beacon frames (every 100 ms typically)
2. Client listens on each channel
3. Client receives beacons
4. Client learns about available APs

Beacon Frame contains:
- SSID
- Supported rates
- Security info
- Channel info
- Capabilities

Timeline:
AP: [Beacon]────[Beacon]────[Beacon]────
     100ms       100ms       100ms

Client: Listens and collects beacons

Advantages:
✅ Power efficient (client only listens)
✅ Less network traffic

Disadvantages:
❌ Slower (must wait for beacons)
```

**Active Scanning:**

```
Process:
1. Client broadcasts Probe Request
2. Includes SSID (or wildcard for all)
3. APs respond with Probe Response
4. Client selects AP

Probe Request:
- Broadcast to FF:FF:FF:FF:FF:FF
- Can specify SSID or ask for all
- Includes supported rates, capabilities

Probe Response:
- Unicast to client
- Contains same info as beacon
- From each AP that hears request

Timeline:
Client: ─[Probe Req]────────────→
AP1:    ←────────[Probe Resp]──
AP2:    ←──────────[Probe Resp]

Advantages:
✅ Faster discovery
✅ Client-initiated

Disadvantages:
❌ More power consumption
❌ More network traffic

Use When:
- SSID not broadcast (hidden network)
- Need quick discovery
- Client actively searching
```

**Step 2: Authentication**

```
Open Authentication (Default):
1. Client sends Authentication Request
2. AP responds Authentication Response (success)
3. No real authentication (legacy)

Process:
Client → [Auth Request] → AP
Client ← [Auth Response: Success] ← AP

Note: Real security happens in Step 3 (Association)
with WPA2/WPA3
```

**Step 3: Association**

```
Process:
1. Client sends Association Request
   - Includes SSID, capabilities
   - Security parameters
   
2. AP validates request
   - Check SSID
   - Check capacity
   - Check security
   
3. AP responds Association Response
   - Association ID (AID)
   - Supported rates
   
4. If WPA2/WPA3:
   - 4-way handshake for encryption keys

Timeline:
Client → [Assoc Request] → AP
Client ← [Assoc Response + AID] ← AP
Client ↔ [4-Way Handshake] ↔ AP
        (if WPA2/WPA3)

Result:
- Client associated
- Can send/receive data
- Encryption enabled
```

**Complete Association Process:**

```
Channel Scanning:
Client: Scan channels 1-11 (2.4 GHz)
        or 36-165 (5 GHz)

Discovery (Passive):
AP: [Beacon]─[Beacon]─[Beacon]
Client: ← Receive beacons

Or Discovery (Active):
Client: [Probe Req] →
AP: ← [Probe Response]

Authentication:
Client → [Auth Request] → AP
Client ← [Auth Success] ← AP

Association:
Client → [Assoc Request] → AP
Client ← [Assoc Success] ← AP

Security (WPA2/WPA3):
Client ↔ [4-Way Handshake] ↔ AP

Connected!
Client ↔ [Data] ↔ AP ↔ Network
```

**Roaming:**

```
Moving between APs in ESS:
1. Signal degrades from current AP
2. Client starts scanning
3. Finds better AP (higher RSSI)
4. Deauthenticate from current AP
5. Authenticate to new AP
6. Associate to new AP
7. Resume data transmission

Fast Roaming (802.11r):
- Pre-authentication
- Faster handoff
- Less interruption
```

---

## 4. CAPWAP Operation

### การทำงานของ CAPWAP

### 4.1 CAPWAP Overview

**CAPWAP (Control and Provisioning of Wireless Access Points):**

```
คืออะไร:
- IEEE standard protocol
- RFC 5415, RFC 5416
- Enables WLC to manage APs
- Encapsulates wireless traffic

Purpose:
✅ Centralized management
✅ Configuration provisioning
✅ Monitoring
✅ Software updates
✅ Security

Architecture:
Lightweight APs + WLC
```

**CAPWAP Benefits:**

```
Centralized Management:
✅ Configure once, deploy many
✅ Consistent policies
✅ Easy updates
✅ Single point of control

Scalability:
✅ Add APs easily
✅ Automatic configuration
✅ Load balancing

Security:
✅ DTLS encryption
✅ Centralized policies
✅ Rogue AP detection

Mobility:
✅ Seamless roaming
✅ Client tracking
✅ Session persistence
```

### 4.2 Split MAC Architecture

**Split MAC Concept:**

```
Traditional AP (Autonomous):
All functions in AP:
- 802.11 encryption
- Beaconing
- Authentication
- Association
- QoS
- Packet bridging
= Fat AP

Split MAC (Lightweight AP + WLC):
Functions split between AP and WLC
= Thin AP

Benefits:
✅ Simpler APs (lower cost)
✅ Centralized intelligence
✅ Easier management
✅ Better coordination
```

**MAC Functions Distribution:**

**AP (Access Point) Functions:**

```
Real-time Functions:
✓ 802.11 beacons and probes
✓ 802.11 MAC layer functions
✓ 802.11 encryption/decryption
✓ RF monitoring
✓ Packet prioritization
✓ Packet buffering and retransmission

Why on AP?
- Time-critical
- Must be local
- RF-related
```

**WLC (Wireless LAN Controller) Functions:**

```
Management Functions:
✓ Authentication
✓ Association/Roaming
✓ Security policies
✓ QoS policies
✓ VLAN tagging
✓ RF management
✓ Client tracking
✓ Software updates

Why on WLC?
- Non-time-critical
- Policy-based
- Centralized coordination
```

**Architecture Diagram:**

```
         [WLC]
           |
    ┌──────┼──────┐
    │      │      │
  [LAP1] [LAP2] [LAP3]
    ↓      ↓      ↓
Wireless Clients

LAP = Lightweight AP
- Minimal config
- Managed by WLC
- CAPWAP tunnels

WLC controls:
- All LAPs
- Client auth
- Policies
- RF management
```

### 4.3 CAPWAP Tunnels

**CAPWAP Tunnel Types:**

**1. Control Tunnel**

```
Purpose:
- Management and control messages
- AP configuration
- Statistics
- Keep-alives

Protocol: UDP
Port: 5246
Encryption: DTLS (Datagram TLS)

Messages:
- Discovery
- Join
- Configuration
- Statistics
- Keep-alive
```

**2. Data Tunnel**

```
Purpose:
- User data traffic
- WLAN frames encapsulation
- Client traffic

Protocol: UDP (default) or DTLS
Port: 5247
Encryption: Optional (DTLS)

Traffic Flow:
Client → AP → [CAPWAP encap] → WLC → Network

Note: Some deployments use local switching
(data doesn't go through WLC)
```

**CAPWAP Header:**

```
┌─────────────────┐
│   IP Header     │ ← L3 (AP to WLC)
├─────────────────┤
│   UDP Header    │ ← Port 5246/5247
├─────────────────┤
│ CAPWAP Header   │ ← Control info
├─────────────────┤
│ 802.11 Frame    │ ← Wireless frame
├─────────────────┤
│   Data          │ ← User data
└─────────────────┘

Encapsulation:
Original 802.11 frame wrapped in CAPWAP
then in IP/UDP for transport across network
```

**CAPWAP Discovery and Join Process:**

```
Step 1: Discovery
AP: Broadcast CAPWAP Discovery Request
WLC: Respond with Discovery Response
     (includes load, capacity info)

Step 2: Join
AP: Send Join Request to selected WLC
WLC: Validate AP (certificate, credentials)
WLC: Send Join Response (accept/reject)

Step 3: Configuration
WLC: Send Configuration to AP
     - SSID settings
     - Security policies
     - RF parameters
     - VLANs

Step 4: Operational
AP: Ready to accept clients
WLC: Monitors and manages AP

Timeline:
AP Boot → Discovery → Join → Config → Run

[AP] ──Discover──> [WLC]
[AP] <──Discovery Response── [WLC]
[AP] ──Join Request──> [WLC]
[AP] <──Join Response── [WLC]
[AP] <──Configuration── [WLC]
[AP] ↔ Operational ↔ [WLC]
```

### 4.4 FlexConnect

**FlexConnect คืออะไร:**

```
AP mode ที่สามารถ:
- Switch traffic locally (at branch)
- Survive WAN outage
- Maintain operations if WLC unreachable

Previous name: HREAP (Hybrid Remote Edge AP)

Use Case:
Branch office with slow WAN to HQ
```

**FlexConnect Modes:**

**1. Connected Mode (Normal)**

```
WAN available:
- AP connected to WLC via CAPWAP
- Full WLC control
- Centralized switching (option)
- Local switching (option)

Topology:
[Branch AP] ──WAN── [WLC at HQ]
     │
  [Clients]

Traffic can be:
- Local switched (stays at branch)
- Central switched (via WLC)
```

**2. Standalone Mode (WAN down)**

```
WAN outage:
- AP continues operation
- Uses cached config
- Local authentication (if configured)
- Local switching

Topology:
[Branch AP] ─X─WAN─X─ [WLC at HQ]
     │                (unreachable)
  [Clients]
     │
  [Local Network] ← Traffic stays local

Capabilities:
✓ Existing clients stay connected
✓ New clients can join (if VLAN cached)
✓ Local switching works
✗ No config changes
✗ No new policies
```

**FlexConnect Benefits:**

```
✅ WAN bandwidth savings (local switching)
✅ Survivability (works without WLC)
✅ Lower latency (local traffic local)
✅ Centralized management (when connected)

Use Cases:
- Branch offices
- Remote sites
- Retail stores
- Locations with limited WAN
```

---

## 5. Channel Management

### การจัดการช่องสัญญาณ

### 5.1 Channels and Frequencies

**2.4 GHz Channels:**

```
Frequency Range: 2.400 - 2.4835 GHz
Channel Width: 22 MHz (actual signal width)
Channel Spacing: 5 MHz (center frequency)
Total Channels: 11 (US), 13 (Europe), 14 (Japan)

Channel List (US):
Channel  Center Frequency   Range
1        2.412 GHz         2.401-2.423 GHz
2        2.417 GHz         2.406-2.428 GHz
3        2.422 GHz         2.411-2.433 GHz
4        2.427 GHz         2.416-2.438 GHz
5        2.432 GHz         2.421-2.443 GHz
6        2.437 GHz         2.426-2.448 GHz
7        2.442 GHz         2.431-2.453 GHz
8        2.447 GHz         2.436-2.458 GHz
9        2.452 GHz         2.441-2.463 GHz
10       2.457 GHz         2.446-2.468 GHz
11       2.462 GHz         2.451-2.473 GHz
```

**Channel Overlap Problem:**

```
22 MHz signal width vs 5 MHz spacing
= Channels overlap!

Example:
Channel 1: 2.401-2.423 GHz
Channel 2: 2.406-2.428 GHz
          ↑ Overlap! ↑

Interference:
Adjacent channels interfere with each other
```

**Non-Overlapping Channels:**

```
Only 3 channels don't overlap:
Channels 1, 6, and 11

Visual:
Ch1 [═══════]
         Ch2 [═══════]  ← Overlaps 1 & 3
                Ch3 [═══════]
                     ...
               Ch6 [═══════]  ← No overlap with 1
                        ...
                           Ch11 [═══════] ← No overlap with 1 or 6

Best Practice:
Use only channels 1, 6, 11 for 2.4 GHz WLANs

Adjacent AP Planning:
AP1: Channel 1
AP2: Channel 6  (no interference)
AP3: Channel 11 (no interference)
AP4: Channel 1  (reuse, if far enough)
```

**5 GHz Channels:**

```
Frequency Range: 5.150 - 5.825 GHz (varies by country)
Channel Width: 20, 40, 80, or 160 MHz
Channel Spacing: 20 MHz
Total Channels: 24+ non-overlapping (20 MHz)

Bands (US):
UNII-1: 36, 40, 44, 48 (indoor only)
UNII-2A: 52, 56, 60, 64 (DFS required)
UNII-2C: 100-144 (DFS required)
UNII-3: 149, 153, 157, 161, 165

Common Channels (20 MHz):
36, 40, 44, 48, 52, 56, 60, 64,
100, 104, 108, 112, 116, 120, 124, 128,
132, 136, 140, 144, 149, 153, 157, 161, 165

Benefits:
✅ No overlap (20 MHz channels)
✅ Many more channels available
✅ Less congestion
✅ Higher speeds possible
```

**DFS (Dynamic Frequency Selection):**

```
Purpose:
- Avoid interfering with radar
- Required on some 5 GHz channels

How it works:
1. AP listens for radar
2. If detected → switch channel
3. Avoid that channel for 30 minutes

Channels requiring DFS (US):
52-144

Impact:
- Initial delay (60 seconds scan)
- Occasional channel switches
- Some clients don't support
```

### 5.2 Channel Planning

**Site Survey และ Planning:**

**Coverage Planning:**

```
Factors to Consider:
- Building size and layout
- Wall materials (affect signal)
- Number of users
- Throughput requirements
- Existing interference

Tools:
- Site survey software
- Spectrum analyzer
- WiFi analyzer apps
- Heat mapping tools
```

**AP Placement Best Practices:**

```
2.4 GHz (Longer range):
✓ Ceiling mount preferred
✓ Central location
✓ 10-15% overlap with neighbors
✓ Use channels 1, 6, 11 only
✓ Power: Adjust for coverage

Coverage Pattern:
  [AP1 - Ch1]     [AP2 - Ch6]     [AP3 - Ch11]
       \             /   \             /
        \___overlap_/     \___overlap_/
              ↑                 ↑
         10-15%            10-15%

5 GHz (Shorter range):
✓ More APs needed
✓ Better for high density
✓ Less penetration
✓ Many channel choices
✓ Higher data rates
```

**Channel Reuse Pattern:**

```
2.4 GHz - Honeycomb Pattern:
    1       6       11      1
       6       11      1       6
    11      1       6       11
       1       6       11      1

Key: Avoid same channel in adjacent APs

5 GHz - More flexible:
    36      40      44      48
       149     153     157     161
    52      56      60      64
       100     104     108     112

Many channels = easier planning
```

**Co-Channel Interference vs Adjacent-Channel Interference:**

```
Co-Channel Interference:
- Same channel on nearby APs
- Reduces throughput (share channel)
- CSMA/CA coordination
- Acceptable if planned

Adjacent-Channel Interference:
- Overlapping channels (e.g., Ch1 & Ch2)
- Causes noise and errors
- Reduces throughput significantly
- Should be AVOIDED!

Solution:
Use only non-overlapping channels (1, 6, 11)
```

**Channel Width Selection:**

```
2.4 GHz:
- 20 MHz only (standard)
- 40 MHz possible but NOT recommended
  (causes massive interference)

5 GHz:
- 20 MHz: Maximum channels, least interference
- 40 MHz: More speed, fewer channels
- 80 MHz: High speed, limited channels (802.11ac)
- 160 MHz: Maximum speed, very few channels

Trade-off:
Wider channel = Higher speed BUT Fewer non-overlapping channels

Recommendation:
- High-density: 20 MHz
- Medium-density: 40 MHz
- Low-density/High-speed: 80 MHz
- Special cases: 160 MHz
```

### 5.3 RF Management

**Automatic RF Management (RRM - Radio Resource Management):**

```
WLC Features:
- Auto channel selection
- Auto power adjustment
- Coverage hole detection
- Interference detection
- Load balancing

Benefits:
✅ Automatic optimization
✅ Adapts to changes
✅ Reduces manual work
✅ Better performance
```

**Power Management:**

```
Transmit Power Control (TPC):
- Adjust AP transmit power
- Prevent overpowering
- Optimize coverage

Goal:
- Adequate coverage
- Minimal overlap
- Reduced interference

Levels (Cisco):
1 = Maximum (20 dBm / 100 mW)
2-8 = Decreasing power
8 = Minimum

Auto-adjust based on:
- Neighbor APs
- Client density
- Interference
```

---

## 6. WLAN Threats

### ภัยคุกคามต่อ WLAN

### 6.1 Wireless Security Concerns

**ทำไม WLAN น้อยกว่า Wired:**

```
Wired Network:
- Physical access required
- Contained within building
- Harder to intercept

Wireless Network:
- Signal extends beyond walls
- Anyone in range can attempt access
- Easier to eavesdrop
- No physical security

Threats:
❌ Unauthorized access
❌ Eavesdropping
❌ Man-in-the-middle
❌ DoS attacks
❌ Rogue APs
```

### 6.2 Common Wireless Attacks

**1. Wireless Intruders (Unauthorized Access)**

```
Wardriving:
- Drive around looking for WLANs
- Map access points
- Find unsecured networks
- Tools: NetStumbler, Kismet

Warwalking:
- Walking version of wardriving
- Mark locations (warchalking)

Prevention:
✓ Strong encryption (WPA3)
✓ Strong passwords
✓ Disable SSID broadcast (limited benefit)
✓ MAC filtering (easily bypassed)
```

**2. Rogue Access Points**

```
คืออะไร:
- Unauthorized AP on network
- Could be:
  * Employee installs own AP
  * Attacker installs AP

Dangers:
❌ Bypass security controls
❌ Unauthorized network access
❌ Data interception
❌ Network breach

Types:
a) Misconfigured Employee AP
   - Good intention, bad execution
   - Weak/no security

b) Evil Twin
   - Attacker-controlled AP
   - Same SSID as legitimate
   - Tricks users to connect

Detection:
✓ Wireless IDS/IPS
✓ WLC rogue AP detection
✓ Regular site surveys
✓ Network monitoring

Prevention:
✓ Policy against unauthorized APs
✓ NAC (Network Admission Control)
✓ 802.1X authentication
✓ Monitoring
```

**3. Man-in-the-Middle (MitM)**

```
Attack Process:
1. Attacker between client and AP
2. Intercepts all traffic
3. Can read/modify data
4. User unaware

Methods:
- Evil Twin AP
- ARP poisoning (over wireless)
- Session hijacking

Data at Risk:
❌ Passwords
❌ Session cookies
❌ Personal data
❌ Financial info

Prevention:
✓ WPA2/WPA3 encryption
✓ VPN
✓ HTTPS (TLS/SSL)
✓ Certificate validation
✓ 802.1X
```

**4. Denial of Service (DoS)**

```
Wireless DoS Types:

a) RF Jamming:
- Flood frequency with noise
- Overwhelm legitimate signal
- Requires specialized equipment

b) Deauthentication Attack:
- Send fake deauth frames
- Disconnect clients
- Unencrypted management frames (802.11)

c) Association Flood:
- Flood AP with association requests
- Exhaust AP resources

d) Disassociation Attack:
- Similar to deauth
- Disconnect clients

Prevention:
✓ Management Frame Protection (802.11w)
✓ Monitoring and detection
✓ Multiple APs (redundancy)
✓ WIPS (Wireless IPS)
```

**5. Eavesdropping**

```
Attack:
- Passively capture wireless traffic
- Analyze packets
- Extract sensitive data

Tools:
- Wireshark
- tcpdump
- Aircrack-ng

Risk:
- Unencrypted networks: HIGH
- WEP: Can be cracked
- WPA/WPA2: Resistant (if strong password)
- WPA3: Best protection

Prevention:
✓ Strong encryption (WPA2/WPA3)
✓ Strong passphrase
✓ VPN
✓ End-to-end encryption (HTTPS)
```

---

## 7. Secure WLANs

### การรักษาความปลอดภัย WLAN

### 7.1 WLAN Authentication Methods

**Authentication Types:**

**1. Open Authentication**

```
Process:
- No password required
- Anyone can connect
- No encryption (by default)

Use:
- Public WiFi (airports, cafes)
- Guest networks

Security: None!

Note: Can combine with captive portal
```

**2. WEP (Wired Equivalent Privacy)**

```
Released: 1999
Encryption: RC4
Key Size: 64-bit or 128-bit

Status: DEPRECATED (broken!)

Vulnerabilities:
❌ Weak encryption
❌ Can be cracked in minutes
❌ Static keys
❌ Known attacks

Do NOT use!
```

**3. WPA (Wi-Fi Protected Access)**

```
Released: 2003
Purpose: Interim solution (after WEP)
Encryption: TKIP (Temporal Key Integrity Protocol)

Improvements over WEP:
✓ Dynamic keys
✓ Stronger encryption
✓ Message integrity check

Status: Deprecated (WPA2 preferred)
```

**4. WPA2 (Wi-Fi Protected Access 2)**

```
Released: 2004
Standard: IEEE 802.11i
Encryption: AES (Advanced Encryption Standard)
Protocol: CCMP (Counter Mode CBC-MAC Protocol)

Two Modes:

a) WPA2-Personal (PSK - Pre-Shared Key):
- Single password for all users
- Good for home/small business
- Simple setup
- Password = authentication

Configuration:
- Set SSID
- Set passphrase (8-63 characters)
- All users use same password

b) WPA2-Enterprise:
- Individual credentials per user
- Uses 802.1X and RADIUS
- Complex but secure
- Suitable for organizations

Components:
- RADIUS server
- 802.1X authentication
- Individual usernames/passwords
- Can use certificates

Security Features:
✓ Strong AES encryption
✓ 4-Way Handshake (key derivation)
✓ PMK (Pairwise Master Key)
✓ PTK (Pairwise Transient Key)

Status: Current standard (widely used)
```

**5. WPA3 (Wi-Fi Protected Access 3)**

```
Released: 2018
Latest standard

Two Modes:

a) WPA3-Personal:
- Uses SAE (Simultaneous Authentication of Equals)
- Replaces PSK 4-way handshake
- Forward secrecy
- Better password protection

Improvements:
✓ Protection against offline dictionary attacks
✓ Forward secrecy (past sessions secure)
✓ Natural password selection (easier)
✓ 192-bit security (optional)

b) WPA3-Enterprise:
- Optional 192-bit mode
- Stronger encryption
- Enhanced security

Benefits:
✓ Best security
✓ Protected Management Frames (required)
✓ Easy Connect (QR code)
✓ Enhanced Open (OWE)

Status: Latest (adoption growing)
```

**Authentication Comparison:**

```
┌──────────┬────────┬────────────┬─────────────┬────────────┐
│ Method   │ Year   │ Encryption │ Security    │ Status     │
├──────────┼────────┼────────────┼─────────────┼────────────┤
│ Open     │ -      │ None       │ None        │ Public use │
│ WEP      │ 1999   │ RC4        │ Very Weak   │ DEPRECATED │
│ WPA      │ 2003   │ TKIP       │ Weak        │ Legacy     │
│ WPA2     │ 2004   │ AES-CCMP   │ Strong      │ Current    │
│ WPA3     │ 2018   │ AES-CCMP   │ Strongest   │ Latest     │
└──────────┴────────┴────────────┴─────────────┴────────────┘

Recommendation:
✅ Use WPA2 (minimum)
✅ Use WPA3 (if available)
❌ Never use WEP
❌ Avoid Open (unless public)
```

### 7.2 WPA2/WPA3 Enterprise (802.1X)

**802.1X Authentication:**

```
Components:
1. Supplicant: Client device
2. Authenticator: AP
3. Authentication Server: RADIUS

Process:
1. Client connects to AP
2. AP blocks traffic (except 802.1X)
3. Client sends credentials to RADIUS
4. RADIUS validates
5. RADIUS informs AP (Accept/Reject)
6. AP allows/denies network access

Diagram:
[Client] ←→ [AP] ←→ [RADIUS Server]
Supplicant  Authenticator  Auth Server
```

**EAP (Extensible Authentication Protocol):**

```
EAP Types:

LEAP (Cisco):
- Lightweight EAP
- Cisco proprietary
- Deprecated (weak)

EAP-FAST (Cisco):
- Flexible Authentication via Secure Tunneling
- No certificates needed
- Cisco proprietary

PEAP (Protected EAP):
- Microsoft/Cisco
- TLS tunnel
- User/password inside tunnel
- Server certificate required
- Common in enterprises

EAP-TLS:
- Most secure
- Mutual authentication
- Client AND server certificates
- Complex deployment
- Best security

EAP-TTLS:
- Tunneled TLS
- Similar to PEAP
- More flexible
```

**RADIUS Configuration (Concept):**

```
RADIUS Server:
- Stores user credentials
- Validates authentication
- Returns authorization info

Example Users:
User: john@company.com
Password: SecurePass123
Group: Employees
VLAN: 10

User: guest@company.com
Password: GuestPass456
Group: Guests
VLAN: 50

AP Configuration:
- Point to RADIUS server IP
- RADIUS shared secret
- 802.1X enabled

Benefits:
✓ Individual user accounts
✓ Centralized management
✓ Audit trails (who connected when)
✓ Dynamic VLAN assignment
✓ Granular access control
```

### 7.3 Additional WLAN Security

**Best Practices:**

**1. SSID Management**

```
✓ Change default SSID
✓ Use descriptive but not revealing name
   Good: "Company_Secure"
   Bad: "Admin_Network_Router_Cisco"

✗ Don't broadcast SSID (limited benefit)
  - Security through obscurity (weak)
  - Causes issues with devices
  - Easily discovered anyway
```

**2. Admin Access**

```
✓ Change default admin password
✓ Use strong password
✓ Disable remote management (if not needed)
✓ Use HTTPS for web interface
✓ Keep firmware updated
✓ Disable WPS (vulnerable)
```

**3. MAC Filtering**

```
Allow only specific MAC addresses

Pros:
✓ Additional layer

Cons:
❌ Easy to spoof MAC
❌ Management overhead
❌ False sense of security

Recommendation:
Use with other security, not alone
```

**4. Guest Network**

```
Isolate guest traffic:
✓ Separate SSID
✓ Separate VLAN
✓ No access to internal resources
✓ Rate limiting
✓ Captive portal (optional)
✓ Time-limited access

Example:
Internal: "Company_Employee" → VLAN 10
Guest: "Company_Guest" → VLAN 99 (isolated)
```

**5. Monitoring**

```
✓ Enable logging
✓ Monitor for rogue APs
✓ Use WIPS (Wireless IPS)
✓ Regular site surveys
✓ Check for unauthorized clients
✓ Review security events
```

**6. Physical Security**

```
✓ Secure AP placement (ceiling, locked areas)
✓ Disable unused ethernet ports on AP
✓ Secure WLC in data center
✓ Cable locks for APs
```

---

## สรุป

### สิ่งที่ได้เรียนรู้ใน Module 12:

✅ **Introduction to Wireless:**

- Types of wireless networks (WPAN, WLAN, WMAN, WWAN)
- 802.11 standards evolution (b/a/g/n/ac/ax/be)
- Frequency bands (2.4 GHz, 5 GHz, 6 GHz)
- Standards organizations (ITU, IEEE, Wi-Fi Alliance)

✅ **Components of WLANs:**

- Wireless clients, APs, WLC
- Antenna types (omnidirectional, directional)
- Infrastructure modes (Ad Hoc, BSS, ESS)
- SSID และ BSSID

✅ **WLAN Operation:**

- 802.11 frame structure
- CSMA/CA operation
- Association process (discovery, authentication, association)
- Passive vs Active scanning

✅ **CAPWAP Operation:**

- Split MAC architecture
- AP functions vs WLC functions
- Control and Data tunnels
- FlexConnect

✅ **Channel Management:**

- 2.4 GHz: 3 non-overlapping channels (1, 6, 11)
- 5 GHz: Many non-overlapping channels
- Channel planning
- RF management

✅ **WLAN Threats:**

- Unauthorized access
- Rogue APs และ Evil Twin
- Man-in-the-middle
- DoS attacks
- Eavesdropping

✅ **Secure WLANs:**

- Authentication methods (Open, WEP, WPA, WPA2, WPA3)
- WPA2-Personal vs WPA2-Enterprise
- 802.1X และ RADIUS
- Security best practices

---

## Key Concepts สรุป

### 802.11 Standards Quick Reference:

```
802.11b: 2.4 GHz, 11 Mbps (Legacy)
802.11a: 5 GHz, 54 Mbps (Legacy)
802.11g: 2.4 GHz, 54 Mbps (Legacy)
802.11n: 2.4/5 GHz, 600 Mbps (Wi-Fi 4)
802.11ac: 5 GHz, 6.9 Gbps (Wi-Fi 5)
802.11ax: 2.4/5/6 GHz, 9.6 Gbps (Wi-Fi 6)
802.11be: 2.4/5/6 GHz, 46 Gbps (Wi-Fi 7)
```

### Channels:

```
2.4 GHz: Use channels 1, 6, 11 only
5 GHz: Many non-overlapping channels available
6 GHz: Wi-Fi 6E (newest)
```

### Security:

```
Best: WPA3
Good: WPA2
Bad: WPA
Never: WEP, Open (without other security)
```

### Architecture:

```
SOHO: Autonomous AP
Enterprise: Lightweight AP + WLC
```

---

## Best Practices

### WLAN Deployment:

```
✅ Site survey before deployment
✅ Use 5 GHz for high-density
✅ 2.4 GHz for coverage
✅ Plan channel reuse (1, 6, 11)
✅ 10-15% AP overlap
✅ Adjust power appropriately
```

### Security:

```
✅ Use WPA2 minimum (WPA3 preferred)
✅ Strong passphrase (20+ characters)
✅ WPA2-Enterprise for organizations
✅ Separate guest network
✅ Regular monitoring
✅ Firmware updates
✅ Disable WPS
```

### Management:

```
✅ WLC for enterprise deployments
✅ Centralized configuration
✅ Automatic RF management
✅ Regular audits
✅ Documentation
```

---

## Common Issues

**Issue 1: Slow Wireless Speeds**

```
Causes:
- Channel interference
- Too far from AP
- Obstacles
- Too many clients
- Old standards

Solutions:
- Change channel
- Add more APs
- Use 5 GHz
- Upgrade to 802.11ac/ax
```

**Issue 2: Connection Drops**

```
Causes:
- Weak signal
- Interference
- Roaming issues
- AP capacity

Solutions:
- Improve coverage
- Adjust power
- Add APs
- Check client limits
```

**Issue 3: Can't Connect**

```
Causes:
- Wrong password
- MAC filtering
- AP at capacity
- Incompatible settings

Solutions:
- Verify credentials
- Check MAC filter
- Check AP capacity
- Verify security mode
```

---

## Lab Activities

### Lab 12.1: Basic WLAN Configuration (Packet Tracer)

- Configure wireless router
- Set SSID and security
- Connect wireless clients
- Verify connectivity

### Lab 12.2: Multiple APs (ESS)

- Configure multiple APs
- Same SSID
- Different channels
- Test roaming

### Lab 12.3: WLAN Security

- Configure WPA2-Personal
- Configure WPA2-Enterprise (concept)
- Test security
- Monitor traffic

---

## คำถามทบทวน

1. 802.11n, ac, ax ใช้ frequency band ใด?
2. MIMO คืออะไร?
3. ความแตกต่างระหว่าง BSS และ ESS?
4. SSID และ BSSID ต่างกันอย่างไร?
5. CSMA/CA ต่างจาก CSMA/CD อย่างไร?
6. Association process มีขั้นตอนอะไรบ้าง?
7. CAPWAP ทำหน้าที่อะไร?
8. ช่องสัญญาณใดใน 2.4 GHz ที่ไม่ทับซ้อนกัน?
9. WPA2-Personal ต่างจาก WPA2-Enterprise อย่างไร?
10. Rogue AP คืออะไร? ป้องกันอย่างไร?

---

## เฉลยคำถาม

1. **Frequency bands:** 802.11n = 2.4/5 GHz; 802.11ac = 5 GHz; 802.11ax = 2.4/5/6 GHz
2. **MIMO:** Multiple Input Multiple Output - ใช้หลาย antennas เพิ่ม throughput
3. **BSS vs ESS:** BSS = 1 AP; ESS = หลาย APs, same SSID, seamless roaming
4. **SSID vs BSSID:** SSID = network name; BSSID = AP MAC address (unique per AP)
5. **CSMA/CA vs CD:** CA = avoid collisions (wireless); CD = detect collisions (wired)
6. **Association:** 1) Discovery (passive/active), 2) Authentication, 3) Association
7. **CAPWAP:** Protocol ที่ WLC ใช้จัดการ lightweight APs
8. **2.4 GHz channels:** 1, 6, และ 11 เท่านั้น (non-overlapping)
9. **Personal vs Enterprise:** Personal = PSK (shared password); Enterprise = 802.1X/RADIUS (individual credentials)
10. **Rogue AP:** Unauthorized AP; ป้องกันด้วย monitoring, NAC, 802.1X, policies

---

**หมายเหตุ:** WLAN เป็นเทคโนโลยีสำคัญในยุคปัจจุบัน การเข้าใจ concepts, standards และ security เป็นพื้นฐานสำคัญสำหรับ network professionals

---

**เอกสารจัดทำโดย:** Claude (Anthropic AI)  
**Module:** CCNA 2 Module 12 - WLAN Concepts  
**Version:** v7.02 SRWE  
**วันที่อัพเดท:** พฤศจิกายน 2025