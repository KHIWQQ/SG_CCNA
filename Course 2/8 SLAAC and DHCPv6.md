# CCNA 2: Module 8 - SLAAC and DHCPv6

## Stateless Address Autoconfiguration and DHCPv6

---

## สารบัญ

1. [วัตถุประสงค์ของ Module](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#%E0%B8%A7%E0%B8%B1%E0%B8%95%E0%B8%96%E0%B8%B8%E0%B8%9B%E0%B8%A3%E0%B8%B0%E0%B8%AA%E0%B8%87%E0%B8%84%E0%B9%8C%E0%B8%82%E0%B8%AD%E0%B8%87-module)
2. [IPv6 Global Unicast Address Assignment](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#1-ipv6-global-unicast-address-assignment)
3. [SLAAC (Stateless Address Autoconfiguration)](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#2-slaac-stateless-address-autoconfiguration)
4. [DHCPv6](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#3-dhcpv6)
5. [Configure DHCPv6 Server](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#4-configure-dhcpv6-server)
6. [สรุป](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#%E0%B8%AA%E0%B8%A3%E0%B8%B8%E0%B8%9B)

---

## วัตถุประสงค์ของ Module

เมื่อจบ Module นี้ คุณจะสามารถ:

- ✅ อธิบายวิธีการกำหนด IPv6 address
- ✅ เข้าใจ SLAAC (Stateless Address Autoconfiguration)
- ✅ อธิบาย ICMPv6 Router Solicitation และ Router Advertisement
- ✅ Configure SLAAC
- ✅ เข้าใจความแตกต่างระหว่าง DHCPv4 และ DHCPv6
- ✅ Configure Stateless DHCPv6
- ✅ Configure Stateful DHCPv6
- ✅ Verify และ Troubleshoot IPv6 addressing

---

## 1. IPv6 Global Unicast Address Assignment

### การกำหนด IPv6 Address

### 1.1 IPv6 Address Types Review

**IPv6 Address Types:**

```
1. Global Unicast Address (GUA)
   - เหมือน public IPv4
   - Routable บน Internet
   - Unique globally
   - Range: 2000::/3

2. Link-Local Address (LLA)
   - เหมือน APIPA (169.254.x.x)
   - ใช้ใน local link เท่านั้น
   - ไม่ routable
   - Range: FE80::/10
   - สร้างอัตโนมัติทุก interface

3. Unique Local Address (ULA)
   - เหมือน private IPv4
   - ใช้ใน organization
   - ไม่ routable บน Internet
   - Range: FC00::/7
```

### 1.2 วิธีการกำหนด IPv6 GUA

**3 วิธีในการกำหนด IPv6 Global Unicast Address:**

```
Method 1: Static Assignment (Manual)
- Administrator กำหนด IP address เอง
- ใช้สำหรับ servers, routers, network devices

Method 2: SLAAC (Stateless Address Autoconfiguration)
- Device สร้าง IP address เอง
- ไม่ต้องใช้ DHCP server
- ใช้ Router Advertisement (RA)
- IEEE standard

Method 3: DHCPv6 (Dynamic Host Configuration Protocol v6)
- เหมือน DHCPv4
- DHCP server กำหนด IP
- 2 รูปแบบ: Stateless และ Stateful
```

### 1.3 Static IPv6 Address Configuration

**Configure Static IPv6 Address:**

```cisco
! บน Router
Router(config)# ipv6 unicast-routing
Router(config)# interface gigabitethernet 0/0
Router(config-if)# ipv6 address 2001:db8:acad:1::1/64
Router(config-if)# ipv6 address fe80::1 link-local
Router(config-if)# no shutdown

! บน PC/Host
Windows:
netsh interface ipv6 set address "Ethernet" 2001:db8:acad:1::10

Linux:
sudo ip -6 addr add 2001:db8:acad:1::10/64 dev eth0
```

### 1.4 EUI-64 Process

**EUI-64 (Extended Unique Identifier):**

- วิธีสร้าง Interface ID (64 bits ล่าง) จาก MAC address
- ใช้กับ SLAAC
- ทำให้ address unique

**EUI-64 Calculation:**

```
MAC Address: 00:50:BA:8D:B4:11 (48 bits)

Step 1: แบ่ง MAC เป็นสองส่วน
00:50:BA | 8D:B4:11

Step 2: แทรก FFFE ตรงกลาง
00:50:BA:FF:FE:8D:B4:11

Step 3: Flip bit ที่ 7 (Universal/Local bit)
00000000 → 00000010
00 → 02

Result: 02:50:BA:FF:FE:8D:B4:11

Interface ID: 0250:BAFF:FE8D:B411

Complete IPv6:
Prefix: 2001:db8:acad:1::
Interface ID: 0250:baff:fe8d:b411
Full Address: 2001:db8:acad:1:0250:baff:fe8d:b411/64
```

**Configure EUI-64:**

```cisco
Router(config)# interface gigabitethernet 0/0
Router(config-if)# ipv6 address 2001:db8:acad:1::/64 eui-64
Router(config-if)# no shutdown

! Router สร้าง Interface ID จาก MAC address อัตโนมัติ
```

**⚠️ Privacy Concern:**

```
EUI-64 ใช้ MAC address
→ สามารถ track device ได้
→ Privacy issue

Solution:
Windows/Linux ใช้ Privacy Extensions (RFC 4941)
- สร้าง random Interface ID
- เปลี่ยนเป็นระยะ
- ไม่ใช้ MAC address
```

---

## 2. SLAAC (Stateless Address Autoconfiguration)

### การกำหนด Address อัตโนมัติแบบไม่เก็บสถานะ

### 2.1 SLAAC คืออะไร?

**SLAAC (Stateless Address Autoconfiguration):**

- RFC 4862
- Device สร้าง IPv6 address เอง
- ไม่ต้องมี DHCP server
- ใช้ Router Advertisement (RA) messages
- "Stateless" = Router ไม่เก็บข้อมูลว่าแจก IP ให้ใคร

**ข้อดี:**

```
✅ ไม่ต้องมี DHCP server (ประหยัด)
✅ Auto-configuration (ง่าย)
✅ Plug-and-play
✅ ไม่ต้อง manual configuration
✅ Scalable
```

**ข้อเสีย:**

```
❌ ไม่มี centralized control
❌ ไม่สามารถกำหนด DNS server
❌ ไม่ track ว่า IP ไหนใช้แล้ว
❌ ยากต่อการ audit
```

### 2.2 ICMPv6 RS and RA Messages

**ICMPv6 Router Solicitation (RS):**

```
- Type 133
- ส่งโดย host เมื่อ boot up
- ถาม: "มี router ไหมครับ?"
- Destination: FF02::2 (all-routers multicast)
- Source: Link-local address

Purpose:
Host ต้องการ prefix information ทันที
ไม่รอ periodic RA
```

**ICMPv6 Router Advertisement (RA):**

```
- Type 134
- ส่งโดย router
- ตอบ RS หรือส่งเป็นระยะ (default ทุก 200 วินาที)
- Destination: 
  * FF02::1 (all-nodes multicast) - periodic
  * Link-local ของ host - response to RS
- Source: Link-local address ของ router

Information ใน RA:
- Network prefix (64 bits)
- Prefix length
- Default gateway (router's link-local)
- MTU
- Hop limit
- Flags (M, O, A)
```

**RA Message Flags:**

```
M Flag (Managed Address Configuration):
0 = ใช้ SLAAC
1 = ใช้ DHCPv6 (Stateful)

O Flag (Other Configuration):
0 = ไม่ใช้ DHCPv6
1 = ใช้ DHCPv6 สำหรับข้อมูลอื่น (Stateless DHCPv6)

A Flag (Autonomous):
1 = ใช้ SLAAC (ปกติ)
0 = ไม่ใช้ SLAAC
```

### 2.3 SLAAC Process

**SLAAC Operation (3 Methods):**

**Method 1: SLAAC Only**

```
Flags: M=0, O=0, A=1

Process:
1. Host ส่ง RS (Router Solicitation)
2. Router ส่ง RA (Router Advertisement) กลับมา
   - Prefix: 2001:db8:acad:1::/64
   - M=0, O=0
3. Host สร้าง GUA:
   - Prefix จาก RA: 2001:db8:acad:1::
   - Interface ID: EUI-64 หรือ random
   - Complete: 2001:db8:acad:1:xxxx:xxxx:xxxx:xxxx/64
4. Host ใช้ router's link-local เป็น default gateway
5. Host ไม่มี DNS (ต้อง manual)

Result:
✅ IPv6 address
✅ Prefix length
✅ Default gateway
❌ DNS server (ต้องตั้ง manual)
```

**Method 2: SLAAC + Stateless DHCPv6**

```
Flags: M=0, O=1, A=1

Process:
1-4. เหมือน SLAAC Only
5. Host contact DHCPv6 server สำหรับข้อมูลอื่น:
   - DNS server
   - Domain name
   - NTP server
   - etc.

Result:
✅ IPv6 address (จาก SLAAC)
✅ Prefix length (จาก SLAAC)
✅ Default gateway (จาก RA)
✅ DNS server (จาก DHCPv6)
✅ Other options (จาก DHCPv6)
```

**Method 3: Stateful DHCPv6**

```
Flags: M=1, O=1, A=0

Process:
1. Host ส่ง RS
2. Router ส่ง RA with M=1
3. Host ติดต่อ DHCPv6 server
4. DHCPv6 server ให้:
   - IPv6 address
   - Prefix length
   - DNS server
   - Other parameters
5. Host ยังใช้ router's link-local เป็น default gateway (จาก RA)

Result:
✅ IPv6 address (จาก DHCPv6)
✅ Prefix length (จาก DHCPv6)
✅ Default gateway (จาก RA)
✅ DNS server (จาก DHCPv6)
✅ Tracking (DHCPv6 เก็บ binding)

Note: Default gateway ยังมาจาก RA เสมอ!
```

### 2.4 Configure SLAAC on Router

**Enable SLAAC (Method 1: SLAAC Only):**

```cisco
! Step 1: Enable IPv6 routing
Router(config)# ipv6 unicast-routing

! Step 2: Configure interface
Router(config)# interface gigabitethernet 0/0
Router(config-if)# description LAN Interface
Router(config-if)# ipv6 address 2001:db8:acad:1::1/64
Router(config-if)# ipv6 address fe80::1 link-local
Router(config-if)# no shutdown

! Step 3: Verify RA (default M=0, O=0)
Router# show ipv6 interface gigabitethernet 0/0

GigabitEthernet0/0 is up, line protocol is up
  IPv6 is enabled, link-local address is FE80::1
  No Virtual link-local address(es):
  Global unicast address(es):
    2001:DB8:ACAD:1::1, subnet is 2001:DB8:ACAD:1::/64
  ...
  ND DAD is enabled, number of DAD attempts: 1
  ND reachable time is 30000 milliseconds
  ND advertised reachable time is 0 milliseconds
  ND advertised retransmit interval is 0 milliseconds
  ND router advertisements are sent every 200 seconds
  ND router advertisements live for 1800 seconds
  ND advertised default router preference is Medium
  Hosts use stateless autoconfig for addresses.
                    ↑ SLAAC enabled
```

**Default Behavior:**

- `ipv6 unicast-routing` เปิดการส่ง RA อัตโนมัติ
- M=0, O=0 (SLAAC only)
- Hosts สร้าง IPv6 address เอง

### 2.5 Modify RA Flags

**Configure SLAAC with Stateless DHCPv6 (Method 2):**

```cisco
Router(config)# interface gigabitethernet 0/0
Router(config-if)# ipv6 address 2001:db8:acad:1::1/64
Router(config-if)# ipv6 nd other-config-flag
! O flag = 1 (ใช้ DHCPv6 สำหรับ DNS, domain, etc.)

! Verify
Router# show ipv6 interface gigabitethernet 0/0
  ...
  Hosts use stateless autoconfig for addresses.
  Hosts use DHCP to obtain other configuration.
```

**Configure Stateful DHCPv6 (Method 3):**

```cisco
Router(config)# interface gigabitethernet 0/0
Router(config-if)# ipv6 address 2001:db8:acad:1::1/64
Router(config-if)# ipv6 nd managed-config-flag
! M flag = 1 (ใช้ DHCPv6 สำหรับ address)
Router(config-if)# ipv6 nd other-config-flag
! O flag = 1 (ใช้ DHCPv6 สำหรับข้อมูลอื่น)

! Verify
Router# show ipv6 interface gigabitethernet 0/0
  ...
  Hosts use DHCP to obtain routable addresses.
  Hosts use DHCP to obtain other configuration.
```

**Disable SLAAC:**

```cisco
Router(config)# interface gigabitethernet 0/0
Router(config-if)# ipv6 nd prefix default no-autoconfig
! A flag = 0 (ปิด SLAAC)
```

### 2.6 DAD (Duplicate Address Detection)

**DAD Process:**

```
เมื่อ host สร้าง IPv6 address ใหม่:

Step 1: Host สร้าง tentative address
Step 2: Host ส่ง Neighbor Solicitation (NS)
        - Target: tentative address
        - "มีใครใช้ address นี้อยู่ไหม?"
Step 3: รอ response
        - ถ้าไม่มี response → address unique → ใช้ได้
        - ถ้ามี response → address duplicate → ไม่ใช้

Status:
- Tentative: กำลังตรวจสอบ
- Preferred: ใช้งานได้ปกติ
- Deprecated: ยังใช้ได้แต่ไม่แนะนำ (หมดอายุใกล้)
- Invalid: ไม่สามารถใช้ได้
```

**Disable DAD (ไม่แนะนำ):**

```cisco
Router(config-if)# ipv6 nd dad attempts 0
```

---

## 3. DHCPv6

### Dynamic Host Configuration Protocol for IPv6

### 3.1 DHCPv6 Overview

**DHCPv6 คืออะไร:**

- DHCP สำหรับ IPv6
- RFC 3315
- คล้าย DHCPv4 แต่มีความแตกต่าง
- ใช้ UDP ports:
    - Server: UDP 547
    - Client: UDP 546

**ความแตกต่างระหว่าง DHCPv4 และ DHCPv6:**

```
Feature               DHCPv4              DHCPv6
──────────────────────────────────────────────────────
Protocol              UDP                 UDP
Server Port           67                  547
Client Port           68                  546
Broadcast/Multicast   Broadcast           Multicast
Default Gateway       ให้                 ไม่ให้ (จาก RA)
Message Types         8 types             13 types
Relay                 ip helper-address   ipv6 dhcp relay
Address Assignment    Required            Optional (SLAAC)
```

**⚠️ สำคัญ:**

```
DHCPv6 ไม่ให้ default gateway!
Default gateway มาจาก Router Advertisement เสมอ
```

### 3.2 Stateless vs Stateful DHCPv6

**Stateless DHCPv6:**

```
SLAAC + DHCPv6 for other information

Address:
✅ จาก SLAAC (host สร้างเอง)

Other Information:
✅ จาก DHCPv6 (DNS, domain, etc.)

Server State:
❌ ไม่เก็บ binding (stateless)
❌ ไม่ track ว่าใครใช้ address อะไร

Use Case:
- Simple networks
- ไม่ต้องการ centralized IP management
- ต้องการ DNS configuration
```

**Stateful DHCPv6:**

```
DHCPv6 for everything (except gateway)

Address:
✅ จาก DHCPv6 server

Other Information:
✅ จาก DHCPv6 server

Default Gateway:
✅ จาก Router Advertisement (ยังคงต้องการ RA!)

Server State:
✅ เก็บ binding (stateful)
✅ Track address assignment

Use Case:
- Enterprise networks
- ต้องการ centralized management
- Audit และ tracking
```

### 3.3 DHCPv6 Message Types

**DHCPv6 Messages:**

```
Stateless DHCPv6:
1. INFORMATION-REQUEST (client → server)
   - ขอข้อมูล (DNS, domain)
2. REPLY (server → client)
   - ให้ข้อมูลที่ขอ

Stateful DHCPv6:
1. SOLICIT (client → server)
   - หา DHCPv6 servers
2. ADVERTISE (server → client)
   - Server แจ้งว่ามีให้บริการ
3. REQUEST (client → server)
   - ขอ address และ parameters
4. REPLY (server → client)
   - ให้ address และ parameters

Additional:
- RENEW (client → server) - ต่ออายุ
- REBIND (client → any server) - ขอ server ใหม่
- RELEASE (client → server) - คืน address
- DECLINE (client → server) - ปฏิเสธ address (duplicate)
- RECONFIGURE (server → client) - บอกให้ reconfigure
- CONFIRM (client → server) - ยืนยัน address
```

### 3.4 DHCPv6 Operation

**Stateful DHCPv6 Process:**

```
Step 1: Router Advertisement
Router → Client: RA message (M=1, O=1)
"ใช้ DHCPv6 นะ"

Step 2: DHCPv6 4-Message Exchange
Client → Server: SOLICIT (multicast FF02::1:2)
"มี DHCPv6 server ไหม?"

Server → Client: ADVERTISE
"ผมมีให้บริการครับ"

Client → Server: REQUEST
"ขอ IPv6 address และ parameters"

Server → Client: REPLY
"ให้ 2001:db8:acad:1::100 และ DNS: 2001:4860:4860::8888"

Step 3: Client Configuration
- ใช้ IPv6 address จาก DHCPv6
- ใช้ DNS จาก DHCPv6
- ใช้ default gateway จาก RA
```

---

## 4. Configure DHCPv6 Server

### การตั้งค่า DHCPv6 Server

### 4.1 Configure Stateless DHCPv6 Server

**Scenario:**

```
Network: 2001:db8:acad:1::/64
- Router: 2001:db8:acad:1::1
- DNS: 2001:4860:4860::8888 (Google DNS)
- Domain: example.com
- Method: SLAAC + Stateless DHCPv6
```

**Configuration:**

```cisco
! Step 1: Enable IPv6 routing
Router(config)# ipv6 unicast-routing

! Step 2: Create DHCPv6 pool
Router(config)# ipv6 dhcp pool IPV6-STATELESS
Router(config-dhcpv6)# dns-server 2001:4860:4860::8888
Router(config-dhcpv6)# dns-server 2001:4860:4860::8844
Router(config-dhcpv6)# domain-name example.com
Router(config-dhcpv6)# exit

! Step 3: Configure interface
Router(config)# interface gigabitethernet 0/0
Router(config-if)# description LAN Interface
Router(config-if)# ipv6 address 2001:db8:acad:1::1/64
Router(config-if)# ipv6 address fe80::1 link-local

! Step 4: Enable DHCPv6 server on interface
Router(config-if)# ipv6 dhcp server IPV6-STATELESS

! Step 5: Set O flag (other-config)
Router(config-if)# ipv6 nd other-config-flag

! Step 6: No shutdown
Router(config-if)# no shutdown
```

**คำอธิบาย:**

```
ipv6 dhcp pool <pool-name>
- สร้าง DHCPv6 pool

ipv6 dhcp server <pool-name>
- Enable DHCPv6 server บน interface
- ใช้ pool ที่สร้างไว้

ipv6 nd other-config-flag
- O flag = 1
- บอก clients ให้ใช้ DHCPv6 สำหรับข้อมูลอื่น
```

### 4.2 Configure Stateful DHCPv6 Server

**Scenario:**

```
Network: 2001:db8:acad:1::/64
- Address pool: 2001:db8:acad:1::100 - ::1ff
- DNS: 2001:4860:4860::8888
- Method: Stateful DHCPv6
```

**Configuration:**

```cisco
! Step 1: Enable IPv6 routing
Router(config)# ipv6 unicast-routing

! Step 2: Create DHCPv6 pool
Router(config)# ipv6 dhcp pool IPV6-STATEFUL

! Step 3: Define address prefix
Router(config-dhcpv6)# address prefix 2001:db8:acad:1::/64

! หรือกำหนด range (optional)
Router(config-dhcpv6)# address prefix 2001:db8:acad:1::100/120

! Step 4: Define DNS servers
Router(config-dhcpv6)# dns-server 2001:4860:4860::8888
Router(config-dhcpv6)# dns-server 2001:4860:4860::8844

! Step 5: Define domain name
Router(config-dhcpv6)# domain-name example.com

! Step 6: Exit pool config
Router(config-dhcpv6)# exit

! Step 7: Configure interface
Router(config)# interface gigabitethernet 0/0
Router(config-if)# ipv6 address 2001:db8:acad:1::1/64
Router(config-if)# ipv6 address fe80::1 link-local

! Step 8: Enable DHCPv6 server
Router(config-if)# ipv6 dhcp server IPV6-STATEFUL

! Step 9: Set M and O flags
Router(config-if)# ipv6 nd managed-config-flag
Router(config-if)# ipv6 nd other-config-flag

! Step 10: No shutdown
Router(config-if)# no shutdown
```

**คำอธิบาย:**

```
address prefix <prefix>
- กำหนด prefix ที่จะแจก
- DHCPv6 server จะสร้าง address จาก prefix นี้

ipv6 nd managed-config-flag
- M flag = 1
- บอก clients ให้ใช้ DHCPv6 สำหรับ address
```

### 4.3 Configure DHCPv6 Client

**Router Interface as DHCPv6 Client:**

```cisco
! Stateless DHCPv6 Client
Router(config)# interface gigabitethernet 0/1
Router(config-if)# ipv6 enable
Router(config-if)# ipv6 address autoconfig
! ใช้ SLAAC + DHCPv6 for other info

! Stateful DHCPv6 Client
Router(config)# interface gigabitethernet 0/1
Router(config-if)# ipv6 enable
Router(config-if)# ipv6 address dhcp
! รับ address จาก DHCPv6
Router(config-if)# no shutdown
```

**PC/Host:**

```
Windows:
- Default: Auto-configuration enabled
- ใช้ SLAAC หรือ DHCPv6 ตาม RA flags

Linux:
- ขึ้นอยู่กับ NetworkManager หรือ dhclient

Check:
ipconfig (Windows)
ip addr show (Linux)
```

### 4.4 Configure DHCPv6 Relay

**DHCPv6 Relay Agent:**

```
Topology:
      [Router]
       /    \
  Gi0/0     Gi0/1
(Server)  (Clients)
   │         │
DHCPv6       2001:db8:acad:2::/64
Server
2001:db8:acad:1::10
```

**Configuration:**

```cisco
! บน interface ที่รับ DHCPv6 requests (Clients side)
Router(config)# interface gigabitethernet 0/1
Router(config-if)# description Client Network
Router(config-if)# ipv6 address 2001:db8:acad:2::1/64
Router(config-if)# ipv6 address fe80::1 link-local

! Configure DHCPv6 relay
Router(config-if)# ipv6 dhcp relay destination 2001:db8:acad:1::10
! 2001:db8:acad:1::10 = DHCPv6 Server address

! Set RA flags (for stateful DHCPv6)
Router(config-if)# ipv6 nd managed-config-flag
Router(config-if)# ipv6 nd other-config-flag

Router(config-if)# no shutdown
```

**Verify Relay:**

```cisco
Router# show ipv6 dhcp interface gigabitethernet 0/1

GigabitEthernet0/1 is in relay mode
  Relay destinations:
    2001:DB8:ACAD:1::10
```

### 4.5 Verify DHCPv6

**Verification Commands:**

**1. Show IPv6 DHCP Pool:**

```cisco
Router# show ipv6 dhcp pool

DHCPv6 pool: IPV6-STATEFUL
  Address allocation prefix: 2001:DB8:ACAD:1::/64 valid 172800 preferred 86400
  DNS server: 2001:4860:4860::8888
  DNS server: 2001:4860:4860::8844
  Domain name: example.com
  Active clients: 5
```

**2. Show IPv6 DHCP Binding:**

```cisco
Router# show ipv6 dhcp binding

Client: FE80::250:BAFF:FE8D:B411
  DUID: 00010001234567890050BA8DB411
  Username : unassigned
  VRF : default
  IA NA: IA ID 0x00040001, T1 43200, T2 69120
    Address: 2001:DB8:ACAD:1::100
            preferred lifetime 86400, valid lifetime 172800
            expires at Nov 20 2025 12:00 PM (172799 seconds)
```

**3. Show IPv6 Interface:**

```cisco
Router# show ipv6 interface gigabitethernet 0/0

GigabitEthernet0/0 is up, line protocol is up
  IPv6 is enabled, link-local address is FE80::1
  Global unicast address(es):
    2001:DB8:ACAD:1::1, subnet is 2001:DB8:ACAD:1::/64
  ...
  ND DAD is enabled, number of DAD attempts: 1
  ND reachable time is 30000 milliseconds
  ND router advertisements are sent every 200 seconds
  Hosts use DHCP to obtain routable addresses.
  Hosts use DHCP to obtain other configuration.
```

**4. Show IPv6 DHCP Statistics:**

```cisco
Router# show ipv6 dhcp

This device's DHCPv6 unique identifier (DUID): 00030001C80174DD4600
  DHCPv6 server enabled on: GigabitEthernet0/0
  Pool: IPV6-STATEFUL
    Prefix pool: 2001:DB8:ACAD:1::/64
    DNS server: 2001:4860:4860::8888
    DNS server: 2001:4860:4860::8844
    Domain name: example.com
```

**5. Debug DHCPv6:**

```cisco
Router# debug ipv6 dhcp detail

! ดู DHCPv6 messages
! อย่าลืมปิด debug
Router# no debug all
```

---

## 5. Troubleshooting

### การแก้ไขปัญหา

### 5.1 Verify IPv6 Addressing

**Check Commands:**

```cisco
! ตรวจสอบ IPv6 addresses
show ipv6 interface brief
show ipv6 interface <interface>

! ตรวจสอบ routing
show ipv6 route

! ตรวจสอบ neighbors
show ipv6 neighbors

! ตรวจสอบ DHCPv6
show ipv6 dhcp pool
show ipv6 dhcp binding
show ipv6 dhcp interface
```

### 5.2 Common Problems

**Problem 1: No IPv6 Address (SLAAC)**

```
Symptom:
- Client มี link-local เท่านั้น
- ไม่มี global unicast address

Cause:
- Router ไม่ส่ง RA
- ipv6 unicast-routing ไม่ได้ enable

Solution:
Router(config)# ipv6 unicast-routing
```

**Problem 2: Wrong Prefix**

```
Symptom:
- Client ได้ IPv6 แต่ prefix ผิด

Cause:
- Router interface configured wrong

Solution:
ตรวจสอบ prefix บน router interface
```

**Problem 3: No DNS (SLAAC Only)**

```
Symptom:
- มี IPv6 address แต่ไม่มี DNS

Cause:
- ใช้ SLAAC only (M=0, O=0)
- DHCPv6 ไม่ได้ configure

Solution:
Configure Stateless DHCPv6
Router(config-if)# ipv6 nd other-config-flag
```

**Problem 4: DHCPv6 Not Working**

```
Symptom:
- Client ไม่ติดต่อ DHCPv6 server

Cause:
- M/O flags ไม่ได้ set
- DHCPv6 pool ไม่ได้ bind กับ interface
- DHCPv6 server ไม่ได้ enable

Troubleshooting:
show ipv6 interface (check M/O flags)
show ipv6 dhcp pool
show ipv6 dhcp interface

Solution:
Router(config-if)# ipv6 nd managed-config-flag
Router(config-if)# ipv6 dhcp server <pool-name>
```

**Problem 5: DHCPv6 Relay Not Working**

```
Symptom:
- Remote clients ไม่ได้ address

Cause:
- Relay destination wrong
- Server unreachable

Troubleshooting:
show ipv6 dhcp interface
ping 2001:db8:acad:1::10 (server IP)

Solution:
ตรวจสอบ relay destination
Router(config-if)# ipv6 dhcp relay destination <server-ipv6>
```

---

## สรุป

### สิ่งที่ได้เรียนรู้ใน Module 8:

✅ **IPv6 Address Assignment Methods:**

- Static assignment
- SLAAC (Stateless Address Autoconfiguration)
- DHCPv6 (Stateless และ Stateful)

✅ **SLAAC:**

- ICMPv6 RS และ RA messages
- RA flags (M, O, A)
- EUI-64 process
- DAD (Duplicate Address Detection)
- 3 SLAAC methods

✅ **DHCPv6:**

- Stateless DHCPv6 (address จาก SLAAC)
- Stateful DHCPv6 (address จาก DHCPv6)
- DHCPv6 message types
- Configuration

✅ **Configuration:**

- SLAAC only
- SLAAC + Stateless DHCPv6
- Stateful DHCPv6
- DHCPv6 relay
- DHCPv6 client

✅ **Verification:**

- show ipv6 interface
- show ipv6 dhcp pool/binding
- Troubleshooting steps

---

## คำสั่งสำคัญสรุป

### SLAAC Configuration:

```cisco
! Enable IPv6 routing (sends RA)
ipv6 unicast-routing

! Interface configuration
interface <interface>
 ipv6 address <prefix>::<interface-id>/<prefix-length>
 ipv6 address <link-local> link-local

! Modify RA flags
 ipv6 nd other-config-flag        (O=1)
 ipv6 nd managed-config-flag      (M=1)
 ipv6 nd prefix default no-autoconfig  (A=0)
```

### DHCPv6 Server:

```cisco
! Create pool
ipv6 dhcp pool <pool-name>
 address prefix <prefix>/<length>
 dns-server <ipv6>
 domain-name <domain>

! Enable on interface
interface <interface>
 ipv6 dhcp server <pool-name>
 ipv6 nd managed-config-flag
 ipv6 nd other-config-flag
```

### DHCPv6 Client:

```cisco
interface <interface>
 ipv6 enable
 ipv6 address autoconfig     (SLAAC + DHCPv6)
 ipv6 address dhcp          (Stateful DHCPv6)
```

### DHCPv6 Relay:

```cisco
interface <interface>
 ipv6 dhcp relay destination <server-ipv6>
```

### Verification:

```cisco
show ipv6 interface [brief | <interface>]
show ipv6 dhcp pool
show ipv6 dhcp binding
show ipv6 dhcp interface
show ipv6 route
show ipv6 neighbors
```

---

## เปรียบเทียบ SLAAC Methods

|Feature|SLAAC Only|SLAAC + Stateless DHCPv6|Stateful DHCPv6|
|---|---|---|---|
|**RA Flags**|M=0, O=0|M=0, O=1|M=1, O=1|
|**Address Source**|SLAAC|SLAAC|DHCPv6|
|**Gateway Source**|RA|RA|RA|
|**DNS Source**|Manual|DHCPv6|DHCPv6|
|**Server State**|-|Stateless|Stateful|
|**Tracking**|No|No|Yes|
|**Best For**|Simple|Medium|Enterprise|

---

## Best Practices

### IPv6 Addressing:

```
✅ ใช้ SLAAC + Stateless DHCPv6 (ได้ทั้ง address และ DNS)
✅ ใช้ Stateful DHCPv6 ถ้าต้องการ tracking
✅ กำหนด link-local address manual (ง่ายต่อการจำ)
✅ ใช้ EUI-64 หรือ random (privacy)
✅ Enable DAD เสมอ
```

### DHCPv6:

```
✅ Document DHCPv6 pools
✅ Use descriptive pool names
✅ Monitor DHCPv6 bindings
✅ Implement DHCPv6 redundancy
✅ Regular configuration backup
```

### Security:

```
✅ RA Guard (prevent rogue RAs)
✅ DHCPv6 Guard (Module 11)
✅ Limit RA messages
✅ Monitor for anomalies
```

---

## Lab Activities

### Lab 8.1: Configure SLAAC

**วัตถุประสงค์:**

- Enable IPv6 routing
- Configure IPv6 addresses
- Verify SLAAC operation
- Test client connectivity

### Lab 8.2: Configure Stateless DHCPv6

**วัตถุประสงค์:**

- Configure SLAAC + DHCPv6
- Set O flag
- Configure DNS via DHCPv6
- Verify operation

### Lab 8.3: Configure Stateful DHCPv6

**วัตถุประสงค์:**

- Configure DHCPv6 pool
- Set M and O flags
- Verify address assignment
- Test DHCPv6 server

---

## Packet Tracer Activities

### PT 8.1: IPv6 SLAAC

**Tasks:**

1. Enable IPv6 routing
2. Configure IPv6 addresses
3. Verify RA messages
4. Test SLAAC on clients

### PT 8.2: Stateless DHCPv6

**Tasks:**

1. Configure DHCPv6 pool (DNS only)
2. Set O flag
3. Enable DHCPv6 server
4. Verify clients get DNS

### PT 8.3: Stateful DHCPv6

**Tasks:**

1. Configure DHCPv6 pool (addresses)
2. Set M and O flags
3. Verify address assignment
4. Test connectivity

---

## คำถามทบทวน

1. SLAAC ย่อมาจากอะไร? ทำงานอย่างไร?
2. RS และ RA คืออะไร?
3. M flag และ O flag ต่างกันอย่างไร?
4. EUI-64 คืออะไร? สร้าง Interface ID อย่างไร?
5. ต่างระหว่าง Stateless และ Stateful DHCPv6 อย่างไร?
6. DHCPv6 ให้ default gateway ไหม?
7. DAD ย่อมาจากอะไร? ทำหน้าที่อะไร?
8. วิธีเปลี่ยน RA flags ต้องใช้คำสั่งอะไร?
9. DHCPv6 relay ใช้คำสั่งอะไร?
10. SLAAC + Stateless DHCPv6 คืออะไร?

---

## เฉลยคำถาม

1. **SLAAC:** Stateless Address Autoconfiguration, host สร้าง IPv6 address เองโดยใช้ prefix จาก RA
2. **RS/RA:** Router Solicitation (host ขอ RA), Router Advertisement (router ส่งข้อมูล prefix)
3. **M/O flags:** M=1 ใช้ DHCPv6 for address, O=1 ใช้ DHCPv6 for other info (DNS, domain)
4. **EUI-64:** สร้าง Interface ID จาก MAC (48 bits) → แทรก FFFE → flip bit 7 → 64 bits
5. **Stateless/Stateful:** Stateless = address จาก SLAAC, DHCPv6 ให้ DNS; Stateful = address จาก DHCPv6
6. **Default Gateway:** ไม่ให้! มาจาก RA เสมอ
7. **DAD:** Duplicate Address Detection, ตรวจสอบว่า address ซ้ำกับใครไหม
8. **RA Flags:** `ipv6 nd managed-config-flag` (M), `ipv6 nd other-config-flag` (O)
9. **DHCPv6 Relay:** `ipv6 dhcp relay destination <server-ipv6>`
10. **SLAAC + Stateless DHCPv6:** address จาก SLAAC, DNS จาก DHCPv6 (M=0, O=1)

---

**หมายเหตุ:** IPv6 autoconfiguration ทำให้การจัดการ network ง่ายและยืดหยุ่นกว่า IPv4 การเข้าใจ SLAAC และ DHCPv6 จำเป็นสำหรับการ deploy IPv6 networks

---

**เอกสารจัดทำโดย:** Claude (Anthropic AI)  
**Module:** CCNA 2 Module 8 - SLAAC and DHCPv6  
**Version:** v7.02 SRWE  
**วันที่อัพเดท:** พฤศจิกายน 2025