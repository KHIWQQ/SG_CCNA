## สารบัญ

### 📚 PART 1: NETWORKING FUNDAMENTALS (Course 1)

- [Network Basics](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#network-basics)
- [OSI Model](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#osi-model)
- [TCP/IP Model](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#tcpip-model)
- [Binary & Hexadecimal](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#binary--hexadecimal)
- [IPv4 Addressing](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#ipv4-addressing)
- [Subnetting](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#subnetting)
- [Network Layer](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#network-layer)
- [Transport Layer](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#transport-layer)
- [Application Layer](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#application-layer)
- [Network Security Basics](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#network-security-basics)
- [Building a Small Network](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#building-a-small-network)

### 📚 PART 2: CISCO IOS COMMANDS (Course 2 & 3)

- [โหมดพื้นฐาน](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#%E0%B9%82%E0%B8%AB%E0%B8%A1%E0%B8%94%E0%B8%9E%E0%B8%B7%E0%B9%89%E0%B8%99%E0%B8%90%E0%B8%B2%E0%B8%99)
- [Global Configuration](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#global-configuration)
- [Interface Configuration](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#interface-configuration)
- [VLAN Configuration](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#vlan-configuration)
- [Routing Configuration](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#routing-configuration)
- [DHCP Configuration](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#dhcp-configuration)
- [ACL (Access Control List)](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#acl-access-control-list)
- [NAT (Network Address Translation)](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#nat-network-address-translation)
- [EtherChannel](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#etherchannel)
- [STP (Spanning Tree Protocol)](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#stp-spanning-tree-protocol)
- [HSRP/VRRP/GLBP](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#hsrp-hot-standby-router-protocol)
- [IPv6](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#ipv6-configuration)
- [Wireless LANs](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#wireless-lans)
- [QoS](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#qos-quality-of-service)
- [Network Management](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#network-management)
- [AAA](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#aaa-authentication-authorization-accounting)
- [Security Configuration](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#security-configuration)
- [Troubleshooting](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#troubleshooting-methodology)
- [Configuration Templates](https://claude.ai/chat/ce7ca724-154f-437b-9dae-51af4154dd4b#configuration-templates)

---

# PART 1: NETWORKING FUNDAMENTALS

## Network Basics

### What is a Network?

**Network (เครือข่าย)** = กลุ่มของอุปกรณ์ที่เชื่อมต่อกัน เพื่อแบ่งปันทรัพยากรและข้อมูล

### Network Components

**End Devices (อุปกรณ์ปลายทาง):**

```
- Computers (Desktop, Laptop)
- Servers
- Smartphones, Tablets
- Printers
- IP Phones
- Smart TVs, IoT Devices
```

**Intermediary Devices (อุปกรณ์กลาง):**

```
- Routers        ทำงานที่ Layer 3 (Network Layer)
- Switches       ทำงานที่ Layer 2 (Data Link Layer)
- Hubs           ทำงานที่ Layer 1 (Physical Layer) - เลิกใช้แล้ว
- Wireless APs   Access Points
- Firewalls      Security devices
- IDS/IPS        Intrusion Detection/Prevention Systems
```

**Network Media (สื่อกลางในการส่งข้อมูล):**

```
Copper Cables:
  - UTP (Unshielded Twisted Pair) - Cat5e, Cat6, Cat6a
  - STP (Shielded Twisted Pair)
  - Coaxial Cable (ไม่ค่อยใช้แล้ว)

Fiber Optic:
  - SMF (Single-Mode Fiber) - ระยะทางไกล (40+ km)
  - MMF (Multi-Mode Fiber) - ระยะทางสั้น (2 km)

Wireless:
  - Wi-Fi (802.11 a/b/g/n/ac/ax)
  - Cellular (3G, 4G, 5G)
  - Bluetooth
  - Satellite
```

### Network Types

**LAN (Local Area Network):**

```
- ครอบคลุมพื้นที่เล็ก (บ้าน, อาคาร, campus)
- ความเร็วสูง (1 Gbps - 100 Gbps)
- เจ้าของควบคุมเอง
- ตัวอย่าง: Home network, Office network
```

**WAN (Wide Area Network):**

```
- ครอบคลุมพื้นที่กว้าง (เมือง, ประเทศ, โลก)
- เชื่อม LANs หลายๆ แห่งเข้าด้วยกัน
- มักใช้บริการจาก ISP
- ตัวอย่าง: Internet, Corporate WAN
```

**MAN (Metropolitan Area Network):**

```
- ครอบคลุมเมืองหรือเขตเมือง
- ใหญ่กว่า LAN แต่เล็กกว่า WAN
```

**PAN (Personal Area Network):**

```
- ครอบคลุมพื้นที่ส่วนบุคคล (ไม่กี่เมตร)
- ตัวอย่าง: Bluetooth, USB
```

**WLAN (Wireless LAN):**

```
- LAN ที่ใช้ wireless
- ใช้ Wi-Fi (802.11)
```

### Network Architectures

**Client-Server:**

```
- มี dedicated servers
- Centralized control
- ปลอดภัยกว่า
- เหมาะกับองค์กรขนาดใหญ่
```

**Peer-to-Peer (P2P):**

```
- ไม่มี dedicated server
- แต่ละเครื่องเป็นทั้ง client และ server
- ง่ายต่อการติดตั้ง
- เหมาะกับเครือข่ายขนาดเล็ก (< 10 devices)
```

### Network Topologies

**Physical Topology (โครงสร้างทางกายภาพ):**

```
Bus Topology:
  - ต่อเป็นแนวเดียว
  - ไม่นิยมใช้แล้ว
  
Ring Topology:
  - ต่อเป็นวงกลม
  - Token Ring (ไม่ใช้แล้ว)
  
Star Topology:
  - ต่อเข้า central device (Switch)
  - นิยมใช้มากที่สุด
  - Fault tolerant
  
Extended Star:
  - Hierarchical star
  
Mesh Topology:
  - ทุกอุปกรณ์ต่อกันหมด
  - Redundancy สูง
  - ใช้ใน WAN
  
Partial Mesh:
  - บางอุปกรณ์ต่อกันหมด
  
Hybrid:
  - ผสมหลาย topologies
```

**Logical Topology (การไหลของข้อมูล):**

```
- แสดงว่าข้อมูลไหลอย่างไร
- อาจแตกต่างจาก physical topology
```

---

## OSI Model

### OSI 7 Layers Overview

```
7. Application Layer     |  User Interface
8. Presentation Layer    |  Data Representation
9. Session Layer         |  Interhost Communication
10. Transport Layer       |  End-to-End Connections
11. Network Layer         |  Path Determination & IP
12. Data Link Layer       |  MAC & LLC (Framing)
13. Physical Layer        |  Media, Signal, Binary
```

### Layer 1 - Physical Layer

**หน้าที่:**

- ส่งและรับ raw bits (0s และ 1s)
- กำหนด electrical, mechanical, procedural specifications
- ไม่สนใจความหมายของข้อมูล

**Devices:**

- Cables (UTP, Fiber)
- Hubs
- Repeaters
- Network Interface Cards (NICs)

**Encoding:**

```
Encoding Schemes:
- Manchester Encoding
- Non-Return to Zero (NRZ)
- 4B/5B Encoding
```

**Media:**

```
Copper:
  - Electrical signals
  - Susceptible to EMI/RFI
  - ถูก, ติดตั้งง่าย
  - ระยะทางจำกัด (100m สำหรับ UTP)

Fiber:
  - Light pulses
  - ไม่ได้รับผลจาก EMI
  - ระยะทางไกล
  - ปลอดภัยกว่า
  - แพง

Wireless:
  - Radio waves
  - Mobility
  - Security concerns
```

**PDU:** Bits

### Layer 2 - Data Link Layer

**หน้าที่:**

- Framing
- Physical addressing (MAC address)
- Error detection
- Media access control

**Sub-layers:**

```
LLC (Logical Link Control):
  - ติดต่อกับ Network Layer
  - Frame identification
  
MAC (Media Access Control):
  - Physical addressing
  - Media access methods (CSMA/CD, CSMA/CA)
```

**MAC Address:**

```
Format: 48 bits (6 bytes)
Example: 00:1A:2B:3C:4D:5E

First 24 bits: OUI (Organizationally Unique Identifier) - Vendor
Last 24 bits: Device identifier

Types:
- Unicast: ส่งถึงอุปกรณ์เดียว
- Broadcast: FF:FF:FF:FF:FF:FF (ส่งถึงทุกคน)
- Multicast: ส่งถึงกลุ่ม
```

**Ethernet Frame:**

```
| Preamble | SFD | Dest MAC | Src MAC | Type/Length | Data | FCS |
  7 bytes   1 byte  6 bytes   6 bytes    2 bytes    46-1500  4 bytes
                                                      bytes

Preamble: Synchronization
SFD: Start Frame Delimiter (10101011)
Type: Protocol (0x0800 = IPv4, 0x0806 = ARP, 0x86DD = IPv6)
FCS: Frame Check Sequence (CRC)

Minimum frame size: 64 bytes
Maximum frame size: 1518 bytes (1522 with 802.1Q tag)
```

**Devices:**

- Switches (Layer 2)
- Bridges
- NICs

**Protocols:**

- Ethernet (802.3)
- Wi-Fi (802.11)
- PPP
- HDLC

**PDU:** Frame

### Layer 3 - Network Layer

**หน้าที่:**

- Logical addressing (IP address)
- Routing
- Path determination
- Packet forwarding

**IP Address:**

```
IPv4: 32 bits (4 octets)
Example: 192.168.1.1

IPv6: 128 bits (8 groups)
Example: 2001:0DB8:0000:0001:0000:0000:0000:0001
```

**IPv4 Packet Header:**

```
| Ver | IHL | ToS | Total Length | Identification | Flags | Fragment Offset |
  4 bits 4 bits 8 bits  16 bits       16 bits        3 bits    13 bits

| TTL | Protocol | Header Checksum | Source IP | Destination IP | Options | Data |
  8 bits  8 bits      16 bits         32 bits      32 bits       variable

Ver: Version (4)
IHL: Internet Header Length
ToS: Type of Service (DSCP)
TTL: Time to Live (hop count)
Protocol: Upper layer protocol (6=TCP, 17=UDP, 1=ICMP)
```

**Devices:**

- Routers (Layer 3)
- Layer 3 Switches
- Firewalls

**Protocols:**

- IP (IPv4, IPv6)
- ICMP
- ARP
- Routing protocols (RIP, OSPF, EIGRP, BGP)

**PDU:** Packet

### Layer 4 - Transport Layer

**หน้าที่:**

- Segmentation and Reassembly
- End-to-end delivery
- Flow control
- Error recovery
- Multiplexing (Port numbers)

**Port Numbers:**

```
0-1023:     Well-known ports (System ports)
1024-49151: Registered ports (User ports)
49152-65535: Dynamic/Private ports (Ephemeral)
```

**TCP (Transmission Control Protocol):**

```
Characteristics:
- Connection-oriented
- Reliable delivery
- Flow control
- Error checking
- Ordered delivery
- Three-way handshake

TCP Header:
| Source Port | Dest Port | Sequence Number | Acknowledgment Number |
  16 bits       16 bits      32 bits           32 bits

| Data Offset | Flags | Window | Checksum | Urgent Pointer | Options | Data |
  4 bits       6-12 bits 16 bits  16 bits    16 bits        variable

Flags:
- SYN: Synchronize
- ACK: Acknowledgment
- FIN: Finish
- RST: Reset
- PSH: Push
- URG: Urgent

Three-Way Handshake:
1. Client → Server: SYN
2. Server → Client: SYN-ACK
3. Client → Server: ACK

Connection Termination:
1. A → B: FIN
2. B → A: ACK
3. B → A: FIN
4. A → B: ACK
```

**UDP (User Datagram Protocol):**

```
Characteristics:
- Connectionless
- Unreliable (best effort)
- No flow control
- No error recovery
- Faster than TCP
- Lower overhead

UDP Header:
| Source Port | Dest Port | Length | Checksum | Data |
  16 bits       16 bits     16 bits  16 bits

Use Cases:
- DNS queries
- DHCP
- TFTP
- VoIP
- Video streaming
- Online gaming
```

**PDU:** Segment (TCP) / Datagram (UDP)

### Layer 5 - Session Layer

**หน้าที่:**

- สร้าง, จัดการ, ยกเลิก sessions
- Dialog control (full-duplex, half-duplex)
- Synchronization

**Protocols:**

- NetBIOS
- RPC (Remote Procedure Call)
- PPTP (Point-to-Point Tunneling Protocol)

**PDU:** Data

### Layer 6 - Presentation Layer

**หน้าที่:**

- Data formatting
- Encryption/Decryption
- Compression
- Translation

**Functions:**

```
- ASCII to EBCDIC conversion
- JPEG, GIF, PNG (image formats)
- MPEG, MP4 (video formats)
- SSL/TLS encryption
- Data compression
```

**PDU:** Data

### Layer 7 - Application Layer

**หน้าที่:**

- User interface
- Application services
- Network resource access

**Protocols:**

```
HTTP/HTTPS   - Web browsing
FTP/SFTP     - File transfer
SMTP/POP3    - Email
            /IMAP
DNS          - Domain name resolution
DHCP         - IP address assignment
Telnet/SSH   - Remote access
SNMP         - Network management
TFTP         - Trivial file transfer
NTP          - Time synchronization
```

**PDU:** Data

### OSI Encapsulation Process

```
Application Layer     Data
     ↓
Presentation Layer    Data
     ↓
Session Layer         Data
     ↓
Transport Layer       Segment/Datagram (Add L4 Header: Port numbers)
     ↓
Network Layer         Packet (Add L3 Header: IP addresses)
     ↓
Data Link Layer       Frame (Add L2 Header & Trailer: MAC addresses)
     ↓
Physical Layer        Bits (Convert to signals)
```

**De-encapsulation:**

```
Physical → Data Link → Network → Transport → Application
(รับ bits → Frame → Packet → Segment → Data)
```

---

## TCP/IP Model

### TCP/IP 4 Layers

```
OSI Model               TCP/IP Model
-------------------------------------------
7. Application    |
8. Presentation   |  → Application Layer
9. Session        |

10. Transport      |  → Transport Layer

11. Network        |  → Internet Layer

12. Data Link      |
13. Physical       |  → Network Access Layer
```

### Comparison: OSI vs TCP/IP

```
Feature          OSI Model        TCP/IP Model
-------------------------------------------------
Layers           7                4
Development      ISO              DoD/ARPANET
Usage            Reference        Practical
Protocols        Protocol-ind.    Protocol-specific
Layer naming     Standardized     Varied
```

### TCP/IP Protocol Suite

**Application Layer:**

- HTTP, HTTPS, FTP, SMTP, POP3, IMAP, DNS, DHCP, Telnet, SSH, SNMP

**Transport Layer:**

- TCP, UDP

**Internet Layer:**

- IP (IPv4, IPv6), ICMP, ARP, IGMP

**Network Access Layer:**

- Ethernet, Wi-Fi, PPP, Frame Relay

---

## Binary & Hexadecimal

### Binary Number System

**Base 2:** Uses only 0 and 1

**Positional Values (8-bit):**

```
Position:  7    6    5    4    3    2    1    0
Value:    128   64   32   16    8    4    2    1
Binary:    2⁷   2⁶   2⁵   2⁴   2³   2²   2¹   2⁰
```

**Example: 192 in Binary**

```
192 = 128 + 64
    = 1×128 + 1×64 + 0×32 + 0×16 + 0×8 + 0×4 + 0×2 + 0×1
    = 11000000
```

### Binary to Decimal Conversion

**Method:**

```
Binary: 10101100

Position: 7  6  5  4  3  2  1  0
Value:   128 64 32 16  8  4  2  1
Binary:   1  0  1  0  1  1  0  0
          ↓     ↓     ↓  ↓
Result = 128 + 32 + 8 + 4 = 172
```

### Decimal to Binary Conversion

**Method 1: Subtraction**

```
Convert 172 to binary:

172 ≥ 128? Yes → 1, remainder = 44
44  ≥ 64?  No  → 0
44  ≥ 32?  Yes → 1, remainder = 12
12  ≥ 16?  No  → 0
12  ≥ 8?   Yes → 1, remainder = 4
4   ≥ 4?   Yes → 1, remainder = 0
0   ≥ 2?   No  → 0
0   ≥ 1?   No  → 0

Result: 10101100
```

**Method 2: Division by 2**

```
172 ÷ 2 = 86 remainder 0
86  ÷ 2 = 43 remainder 0
43  ÷ 2 = 21 remainder 1
21  ÷ 2 = 10 remainder 1
10  ÷ 2 = 5  remainder 0
5   ÷ 2 = 2  remainder 1
2   ÷ 2 = 1  remainder 0
1   ÷ 2 = 0  remainder 1

Read from bottom to top: 10101100
```

### Hexadecimal Number System

**Base 16:** Uses 0-9 and A-F

**Hex to Decimal:**

```
Hex:    0  1  2  3  4  5  6  7  8  9  A  B  C  D  E  F
Dec:    0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15
```

**Example: 0x2A in Decimal**

```
0x2A = 2×16¹ + 10×16⁰
     = 32 + 10
     = 42
```

**Binary to Hex Conversion:**

```
Group binary in 4s from right:

Binary: 10101100
Group:  1010  1100
Hex:     A     C

Result: 0xAC
```

**Hex to Binary:**

```
Hex: 0x5F
     5    F
    0101 1111

Result: 01011111
```

### IPv4 Address in Different Formats

**Example: 192.168.1.1**

```
Dotted Decimal: 192.168.1.1

Binary:         11000000.10101000.00000001.00000001

Hexadecimal:    0xC0.0xA8.0x01.0x01
```

---

## IPv4 Addressing

### IPv4 Address Structure

**Format:** 32 bits (4 octets)

```
Example: 192.168.1.1

Binary: 11000000.10101000.00000001.00000001

Each octet: 0-255 (8 bits)
```

### IPv4 Address Classes

**Class A:**

```
Range:      1.0.0.0 - 126.255.255.255
First bit:  0
Default Mask: 255.0.0.0 (/8)
Networks:   126 (128 - network 0 - network 127)
Hosts/Net:  16,777,214 (2²⁴ - 2)
Usage:      Very large networks

Format: N.H.H.H (N = Network, H = Host)
```

**Class B:**

```
Range:      128.0.0.0 - 191.255.255.255
First bits: 10
Default Mask: 255.255.0.0 (/16)
Networks:   16,384
Hosts/Net:  65,534 (2¹⁶ - 2)
Usage:      Medium to large networks

Format: N.N.H.H
```

**Class C:**

```
Range:      192.0.0.0 - 223.255.255.255
First bits: 110
Default Mask: 255.255.255.0 (/24)
Networks:   2,097,152
Hosts/Net:  254 (2⁸ - 2)
Usage:      Small networks

Format: N.N.N.H
```

**Class D (Multicast):**

```
Range:      224.0.0.0 - 239.255.255.255
First bits: 1110
Usage:      Multicast groups
```

**Class E (Reserved):**

```
Range:      240.0.0.0 - 255.255.255.255
First bits: 1111
Usage:      Research & experimental
```

### Special IPv4 Addresses

**Network Address:**

```
- All host bits = 0
- Identifies the network
- Cannot be assigned to hosts
Example: 192.168.1.0/24
```

**Broadcast Address:**

```
- All host bits = 1
- Sends to all hosts in network
- Cannot be assigned to hosts
Example: 192.168.1.255/24
```

**Loopback:**

```
127.0.0.0/8
127.0.0.1 - commonly used
Testing internal TCP/IP stack
```

**Private IP Addresses (RFC 1918):**

```
Class A: 10.0.0.0/8        (10.0.0.0 - 10.255.255.255)
Class B: 172.16.0.0/12     (172.16.0.0 - 172.31.255.255)
Class C: 192.168.0.0/16    (192.168.0.0 - 192.168.255.255)

- Used internally
- Not routable on Internet
- Requires NAT for Internet access
```

**APIPA (Automatic Private IP Addressing):**

```
169.254.0.0/16
- Auto-assigned when DHCP fails
- Link-local only
- Cannot route
```

**Link-Local:**

```
169.254.0.0/16 (IPv4)
FE80::/10 (IPv6)
```

**Default Route:**

```
0.0.0.0/0
Matches any destination
```

### Subnet Mask

**Purpose:**

- แยก Network portion และ Host portion
- กำหนดขนาดของ subnet

**Format:**

```
Dotted Decimal: 255.255.255.0
CIDR Notation:  /24
Binary:         11111111.11111111.11111111.00000000

Network bits = 1
Host bits = 0
```

**Common Subnet Masks:**

```
CIDR    Subnet Mask         Binary (last octet)
/24     255.255.255.0       00000000  (256 addresses)
/25     255.255.255.128     10000000  (128 addresses)
/26     255.255.255.192     11000000  (64 addresses)
/27     255.255.255.224     11100000  (32 addresses)
/28     255.255.255.240     11110000  (16 addresses)
/29     255.255.255.248     11111000  (8 addresses)
/30     255.255.255.252     11111100  (4 addresses)
/31     255.255.255.254     11111110  (2 addresses) - Point-to-point
/32     255.255.255.255     11111111  (1 address) - Host route
```

### CIDR (Classless Inter-Domain Routing)

**CIDR Notation:**

```
Format: IP Address/Prefix Length
Example: 192.168.1.0/24

/24 = 24 network bits, 8 host bits
```

**Benefits:**

- More efficient IP allocation
- Reduces routing table size
- Supports VLSM

---

## Subnetting

### Why Subnet?

**Reasons:**

```
1. Efficient IP address utilization
2. Reduce broadcast domains
3. Improve security
4. Better management
5. Control network traffic
```

### Subnetting Formula

```
Number of Subnets:     2ⁿ (n = borrowed bits)
Number of Hosts/Subnet: 2ʰ - 2 (h = host bits)

-2 because:
  - 1 address for network
  - 1 address for broadcast
```

### Subnet Calculation Steps

**Given: 192.168.1.0/24, need 4 subnets**

**Step 1: Determine bits to borrow**

```
2ⁿ ≥ 4
2² = 4 subnets
Borrow 2 bits
```

**Step 2: New subnet mask**

```
Original: /24 (255.255.255.0)
New: /24 + 2 = /26 (255.255.255.192)
```

**Step 3: Calculate block size**

```
Block size = 256 - subnet mask value
           = 256 - 192
           = 64
```

**Step 4: List subnets**

```
Subnet 0: 192.168.1.0/26
  Network:    192.168.1.0
  First Host: 192.168.1.1
  Last Host:  192.168.1.62
  Broadcast:  192.168.1.63

Subnet 1: 192.168.1.64/26
  Network:    192.168.1.64
  First Host: 192.168.1.65
  Last Host:  192.168.1.126
  Broadcast:  192.168.1.127

Subnet 2: 192.168.1.128/26
  Network:    192.168.1.128
  First Host: 192.168.1.129
  Last Host:  192.168.1.190
  Broadcast:  192.168.1.191

Subnet 3: 192.168.1.192/26
  Network:    192.168.1.192
  First Host: 192.168.1.193
  Last Host:  192.168.1.254
  Broadcast:  192.168.1.255
```

### Subnetting Examples

**Example 1: 192.168.10.0/24 → 8 subnets**

```
2³ = 8 subnets
Borrow 3 bits
New mask: /27 (255.255.255.224)
Block size: 256 - 224 = 32
Hosts/subnet: 2⁵ - 2 = 30

Subnets:
192.168.10.0/27     (0-31)
192.168.10.32/27    (32-63)
192.168.10.64/27    (64-95)
192.168.10.96/27    (96-127)
192.168.10.128/27   (128-159)
192.168.10.160/27   (160-191)
192.168.10.192/27   (192-223)
192.168.10.224/27   (224-255)
```

**Example 2: 172.16.0.0/16 → 100 subnets**

```
2⁷ = 128 subnets (enough for 100)
Borrow 7 bits from 3rd octet
New mask: /23 (255.255.254.0)
Block size: 256 - 254 = 2 (in 3rd octet)
Hosts/subnet: 2⁹ - 2 = 510

First few subnets:
172.16.0.0/23       (172.16.0.0 - 172.16.1.255)
172.16.2.0/23       (172.16.2.0 - 172.16.3.255)
172.16.4.0/23       (172.16.4.0 - 172.16.5.255)
...
```

**Example 3: Point-to-Point Link**

```
Need: 2 usable IPs
Use: /30 (255.255.255.252)
Hosts: 2³⁰ - 2 = 2

Example: 10.1.1.0/30
Network:   10.1.1.0
Router 1:  10.1.1.1
Router 2:  10.1.1.2
Broadcast: 10.1.1.3
```

### VLSM (Variable Length Subnet Mask)

**Concept:**

- Different subnet masks for different subnets
- More efficient IP utilization
- Supported by modern routing protocols (OSPF, EIGRP, BGP)

**Example: 192.168.1.0/24**

```
Requirements:
- LAN A: 100 hosts
- LAN B: 50 hosts
- LAN C: 25 hosts
- Links: 3 point-to-point (2 hosts each)

Solution:
LAN A:  192.168.1.0/25    (126 hosts) - 128 addresses
LAN B:  192.168.1.128/26  (62 hosts)  - 64 addresses
LAN C:  192.168.1.192/27  (30 hosts)  - 32 addresses
Link 1: 192.168.1.224/30  (2 hosts)   - 4 addresses
Link 2: 192.168.1.228/30  (2 hosts)   - 4 addresses
Link 3: 192.168.1.232/30  (2 hosts)   - 4 addresses

Total used: 128+64+32+4+4+4 = 236 out of 256
```

**VLSM Steps:**

```
1. List requirements from largest to smallest
2. Assign subnets starting from the beginning
3. Use appropriate mask for each requirement
4. Ensure no overlap
```

### Supernetting (Route Aggregation)

**Concept:**

- Combine multiple networks into one larger network
- Reduces routing table size
- Opposite of subnetting

**Example:**

```
Combine:
192.168.0.0/24
192.168.1.0/24
192.168.2.0/24
192.168.3.0/24

Into: 192.168.0.0/22

Binary:
192.168.00000000.0   (/24)
192.168.00000001.0   (/24)
192.168.00000010.0   (/24)
192.168.00000011.0   (/24)
         ^^^^^^
       Common bits = 22

Result: 192.168.0.0/22
```

---

## Network Layer

### IP (Internet Protocol)

**Characteristics:**

- Connectionless
- Best effort delivery (unreliable)
- No acknowledgments
- No flow control
- No error recovery

**Functions:**

```
1. Addressing (IP addresses)
2. Routing (path determination)
3. Fragmentation (MTU handling)
4. Encapsulation
```

### Routing

**Static Routing:**

```
Pros:
- Simple configuration
- Predictable
- No routing protocol overhead
- More secure

Cons:
- Not scalable
- Manual configuration
- No automatic failover
- Admin overhead

Use case: Small networks, stub networks
```

**Dynamic Routing:**

```
Pros:
- Automatic route discovery
- Adapts to topology changes
- Scalable
- Load balancing

Cons:
- More complex
- Uses bandwidth
- Uses CPU/memory
- Security concerns

Use case: Large networks, redundant paths
```

### Routing Protocols Classification

**By Algorithm:**

```
Distance Vector:
- Uses hop count or metrics
- Periodic updates
- Sends entire routing table
- Examples: RIP, EIGRP (hybrid)

Link State:
- Uses topology database
- Event-triggered updates
- Sends only changes
- Fast convergence
- Examples: OSPF, IS-IS

Path Vector:
- Uses path attributes
- Example: BGP
```

**By Autonomy:**

```
IGP (Interior Gateway Protocol):
- Within same AS
- Examples: RIP, OSPF, EIGRP

EGP (Exterior Gateway Protocol):
- Between different AS
- Example: BGP
```

### ARP (Address Resolution Protocol)

**Purpose:** Map IP address to MAC address

**Operation:**

```
1. Host A wants to send to Host B (knows IP, needs MAC)
2. Host A sends ARP Request (broadcast):
   "Who has 192.168.1.10? Tell 192.168.1.5"
3. Host B replies with ARP Reply (unicast):
   "192.168.1.10 is at 00:11:22:33:44:55"
4. Host A caches MAC in ARP table
5. Communication proceeds using MAC address
```

**ARP Table:**

```
IP Address      MAC Address           Type
192.168.1.1     00:11:22:33:44:55    Dynamic
192.168.1.2     00:AA:BB:CC:DD:EE    Dynamic

Entry timeout: Usually 2-10 minutes
```

**ARP Types:**

```
- Gratuitous ARP: Announce own IP/MAC (duplicate detection)
- Proxy ARP: Router responds on behalf of another device
- Reverse ARP (RARP): Find IP from MAC (obsolete)
```

### ICMP (Internet Control Message Protocol)

**Purpose:** Error reporting and diagnostics

**Common ICMP Messages:**

```
Type 0:  Echo Reply (Ping reply)
Type 3:  Destination Unreachable
  Code 0: Network unreachable
  Code 1: Host unreachable
  Code 2: Protocol unreachable
  Code 3: Port unreachable
  Code 4: Fragmentation needed
Type 5:  Redirect
Type 8:  Echo Request (Ping request)
Type 11: Time Exceeded (TTL=0)
Type 12: Parameter Problem
```

**Ping:**

```
Uses ICMP Echo Request/Reply
Tests connectivity
Measures round-trip time (RTT)

Example:
ping 192.168.1.1
Reply from 192.168.1.1: bytes=32 time<1ms TTL=64
```

**Traceroute:**

```
Uses ICMP Time Exceeded messages
Shows path to destination
Increments TTL to discover routers

Windows: tracert
Linux/Mac: traceroute
```

---

## Transport Layer

### Port Numbers

**Well-Known Ports (0-1023):**

```
20/21   FTP (Data/Control)
22      SSH
23      Telnet
25      SMTP
53      DNS
67/68   DHCP (Server/Client)
69      TFTP
80      HTTP
110     POP3
143     IMAP
161/162 SNMP (Agent/Manager)
443     HTTPS
```

**Registered Ports (1024-49151):**

```
1433    Microsoft SQL
1521    Oracle
3306    MySQL
3389    RDP
5060    SIP
8080    HTTP Alternate
```

**Dynamic Ports (49152-65535):**

```
Ephemeral ports
Client-side ports
Temporary assignments
```

### Socket

**Format:** IP:Port

```
Examples:
192.168.1.10:80    (Web server)
192.168.1.10:443   (HTTPS server)
10.0.0.5:12345     (Client connection)
```

### TCP vs UDP

**TCP:**

```
✓ Connection-oriented
✓ Reliable
✓ Ordered delivery
✓ Flow control
✓ Error checking
✓ Congestion control
✗ Higher overhead
✗ Slower

Use cases:
- HTTP/HTTPS
- FTP
- SSH
- Email
- File transfers
```

**UDP:**

```
✓ Connectionless
✓ Faster
✓ Lower overhead
✓ No handshake delay
✗ Unreliable
✗ No flow control
✗ No ordering
✗ No error recovery

Use cases:
- DNS
- DHCP
- TFTP
- VoIP
- Video streaming
- Online gaming
```

### TCP Features

**Flow Control:**

```
- Window size mechanism
- Prevents receiver overflow
- Sliding window protocol
```

**Congestion Control:**

```
- Slow start
- Congestion avoidance
- Fast retransmit
- Fast recovery
```

**Error Detection:**

```
- Checksum
- Acknowledgments
- Retransmissions
- Sequence numbers
```

---

## Application Layer

### Common Protocols

**HTTP (HyperText Transfer Protocol):**

```
Port: 80
Purpose: Web browsing
Methods: GET, POST, PUT, DELETE, HEAD
Stateless protocol
Plain text (not secure)
```

**HTTPS (HTTP Secure):**

```
Port: 443
HTTP over SSL/TLS
Encrypted communication
Certificate-based authentication
```

**DNS (Domain Name System):**

```
Port: 53 (UDP for queries, TCP for zone transfers)
Purpose: Resolve domain names to IP addresses

Query Types:
- A record: IPv4 address
- AAAA record: IPv6 address
- MX record: Mail server
- CNAME: Canonical name (alias)
- PTR: Reverse DNS
- NS: Name server
- SOA: Start of Authority

DNS Hierarchy:
Root (.)
  → TLD (.com, .org, .net)
    → Second Level (google.com)
      → Subdomain (www.google.com)

Example Query:
www.google.com
1. Client → Local DNS Server
2. Local DNS → Root Server → .com TLD Server → google.com NS
3. Response: 142.250.185.46
4. Client caches result
```

**DHCP (Dynamic Host Configuration Protocol):**

```
Port: 67 (Server), 68 (Client)
Purpose: Automatic IP configuration

DORA Process:
1. Discover: Client broadcasts "I need an IP"
2. Offer: Server offers IP address
3. Request: Client requests the offered IP
4. Acknowledge: Server confirms assignment

DHCP provides:
- IP address
- Subnet mask
- Default gateway
- DNS servers
- Domain name
- Lease time
```

**FTP (File Transfer Protocol):**

```
Port: 20 (Data), 21 (Control)
Purpose: File transfer
Authentication required
Active vs Passive modes

Active FTP:
- Client opens random port
- Server initiates data connection

Passive FTP:
- Server opens random port
- Client initiates data connection (firewall-friendly)
```

**TFTP (Trivial FTP):**

```
Port: 69 (UDP)
Purpose: Simple file transfer
No authentication
No directory listing
Smaller than FTP
Use: IOS upgrades, configuration backup
```

**SMTP (Simple Mail Transfer Protocol):**

```
Port: 25
Purpose: Send email (client to server, server to server)
Push protocol
```

**POP3 (Post Office Protocol v3):**

```
Port: 110
Purpose: Retrieve email
Downloads and deletes from server
Pull protocol
```

**IMAP (Internet Message Access Protocol):**

```
Port: 143
Purpose: Retrieve email
Keeps email on server
Synchronization across devices
More features than POP3
```

**Telnet:**

```
Port: 23
Purpose: Remote terminal access
Plain text (not secure!)
Replaced by SSH
```

**SSH (Secure Shell):**

```
Port: 22
Purpose: Secure remote access
Encrypted
Authentication methods:
- Password
- Public key
Replaces Telnet, rlogin
```

**SNMP (Simple Network Management Protocol):**

```
Port: 161 (Agent), 162 (Manager - Traps)
Purpose: Network management

Components:
- Manager: Management station
- Agent: Managed device
- MIB: Management Information Base

Versions:
- SNMPv1: No security
- SNMPv2c: Community strings
- SNMPv3: Authentication & encryption (recommended)

Operations:
- GET: Retrieve information
- SET: Modify configuration
- TRAP: Asynchronous notification
```

**NTP (Network Time Protocol):**

```
Port: 123 (UDP)
Purpose: Time synchronization
Stratum levels (0-15)
Accuracy: Milliseconds

Stratum 0: Atomic clocks, GPS
Stratum 1: Directly connected to Stratum 0
Stratum 2+: Synchronized to higher stratum
```

---

## Network Security Basics

### Security Threats

**Types of Attacks:**

```
Reconnaissance:
- Port scanning
- Ping sweeps
- DNS queries
- SNMP queries

Access Attacks:
- Password attacks (brute force, dictionary)
- Trust exploitation
- Port redirection
- Man-in-the-middle

Denial of Service (DoS):
- Ping flood
- SYN flood
- DDoS (Distributed DoS)
- Smurf attack

Data Attacks:
- Packet sniffing
- Spoofing
- Session hijacking
```

### Security Measures

**Physical Security:**

```
- Lock server rooms
- Cable locks
- Secure disposal
- Environmental controls
```

**Password Security:**

```
Best Practices:
- Minimum 8 characters
- Mix: uppercase, lowercase, numbers, symbols
- No dictionary words
- Change regularly
- Don't reuse
- Don't share
- Use password managers
```

**Authentication Methods:**

```
Something you know:  Password, PIN
Something you have:  Token, Smart card
Something you are:   Biometrics (fingerprint, face)

Multi-Factor Authentication (MFA):
- Combines 2+ methods
- More secure
```

**Encryption:**

```
Symmetric:
- Same key for encrypt/decrypt
- Fast
- Key distribution challenge
- Examples: AES, 3DES

Asymmetric:
- Public/Private key pair
- Slower
- Used for key exchange
- Examples: RSA, ECC

Hashing:
- One-way function
- Integrity verification
- Examples: MD5 (weak), SHA-256
```

**Firewall:**

```
Types:
- Packet filtering
- Stateful inspection
- Application layer (proxy)
- Next-generation (NGFW)

Functions:
- Block unauthorized access
- Allow legitimate traffic
- NAT
- VPN termination
- Logging
```

**ACLs (Access Control Lists):**

```
Purpose: Filter traffic
Types:
- Standard ACL: Filter by source IP
- Extended ACL: Filter by source, dest, protocol, port

Placement:
- Standard: Close to destination
- Extended: Close to source
```

**VPN (Virtual Private Network):**

```
Purpose: Secure remote access
Types:
- Site-to-Site: Connect networks
- Remote Access: Connect users

Protocols:
- IPsec: Industry standard
- SSL/TLS: Browser-based
- PPTP: Obsolete
- L2TP: Often with IPsec
```

### Defense in Depth

**Layered Security:**

```
1. Physical security
2. Perimeter security (firewall)
3. Network security (segmentation, ACLs)
4. Host security (antivirus, patches)
5. Application security
6. Data security (encryption, backups)
```

**Best Practices:**

```
✓ Keep systems patched
✓ Use strong passwords
✓ Enable logging
✓ Implement least privilege
✓ Disable unused services
✓ Use encryption
✓ Regular backups
✓ Security awareness training
✓ Incident response plan
✓ Regular security audits
```

---

## Building a Small Network

### Network Design Principles

**Hierarchical Design:**

```
Access Layer:
- User connectivity
- Port security
- VLANs
- PoE

Distribution Layer:
- Routing between VLANs
- Policy enforcement
- Aggregation
- Redundancy

Core Layer:
- High-speed switching
- Redundancy
- Minimal processing
```

**Small Network Design:**

```
Components:
- Router (Internet gateway)
- Switches (User connectivity)
- Wireless AP
- Server(s)
- End devices

Features:
- DHCP server
- NAT/PAT
- Security (ACLs, firewall)
- Wireless security
- Redundancy (if possible)
```

### Network Planning Steps

**1. Gather Requirements:**

```
- Number of users
- Applications needed
- Bandwidth requirements
- Security requirements
- Budget
- Growth projections
```

**2. Design IP Addressing:**

```
- Choose private IP range
- Plan subnets (VLANs)
- Document addressing scheme
- Reserve IPs for servers/devices
```

**3. Select Equipment:**

```
- Router capacity
- Switch port count & speed
- PoE requirements
- Wireless coverage
- Backup devices
```

**4. Implement Security:**

```
- Firewall rules
- ACLs
- Port security
- Wireless encryption
- User authentication
- Password policies
```

**5. Configure Devices:**

```
- Router: WAN, LAN, NAT, routing
- Switches: VLANs, trunks, security
- AP: SSIDs, security, channels
- Servers: DHCP, DNS, file/print
```

**6. Test & Document:**

```
- Test connectivity
- Test applications
- Performance testing
- Document configuration
- Create network diagram
- Write procedures
```

### Small Office Network Example

**Requirements:**

```
- 25 users
- 1 server
- Wireless access
- Internet access
- Printer
```

**IP Plan:**

```
Network: 192.168.1.0/24

Subnets:
- Management: 192.168.1.0/27    (.1-.30)
- Users:      192.168.1.32/27   (.33-.62)
- Wireless:   192.168.1.64/27   (.65-.94)
- Servers:    192.168.1.96/27   (.97-.126)
- Future:     192.168.1.128/25  (.129-.254)

Static assignments:
- Router:     192.168.1.1
- Switch:     192.168.1.2
- AP:         192.168.1.3
- Server:     192.168.1.100
- Printer:    192.168.1.101

DHCP Pools:
- Users:      192.168.1.33 - .62
- Wireless:   192.168.1.65 - .94
```

**VLAN Design:**

```
VLAN 1:  Management (Default)
VLAN 10: Users
VLAN 20: Wireless
VLAN 30: Servers
VLAN 99: Native (unused)
```

**Basic Configuration:**

```
Router:
- WAN: DHCP from ISP (or static)
- LAN: 192.168.1.1/24
- NAT: Overload (PAT)
- DHCP: Serve to clients
- ACL: Block incoming, allow established
- Firewall: Basic rules

Switch:
- VLANs: 1, 10, 20, 30
- Trunk to router
- Access ports by VLAN
- Port security on user ports
- Management: VLAN 1, IP 192.168.1.2

Wireless AP:
- SSID: OfficeWiFi
- Security: WPA2-PSK
- VLAN: 20
- Channel: Auto (or survey)
```

---

# PART 2: CISCO IOS COMMANDS

---

## โหมดพื้นฐาน

### User EXEC Mode (>)

```
enable                          เข้าสู่ Privileged EXEC Mode
```

### Privileged EXEC Mode (#)

```
configure terminal              เข้าสู่ Global Configuration Mode
show running-config             แสดง configuration ที่กำลังใช้งาน
show startup-config             แสดง configuration ที่บันทึกไว้
show ip interface brief         แสดงสถานะ interface แบบสรุป
show interfaces                 แสดงรายละเอียด interface ทั้งหมด
show ip route                   แสดง routing table
show vlan brief                 แสดงข้อมูล VLAN แบบสรุป
copy running-config startup-config  บันทึก config (หรือใช้ write memory)
reload                          รีสตาร์ทอุปกรณ์
ping [IP]                       ทดสอบการเชื่อมต่อ
traceroute [IP]                 ตรวจสอบเส้นทาง
show version                    แสดงข้อมูล IOS และฮาร์ดแวร์
```

---

## Global Configuration

### การตั้งชื่อและรหัสผ่าน

```
hostname [ชื่อ]                 ตั้งชื่ออุปกรณ์
enable secret [รหัสผ่าน]        ตั้งรหัสผ่าน enable (เข้ารหัส MD5)
enable password [รหัสผ่าน]      ตั้งรหัสผ่าน enable (ไม่เข้ารหัส - ไม่แนะนำ)
service password-encryption     เข้ารหัสรหัสผ่านทั้งหมดด้วย Type 7
banner motd # [ข้อความ] #      ตั้งข้อความต้อนรับ
no ip domain-lookup             ปิดการค้นหา DNS (ป้องกันค้าง)
```

### Line Configuration

```
line console 0                  เข้าสู่ console line config
line vty 0 4                    เข้าสู่ telnet/SSH line config (5 sessions)
line vty 0 15                   รองรับ 16 sessions
  password [รหัสผ่าน]           ตั้งรหัสผ่านสำหรับ line
  login                         เปิดใช้งานการตรวจสอบรหัสผ่าน
  login local                   ใช้ local username/password database
  transport input ssh           อนุญาตเฉพาะ SSH
  transport input telnet ssh    อนุญาตทั้ง telnet และ SSH
  logging synchronous           ป้องกันข้อความรบกวนการพิมพ์
  exec-timeout [นาที] [วินาที]  ตั้งเวลา timeout (0 0 = ไม่ timeout)
```

### User Account

```
username [ชื่อ] secret [รหัสผ่าน]              สร้าง user account (เข้ารหัส)
username [ชื่อ] privilege [0-15] secret [รหัสผ่าน]  สร้าง user พร้อมกำหนดสิทธิ์
```

**หมายเหตุ:** Privilege level 15 = full access, 1 = user mode, 0 = limited

---

## Interface Configuration

### Basic Interface Commands

```
interface [ชนิด] [หมายเลข]      เข้าสู่ interface config
                                เช่น: interface gigabitethernet 0/1
                                     interface g0/1 (ย่อได้)
                                     
  ip address [IP] [Subnet Mask] ตั้ง IP address
  description [คำอธิบาย]        ใส่คำอธิบาย interface
  no shutdown                   เปิดใช้งาน interface (สำคัญมาก!)
  shutdown                      ปิด interface
  duplex [auto|full|half]       ตั้งค่า duplex mode
  speed [auto|10|100|1000]      ตั้งความเร็ว
```

### Switch Port Configuration

```
interface [ชนิด] [หมายเลข]
  switchport mode access                    ตั้งเป็น access port
  switchport access vlan [หมายเลข]         กำหนด VLAN
  switchport mode trunk                     ตั้งเป็น trunk port
  switchport trunk allowed vlan [หมายเลข]  กำหนด VLAN ที่อนุญาต
  switchport trunk allowed vlan add [หมายเลข]  เพิ่ม VLAN
  switchport trunk native vlan [หมายเลข]   กำหนด native VLAN
  switchport nonegotiate                    ปิด DTP negotiation
```

**ตัวอย่าง:**

```
interface fastethernet 0/1
  switchport mode access
  switchport access vlan 10
  no shutdown
```

### Router Subinterface (Router-on-a-Stick)

```
interface [ชนิด].[หมายเลข subinterface]
  encapsulation dot1Q [VLAN ID]        กำหนด VLAN tagging (802.1Q)
  encapsulation dot1Q [VLAN ID] native กำหนด native VLAN
  ip address [IP] [Subnet Mask]        ตั้ง IP สำหรับ VLAN นั้น
```

**ตัวอย่าง:**

```
interface g0/0.10
  encapsulation dot1Q 10
  ip address 192.168.10.1 255.255.255.0
  
interface g0/0.20
  encapsulation dot1Q 20
  ip address 192.168.20.1 255.255.255.0
```

---

## VLAN Configuration

### สร้างและจัดการ VLAN

```
vlan [หมายเลข]                 สร้าง VLAN (1-4094, โดย 1, 1002-1005 สงวนไว้)
  name [ชื่อ]                   ตั้งชื่อ VLAN
  
show vlan brief                 แสดง VLAN ทั้งหมด
show vlan id [หมายเลข]         แสดงข้อมูล VLAN เฉพาะ
show interfaces trunk           แสดง trunk ports
show interfaces switchport      แสดงรายละเอียด switchport
```

**ตัวอย่าง:**

```
vlan 10
  name SALES
vlan 20
  name MARKETING
vlan 30
  name IT
```

### SVI (Switch Virtual Interface)

```
interface vlan [หมายเลข]
  ip address [IP] [Subnet Mask]
  no shutdown
```

**ตัวอย่าง:**

```
interface vlan 1
  ip address 192.168.1.1 255.255.255.0
  no shutdown
```

---

## Routing Configuration

### Static Routing

```
ip route [destination network] [subnet mask] [next-hop IP]
ip route [destination network] [subnet mask] [exit interface]
ip route [destination network] [subnet mask] [exit interface] [next-hop IP]
ip route 0.0.0.0 0.0.0.0 [next-hop IP]                    Default route
```

**ตัวอย่าง:**

```
ip route 192.168.2.0 255.255.255.0 10.0.0.2
ip route 192.168.3.0 255.255.255.0 g0/1
ip route 0.0.0.0 0.0.0.0 203.0.113.1
```

### RIP (Routing Information Protocol)

```
router rip
  version 2                             ใช้ RIPv2 (รองรับ VLSM และ CIDR)
  network [classful network]            ประกาศ network (ต้องเป็น classful)
  no auto-summary                       ปิด auto-summarization
  passive-interface [ชนิด] [หมายเลข]   ไม่ส่ง RIP updates ออกไป
  passive-interface default             ตั้งทุก interface เป็น passive
  no passive-interface [ชนิด] [หมายเลข] ยกเลิก passive
  default-information originate         ประกาศ default route
```

**ตัวอย่าง:**

```
router rip
  version 2
  network 192.168.1.0
  network 10.0.0.0
  no auto-summary
  passive-interface g0/0
```

**Verification:**

```
show ip protocols
show ip rip database
debug ip rip
```

### OSPF (Open Shortest Path First)

```
router ospf [process ID]                        เริ่ม OSPF (Process ID 1-65535)
  router-id [IP]                                ตั้ง router ID (แนะนำให้ตั้งเอง)
  network [IP] [wildcard mask] area [area ID]   ประกาศ network
  passive-interface [ชนิด] [หมายเลข]           ไม่ส่ง OSPF packets
  passive-interface default                     ตั้งทุก interface เป็น passive
  no passive-interface [ชนิด] [หมายเลข]        ยกเลิก passive
  default-information originate                 ประกาศ default route
  auto-cost reference-bandwidth [Mbps]          ปรับ reference bandwidth
```

**ตัวอย่าง:**

```
router ospf 1
  router-id 1.1.1.1
  network 192.168.1.0 0.0.0.255 area 0
  network 10.0.0.0 0.0.0.3 area 0
  passive-interface g0/0
  auto-cost reference-bandwidth 10000
```

**Wildcard Mask คำนวณ:** 255.255.255.255 - Subnet Mask

- /24 (255.255.255.0) → 0.0.0.255
- /30 (255.255.255.252) → 0.0.0.3
- /16 (255.255.0.0) → 0.0.255.255

**Interface Level OSPF (Alternative):**

```
interface g0/1
  ip ospf 1 area 0
  ip ospf cost 10
  ip ospf priority 100
```

**Verification:**

```
show ip ospf neighbor
show ip ospf interface
show ip ospf database
show ip protocols
```

---

## DHCP Configuration

### DHCP Server

```
ip dhcp excluded-address [IP เริ่ม] [IP สุดท้าย]   กำหนด IP ที่ไม่แจกจ่าย
ip dhcp pool [ชื่อ pool]                           สร้าง DHCP pool
  network [network] [subnet mask]                   กำหนด network
  default-router [gateway IP]                       กำหนด default gateway
  dns-server [DNS IP] [DNS IP ...]                  กำหนด DNS servers
  domain-name [ชื่อ domain]                         กำหนด domain name
  lease [วัน] [ชั่วโมง] [นาที]                     กำหนดระยะเวลา lease
  lease infinite                                    ไม่มีการหมดอายุ
```

**ตัวอย่าง:**

```
ip dhcp excluded-address 192.168.1.1 192.168.1.10
ip dhcp pool LAN-POOL
  network 192.168.1.0 255.255.255.0
  default-router 192.168.1.1
  dns-server 8.8.8.8 8.8.4.4
  domain-name example.com
  lease 7
```

### DHCP Relay (IP Helper)

```
interface [ชนิด] [หมายเลข]
  ip helper-address [DHCP server IP]   ส่งต่อ DHCP requests ไปยัง server
```

**ตัวอย่าง:**

```
interface g0/1
  ip helper-address 192.168.1.10
```

### DHCP Client

```
interface [ชนิด] [หมายเลข]
  ip address dhcp                      รับ IP จาก DHCP server
```

### Verification

```
show ip dhcp binding                   แสดง DHCP leases
show ip dhcp pool                      แสดงข้อมูล DHCP pools
show ip dhcp conflict                  แสดง IP conflicts
show ip dhcp server statistics         แสดงสถิติ DHCP
```

---

## ACL (Access Control List)

### Standard ACL (1-99, 1300-1999)

**กรอง based on source IP เท่านั้น**

**Numbered Standard ACL:**

```
access-list [1-99] [permit|deny] [source] [wildcard]
access-list [1-99] [permit|deny] host [IP]
access-list [1-99] [permit|deny] any
```

**ตัวอย่าง:**

```
access-list 10 permit 192.168.1.0 0.0.0.255
access-list 10 deny 192.168.2.10 0.0.0.0
access-list 10 deny host 192.168.2.10        (เหมือนบรรทัดบน)
access-list 10 permit any
```

**Named Standard ACL:**

```
ip access-list standard [ชื่อ]
  [sequence] [permit|deny] [source] [wildcard]
  [sequence] [permit|deny] host [IP]
  [sequence] [permit|deny] any
```

**ตัวอย่าง:**

```
ip access-list standard BLOCK-HOST
  10 deny host 192.168.1.100
  20 permit any
```

### Extended ACL (100-199, 2000-2699)

**กรอง based on source IP, destination IP, protocol, port**

**Numbered Extended ACL:**

```
access-list [100-199] [permit|deny] [protocol] [source] [wildcard] [destination] [wildcard]
access-list [100-199] [permit|deny] [protocol] [source] [wildcard] [operator] [port] [destination] [wildcard] [operator] [port]
```

**Protocol:** ip, tcp, udp, icmp, eigrp, ospf, gre, etc. **Operator:** eq (equal), neq (not equal), lt (less than), gt (greater than), range

**ตัวอย่าง:**

```
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 80
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 443
access-list 100 deny tcp any any eq 23
access-list 100 permit icmp any any
access-list 100 deny ip any any
```

**Named Extended ACL:**

```
ip access-list extended [ชื่อ]
  [sequence] [permit|deny] [protocol] [source] [wildcard] [destination] [wildcard]
  [sequence] [permit|deny] [protocol] [source] [wildcard] [operator] [port] [destination] [wildcard] [operator] [port]
```

**ตัวอย่าง:**

```
ip access-list extended WEB-TRAFFIC
  10 permit tcp any any eq 80
  20 permit tcp any any eq 443
  30 deny ip any any
  
ip access-list extended BLOCK-TELNET
  10 deny tcp 192.168.1.0 0.0.0.255 any eq 23
  20 permit ip any any
```

### การนำ ACL ไปใช้งาน

**บน Interface:**

```
interface [ชนิด] [หมายเลข]
  ip access-group [ACL number/name] [in|out]
```

- **in** = กรอง traffic ที่เข้ามา interface
- **out** = กรอง traffic ที่ออกจาก interface

**ตัวอย่าง:**

```
interface g0/0
  ip access-group 10 in
  
interface g0/1
  ip access-group WEB-TRAFFIC out
```

**บน VTY Line (Remote Access):**

```
line vty 0 15
  access-class [ACL number/name] in
```

**ตัวอย่าง:**

```
access-list 20 permit 192.168.1.0 0.0.0.255
access-list 20 deny any

line vty 0 15
  access-class 20 in
```

### การแก้ไข Named ACL

```
ip access-list [standard|extended] [ชื่อ]
  no [sequence number]               ลบ entry
  [sequence] [คำสั่งใหม่]            เพิ่ม entry ที่ตำแหน่งที่ต้องการ
```

**ตัวอย่าง:**

```
ip access-list extended WEB-TRAFFIC
  no 20                             ลบ sequence 20
  25 permit tcp any any eq 8080     เพิ่ม entry ใหม่
```

### Port Numbers ที่ควรจำ

```
FTP Data:    20
FTP Control: 21
SSH:         22
Telnet:      23
SMTP:        25
DNS:         53
HTTP:        80
POP3:        110
HTTPS:       443
```

### Verification

```
show access-lists                     แสดง ACLs ทั้งหมด
show access-lists [number/name]       แสดง ACL เฉพาะ
show ip access-lists                  แสดง IP ACLs
show ip interface [ชนิด] [หมายเลข]   แสดง ACL ที่ apply บน interface
show run | include access-list        แสดง ACL configuration
```

### ACL Best Practices

1. **Standard ACL:** วางใกล้ destination
2. **Extended ACL:** วางใกล้ source
3. มี implicit deny any ที่ท้าย ACL เสมอ
4. เรียงจากเฉพาะเจาะจง → ทั่วไป
5. ใช้ Named ACL สำหรับการจัดการที่ง่าย
6. อย่าลืม **permit** traffic ที่ต้องการ
7. ทดสอบก่อนนำไปใช้งานจริง

---

## NAT (Network Address Translation)

### ประเภทของ NAT

1. **Static NAT** - 1:1 mapping (ถาวร)
2. **Dynamic NAT** - many-to-many จาก pool
3. **PAT (NAT Overload)** - many-to-one หรือ many-to-few ใช้ port

### Static NAT

**แปลง inside local เป็น inside global แบบถาวร**

```
ip nat inside source static [inside local IP] [inside global IP]

interface [inside interface]
  ip nat inside
  
interface [outside interface]
  ip nat outside
```

**ตัวอย่าง:**

```
ip nat inside source static 192.168.1.10 203.0.113.10

interface g0/0
  ip address 192.168.1.1 255.255.255.0
  ip nat inside
  
interface g0/1
  ip address 203.0.113.1 255.255.255.0
  ip nat outside
```

### Dynamic NAT

**แปลงจาก pool ของ public IPs**

```
ip nat pool [ชื่อ pool] [start IP] [end IP] netmask [subnet mask]
access-list [number] permit [source network] [wildcard]
ip nat inside source list [ACL number] pool [pool name]

interface [inside interface]
  ip nat inside
  
interface [outside interface]
  ip nat outside
```

**ตัวอย่าง:**

```
ip nat pool PUBLIC-POOL 203.0.113.10 203.0.113.20 netmask 255.255.255.0
access-list 1 permit 192.168.1.0 0.0.0.255
ip nat inside source list 1 pool PUBLIC-POOL

interface g0/0
  ip nat inside
  
interface g0/1
  ip nat outside
```

### PAT (Port Address Translation / NAT Overload)

**Using Interface:**

```
access-list [number] permit [source network] [wildcard]
ip nat inside source list [ACL number] interface [outside interface] overload

interface [inside interface]
  ip nat inside
  
interface [outside interface]
  ip nat outside
```

**ตัวอย่าง:**

```
access-list 1 permit 192.168.1.0 0.0.0.255
ip nat inside source list 1 interface g0/1 overload

interface g0/0
  ip nat inside
  
interface g0/1
  ip nat outside
```

**Using Pool:**

```
ip nat pool [ชื่อ pool] [start IP] [end IP] netmask [subnet mask]
access-list [number] permit [source network] [wildcard]
ip nat inside source list [ACL number] pool [pool name] overload

interface [inside interface]
  ip nat inside
  
interface [outside interface]
  ip nat outside
```

**ตัวอย่าง:**

```
ip nat pool PAT-POOL 203.0.113.1 203.0.113.5 netmask 255.255.255.0
access-list 1 permit 192.168.1.0 0.0.0.255
ip nat inside source list 1 pool PAT-POOL overload

interface g0/0
  ip nat inside
  
interface g0/1
  ip nat outside
```

### Port Forwarding (Static NAT with Port)

**แปลง port เฉพาะ**

```
ip nat inside source static [tcp|udp] [inside local IP] [inside port] [inside global IP] [outside port]
```

**ตัวอย่าง:**

```
ip nat inside source static tcp 192.168.1.10 80 203.0.113.1 8080
ip nat inside source static tcp 192.168.1.20 22 203.0.113.1 2222
```

เมื่อมีคนเข้า 203.0.113.1:8080 จะถูกส่งไปที่ 192.168.1.10:80

### NAT Terminology

- **Inside Local:** IP ภายใน network (Private IP)
- **Inside Global:** IP ที่ใช้แทนภายนอก (Public IP)
- **Outside Local:** IP ของ destination จากมุมมอง inside
- **Outside Global:** IP ของ destination จริงๆ

### Verification

```
show ip nat translations               แสดง NAT translation table
show ip nat statistics                 แสดงสถิติ NAT
show ip nat translations verbose       แสดงรายละเอียด translations
debug ip nat                          แสดง NAT real-time
debug ip nat detailed                 แสดงรายละเอียดเพิ่มเติม
clear ip nat translation *            ลบ translations ทั้งหมด
```

### NAT Troubleshooting

1. ตรวจสอบ **ip nat inside/outside** บน interface
2. ตรวจสอบ ACL ให้ถูกต้อง
3. ตรวจสอบ routing ให้ครบ (inside และ outside)
4. ใช้ **debug ip nat** ดูการทำงาน
5. ใช้ **show ip nat statistics** ดู hits

---

## EtherChannel

### ภาพรวม

**EtherChannel** = รวม physical links หลายๆ เส้นเป็น logical link เดียว

**ข้อดี:**

- เพิ่ม bandwidth (2 links = 2Gbps, 4 links = 4Gbps)
- Redundancy
- Load balancing
- ไม่มี STP blocking

**Protocols:**

1. **PAgP (Port Aggregation Protocol)** - Cisco proprietary
2. **LACP (Link Aggregation Control Protocol)** - IEEE 802.3ad, open standard

### ข้อกำหนด

- Interface เหมือนกันทุกอย่าง (speed, duplex, VLAN)
- Maximum 8 active links ต่อ EtherChannel
- ต้องเป็น access หรือ trunk เหมือนกันทั้งหมด

### PAgP Modes

```
on          บังคับเปิด EtherChannel (ไม่มี negotiation)
desirable   พยายาม initiate PAgP
auto        รอ PAgP จากอีกฝั่ง (passive)
```

**Combinations ที่ทำงานได้:**

- desirable ↔ desirable
- desirable ↔ auto
- on ↔ on (ไม่แนะนำ)

### LACP Modes

```
on          บังคับเปิด EtherChannel (ไม่มี negotiation)
active      พยายาม initiate LACP
passive     รอ LACP จากอีกฝั่ง
```

**Combinations ที่ทำงานได้:**

- active ↔ active
- active ↔ passive
- on ↔ on (ไม่แนะนำ)

### Layer 2 EtherChannel Configuration

**Using PAgP:**

```
interface range [ชนิด] [เริ่ม] - [สุดท้าย]
  channel-group [number] mode [desirable|auto|on]
  switchport mode trunk
  switchport trunk allowed vlan [หมายเลข]
  
interface port-channel [number]
  switchport mode trunk
  switchport trunk allowed vlan [หมายเลข]
```

**ตัวอย่าง PAgP:**

```
interface range fastethernet 0/1 - 2
  channel-group 1 mode desirable
  switchport mode trunk
  
interface port-channel 1
  switchport mode trunk
```

**Using LACP:**

```
interface range [ชนิด] [เริ่ม] - [สุดท้าย]
  channel-group [number] mode [active|passive|on]
  switchport mode trunk
  switchport trunk allowed vlan [หมายเลข]
  
interface port-channel [number]
  switchport mode trunk
  switchport trunk allowed vlan [หมายเลข]
```

**ตัวอย่าง LACP:**

```
interface range gigabitethernet 0/1 - 4
  channel-group 1 mode active
  switchport mode trunk
  
interface port-channel 1
  switchport mode trunk
```

### Layer 3 EtherChannel Configuration

```
interface range [ชนิด] [เริ่ม] - [สุดท้าย]
  no switchport                          เปลี่ยนเป็น Layer 3
  channel-group [number] mode [active|passive|desirable|auto|on]
  
interface port-channel [number]
  no switchport
  ip address [IP] [Subnet Mask]
```

**ตัวอย่าง:**

```
interface range gigabitethernet 0/1 - 2
  no switchport
  channel-group 1 mode active
  
interface port-channel 1
  no switchport
  ip address 10.0.0.1 255.255.255.252
```

### Load Balancing Configuration

```
port-channel load-balance [method]
```

**Methods:**

- **src-mac** - ตาม source MAC
- **dst-mac** - ตาม destination MAC
- **src-dst-mac** - ตาม source และ destination MAC
- **src-ip** - ตาม source IP
- **dst-ip** - ตาม destination IP
- **src-dst-ip** - ตาม source และ destination IP
- **src-port** - ตาม source port
- **dst-port** - ตาม destination port
- **src-dst-port** - ตาม source และ destination port

**ตัวอย่าง:**

```
port-channel load-balance src-dst-ip
```

### Verification

```
show etherchannel summary               แสดงสรุป EtherChannel
show etherchannel port-channel          แสดงข้อมูล port-channel
show etherchannel load-balance          แสดง load balancing method
show interfaces port-channel [number]   แสดงรายละเอียด port-channel
show running-config interface port-channel [number]
show pagp neighbor                      แสดง PAgP neighbors
show lacp neighbor                      แสดง LACP neighbors
```

**Output ของ show etherchannel summary:**

```
Flags:  D - down        P - bundled in port-channel
        I - stand-alone s - suspended
        H - Hot-standby (LACP only)
        R - Layer3      S - Layer2
        U - in use      N - not in use, no aggregation
        f - failed to allocate aggregator
        M - not in use, minimum links not met
        m - not in use, port not aggregated due to minimum links not met
        u - unsuitable for bundling
        w - waiting to be aggregated
```

### EtherChannel Troubleshooting

1. **ตรวจสอบ modes ทั้งสองฝั่ง** - ต้อง compatible
2. **ตรวจสอบ interface configuration** - ต้องเหมือนกันทุกอย่าง:
    - Speed
    - Duplex
    - VLAN (access/trunk)
    - Native VLAN
3. **ตรวจสอบ STP** - อาจมี inconsistency
4. ใช้ **show etherchannel summary** ดู status
5. ใช้ **show interfaces** ดู errors

### EtherChannel Best Practices

1. ใช้ **LACP** แทน PAgP (open standard)
2. ใช้ **active-active** แทน active-passive
3. หลีกเลี่ยง **on mode** (ใช้ใน lab เท่านั้น)
4. กำหนด load-balance method ให้เหมาะสม
5. ตรวจสอบให้ configuration เหมือนกันทุก port

---

## Security Configuration

### Port Security

```
interface [ชนิด] [หมายเลข]
  switchport mode access
  switchport port-security                              เปิด port security
  switchport port-security maximum [1-8192]             จำนวน MAC สูงสุด (default 1)
  switchport port-security mac-address [MAC]            กำหนด MAC address
  switchport port-security mac-address sticky           เรียนรู้ MAC แบบ sticky
  switchport port-security violation [shutdown|restrict|protect]
  switchport port-security aging time [นาที]           เวลาก่อน MAC หมดอายุ
  switchport port-security aging type [absolute|inactivity]
```

**Violation Modes:**

- **shutdown** - ปิด port, ส่ง SNMP trap, log (default)
- **restrict** - drop packets, เพิ่ม violation counter, log
- **protect** - drop packets เท่านั้น (ไม่ log)

**ตัวอย่าง:**

```
interface fastethernet 0/1
  switchport mode access
  switchport port-security
  switchport port-security maximum 2
  switchport port-security mac-address sticky
  switchport port-security violation restrict
```

**กู้คืน port ที่ถูก shutdown:**

```
interface fastethernet 0/1
  shutdown
  no shutdown
```

**หรือใช้ auto-recovery:**

```
errdisable recovery cause psecure-violation
errdisable recovery interval [วินาที]
```

### SSH Configuration

```
hostname [ชื่อ]                                     ต้องมี hostname
ip domain-name [domain]                             ต้องมี domain name
crypto key generate rsa                             สร้าง RSA key
  [แนะนำ 2048 bits]
ip ssh version 2                                    ใช้ SSH version 2
ip ssh time-out [วินาที]                            timeout (default 120)
ip ssh authentication-retries [ครั้ง]               จำนวนครั้งที่พยายาม (default 3)

username [ชื่อ] secret [รหัสผ่าน]                   สร้าง user account

line vty 0 15
  transport input ssh                               อนุญาตเฉพาะ SSH
  login local                                       ใช้ local database
  exec-timeout [นาที] [วินาที]
```

**ตัวอย่าง:**

```
hostname R1
ip domain-name example.com
crypto key generate rsa
  2048
ip ssh version 2
ip ssh time-out 60
ip ssh authentication-retries 2

username admin secret Cisco123!

line vty 0 15
  transport input ssh
  login local
  exec-timeout 5 0
```

**ลบ SSH keys:**

```
crypto key zeroize rsa
```

### Verification - Security

```
show port-security                                  แสดง port security ทั้งหมด
show port-security interface [ชนิด] [หมายเลข]      แสดง port security เฉพาะ
show port-security address                          แสดง MAC addresses
show ip ssh                                         แสดง SSH configuration
show ssh                                            แสดง SSH sessions
show crypto key mypubkey rsa                        แสดง RSA public key
```

---

## Verification Commands

### General

```
show running-config                     แสดง configuration ปัจจุบัน (RAM)
show startup-config                     แสดง configuration ที่บันทึก (NVRAM)
show version                            แสดงข้อมูล IOS, hardware, uptime
show clock                              แสดงเวลา
show history                            แสดง command history
show processes                          แสดง processes ที่ทำงาน
show memory                             แสดงการใช้งาน memory
show flash                              แสดงไฟล์ใน flash memory
```

### Interface

```
show ip interface brief                 สรุปสถานะ interfaces
show interfaces                         รายละเอียดทุก interfaces
show interfaces [ชนิด] [หมายเลข]        รายละเอียด interface เฉพาะ
show interfaces status                  สถานะ interfaces แบบ table
show interfaces description             แสดง descriptions
show interfaces trunk                   แสดง trunk ports
show interfaces switchport              แสดง switchport configuration
show controllers                        แสดงข้อมูล hardware interfaces
```

### IP & Routing

```
show ip route                           แสดง routing table
show ip protocols                       แสดง routing protocols ที่ active
show ip interface                       แสดงข้อมูล IP บน interfaces
show arp                                แสดง ARP table
show ip arp                             เหมือน show arp
```

### VLAN & Switching

```
show vlan                               แสดง VLAN ทั้งหมด
show vlan brief                         แสดง VLAN แบบสรุป
show vlan id [หมายเลข]                 แสดง VLAN เฉพาะ
show vlan name [ชื่อ]                   แสดง VLAN ตามชื่อ
show mac address-table                  แสดง MAC address table
show mac address-table dynamic          แสดง MAC ที่เรียนรู้
show mac address-table static           แสดง MAC ที่กำหนด
show spanning-tree                      แสดง STP ทั้งหมด
show spanning-tree summary              สรุป STP
```

### Routing Protocols

```
show ip rip database                    แสดง RIP database
show ip ospf                            แสดงข้อมูล OSPF
show ip ospf neighbor                   แสดง OSPF neighbors
show ip ospf interface                  แสดง OSPF interfaces
show ip ospf database                   แสดง OSPF link-state database
show ip eigrp neighbors                 แสดง EIGRP neighbors
show ip eigrp topology                  แสดง EIGRP topology
```

### CDP (Cisco Discovery Protocol)

```
show cdp                                แสดงสถานะ CDP
show cdp neighbors                      แสดงอุปกรณ์เพื่อนบ้าน
show cdp neighbors detail               แสดงรายละเอียดเพื่อนบ้าน
show cdp interface                      แสดง CDP บน interfaces
show cdp entry [device name]            แสดงข้อมูลอุปกรณ์เฉพาะ
```

**ปิด CDP:**

```
no cdp run                              ปิด CDP ทั้งระบบ
interface [ชนิด] [หมายเลข]
  no cdp enable                         ปิด CDP บน interface
```

### Troubleshooting

```
ping [IP/hostname]                      ทดสอบ connectivity
traceroute [IP/hostname]                ตรวจสอบ path
debug [protocol]                        แสดงข้อมูล real-time (ระวัง! อาจใช้ CPU สูง)
undebug all                             ปิด debug ทั้งหมด
no debug all                            เหมือน undebug all
terminal monitor                        แสดง debug ผ่าน telnet/SSH
show logging                            แสดง log messages
show debugging                          แสดง debug ที่เปิดอยู่
```

---

## คำสั่งเพิ่มเติม

### การจัดการไฟล์

```
copy running-config startup-config      บันทึก config (เหมือน write memory)
copy startup-config running-config      โหลด saved config
copy running-config tftp               backup config ไป TFTP
copy tftp running-config               restore config จาก TFTP
erase startup-config                   ลบ startup config
delete flash:[filename]                ลบไฟล์ใน flash
dir flash:                             แสดงไฟล์ใน flash
```

### Reset/Reload

```
reload                                 รีสตาร์ทอุปกรณ์
reload in [นาที]                        กำหนดเวลารีสตาร์ท
reload at [HH:MM]                      กำหนดเวลารีสตาร์ท
reload cancel                          ยกเลิกการรีสตาร์ท
write erase                            ลบ startup config
erase startup-config                   ลบ startup config
```

### การใช้งานทั่วไป

```
do [คำสั่ง]                            ใช้ privileged command ใน config mode
exit                                   ออกจาก mode ปัจจุบัน (ทีละ level)
end                                    กลับไป privileged mode ทันที (Ctrl+Z)
no [คำสั่ง]                            ยกเลิกคำสั่ง
! [comment]                            ใส่ comment ใน config
terminal length 0                      ปิด pagination (แสดงทั้งหมดไม่หยุด)
terminal length 24                     เปิด pagination (default)
```

### Keyboard Shortcuts

```
Ctrl+A                                 ไปต้นบรรทัด
Ctrl+E                                 ไปท้ายบรรทัด
Ctrl+Z                                 กลับ privileged mode
Ctrl+C                                 ยกเลิกคำสั่ง
Ctrl+Shift+6                          หยุดกระบวนการ (เช่น ping, traceroute)
Tab                                    auto-complete คำสั่ง
?                                      แสดงคำสั่งที่ใช้ได้
```

---

## Tips & Best Practices

### Configuration Management

1. **บันทึก config เสมอ** - `copy run start` หรือ `write memory`
2. **ใช้ descriptions** - ใส่คำอธิบายใน interfaces, ACLs
3. **ใช้ Named ACLs** - จัดการง่ายกว่า Numbered
4. **Backup configs** - backup ไป TFTP/USB เป็นประจำ
5. **Test ก่อนใช้** - ทดสอบใน lab ก่อน production

### Security Best Practices

1. ใช้ **enable secret** แทน enable password
2. เปิด **service password-encryption**
3. ใช้ **SSH แทน Telnet**
4. ตั้ง **exec-timeout** บน console และ vty
5. ใช้ **ACL** บน vty lines
6. เปิด **port security** บน access ports
7. ปิด **unused ports**
8. เปลี่ยน **native VLAN** จาก VLAN 1

### Network Design

1. **Plan IP addressing** - ใช้ VLSM อย่างมีประสิทธิภาพ
2. **Document everything** - network diagram, IP plan
3. **Use VLANs** - แยก traffic ตาม function
4. **Implement redundancy** - HSRP, EtherChannel
5. **Monitor regularly** - ใช้ logging, SNMP

### Troubleshooting Approach

1. **Verify physical layer** - cables, interfaces status
2. **Check IP configuration** - address, subnet, gateway
3. **Test connectivity** - ping, traceroute
4. **Verify routing** - routing table, next hops
5. **Check ACLs** - อาจ block traffic
6. **Review logs** - `show logging`

---

## Common Scenarios

### ตัวอย่าง: Basic Router Setup

```
enable
configure terminal
hostname R1
enable secret Cisco123!
no ip domain-lookup
banner motd #Unauthorized access prohibited#

line console 0
  password Console123!
  login
  logging synchronous
  exec-timeout 5 0
  
line vty 0 4
  password VTY123!
  login
  exec-timeout 5 0
  
interface g0/0
  description Link to ISP
  ip address 203.0.113.1 255.255.255.252
  no shutdown
  
interface g0/1
  description LAN Network
  ip address 192.168.1.1 255.255.255.0
  no shutdown
  
ip route 0.0.0.0 0.0.0.0 203.0.113.2

end
copy running-config startup-config
```

### ตัวอย่าง: Basic Switch Setup

```
enable
configure terminal
hostname SW1
enable secret Cisco123!
no ip domain-lookup

vlan 10
  name SALES
vlan 20
  name IT
vlan 99
  name MANAGEMENT
  
interface vlan 99
  ip address 192.168.99.1 255.255.255.0
  no shutdown
  
ip default-gateway 192.168.99.254

interface range f0/1-10
  switchport mode access
  switchport access vlan 10
  
interface range f0/11-20
  switchport mode access
  switchport access vlan 20
  
interface g0/1
  description Trunk to Router
  switchport mode trunk
  switchport trunk native vlan 99
  
interface range f0/21-24
  shutdown
  
line console 0
  password Console123!
  login
  logging synchronous
  
line vty 0 15
  password VTY123!
  login
  
end
copy running-config startup-config
```

### ตัวอย่าง: Router-on-a-Stick

```
interface g0/0
  no shutdown

interface g0/0.10
  description VLAN 10 - Sales
  encapsulation dot1Q 10
  ip address 192.168.10.1 255.255.255.0
  
interface g0/0.20
  description VLAN 20 - IT
  encapsulation dot1Q 20
  ip address 192.168.20.1 255.255.255.0
  
interface g0/0.99
  description VLAN 99 - Management
  encapsulation dot1Q 99 native
  ip address 192.168.99.1 255.255.255.0
```

---

## สรุป Command Modes

```
User EXEC Mode (Switch>)
    |
    | enable
    v
Privileged EXEC Mode (Switch#)
    |
    | configure terminal
    v
Global Configuration Mode (Switch(config)#)
    |
    |-- interface [type] [number]     → Interface Configuration Mode
    |-- line console 0                → Line Configuration Mode
    |-- line vty 0 15                 → Line Configuration Mode
    |-- router ospf [id]              → Router Configuration Mode
    |-- ip access-list [type] [name]  → ACL Configuration Mode
    |-- vlan [number]                 → VLAN Configuration Mode
```

**การออกจาก modes:**

- `exit` - ออกทีละ level
- `end` หรือ `Ctrl+Z` - กลับไป Privileged EXEC ทันที

---

## เพิ่มเติม

### Wildcard Mask คำนวณเร็ว

```
/8  → 0.255.255.255
/16 → 0.0.255.255
/24 → 0.0.0.255
/30 → 0.0.0.3
/32 → 0.0.0.0 (หรือใช้ keyword "host")
```

**สูตร:** 255.255.255.255 - Subnet Mask = Wildcard Mask

### Private IP Ranges

```
Class A: 10.0.0.0/8        (10.0.0.0 - 10.255.255.255)
Class B: 172.16.0.0/12     (172.16.0.0 - 172.31.255.255)
Class C: 192.168.0.0/16    (192.168.0.0 - 192.168.255.255)
```

### Well-Known Ports

```
20/21    FTP
22       SSH
23       Telnet
25       SMTP
53       DNS
67/68    DHCP
69       TFTP
80       HTTP
110      POP3
143      IMAP
443      HTTPS
3389     RDP
```

---

**หมายเหตุ:**

- คำสั่งบางตัวอาจแตกต่างไปตามรุ่น IOS
- ควรทดสอบใน lab environment ก่อนใช้งานจริง
- เก็บ config backups เป็นประจำ
- อ่าน documentation ของ Cisco สำหรับรายละเอียดเพิ่มเติม

---

---

## STP (Spanning Tree Protocol)

### ภาพรวม STP

**Spanning Tree Protocol** = ป้องกัน Layer 2 loops ใน network

**STP Versions:**

- **STP (802.1D)** - Original, convergence ช้า (30-50 วินาที)
- **RSTP (802.1w)** - Rapid STP, convergence เร็ว (6 วินาที)
- **PVST+** - Per-VLAN STP (Cisco proprietary)
- **Rapid PVST+** - Per-VLAN RSTP (Cisco proprietary, default บน Cisco)
- **MSTP (802.1s)** - Multiple Spanning Tree Protocol

### Port States

**STP (802.1D) States:**

```
Disabled    - administratively down
Blocking    - รับ BPDU, ไม่ forward frames (20 วินาที)
Listening   - รับและส่ง BPDU, ไม่ forward frames (15 วินาที)
Learning    - เรียนรู้ MAC addresses (15 วินาที)
Forwarding  - forward frames ปกติ
```

**RSTP (802.1w) States:**

```
Discarding  - รวม Disabled, Blocking, Listening
Learning    - เรียนรู้ MAC addresses
Forwarding  - forward frames ปกติ
```

### Port Roles

```
Root Port (RP)       - port ที่มี cost ต่ำที่สุดไป root bridge
Designated Port (DP) - port ที่ forward บน segment
Alternate Port (AP)  - backup ของ root port (RSTP)
Backup Port (BP)     - backup ของ designated port (RSTP)
```

### STP Configuration

**เปลี่ยน STP Mode:**

```
spanning-tree mode [pvst|rapid-pvst|mst]
```

**กำหนด Root Bridge:**

```
spanning-tree vlan [vlan-id] root primary            ตั้งเป็น root (priority 24576)
spanning-tree vlan [vlan-id] root secondary          ตั้งเป็น secondary (priority 28672)
spanning-tree vlan [vlan-id] priority [0-61440]      กำหนด priority (ต้องเป็นพหุคูณของ 4096)
```

**ตัวอย่าง:**

```
spanning-tree mode rapid-pvst
spanning-tree vlan 1 root primary
spanning-tree vlan 10 priority 4096
spanning-tree vlan 20 priority 8192
```

**Interface Level STP:**

```
interface [ชนิด] [หมายเลข]
  spanning-tree port-priority [0-240]               ตั้ง port priority (ทุก 16)
  spanning-tree cost [1-200000000]                  กำหนด path cost
  spanning-tree vlan [vlan-id] port-priority [0-240]
  spanning-tree vlan [vlan-id] cost [1-200000000]
```

**Default Port Costs (RSTP/PVST+):**

```
10 Mbps    = 2,000,000
100 Mbps   = 200,000
1 Gbps     = 20,000
10 Gbps    = 2,000
100 Gbps   = 200
```

### PortFast

**เปิดใช้งาน port ทันที (ใช้กับ access ports ที่ต่อ end devices)**

```
interface [ชนิด] [หมายเลข]
  spanning-tree portfast                            เปิด PortFast บน interface
  spanning-tree portfast trunk                      เปิด PortFast บน trunk (ไม่แนะนำ)
  
spanning-tree portfast default                      เปิด PortFast บนทุก access ports
spanning-tree portfast bpduguard default            เปิด BPDU Guard ทุก PortFast ports
```

**ตัวอย่าง:**

```
interface range f0/1-20
  switchport mode access
  spanning-tree portfast
```

### BPDU Guard

**ปิด port ถ้าได้รับ BPDU (ป้องกัน rogue switches)**

```
interface [ชนิด] [หมายเลข]
  spanning-tree bpduguard enable                    เปิด BPDU Guard
  
spanning-tree portfast bpduguard default            เปิดทุก PortFast ports (แนะนำ)
```

### BPDU Filter

**ไม่ส่งและไม่รับ BPDU**

```
interface [ชนิด] [หมายเลข]
  spanning-tree bpdufilter enable
  
spanning-tree portfast bpdufilter default
```

### Root Guard

**ป้องกันไม่ให้ port กลายเป็น root port**

```
interface [ชนิด] [หมายเลข]
  spanning-tree guard root
```

### Loop Guard

**ป้องกัน loops จาก unidirectional links**

```
spanning-tree loopguard default                     เปิดทั้งระบบ

interface [ชนิด] [หมายเลข]
  spanning-tree guard loop                          เปิดเฉพาะ interface
```

### Verification - STP

```
show spanning-tree                                  แสดง STP ทั้งหมด
show spanning-tree summary                          สรุป STP
show spanning-tree vlan [vlan-id]                   แสดง STP เฉพาะ VLAN
show spanning-tree interface [ชนิด] [หมายเลข]      แสดง STP บน interface
show spanning-tree root                             แสดง root bridge
show spanning-tree bridge                           แสดงข้อมูล local bridge
show spanning-tree inconsistentports                แสดง inconsistent ports
debug spanning-tree events                          debug STP events
```

### STP Troubleshooting

```
show spanning-tree                                  ดู topology
show spanning-tree blockedports                     ดู blocked ports
show spanning-tree detail                           ดูรายละเอียดทั้งหมด
show spanning-tree inconsistentports                ตรวจสอบ inconsistencies
```

---

## HSRP (Hot Standby Router Protocol)

### ภาพรวม HSRP

**HSRP** = Cisco proprietary FHRP (First Hop Redundancy Protocol)

- ใช้ virtual IP และ virtual MAC
- Priority สูงสุดเป็น active router
- Default priority = 100
- Multicast: 224.0.0.2 (v1), 224.0.0.102 (v2)

**HSRP States:**

```
Initial     - เริ่มต้น
Learn       - รอการกำหนด virtual IP
Listen      - ฟัง hello messages
Speak       - ส่ง hello messages
Standby     - standby router
Active      - active router (forward traffic)
```

### HSRP Configuration

```
interface [ชนิด] [หมายเลข]
  ip address [IP] [Subnet Mask]
  standby [group] ip [virtual IP]                   กำหนด virtual IP
  standby [group] priority [1-255]                  กำหนด priority (default 100)
  standby [group] preempt                           เปิด preemption
  standby [group] preempt delay minimum [วินาที]   delay ก่อน preempt
  standby [group] timers [hello] [hold]             กำหนด timers (default 3, 10)
  standby [group] track [interface] [decrement]     track interface
  standby version [1|2]                             กำหนด HSRP version
  standby [group] authentication [password]         ตั้งรหัสผ่าน (v1)
  standby [group] authentication md5 key-string [password]  MD5 (v2)
```

**ตัวอย่าง Basic HSRP:**

```
! Router 1 (Active)
interface g0/0
  ip address 192.168.1.2 255.255.255.0
  standby 1 ip 192.168.1.1
  standby 1 priority 110
  standby 1 preempt

! Router 2 (Standby)
interface g0/0
  ip address 192.168.1.3 255.255.255.0
  standby 1 ip 192.168.1.1
  standby 1 priority 100
```

**ตัวอย่าง HSRP with Tracking:**

```
interface g0/0
  ip address 192.168.1.2 255.255.255.0
  standby 1 ip 192.168.1.1
  standby 1 priority 110
  standby 1 preempt
  standby 1 track g0/1 20
```

เมื่อ g0/1 down, priority จะลดลง 20 (110-20=90)

**HSRP Version 2:**

```
interface g0/0
  standby version 2
  standby 1 ip 192.168.1.1
  standby 1 priority 110
  standby 1 preempt
```

### HSRP Load Balancing

**ใช้หลาย groups**

```
! Router 1
interface g0/0
  ip address 192.168.1.2 255.255.255.0
  standby 1 ip 192.168.1.1
  standby 1 priority 110
  standby 1 preempt
  standby 2 ip 192.168.1.254
  standby 2 priority 90

! Router 2
interface g0/0
  ip address 192.168.1.3 255.255.255.0
  standby 1 ip 192.168.1.1
  standby 1 priority 90
  standby 2 ip 192.168.1.254
  standby 2 priority 110
  standby 2 preempt
```

### Verification - HSRP

```
show standby                                        แสดง HSRP ทั้งหมด
show standby brief                                  แสดง HSRP แบบสรุป
show standby [interface]                            แสดง HSRP บน interface
debug standby                                       debug HSRP
debug standby events                                debug HSRP events
debug standby packets                               debug HSRP packets
```

---

## VRRP (Virtual Router Redundancy Protocol)

### ภาพรวม VRRP

**VRRP** = Open standard FHRP (RFC 5798)

- คล้าย HSRP แต่เป็น standard
- Priority สูงสุดเป็น master
- Default priority = 100
- Multicast: 224.0.0.18

### VRRP Configuration

```
interface [ชนิด] [หมายเลข]
  vrrp [group] ip [virtual IP]                      กำหนด virtual IP
  vrrp [group] priority [1-254]                     กำหนด priority
  vrrp [group] preempt                              เปิด preemption
  vrrp [group] timers advertise [วินาที]           hello timer
  vrrp [group] track [interface] [decrement]        track interface
  vrrp [group] authentication [password]            ตั้งรหัสผ่าน
```

**ตัวอย่าง:**

```
! Router 1 (Master)
interface g0/0
  ip address 192.168.1.2 255.255.255.0
  vrrp 1 ip 192.168.1.1
  vrrp 1 priority 110
  vrrp 1 preempt

! Router 2 (Backup)
interface g0/0
  ip address 192.168.1.3 255.255.255.0
  vrrp 1 ip 192.168.1.1
```

### Verification - VRRP

```
show vrrp                                           แสดง VRRP
show vrrp brief                                     แสดง VRRP แบบสรุป
debug vrrp all                                      debug VRRP
```

---

## GLBP (Gateway Load Balancing Protocol)

### ภาพรวม GLBP

**GLBP** = Cisco proprietary FHRP with true load balancing

- 1 AVG (Active Virtual Gateway)
- ถึง 4 AVF (Active Virtual Forwarder)
- แต่ละ AVF มี unique virtual MAC
- Multicast: 224.0.0.102

### GLBP Configuration

```
interface [ชนิด] [หมายเลข]
  glbp [group] ip [virtual IP]                      กำหนด virtual IP
  glbp [group] priority [1-255]                     กำหนด priority
  glbp [group] preempt                              เปิด preemption
  glbp [group] load-balancing [round-robin|weighted|host-dependent]
  glbp [group] timers [hello] [hold]                กำหนด timers
  glbp [group] weighting [value] lower [threshold] upper [threshold]
  glbp [group] weighting track [interface] [decrement]
```

**Load Balancing Methods:**

- **round-robin** - แจก MAC แบบหมุนเวียน (default)
- **weighted** - แจกตาม weight
- **host-dependent** - host เดิมได้ MAC เดิม

**ตัวอย่าง:**

```
! Router 1
interface g0/0
  ip address 192.168.1.2 255.255.255.0
  glbp 1 ip 192.168.1.1
  glbp 1 priority 110
  glbp 1 preempt
  glbp 1 load-balancing round-robin

! Router 2
interface g0/0
  ip address 192.168.1.3 255.255.255.0
  glbp 1 ip 192.168.1.1
  glbp 1 priority 100
  glbp 1 load-balancing round-robin
```

### Verification - GLBP

```
show glbp                                           แสดง GLBP
show glbp brief                                     แสดง GLBP แบบสรุป
debug glbp                                          debug GLBP
```

---

## DTP (Dynamic Trunking Protocol)

### ภาพรวม DTP

**DTP** = Cisco proprietary protocol สำหรับ auto-negotiate trunk

- ไม่แนะนำให้ใช้ใน production (security risk)
- ควรตั้ง manual เป็น trunk หรือ access

### DTP Modes

```
access          - บังคับเป็น access port
trunk           - บังคับเป็น trunk port
dynamic auto    - รอให้อีกฝั่ง initiate (default)
dynamic desirable - พยายาม negotiate เป็น trunk
```

**Combinations:**

```
trunk ↔ trunk           = Trunk
trunk ↔ dynamic auto    = Trunk
trunk ↔ dynamic desirable = Trunk
dynamic desirable ↔ dynamic desirable = Trunk
dynamic desirable ↔ dynamic auto = Trunk
dynamic auto ↔ dynamic auto = Access
access ↔ any            = Access
```

### DTP Configuration

```
interface [ชนิด] [หมายเลข]
  switchport mode trunk                             trunk แบบ manual
  switchport mode access                            access แบบ manual
  switchport mode dynamic auto                      DTP auto mode
  switchport mode dynamic desirable                 DTP desirable mode
  switchport nonegotiate                            ปิด DTP (ใช้กับ trunk)
```

**Best Practice:**

```
interface g0/1
  switchport mode trunk
  switchport nonegotiate
```

### Verification - DTP

```
show interfaces [ชนิด] [หมายเลข] switchport        แสดง DTP status
show dtp interface [ชนิด] [หมายเลข]                แสดงรายละเอียด DTP
```

---

## VTP (VLAN Trunking Protocol)

### ภาพรวม VTP

**VTP** = Cisco proprietary protocol สำหรับ synchronize VLAN database

- **ไม่แนะนำให้ใช้** - อันตรายมาก (อาจลบ VLAN ทั้งหมด)
- ใช้ VTP Transparent หรือ Off แทน

### VTP Modes

```
Server      - สร้าง, แก้ไข, ลบ VLAN, ส่ง updates (default)
Client      - ไม่สามารถแก้ไข VLAN, รับ updates
Transparent - สร้าง/แก้ไข VLAN ได้ แต่ไม่ sync, forward VTP messages
Off         - ปิด VTP สนิท
```

### VTP Configuration

```
vtp mode [server|client|transparent|off]            ตั้ง VTP mode
vtp domain [ชื่อ]                                   ตั้ง VTP domain
vtp password [password]                             ตั้งรหัสผ่าน
vtp version [1|2|3]                                 ตั้ง VTP version
vtp pruning                                         เปิด VTP pruning
```

**Best Practice (ปิด VTP):**

```
vtp mode transparent
หรือ
vtp mode off
```

### Verification - VTP

```
show vtp status                                     แสดงสถานะ VTP
show vtp password                                   แสดงรหัสผ่าน VTP
show vtp counters                                   แสดง VTP statistics
```

---

## IPv6 Configuration

### IPv6 Address Types

```
Global Unicast    - 2000::/3 (เทียบ Public IPv4)
Link-Local        - FE80::/10 (auto-generated)
Unique Local      - FC00::/7 (เทียบ Private IPv4)
Multicast         - FF00::/8
Loopback          - ::1/128
Unspecified       - ::/128
```

### IPv6 Basic Configuration

```
ipv6 unicast-routing                                เปิด IPv6 routing

interface [ชนิด] [หมายเลข]
  ipv6 address [IPv6]/[prefix]                      กำหนด IPv6 address
  ipv6 address [IPv6] link-local                    กำหนด link-local
  ipv6 address autoconfig                           ใช้ SLAAC
  ipv6 enable                                       เปิด IPv6 (ได้ link-local)
  no shutdown
```

**ตัวอย่าง:**

```
ipv6 unicast-routing

interface g0/0
  ipv6 address 2001:DB8:1:1::1/64
  ipv6 address FE80::1 link-local
  no shutdown
```

### IPv6 Static Routing

```
ipv6 route [destination]/[prefix] [next-hop IPv6]
ipv6 route [destination]/[prefix] [exit interface]
ipv6 route [destination]/[prefix] [exit interface] [next-hop IPv6]
ipv6 route ::/0 [next-hop]                          default route
```

**ตัวอย่าง:**

```
ipv6 route 2001:DB8:2::/64 2001:DB8:1::2
ipv6 route ::/0 g0/1
```

### OSPFv3 (OSPF for IPv6)

```
ipv6 router ospf [process-id]
  router-id [IPv4 format]                           ต้องกำหนด router-id

interface [ชนิด] [หมายเลข]
  ipv6 ospf [process-id] area [area-id]             enable OSPFv3
  ipv6 ospf priority [0-255]                        กำหนด priority
  ipv6 ospf cost [1-65535]                          กำหนด cost
```

**ตัวอย่าง:**

```
ipv6 router ospf 1
  router-id 1.1.1.1

interface g0/0
  ipv6 address 2001:DB8:1:1::1/64
  ipv6 ospf 1 area 0
```

### EIGRPv6

```
ipv6 router eigrp [AS]
  router-id [IPv4 format]
  no shutdown                                       ต้อง no shutdown!

interface [ชนิด] [หมายเลข]
  ipv6 eigrp [AS]
```

**ตัวอย่าง:**

```
ipv6 router eigrp 1
  router-id 1.1.1.1
  no shutdown

interface g0/0
  ipv6 address 2001:DB8:1:1::1/64
  ipv6 eigrp 1
```

### DHCPv6

**Stateless DHCPv6 (SLAAC + DHCPv6):**

```
ipv6 dhcp pool [ชื่อ]
  dns-server [IPv6]
  domain-name [domain]

interface [ชนิด] [หมายเลข]
  ipv6 dhcp server [ชื่อ pool]
  ipv6 nd other-config-flag
```

**Stateful DHCPv6:**

```
ipv6 dhcp pool [ชื่อ]
  address prefix [IPv6]/[prefix]
  dns-server [IPv6]
  domain-name [domain]

interface [ชนิด] [หมายเลข]
  ipv6 dhcp server [ชื่อ pool]
  ipv6 nd managed-config-flag
  ipv6 nd prefix default no-autoconfig
```

### IPv6 ACL

```
ipv6 access-list [ชื่อ]
  permit|deny [protocol] [source] [destination]
  permit|deny ipv6 any any

interface [ชนิด] [หมายเลข]
  ipv6 traffic-filter [ชื่อ] [in|out]
```

**ตัวอย่าง:**

```
ipv6 access-list BLOCK-TELNET
  deny tcp any any eq 23
  permit ipv6 any any

interface g0/0
  ipv6 traffic-filter BLOCK-TELNET in
```

### Verification - IPv6

```
show ipv6 interface brief                           สรุป IPv6 interfaces
show ipv6 interface [ชนิด] [หมายเลข]                รายละเอียด interface
show ipv6 route                                     แสดง IPv6 routing table
show ipv6 neighbors                                 แสดง IPv6 neighbor table
show ipv6 protocols                                 แสดง IPv6 routing protocols
show ipv6 ospf neighbor                             แสดง OSPFv3 neighbors
show ipv6 eigrp neighbors                           แสดง EIGRPv6 neighbors
ping ipv6 [IPv6]                                    ping IPv6 address
traceroute ipv6 [IPv6]                              traceroute IPv6
```

---

## Wireless LANs

### Wireless Standards

```
802.11a   - 5 GHz, 54 Mbps
802.11b   - 2.4 GHz, 11 Mbps
802.11g   - 2.4 GHz, 54 Mbps
802.11n   - 2.4/5 GHz, 600 Mbps
802.11ac  - 5 GHz, 1.3 Gbps+
802.11ax  - 2.4/5/6 GHz, 9.6 Gbps+ (Wi-Fi 6)
```

### Wireless Security

```
WEP       - เก่า, ไม่ปลอดภัย (ห้ามใช้)
WPA       - TKIP, better than WEP
WPA2      - AES-CCMP, แนะนำ
WPA3      - ปลอดภัยที่สุด, ใหม่ล่าสุด
```

**Authentication Modes:**

- **Personal (PSK)** - Pre-shared key
- **Enterprise (802.1X)** - RADIUS server

### Lightweight AP Configuration

**WLC (Wireless LAN Controller) Basic:**

```
หมายเหตุ: WLC ใช้ GUI เป็นหลัก, แต่มี CLI ด้วย
```

**Access Point Registration:**

```
Switch Configuration for AP:
interface [ชนิด] [หมายเลข]
  switchport mode access
  switchport access vlan [management-vlan]
  power inline auto                                 enable PoE
  spanning-tree portfast
```

### Wireless VLAN

```
interface [ชนิด] [หมายเลข]
  switchport mode trunk
  switchport trunk native vlan [management-vlan]
  switchport trunk allowed vlan [data-vlans]
```

---

## QoS (Quality of Service)

### QoS Models

```
Best Effort   - ไม่มี QoS
IntServ       - Reservation-based (RSVP)
DiffServ      - Class-based (แนะนำ)
```

### QoS Mechanisms

```
Classification  - จำแนก traffic
Marking         - mark packets (DSCP, CoS, IP Precedence)
Policing        - จำกัด bandwidth
Shaping         - smooth traffic flow
Queuing         - จัดลำดับ packets
```

### Classification & Marking

```
class-map match-any [ชื่อ]
  match protocol [protocol]
  match access-group [ACL]
  match ip dscp [value]
  match ip precedence [value]

policy-map [ชื่อ]
  class [class-map name]
    set ip dscp [value]
    set ip precedence [value]
    set cos [value]
    police [bps]
    bandwidth [kbps]
    priority [kbps]

interface [ชนิด] [หมายเลข]
  service-policy input [policy-map]
  service-policy output [policy-map]
```

**ตัวอย่าง:**

```
class-map match-any VOICE
  match protocol rtp audio

policy-map QOS-POLICY
  class VOICE
    priority 128
    set ip dscp ef

interface g0/0
  service-policy output QOS-POLICY
```

### DSCP Values (ควรจำ)

```
EF (Expedited Forwarding)  - 46  - Voice
AF41                       - 34  - Video
AF31                       - 26  - Mission-critical data
AF21                       - 18  - Transactional data
AF11                       - 10  - Bulk data
BE (Best Effort)           - 0   - Default
```

### Verification - QoS

```
show policy-map                                     แสดง policy maps
show policy-map interface [ชนิด] [หมายเลข]         แสดง QoS statistics
show class-map                                      แสดง class maps
show mls qos                                        แสดง QoS status (Switch)
```

---

## Network Management

### Syslog

```
logging [server IP]                                 กำหนด syslog server
logging trap [level]                                กำหนด severity level
logging source-interface [ชนิด] [หมายเลข]          กำหนด source interface
logging buffered [size]                             เก็บ log ใน buffer
logging console [level]                             แสดง log บน console
service timestamps log datetime msec                timestamp บน logs
```

**Severity Levels:**

```
0 - emergencies        ระบบไม่สามารถใช้งานได้
1 - alerts             ต้องดำเนินการทันที
2 - critical           critical conditions
3 - errors             error conditions
4 - warnings           warning conditions
5 - notifications      normal but significant
6 - informational      informational messages
7 - debugging          debug messages
```

**ตัวอย่าง:**

```
logging 192.168.1.100
logging trap warnings
logging source-interface g0/0
service timestamps log datetime msec
```

### SNMP (Simple Network Management Protocol)

```
snmp-server community [string] [ro|rw]              กำหนด community string
snmp-server location [text]                         ตั้งค่า location
snmp-server contact [text]                          ตั้งค่า contact
snmp-server host [IP] version [1|2c|3] [community]  กำหนด SNMP manager
snmp-server enable traps                            เปิด SNMP traps
```

**SNMP Versions:**

- **v1** - ไม่มีความปลอดภัย
- **v2c** - มี bulk transfers, ยังไม่ปลอดภัย
- **v3** - มี authentication และ encryption (แนะนำ)

**ตัวอย่าง:**

```
snmp-server community public ro
snmp-server community private rw
snmp-server location "Server Room A"
snmp-server contact "admin@example.com"
snmp-server host 192.168.1.100 version 2c public
snmp-server enable traps
```

### NTP (Network Time Protocol)

```
ntp server [IP]                                     กำหนด NTP server
ntp master [stratum]                                ตั้งเป็น NTP master
ntp authenticate                                    เปิด NTP authentication
ntp authentication-key [key-id] md5 [password]      กำหนด authentication key
ntp trusted-key [key-id]                            กำหนด trusted key
ntp source [interface]                              กำหนด source interface
```

**ตัวอย่าง:**

```
ntp server 129.6.15.28
ntp server 132.163.96.1
clock timezone ICT 7
```

### CDP (Cisco Discovery Protocol)

```
cdp run                                             เปิด CDP (default on)
no cdp run                                          ปิด CDP ทั้งระบบ
cdp timer [seconds]                                 กำหนด update interval
cdp holdtime [seconds]                              กำหนด holdtime

interface [ชนิด] [หมายเลข]
  cdp enable                                        เปิด CDP
  no cdp enable                                     ปิด CDP บน interface
```

### LLDP (Link Layer Discovery Protocol)

```
lldp run                                            เปิด LLDP
no lldp run                                         ปิด LLDP
lldp timer [seconds]                                update interval
lldp holdtime [seconds]                             holdtime

interface [ชนิด] [หมายเลข]
  lldp transmit                                     ส่ง LLDP
  lldp receive                                      รับ LLDP
  no lldp transmit                                  ไม่ส่ง LLDP
  no lldp receive                                   ไม่รับ LLDP
```

**Verification:**

```
show cdp                                            แสดงสถานะ CDP
show cdp neighbors                                  แสดง CDP neighbors
show cdp neighbors detail                           รายละเอียด neighbors
show cdp interface                                  แสดง CDP interfaces
show cdp entry [hostname]                           แสดงข้อมูลเฉพาะ device

show lldp                                           แสดงสถานะ LLDP
show lldp neighbors                                 แสดง LLDP neighbors
show lldp neighbors detail                          รายละเอียด neighbors
show lldp interface                                 แสดง LLDP interfaces
```

---

## AAA (Authentication, Authorization, Accounting)

### AAA Overview

```
Authentication  - ตรวจสอบ identity (who are you?)
Authorization   - ตรวจสอบ permission (what can you do?)
Accounting      - บันทึก activity (what did you do?)
```

### Local AAA (Username/Password)

```
aaa new-model                                       เปิด AAA

username [ชื่อ] privilege [level] secret [password]

aaa authentication login default local
aaa authentication login [list-name] local
aaa authorization exec default local
aaa accounting exec default start-stop local

line vty 0 15
  login authentication default
  authorization exec default
  accounting exec default
```

### AAA with RADIUS/TACACS+

```
aaa new-model

radius server [ชื่อ]
  address ipv4 [IP] auth-port 1812 acct-port 1813
  key [shared-secret]

หรือ

tacacs server [ชื่อ]
  address ipv4 [IP]
  key [shared-secret]
  
aaa authentication login default group radius local
aaa authentication login default group tacacs+ local
aaa authorization exec default group radius local
aaa accounting exec default start-stop group radius

line vty 0 15
  login authentication default
  authorization exec default
  accounting exec default
```

**ตัวอย่าง RADIUS:**

```
aaa new-model

radius server RADIUS-SERVER
  address ipv4 192.168.1.50 auth-port 1812 acct-port 1813
  key Cisco123!

aaa authentication login default group radius local
aaa authorization exec default group radius local

line vty 0 15
  login authentication default
```

### Verification - AAA

```
show aaa servers                                    แสดง AAA servers
show aaa sessions                                   แสดง active sessions
debug aaa authentication                            debug authentication
debug radius                                        debug RADIUS
debug tacacs                                        debug TACACS+
```

---

## Device Management

### Password Recovery

**Router:**

```
1. รีสตาร์ทและกด Ctrl+Break เข้า ROMMON
2. confreg 0x2142                                   bypass startup-config
3. reset
4. enable
5. copy startup-config running-config               โหลด config เก่า
6. configure terminal
7. enable secret [new password]
8. config-register 0x2102                           คืนค่า config register
9. copy running-config startup-config
10. reload
```

**Switch:**

```
1. ปิดเครื่อง, กด Mode button ค้างไว้แล้วเปิดเครื่อง
2. flash_init
3. load_helper
4. rename flash:config.text flash:config.old
5. boot
6. enable
7. rename flash:config.old flash:config.text
8. copy flash:config.text running-config
9. configure terminal
10. enable secret [new password]
11. copy running-config startup-config
```

### IOS Management

```
show flash:                                         แสดงไฟล์ใน flash
dir flash:                                          แสดงไฟล์ใน flash
show version                                        แสดง IOS version

delete flash:[filename]                             ลบไฟล์
copy tftp flash                                     copy IOS จาก TFTP
copy flash tftp                                     backup IOS ไป TFTP
boot system flash:[filename]                        กำหนด boot image
```

**Upgrade IOS:**

```
1. backup current IOS
   copy flash: tftp:

2. verify new IOS MD5
   verify /md5 flash:[filename]

3. copy new IOS to flash
   copy tftp flash

4. set boot system
   configure terminal
   boot system flash:[new-ios-filename]
   exit

5. save and reload
   copy running-config startup-config
   reload
```

### License Management (ISR G2)

```
license boot module [module] technology-package [package]
license install [url]
license save flash:[filename]
license clear [feature]

show license
show license feature
show license udi
show version
```

---

## Troubleshooting Methodology

### OSI Model Troubleshooting

**Bottom-Up Approach:**

```
Layer 1 (Physical)
  - cables, connectors
  - show interfaces
  - show controllers

Layer 2 (Data Link)
  - MAC addresses, VLANs, trunking
  - show mac address-table
  - show vlan
  - show interfaces trunk

Layer 3 (Network)
  - IP addressing, routing
  - show ip interface brief
  - show ip route
  - ping

Layer 4 (Transport)
  - TCP/UDP ports
  - show ip sockets
  - show control-plane host

Layer 5-7 (Application)
  - services, protocols
  - telnet, HTTP, DNS testing
```

### Common Issues

**Interface Down:**

```
1. show interfaces [ชนิด] [หมายเลข]
2. ตรวจสอบ:
   - administratively down → no shutdown
   - line protocol down → cable, speed/duplex mismatch
   - err-disabled → port security violation, loop
3. show interfaces status
4. show interfaces counters errors
```

**Connectivity Issues:**

```
1. ping [destination]
   - ถ้าไม่ผ่าน → ตรวจสอบ Layer 1-3
2. traceroute [destination]
   - ดูว่าติดที่ hop ไหน
3. show ip route
   - ตรวจสอบ routing table
4. show arp
   - ตรวจสอบ ARP table
5. show ip interface brief
   - status interfaces
```

**VLAN Issues:**

```
1. show vlan brief
   - ตรวจสอบว่า VLAN มี
2. show interfaces trunk
   - trunk ขึ้นไหม, allowed VLANs ถูกไหม
3. show interfaces switchport
   - port อยู่ใน VLAN ถูกไหม
4. show spanning-tree vlan [vlan-id]
   - STP blocking ไหม
```

**Routing Issues:**

```
1. show ip route
   - มี route ไหม
2. show ip protocols
   - routing protocol ทำงานไหม
3. show ip ospf neighbor (ถ้าใช้ OSPF)
   - neighbor ขึ้นไหม
4. show ip rip database (ถ้าใช้ RIP)
   - RIP routes
5. debug ip routing
   - ดู routing updates
```

### Useful Debug Commands

```
debug ip packet                                     debug IP packets (ระวัง CPU!)
debug ip icmp                                       debug ICMP
debug ip routing                                    debug routing changes
debug ip rip                                        debug RIP
debug ip ospf events                                debug OSPF events
debug ip eigrp                                      debug EIGRP
debug arp                                           debug ARP
debug spanning-tree events                          debug STP events
debug etherchannel                                  debug EtherChannel

undebug all                                         ปิด debug ทั้งหมด
no debug all                                        ปิด debug ทั้งหมด
```

**หมายเหตุ:** debug commands ใช้ CPU สูง! ใช้ด้วยความระมัดระวัง

---

## Advanced Features

### Port Mirroring (SPAN)

**Local SPAN:**

```
monitor session [1-66] source interface [ชนิด] [หมายเลข]
monitor session [1-66] destination interface [ชนิด] [หมายเลข]
```

**ตัวอย่าง:**

```
monitor session 1 source interface g0/1
monitor session 1 destination interface g0/24
```

**RSPAN (Remote SPAN):**

```
vlan [vlan-id]
  remote-span

monitor session [1-66] source interface [ชนิด] [หมายเลข]
monitor session [1-66] destination remote vlan [vlan-id]
```

### Storm Control

```
interface [ชนิด] [หมายเลข]
  storm-control broadcast level [percentage]        จำกัด broadcast
  storm-control multicast level [percentage]        จำกัด multicast
  storm-control unicast level [percentage]          จำกัด unknown unicast
  storm-control action [shutdown|trap]              action เมื่อเกิน threshold
```

**ตัวอย่าง:**

```
interface g0/1
  storm-control broadcast level 10
  storm-control action shutdown
```

### Private VLANs

```
vlan [primary-vlan-id]
  private-vlan primary
  private-vlan association [secondary-vlan-list]

vlan [secondary-vlan-id]
  private-vlan [isolated|community]

interface [ชนิด] [หมายเลข]
  switchport mode private-vlan host
  switchport private-vlan host-association [primary] [secondary]
```

### Dynamic ARP Inspection (DAI)

```
ip arp inspection vlan [vlan-range]                 เปิด DAI บน VLAN

interface [ชนิด] [หมายเลข]
  ip arp inspection trust                           ตั้ง trusted port
  ip arp inspection limit rate [pps]                จำกัด ARP rate
```

**ตัวอย่าง:**

```
ip arp inspection vlan 10,20
ip arp inspection validate src-mac dst-mac ip

interface g0/1
  ip arp inspection trust
```

### IP Source Guard

```
interface [ชนิด] [หมายเลข]
  ip verify source                                  เปิด IP Source Guard
  ip verify source port-security                    กับ MAC address
```

### DHCP Snooping

```
ip dhcp snooping                                    เปิด DHCP Snooping
ip dhcp snooping vlan [vlan-range]                  เปิดบน VLANs

interface [ชนิด] [หมายเลข]
  ip dhcp snooping trust                            trusted port (ต่อ DHCP server)
  ip dhcp snooping limit rate [pps]                 จำกัด DHCP rate
```

**ตัวอย่าง:**

```
ip dhcp snooping
ip dhcp snooping vlan 10,20

interface g0/1
  ip dhcp snooping trust

interface range f0/1-20
  ip dhcp snooping limit rate 10
```

---

## Best Practices Summary

### Security Hardening

```
! Disable unused services
no ip http server
no ip http secure-server
no cdp run
no service pad
no ip bootp server
no ip domain-lookup

! Strong passwords
enable secret [strong-password]
service password-encryption
security passwords min-length 8

! SSH only
ip domain-name [domain]
crypto key generate rsa modulus 2048
ip ssh version 2
line vty 0 15
  transport input ssh
  login local
  exec-timeout 5 0

! Console security
line console 0
  password [password]
  login
  exec-timeout 5 0
  logging synchronous

! Banner
banner motd #
Unauthorized access prohibited!
All activities are monitored and logged.
#

! AAA if possible
aaa new-model
aaa authentication login default local

! Port security on access ports
interface range f0/1-24
  switchport mode access
  switchport port-security
  switchport port-security maximum 2
  switchport port-security mac-address sticky
  switchport port-security violation restrict

! Disable unused ports
interface range f0/21-24
  shutdown
  switchport mode access
  switchport access vlan 999
```

### Network Design Best Practices

```
1. Plan IP addressing (use VLSM)
2. Segment with VLANs
3. Use hierarchical design (Core-Distribution-Access)
4. Implement redundancy (EtherChannel, HSRP/VRRP/GLBP)
5. Use routing protocols (OSPF/EIGRP) แทน static routes
6. Document everything!
```

### Documentation Template

```
Network Name: __________
Date: __________

Devices:
  - Hostname, Model, IOS Version, Management IP

IP Addressing:
  - Network ranges, VLANs, subnets

Routing:
  - Protocols used, AS numbers, areas

Security:
  - ACLs, port security, authentication methods

Topology Diagram: [attach]
```

---

## Quick Reference Tables

### Subnet Masks

```
CIDR    Subnet Mask       Wildcard Mask    Hosts
/24     255.255.255.0     0.0.0.255        254
/25     255.255.255.128   0.0.0.127        126
/26     255.255.255.192   0.0.0.63         62
/27     255.255.255.224   0.0.0.31         30
/28     255.255.255.240   0.0.0.15         14
/29     255.255.255.248   0.0.0.7          6
/30     255.255.255.252   0.0.0.3          2
```

### Common Port Numbers

```
Service         TCP/UDP     Port
--------------------------------------
FTP Data        TCP         20
FTP Control     TCP         21
SSH             TCP         22
Telnet          TCP         23
SMTP            TCP         25
DNS             TCP/UDP     53
DHCP Server     UDP         67
DHCP Client     UDP         68
TFTP            UDP         69
HTTP            TCP         80
POP3            TCP         110
NTP             UDP         123
NetBIOS         TCP/UDP     137-139
SNMP            UDP         161
SNMP Trap       UDP         162
HTTPS           TCP         443
Syslog          UDP         514
RIP             UDP         520
RADIUS Auth     UDP         1812
RADIUS Acct     UDP         1813
```

### EtherChannel Protocol Compatibility

```
Mode        PAgP        LACP        Result
------------------------------------------------
on          on          on          EtherChannel (no negotiation)
desirable   desirable   -           EtherChannel
desirable   auto        -           EtherChannel
auto        auto        -           No EtherChannel
active      -           active      EtherChannel
active      -           passive     EtherChannel
passive     -           passive     No EtherChannel
```

### HSRP/VRRP/GLBP Comparison

```
Feature         HSRP            VRRP            GLBP
---------------------------------------------------------------
Standard        Cisco           Open (RFC 5798) Cisco
Multicast       224.0.0.2       224.0.0.18      224.0.0.102
                (v2: 224.0.0.102)
Default Pr.     100             100             100
Preempt         Disabled        Enabled         Disabled
Load Bal.       No              No              Yes (4 routers)
Active          1               1               1 AVG + 4 AVF
Virtual MAC     0000.0c07.acXX  0000.5e00.01XX  0007.b400.XXYY
```

### STP Port States

```
802.1D          802.1w (RSTP)   Forward?    Learn MAC?
------------------------------------------------------------
Disabled        Discarding      No          No
Blocking        Discarding      No          No
Listening       Discarding      No          No
Learning        Learning        No          Yes
Forwarding      Forwarding      Yes         Yes
```

### Default Administrative Distances

```
Protocol                AD
---------------------------------
Directly Connected      0
Static Route            1
EIGRP Summary           5
eBGP                    20
EIGRP (internal)        90
IGRP                    100
OSPF                    110
IS-IS                   115
RIP                     120
EIGRP (external)        170
iBGP                    200
```

---

## Keyboard Shortcuts & CLI Tips

### Navigation Shortcuts

```
Ctrl+A          ไปต้นบรรทัด
Ctrl+E          ไปท้ายบรรทัด
Ctrl+B          ถอยหลัง 1 ตัวอักษร
Ctrl+F          ไปหน้า 1 ตัวอักษร
Esc+B           ถอยหลัง 1 คำ
Esc+F           ไปหน้า 1 คำ
Ctrl+D          ลบตัวอักษรที่ cursor
Ctrl+K          ลบจาก cursor ถึงท้ายบรรทัด
Ctrl+U          ลบจาก cursor ถึงต้นบรรทัด
Ctrl+W          ลบคำก่อนหน้า
Ctrl+Y          paste คำที่ลบล่าสุด
```

### Command Shortcuts

```
Ctrl+C          ยกเลิกคำสั่ง
Ctrl+Z          กลับ privileged mode (เหมือน end)
Ctrl+Shift+6    หยุด ping, traceroute
Tab             auto-complete
?               help
Space           หน้าถัดไปใน output
Enter           บรรทัดถัดไปใน output
```

### CLI Tips

```
terminal length 0               ปิด pagination
terminal length 24              pagination (default)
terminal history size 256       เพิ่ม history size
show history                    แสดง command history
```

**Command Abbreviation:**

```
sh run                  = show running-config
sh ip int br            = show ip interface brief
conf t                  = configure terminal
int g0/0                = interface gigabitethernet 0/0
do sh ip route          = ใช้ show ใน config mode
```

---

## Configuration Templates

### Small Office Router Template

```
!
hostname OFFICE-R1
!
enable secret Cisco123!
service password-encryption
no ip domain-lookup
!
username admin privilege 15 secret Admin123!
!
ip domain-name office.local
crypto key generate rsa modulus 2048
ip ssh version 2
ip ssh time-out 60
ip ssh authentication-retries 2
!
banner motd #
*************************************************
*  Authorized Access Only                       *
*  All activities are logged and monitored      *
*************************************************
#
!
interface GigabitEthernet0/0
 description WAN Link to ISP
 ip address dhcp
 ip nat outside
 no shutdown
!
interface GigabitEthernet0/1
 description LAN Network
 ip address 192.168.1.1 255.255.255.0
 ip nat inside
 no shutdown
!
ip nat inside source list 1 interface GigabitEthernet0/0 overload
access-list 1 permit 192.168.1.0 0.0.0.255
!
ip route 0.0.0.0 0.0.0.0 GigabitEthernet0/0
!
ip dhcp excluded-address 192.168.1.1 192.168.1.10
ip dhcp pool LAN-POOL
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 8.8.8.8 8.8.4.4
 lease 7
!
line console 0
 password Console123!
 login
 logging synchronous
 exec-timeout 5 0
!
line vty 0 4
 transport input ssh
 login local
 exec-timeout 5 0
!
ntp server 129.6.15.28
ntp server 132.163.96.1
!
logging 192.168.1.100
logging trap warnings
!
end
```

### Access Switch Template

```
!
hostname ACCESS-SW1
!
enable secret Cisco123!
service password-encryption
no ip domain-lookup
!
username admin privilege 15 secret Admin123!
!
ip domain-name office.local
crypto key generate rsa modulus 2048
ip ssh version 2
!
banner motd # Authorized Access Only #
!
vlan 10
 name DATA
vlan 20
 name VOICE
vlan 99
 name MANAGEMENT
vlan 999
 name UNUSED
!
interface vlan 99
 ip address 192.168.99.10 255.255.255.0
 no shutdown
!
ip default-gateway 192.168.99.1
!
spanning-tree mode rapid-pvst
spanning-tree portfast bpduguard default
spanning-tree portfast default
!
interface range FastEthernet0/1-20
 description User Access Ports
 switchport mode access
 switchport access vlan 10
 switchport port-security
 switchport port-security maximum 2
 switchport port-security mac-address sticky
 switchport port-security violation restrict
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface range FastEthernet0/21-24
 description Unused Ports
 switchport mode access
 switchport access vlan 999
 shutdown
!
interface GigabitEthernet0/1
 description Trunk to Core Switch
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,99
 switchport nonegotiate
!
line console 0
 password Console123!
 login
 logging synchronous
 exec-timeout 5 0
!
line vty 0 15
 transport input ssh
 login local
 exec-timeout 5 0
!
ntp server 192.168.99.1
!
logging 192.168.1.100
!
end
```

### Core Switch/Router Template

```
!
hostname CORE-SW1
!
enable secret Cisco123!
service password-encryption
!
ip routing
!
vlan 10
 name DATA
vlan 20
 name VOICE
vlan 99
 name MANAGEMENT
!
interface vlan 10
 description Data VLAN
 ip address 192.168.10.1 255.255.255.0
 ip helper-address 192.168.99.5
 no shutdown
!
interface vlan 20
 description Voice VLAN
 ip address 192.168.20.1 255.255.255.0
 no shutdown
!
interface vlan 99
 description Management VLAN
 ip address 192.168.99.1 255.255.255.0
 no shutdown
!
spanning-tree mode rapid-pvst
spanning-tree vlan 1,10,20,99 root primary
!
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp pool DATA-POOL
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8 8.8.4.4
!
interface GigabitEthernet0/1
 description Trunk to ACCESS-SW1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
!
interface GigabitEthernet0/2
 description Trunk to ACCESS-SW2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
!
interface GigabitEthernet1/1
 description Link to Router
 no switchport
 ip address 10.0.0.1 255.255.255.252
 no shutdown
!
router ospf 1
 router-id 1.1.1.1
 network 192.168.10.0 0.0.0.255 area 0
 network 192.168.20.0 0.0.0.255 area 0
 network 192.168.99.0 0.0.0.255 area 0
 network 10.0.0.0 0.0.0.3 area 0
 passive-interface vlan 10
 passive-interface vlan 20
 passive-interface vlan 99
!
line vty 0 15
 transport input ssh
 login local
!
end
```

---

**Created for:** NetAcad Course 2 & 3 Preparation  
**Last Updated:** November 2025  
**Version:** 2.0 - Comprehensive Edition

**หมายเหตุสำคัญ:**

- คำสั่งบางตัวอาจแตกต่างตามรุ่น IOS และ platform
- ควรทดสอบใน lab ก่อนใช้งานจริงเสมอ
- อ่าน Cisco documentation สำหรับรายละเอียดเพิ่มเติม
- Backup configuration เป็นประจำ
- จดบันทึก network changes ทุกครั้ง

**แหล่งข้อมูลเพิ่มเติม:**

- Cisco.com Documentation
- Cisco Learning Network
- PacketTracer Labs
- GNS3 Labs