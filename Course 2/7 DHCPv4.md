# CCNA 2: Module 7 - DHCPv4

## Dynamic Host Configuration Protocol for IPv4

---

## สารบัญ

1. [วัตถุประสงค์ของ Module](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#%E0%B8%A7%E0%B8%B1%E0%B8%95%E0%B8%96%E0%B8%B8%E0%B8%9B%E0%B8%A3%E0%B8%B0%E0%B8%AA%E0%B8%87%E0%B8%84%E0%B9%8C%E0%B8%82%E0%B8%AD%E0%B8%87-module)
2. [DHCPv4 Concepts](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#1-dhcpv4-concepts)
3. [Configure DHCPv4 Server](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#2-configure-dhcpv4-server)
4. [Configure DHCPv4 Client](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#3-configure-dhcpv4-client)
5. [สรุป](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#%E0%B8%AA%E0%B8%A3%E0%B8%B8%E0%B8%9B)

---

## วัตถุประสงค์ของ Module

เมื่อจบ Module นี้ คุณจะสามารถ:

- ✅ อธิบายวัตถุประสงค์ของ DHCPv4
- ✅ เข้าใจการทำงานของ DHCPv4
- ✅ อธิบาย DHCPv4 message types
- ✅ Configure DHCPv4 server บน Cisco router
- ✅ Configure DHCPv4 client
- ✅ Configure DHCPv4 relay agent
- ✅ Verify และ Troubleshoot DHCPv4
- ✅ เข้าใจ DHCP attacks และ mitigation

---

## 1. DHCPv4 Concepts

### แนวคิดของ DHCPv4

### 1.1 DHCPv4 คืออะไร?

**DHCP (Dynamic Host Configuration Protocol):**

- โปรโตคอลสำหรับกำหนด IP address อัตโนมัติ
- RFC 2131
- Client/Server model
- ใช้ UDP ports:
    - Server: UDP 67
    - Client: UDP 68

**ปัญหาของ Static IP:**

```
สถานการณ์: บริษัทมี 500 คอมพิวเตอร์

Static IP Assignment:
❌ ต้อง configure IP manual ทุกเครื่อง
❌ ใช้เวลานาน (500 เครื่อง!)
❌ ผิดพลาดง่าย (typo, duplicate IP)
❌ ยากต่อการจัดการ
❌ เปลี่ยน subnet ต้อง reconfigure ทั้งหมด
❌ Tracking ว่า IP ไหนใช้แล้ว/ว่าง
```

**วิธีแก้: DHCP**

```
DHCP Server:
✅ กำหนด IP address อัตโนมัติ
✅ ไม่ต้อง configure manual
✅ ไม่มี duplicate IP
✅ Centralized management
✅ เปลี่ยน configuration ได้ง่าย
✅ Track IP usage อัตโนมัติ
✅ Temporary IP assignment (lease)
```

### 1.2 DHCPv4 Operation

**DHCPv4 4-Step Process (DORA):**

```
Step 1: DHCP Discover (Client → Server)
Client: "ใครเป็น DHCP server บ้าง? ผมต้องการ IP!"
- Broadcast: 255.255.255.255
- Source IP: 0.0.0.0
- Destination IP: 255.255.255.255
- Client ยังไม่มี IP address

Step 2: DHCP Offer (Server → Client)
Server: "ผมเป็น DHCP server นี่ มี IP 192.168.1.10 ให้ครับ"
- Unicast/Broadcast ไป client
- เสนอ IP address
- พร้อม subnet mask, gateway, DNS

Step 3: DHCP Request (Client → Server)
Client: "ขอบคุณครับ ผมขอใช้ 192.168.1.10 นะ"
- Broadcast: 255.255.255.255
- บอกว่ายอมรับข้อเสนอ
- แจ้ง servers อื่นที่ offer มาว่าปฏิเสธ

Step 4: DHCP Acknowledge (Server → Client)
Server: "ยืนยันครับ 192.168.1.10 เป็นของคุณแล้ว"
- Unicast/Broadcast ไป client
- ยืนยัน IP assignment
- ให้ lease time
- Client เริ่มใช้ IP ได้
```

**DORA Acronym:**

```
D - Discover
O - Offer
R - Request
A - Acknowledge (ACK)
```

**Timeline:**

```
Client                                Server
  │                                      │
  ├─────── DHCP Discover (Broadcast)────►│
  │                                      │
  │◄────── DHCP Offer ────────────────────┤
  │                                      │
  ├─────── DHCP Request (Broadcast)─────►│
  │                                      │
  │◄────── DHCP ACK ──────────────────────┤
  │                                      │
  ▼ Client ใช้ IP ได้แล้ว               ▼
```

### 1.3 DHCPv4 Message Format

**DHCP Message Types:**

```
Message Type    Description
─────────────────────────────────────────────────
DHCPDISCOVER    Client ค้นหา DHCP servers
DHCPOFFER       Server เสนอ IP address
DHCPREQUEST     Client ขอใช้ IP address
DHCPACK         Server ยืนยัน IP assignment
DHCPNAK         Server ปฏิเสธ request
DHCPDECLINE     Client ปฏิเสธ IP (duplicate)
DHCPRELEASE     Client คืน IP address
DHCPINFORM      Client ขอข้อมูลเพิ่ม (มี IP แล้ว)
```

**DHCP Packet Fields:**

```
┌─────────────────────────────────┐
│ Operation Code (1=request, 2=reply)│
│ Hardware Type (1=Ethernet)      │
│ Hardware Address Length (6)     │
│ Hops (0)                        │
│ Transaction ID                  │
│ Seconds                         │
│ Flags                           │
│ Client IP Address               │
│ Your IP Address                 │
│ Server IP Address               │
│ Gateway IP Address              │
│ Client Hardware Address (MAC)   │
│ Server Name                     │
│ Boot Filename                   │
│ Options (DHCP message type, etc.)│
└─────────────────────────────────┘
```

### 1.4 DHCP Information Provided

**ข้อมูลที่ DHCP Server ให้:**

```
Required (must have):
✅ IP Address
✅ Subnet Mask

Optional (commonly provided):
✅ Default Gateway
✅ DNS Server(s)
✅ Domain Name
✅ Lease Time
✅ WINS Server (legacy)
✅ TFTP Server
✅ Boot Filename
✅ NTP Server
✅ และอื่นๆ (DHCP Options)
```

**DHCP Options (ตัวอย่าง):**

```
Option 1: Subnet Mask
Option 3: Default Gateway (Router)
Option 6: DNS Server
Option 15: Domain Name
Option 51: Lease Time
Option 53: DHCP Message Type
Option 54: Server Identifier
Option 58: Renewal (T1) Time
Option 59: Rebinding (T2) Time
Option 150: TFTP Server (Cisco IP Phones)
```

### 1.5 DHCP Lease Process

**Lease Time:**

- ระยะเวลาที่ client สามารถใช้ IP address
- Default: 24 hours (1 day)
- สามารถตั้งค่าได้ (minutes to forever)

**Lease Timeline:**

```
Lease Time: 24 hours
T1 (Renewal): 50% = 12 hours
T2 (Rebinding): 87.5% = 21 hours

Timeline:
0h ─────────── 12h ─────────── 21h ────── 24h
│              │               │          │
Lease Start    T1: Renewal     T2: Rebind Lease Expires
               (try renew      (try any
                with same       DHCP server)
                server)
```

**Lease Renewal (T1 - 50%):**

```
เมื่อถึง 50% ของ lease time:
- Client พยายามต่ออายุกับ server เดิม
- ส่ง DHCP Request (unicast) ไป server
- ถ้า server ตอบกลับ DHCP ACK
  → Lease ต่ออายุ (reset timer)
- ถ้า server ไม่ตอบ
  → รอถึง T2
```

**Lease Rebinding (T2 - 87.5%):**

```
เมื่อถึง 87.5% ของ lease time:
- Server เดิมไม่ตอบ
- Client broadcast DHCP Request
- ขอ server ใดก็ได้ต่ออายุให้
- ถ้ามี server ตอบ
  → Lease ต่ออายุ
- ถ้าไม่มี server ตอบ
  → รอจนหมดอายุ
```

**Lease Expiration (100%):**

```
เมื่อ lease หมดอายุ:
- Client ต้องหยุดใช้ IP address
- Client เริ่ม DORA process ใหม่
- DHCP Discover → Offer → Request → ACK
```

### 1.6 DHCP Relay Agent

**ปัญหา: DHCP Server อยู่คนละ Subnet**

```
Scenario:
         [Router]
           /    \
    VLAN 10      VLAN 20
    Subnet A     Subnet B
      │            │
   Clients      Clients

DHCP Server อยู่ที่ VLAN 10

Problem:
- DHCP Discover เป็น broadcast (255.255.255.255)
- Routers ไม่ forward broadcasts
- Clients ใน VLAN 20 ไม่เจอ DHCP Server!
```

**วิธีแก้ที่ไม่ดี:**

```
วาง DHCP Server ในทุก subnet
- ต้องมี server เยอะ
- แพง
- ยากต่อการจัดการ
```

**วิธีแก้ที่ดี: DHCP Relay Agent**

```
[Router] configured as DHCP Relay Agent
  /    \
VLAN 10  VLAN 20
  │        │
Server   Clients

Router ทำหน้าที่:
1. รับ DHCP broadcasts จาก clients
2. แปลงเป็น unicast
3. Forward ไป DHCP Server
4. รับ response จาก server
5. Forward กลับไป client

Result:
✅ 1 DHCP Server สำหรับหลาย subnets
✅ Centralized management
✅ Cost-effective
```

**DHCP Relay Operation:**

```
Client (VLAN 20) → Router (Relay) → Server (VLAN 10)

Step 1: Client broadcasts DHCP Discover
Step 2: Router รับ broadcast (on VLAN 20 interface)
Step 3: Router แปลงเป็น unicast
Step 4: Router forward ไป DHCP Server
Step 5: Server ส่ง DHCP Offer กลับ
Step 6: Router forward ไป Client
... (DORA process continues)
```

---

## 2. Configure DHCPv4 Server

### การตั้งค่า DHCPv4 Server

### 2.1 Cisco Router as DHCP Server

**Cisco Router สามารถเป็น DHCP Server ได้:**

- Configure DHCP pools
- Assign IP addresses dynamically
- Provide network parameters
- เหมาะกับ small-medium networks

**Configuration Steps:**

```
1. สร้าง DHCP pool
2. กำหนด network/subnet
3. กำหนด default gateway
4. กำหนด DNS server
5. กำหนด lease time (optional)
6. Exclude static IP addresses
7. Verify configuration
```

### 2.2 Basic DHCPv4 Server Configuration

**Scenario:**

```
Network: 192.168.10.0/24
- Router IP: 192.168.10.1 (gateway)
- DNS Server: 8.8.8.8
- DHCP Range: 192.168.10.10 - 192.168.10.254
- Reserved: 192.168.10.1 - 192.168.10.9 (static)
```

**Configuration:**

```cisco
! Step 1: Exclude static IP addresses
Router(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.9

! Step 2: Create DHCP pool
Router(config)# ip dhcp pool LAN-POOL-1

! Step 3: Define network
Router(dhcp-config)# network 192.168.10.0 255.255.255.0

! Step 4: Define default gateway
Router(dhcp-config)# default-router 192.168.10.1

! Step 5: Define DNS server
Router(dhcp-config)# dns-server 8.8.8.8

! Step 6: Define domain name (optional)
Router(dhcp-config)# domain-name example.com

! Step 7: Define lease time (optional)
Router(dhcp-config)# lease 7
! lease 7 = 7 days
! lease 0 2 = 2 hours
! lease 0 0 30 = 30 minutes
! lease infinite = ไม่มีวันหมดอายุ

Router(dhcp-config)# exit
```

**คำอธิบายคำสั่ง:**

```cisco
ip dhcp excluded-address <start-ip> [end-ip]
- กันไว้ไม่ให้ DHCP แจก IP addresses นี้
- ใช้สำหรับ static assignments (routers, servers, printers)

ip dhcp pool <pool-name>
- สร้าง DHCP pool
- ชื่ออะไรก็ได้ (case-sensitive)

network <network-address> <subnet-mask>
- ระบุ network ที่จะแจก IP

default-router <gateway-ip>
- Default gateway สำหรับ clients

dns-server <dns-ip> [dns-ip2] [dns-ip3]
- DNS servers (สูงสุด 8 servers)

domain-name <domain>
- Domain name suffix

lease {days [hours] [minutes] | infinite}
- Lease duration
- Default: 1 day (24 hours)
```

### 2.3 Configure Multiple DHCP Pools

**Scenario: หลาย Subnets**

```
Router มี 3 interfaces:
- Gi0/0: 192.168.10.0/24 (VLAN 10 - Sales)
- Gi0/1: 192.168.20.0/24 (VLAN 20 - Engineering)
- Gi0/2: 192.168.30.0/24 (VLAN 30 - HR)
```

**Configuration:**

```cisco
! Exclude addresses (static assignments)
Router(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.10
Router(config)# ip dhcp excluded-address 192.168.20.1 192.168.20.10
Router(config)# ip dhcp excluded-address 192.168.30.1 192.168.30.10

! DHCP Pool for VLAN 10 (Sales)
Router(config)# ip dhcp pool VLAN10_POOL
Router(dhcp-config)# network 192.168.10.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.10.1
Router(dhcp-config)# dns-server 8.8.8.8 8.8.4.4
Router(dhcp-config)# domain-name sales.example.com
Router(dhcp-config)# lease 7
Router(dhcp-config)# exit

! DHCP Pool for VLAN 20 (Engineering)
Router(config)# ip dhcp pool VLAN20_POOL
Router(dhcp-config)# network 192.168.20.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.20.1
Router(dhcp-config)# dns-server 8.8.8.8 8.8.4.4
Router(dhcp-config)# domain-name engineering.example.com
Router(dhcp-config)# lease 7
Router(dhcp-config)# exit

! DHCP Pool for VLAN 30 (HR)
Router(config)# ip dhcp pool VLAN30_POOL
Router(dhcp-config)# network 192.168.30.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.30.1
Router(dhcp-config)# dns-server 8.8.8.8 8.8.4.4
Router(dhcp-config)# domain-name hr.example.com
Router(dhcp-config)# lease 7
Router(dhcp-config)# exit
```

### 2.4 Disable DHCP Service

**ปิด/เปิด DHCP Service:**

```cisco
! ปิด DHCP service (ชั่วคราว)
Router(config)# no service dhcp

! เปิด DHCP service (default is enabled)
Router(config)# service dhcp

! ตรวจสอบ
Router# show running-config | include dhcp
service dhcp
```

### 2.5 Verify DHCPv4 Server

**คำสั่งตรวจสอบ:**

**1. Show IP DHCP Pool:**

```cisco
Router# show ip dhcp pool

Pool VLAN10_POOL :
 Utilization mark (high/low)    : 100 / 0
 Subnet size (first/next)       : 0 / 0
 Total addresses                : 254
 Leased addresses               : 15
 Pending event                  : none
 1 subnet is currently in the pool :
 Current index        IP address range                    Leased addresses
 192.168.10.11        192.168.10.1     - 192.168.10.254   15
```

**2. Show IP DHCP Binding:**

```cisco
Router# show ip dhcp binding

Bindings from all pools not associated with VRF:
IP address          Client-ID/              Lease expiration        Type
                    Hardware address/
                    User name
192.168.10.11       0100.50ba.8db4.11       Dec 02 2025 12:00 AM    Automatic
192.168.10.12       0100.50ba.8db4.12       Dec 02 2025 01:30 AM    Automatic
192.168.10.13       0100.50ba.8db4.13       Dec 02 2025 02:45 AM    Automatic
```

**3. Show IP DHCP Server Statistics:**

```cisco
Router# show ip dhcp server statistics

Memory usage         25364
Address pools        3
Database agents      0
Automatic bindings   15
Manual bindings      0
Expired bindings     2
Malformed messages   0
Secure arp entries   0

Message              Received
BOOTREQUEST          0
DHCPDISCOVER         23
DHCPREQUEST          18
DHCPDECLINE          0
DHCPRELEASE          2
DHCPINFORM           0

Message              Sent
BOOTREPLY            0
DHCPOFFER            23
DHCPACK              18
DHCPNAK              0
```

**4. Show IP DHCP Conflict:**

```cisco
Router# show ip dhcp conflict

IP address        Detection method   Detection time          VRF

! ถ้าไม่มี conflict จะไม่แสดงอะไร (ดี)
! ถ้ามี conflict จะแสดง IP ที่ขัดแย้ง
```

**5. Show Running-Config:**

```cisco
Router# show running-config | section dhcp

ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp pool VLAN10_POOL
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8 8.8.4.4
 domain-name sales.example.com
 lease 7
```

### 2.6 DHCPv4 Relay Agent Configuration

**Configure Router as DHCP Relay:**

```
Topology:
      [Router]
       /    \
  Gi0/0     Gi0/1
(Server)  (Clients)
   │         │
DHCP Server  VLAN 20
VLAN 10      192.168.20.0/24
```

**Configuration:**

```cisco
! บน interface ที่รับ DHCP requests (Clients side)
Router(config)# interface gigabitethernet 0/1
Router(config-if)# description Client Network VLAN 20
Router(config-if)# ip address 192.168.20.1 255.255.255.0

! กำหนด DHCP relay (ip helper-address)
Router(config-if)# ip helper-address 192.168.10.10

! 192.168.10.10 = DHCP Server IP address
Router(config-if)# exit
```

**คำอธิบาย:**

```cisco
ip helper-address <dhcp-server-ip>
- Forward DHCP broadcasts ไป DHCP server
- แปลง broadcast เป็น unicast
- ใช้กับ interface ที่รับ DHCP requests

Forward ได้ไม่เฉพาะ DHCP:
- DHCP/BOOTP (ports 67, 68)
- TFTP (port 69)
- DNS (port 53)
- Time (port 37)
- NetBIOS (ports 137, 138)
- TACACS (port 49)
```

**Multiple DHCP Servers:**

```cisco
! สามารถระบุหลาย servers (redundancy)
Router(config-if)# ip helper-address 192.168.10.10
Router(config-if)# ip helper-address 192.168.10.11

! Relay จะ forward ไปทั้ง 2 servers
```

**Verify Relay:**

```cisco
Router# show ip interface gigabitethernet 0/1

GigabitEthernet0/1 is up, line protocol is up
  Internet address is 192.168.20.1/24
  Helper address is 192.168.10.10
  ...
```

### 2.7 Configure DHCP Options

**DHCP Options (Additional Configuration):**

```cisco
Router(config)# ip dhcp pool ADVANCED_POOL
Router(dhcp-config)# network 192.168.40.0 255.255.255.0

! Option 3: Default Gateway
Router(dhcp-config)# default-router 192.168.40.1

! Option 6: DNS Server
Router(dhcp-config)# dns-server 8.8.8.8 8.8.4.4

! Option 15: Domain Name
Router(dhcp-config)# domain-name example.com

! Option 42: NTP Server
Router(dhcp-config)# option 42 ip 192.168.40.5

! Option 66: TFTP Server Name
Router(dhcp-config)# option 66 ascii tftp.example.com

! Option 150: TFTP Server IP (Cisco IP Phones)
Router(dhcp-config)# option 150 ip 192.168.40.10

! NetBIOS Name Server (WINS)
Router(dhcp-config)# netbios-name-server 192.168.40.20

! NetBIOS Node Type
Router(dhcp-config)# netbios-node-type h-node

! Lease Time
Router(dhcp-config)# lease 0 12 0
! 0 days, 12 hours, 0 minutes
```

**Lease Time Examples:**

```cisco
! 7 days
lease 7

! 12 hours
lease 0 12

! 30 minutes
lease 0 0 30

! Infinite (no expiration)
lease infinite

! Default (if not specified): 1 day
```

---

## 3. Configure DHCPv4 Client

### การตั้งค่า DHCPv4 Client

### 3.1 Configure Router Interface as DHCP Client

**Router Interface เป็น DHCP Client:**

```cisco
! Configure interface เป็น DHCP client
Router(config)# interface gigabitethernet 0/1
Router(config-if)# ip address dhcp
Router(config-if)# no shutdown

! Router จะ:
! 1. ส่ง DHCP Discover
! 2. รับ IP address จาก DHCP server
! 3. ใช้ IP address ที่ได้รับ
```

**Verify:**

```cisco
Router# show ip interface brief

Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0     192.168.1.1     YES manual up                    up
GigabitEthernet0/1     192.168.2.50    YES DHCP   up                    up
                       ↑ ได้จาก DHCP

Router# show dhcp lease

Temp IP addr: 192.168.2.50 for peer on Interface: GigabitEthernet0/1
Temp sub net mask: 255.255.255.0
   DHCP Lease server: 192.168.2.1, state: 3 Bound
   DHCP transaction id: 1966
   Lease: 86400 seconds, Renewal: 43200 seconds, Rebind: 75600 seconds
   Next timer fires after: 11:59:58
   Retry count: 0   Client-ID: cisco-0cd9.96d2.4801-Gi0/1
```

### 3.2 Configure Switch as DHCP Client

**Switch Management Interface เป็น DHCP Client:**

```cisco
! Configure VLAN interface
Switch(config)# interface vlan 1
Switch(config-if)# ip address dhcp
Switch(config-if)# no shutdown

! ไม่ต้องกำหนด default gateway
! DHCP server จะให้มาเอง
```

**Manual Configuration (สำหรับเปรียบเทียบ):**

```cisco
! Static IP (old way)
Switch(config)# interface vlan 1
Switch(config-if)# ip address 192.168.1.10 255.255.255.0
Switch(config-if)# no shutdown
Switch(config-if)# exit
Switch(config)# ip default-gateway 192.168.1.1

! DHCP (modern way)
Switch(config)# interface vlan 1
Switch(config-if)# ip address dhcp
Switch(config-if)# no shutdown
! Default gateway มาจาก DHCP อัตโนมัติ
```

### 3.3 PC/Workstation DHCP Configuration

**Windows:**

```
1. Open Network Connections
2. Right-click adapter → Properties
3. Select "Internet Protocol Version 4 (TCP/IPv4)"
4. Select "Obtain an IP address automatically"
5. Select "Obtain DNS server address automatically"
6. Click OK

Command Line:
C:\> ipconfig /release    (คืน IP)
C:\> ipconfig /renew      (ขอ IP ใหม่)
C:\> ipconfig /all        (ดูข้อมูลทั้งหมด)
```

**Linux:**

```bash
# Ubuntu/Debian
sudo dhclient -r    # Release
sudo dhclient       # Renew

# Check current IP
ip addr show
ifconfig

# View DHCP lease
cat /var/lib/dhcp/dhclient.leases
```

**macOS:**

```
1. System Preferences → Network
2. Select interface → Advanced
3. TCP/IP tab
4. Configure IPv4: Using DHCP
5. Click OK

Terminal:
sudo ipconfig set en0 DHCP
```

### 3.4 Troubleshoot DHCP Client

**ปัญหา: Client ไม่ได้ IP จาก DHCP**

**Troubleshooting Steps:**

```
Step 1: ตรวจสอบ physical connectivity
- สายเคเบิลเสีย?
- Switch port up?
- Link light?

Step 2: ตรวจสอบ VLAN assignment
- Client อยู่ VLAN ถูกต้อง?

Step 3: ตรวจสอบ DHCP server
- DHCP service running?
- show ip dhcp pool
- show ip dhcp binding
- มี IP ว่างให้ไหม?

Step 4: ตรวจสอบ DHCP relay (ถ้ามี)
- ip helper-address configured?
- Correct server IP?

Step 5: Release และ Renew
Windows: ipconfig /release, ipconfig /renew
Linux: sudo dhclient -r, sudo dhclient

Step 6: ตรวจสอบ firewall
- Block DHCP traffic?
- Allow UDP 67, 68?
```

**Common Issues:**

```
Issue 1: APIPA Address (169.254.x.x)
Cause: ไม่เจอ DHCP server
Solution: ตรวจสอบ server, connectivity, relay

Issue 2: Wrong Subnet
Cause: DHCP pool misconfigured
Solution: ตรวจสอบ network statement

Issue 3: No Default Gateway
Cause: ไม่ได้ configure default-router
Solution: เพิ่ม default-router ใน pool

Issue 4: IP Conflict
Cause: Duplicate IP address
Solution: Clear conflict, exclude static IPs

Issue 5: Lease Expired
Cause: Server down ตอนต่ออายุ
Solution: Release/Renew
```

---

## 4. DHCP Security

### ความปลอดภัยของ DHCP

### 4.1 DHCP Attacks

**1. DHCP Starvation Attack:**

```
Attack:
- ผู้โจมตีส่ง DHCP Discover มากมาย
- ใช้ MAC addresses ปลอมต่างกัน
- DHCP server แจก IP จนหมด pool
- Legitimate users ไม่ได้ IP

Impact:
- Denial of Service (DoS)
- Network unavailable

Tool: Gobbler, Yersinia
```

**2. DHCP Spoofing (Rogue DHCP Server):**

```
Attack:
- ผู้โจมตีตั้ง fake DHCP server
- ตอบ DHCP requests เร็วกว่า legitimate server
- ให้ wrong gateway (attacker's IP)
- Clients ส่ง traffic ผ่านผู้โจมตี

Impact:
- Man-in-the-Middle attack
- Data interception
- Information theft
```

### 4.2 DHCP Snooping (จะเรียนรู้ใน Module 11)

**DHCP Snooping:**

- Security feature บน switches
- ป้องกัน rogue DHCP servers
- Validate DHCP messages
- จะเรียนรู้โดยละเอียดใน Module 11

**Basic Concept:**

```
Trusted Ports:
- อนุญาตให้ส่ง DHCP Offer/ACK
- เชื่อมต่อกับ legitimate DHCP servers

Untrusted Ports:
- ไม่อนุญาตให้ส่ง DHCP Offer/ACK
- เชื่อมต่อกับ clients
- ถ้าพยายามส่ง → port disabled

Result:
✅ Block rogue DHCP servers
✅ Protect clients
```

---

## สรุป

### สิ่งที่ได้เรียนรู้ใน Module 7:

✅ **DHCPv4 Concepts:**

- Dynamic IP address assignment
- DORA process (Discover, Offer, Request, ACK)
- DHCP message types
- Lease process (renewal, rebinding)

✅ **DHCP Server Configuration:**

- Configure DHCP pools
- Exclude addresses
- Define network parameters
- Multiple pools
- DHCP options

✅ **DHCP Relay:**

- ip helper-address
- Forward DHCP across subnets
- Centralized DHCP server

✅ **DHCP Client:**

- Configure interfaces as DHCP clients
- Troubleshoot DHCP client issues
- Release/Renew

✅ **DHCP Security:**

- DHCP attacks (starvation, spoofing)
- DHCP Snooping concept

✅ **Verification:**

- show ip dhcp pool
- show ip dhcp binding
- show ip dhcp server statistics
- show ip dhcp conflict

---

## คำสั่งสำคัญสรุป

### DHCP Server Configuration:

```cisco
! Exclude addresses
ip dhcp excluded-address <start-ip> [end-ip]

! Create pool
ip dhcp pool <pool-name>
 network <network> <mask>
 default-router <gateway>
 dns-server <dns-ip>
 domain-name <domain>
 lease {days [hours] [minutes] | infinite}

! Disable/Enable service
no service dhcp
service dhcp
```

### DHCP Relay:

```cisco
interface <interface>
 ip helper-address <dhcp-server-ip>
```

### DHCP Client:

```cisco
interface <interface>
 ip address dhcp
 no shutdown
```

### Verification:

```cisco
show ip dhcp pool [pool-name]
show ip dhcp binding
show ip dhcp server statistics
show ip dhcp conflict
show ip interface <interface>
show dhcp lease
show running-config | section dhcp
```

### Troubleshooting:

```cisco
! Clear bindings
clear ip dhcp binding {* | address}

! Clear conflict
clear ip dhcp conflict {* | address}

! Debug
debug ip dhcp server events
debug ip dhcp server packet
no debug all (ปิด debug)
```

---

## Best Practices

### DHCP Server:

```
✅ Exclude static IP ranges
✅ Use descriptive pool names
✅ Document DHCP configuration
✅ Set appropriate lease times
✅ Monitor DHCP usage (utilization)
✅ Regular backup configuration
✅ Implement DHCP redundancy (2 servers)
```

### DHCP Pools:

```
✅ Separate pools per subnet/VLAN
✅ Leave room for static assignments
✅ Don't allocate entire subnet
✅ Reserve addresses for:
   - Routers/Gateways
   - Servers
   - Printers
   - Network devices
```

### Lease Time:

```
Desktop/Office: 1-7 days
Laptops/Mobile: 8-24 hours
Guest Network: 1-4 hours
IoT Devices: 7-30 days
```

### Security:

```
✅ Implement DHCP Snooping (Module 11)
✅ Limit DHCP servers
✅ Monitor for rogue servers
✅ Use separate VLAN for DHCP servers
✅ Enable port security
```

---

## Common Mistakes

❌ **ไม่ exclude static IPs** → IP conflicts

❌ **Network statement ผิด** → DHCP ไม่แจก IP

❌ **ลืม default-router** → Clients ไม่มี gateway

❌ **ลืม dns-server** → Clients แปล DNS ไม่ได้

❌ **Relay misconfigured** → Cross-subnet DHCP ไม่ทำงาน

❌ **Lease time สั้นเกินไป** → Network congestion (renewals)

❌ **Lease time ยาวเกินไป** → IP exhaustion

---

## Lab Activities

### Lab 7.1: Configure DHCPv4 Server

**วัตถุประสงค์:**

- Configure DHCP pool
- Set network parameters
- Exclude addresses
- Verify operation

### Lab 7.2: Configure DHCP Relay

**วัตถุประสงค:**

- Configure ip helper-address
- Test cross-subnet DHCP
- Verify relay operation

### Lab 7.3: Troubleshoot DHCP

**วัตถุประสงค์:**

- Identify DHCP problems
- Fix misconfigurations
- Verify solutions
- Test client connectivity

---

## Packet Tracer Activities

### PT 7.1: Basic DHCP Server

**Tasks:**

1. Configure DHCP pool
2. Exclude static range
3. Set gateway และ DNS
4. Configure clients
5. Verify IP assignment

### PT 7.2: Multiple DHCP Pools

**Tasks:**

1. Configure pools for 3 VLANs
2. Different network parameters
3. Verify each pool
4. Test clients

### PT 7.3: DHCP Relay

**Tasks:**

1. Configure DHCP server on router
2. Configure relay on another router
3. Test remote clients
4. Verify relay operation

---

## คำถามทบทวน

1. DHCP ย่อมาจากอะไร? ทำหน้าที่อะไร?
2. DORA process คืออะไร?
3. DHCP ใช้ UDP ports อะไรบ้าง?
4. Lease time คืออะไร? T1 และ T2 คืออะไร?
5. DHCP relay ใช้ทำอะไร?
6. ip helper-address คำสั่งนี้ทำอะไร?
7. จะ exclude IP addresses ได้อย่างไร?
8. DHCP Discover เป็น broadcast หรือ unicast?
9. DHCP attacks มีอะไรบ้าง?
10. จะตรวจสอบ DHCP bindings ได้อย่างไร?

---

## เฉลยคำถาม

1. **DHCP:** Dynamic Host Configuration Protocol, กำหนด IP address และ network parameters อัตโนมัติ
2. **DORA:** Discover → Offer → Request → Acknowledge, กระบวนการขอและรับ IP
3. **UDP Ports:** Server = 67, Client = 68
4. **Lease Time:** ระยะเวลาที่ใช้ IP ได้; T1 = 50% (renewal), T2 = 87.5% (rebinding)
5. **DHCP Relay:** Forward DHCP broadcasts ข้าม subnets ไป DHCP server
6. **ip helper-address:** กำหนด IP ของ DHCP server สำหรับ relay
7. **Exclude:** `ip dhcp excluded-address <start-ip> [end-ip]`
8. **Discover:** Broadcast (255.255.255.255) เพราะ client ยังไม่มี IP
9. **Attacks:** DHCP Starvation, DHCP Spoofing (Rogue Server)
10. **Bindings:** `show ip dhcp binding`

---

## DHCP Message Flow Summary

```
Client                                DHCP Server
  │                                        │
  ├──── DHCPDISCOVER (Broadcast) ─────────►│
  │     "Who can give me an IP?"           │
  │                                        │
  │◄──── DHCPOFFER ─────────────────────────┤
  │     "I can give you 192.168.1.50"      │
  │                                        │
  ├──── DHCPREQUEST (Broadcast) ──────────►│
  │     "I want 192.168.1.50"              │
  │                                        │
  │◄──── DHCPACK ───────────────────────────┤
  │     "OK, 192.168.1.50 is yours"        │
  │                                        │
  ▼                                        ▼
Client starts using IP address
```

---

## Configuration Template

```cisco
!═══════════════════════════════════════════════════
! DHCP SERVER CONFIGURATION TEMPLATE
!═══════════════════════════════════════════════════

hostname DHCP-Router

! Exclude static IP addresses
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp excluded-address 192.168.20.1 192.168.20.10

! DHCP Pool for VLAN 10
ip dhcp pool VLAN10-POOL
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8 8.8.4.4
 domain-name vlan10.example.com
 lease 7

! DHCP Pool for VLAN 20
ip dhcp pool VLAN20-POOL
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 8.8.8.8 8.8.4.4
 domain-name vlan20.example.com
 lease 7

! Interface Configuration
interface GigabitEthernet0/0
 description VLAN 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown

interface GigabitEthernet0/1
 description VLAN 20
 ip address 192.168.20.1 255.255.255.0
 no shutdown

! Enable DHCP service (default)
service dhcp
```

---

**หมายเหตุ:** DHCPv4 เป็นโปรโตคอลสำคัญในการจัดการ IP addresses อัตโนมัติ ทำให้ network administration ง่ายและมีประสิทธิภาพมากขึ้น

---

**เอกสารจัดทำโดย:** Claude (Anthropic AI)  
**Module:** CCNA 2 Module 7 - DHCPv4  
**Version:** v7.02 SRWE  
**วันที่อัพเดท:** พฤศจิกายน 2025