# CCNA 2: Module 4 - Inter-VLAN Routing

## การกำหนดเส้นทางระหว่าง VLANs

---

## สารบัญ

1. [วัตถุประสงค์ของ Module](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#%E0%B8%A7%E0%B8%B1%E0%B8%95%E0%B8%96%E0%B8%B8%E0%B8%9B%E0%B8%A3%E0%B8%B0%E0%B8%AA%E0%B8%87%E0%B8%84%E0%B9%8C%E0%B8%82%E0%B8%AD%E0%B8%87-module)
2. [Inter-VLAN Routing Operation](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#1-inter-vlan-routing-operation)
3. [Router-on-a-Stick Inter-VLAN Routing](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#2-router-on-a-stick-inter-vlan-routing)
4. [Layer 3 Switch Inter-VLAN Routing](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#3-layer-3-switch-inter-vlan-routing)
5. [Troubleshoot Inter-VLAN Routing](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#4-troubleshoot-inter-vlan-routing)
6. [สรุป](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#%E0%B8%AA%E0%B8%A3%E0%B8%B8%E0%B8%9B)

---

## วัตถุประสงค์ของ Module

เมื่อจบ Module นี้ คุณจะสามารถ:

- ✅ อธิบายความจำเป็นของ inter-VLAN routing
- ✅ เข้าใจ legacy inter-VLAN routing
- ✅ Configure router-on-a-stick inter-VLAN routing
- ✅ Configure subinterfaces บน router
- ✅ Configure Layer 3 switch inter-VLAN routing
- ✅ Configure Switch Virtual Interfaces (SVIs)
- ✅ Configure routed ports บน Layer 3 switch
- ✅ Troubleshoot inter-VLAN routing problems
- ✅ เปรียบเทียบ router-on-a-stick vs Layer 3 switching

---

## 1. Inter-VLAN Routing Operation

### การทำงานของ Inter-VLAN Routing

### 1.1 ทำไมต้องมี Inter-VLAN Routing?

**ปัญหา:**

```
VLAN 10 (Sales)     VLAN 20 (Engineering)
192.168.10.0/24     192.168.20.0/24
    │                      │
    └──────[Switch]────────┘

- อุปกรณ์ใน VLAN 10 ไม่สามารถสื่อสารกับ VLAN 20 ได้
- VLANs = แยก broadcast domains
- Layer 2 switching ไม่ส่งต่อระหว่าง VLANs
```

**วิธีแก้:**

```
ต้องใช้ Layer 3 device (Router หรือ Layer 3 Switch)

VLAN 10 ──┐
          ├─ [Router] ─ เชื่อมต่อระหว่าง VLANs
VLAN 20 ──┘

Router ทำหน้าที่:
- รับ packets จาก VLAN หนึ่ง
- ตัดสินใจเส้นทาง (routing)
- ส่งต่อไปยังอีก VLAN
```

**ตัวอย่างการสื่อสาร:**

```
PC-A (VLAN 10)          PC-B (VLAN 20)
192.168.10.10           192.168.20.10

Step 1: PC-A ต้องการส่งข้อมูลไป PC-B
Step 2: PC-A ตรวจสอบว่า PC-B อยู่คนละ subnet
Step 3: PC-A ส่ง packet ไป default gateway (Router)
Step 4: Router รับ packet จาก VLAN 10
Step 5: Router ตรวจสอบ routing table
Step 6: Router ส่งต่อ packet ไป VLAN 20
Step 7: PC-B รับ packet
```

### 1.2 Legacy Inter-VLAN Routing

**แนวทางแรก (ไม่แนะนำแล้ว):**

```
Topology:
         [Router]
           │  │
       Fa0/0  Fa0/1
         │    │
    ┌────┴────┴────┐
    │    Switch    │
    ├────────┬─────┤
  VLAN 10  VLAN 20

Configuration:
- Router มี interface แยกสำหรับแต่ละ VLAN
- แต่ละ interface เชื่อมกับ switch port ที่เป็น access mode
- แต่ละ VLAN ใช้สายแยกกัน
```

**Configuration Example:**

```cisco
! Router Configuration
Router(config)# interface fastethernet 0/0
Router(config-if)# ip address 192.168.10.1 255.255.255.0
Router(config-if)# no shutdown

Router(config)# interface fastethernet 0/1
Router(config-if)# ip address 192.168.20.1 255.255.255.0
Router(config-if)# no shutdown

! Switch Configuration
Switch(config)# interface fastethernet 0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10

Switch(config)# interface fastethernet 0/2
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 20
```

**ข้อเสีย:**

- ❌ ใช้ router interface เยอะ (1 interface ต่อ 1 VLAN)
- ❌ ใช้ switch port เยอะ
- ❌ ต้องใช้สายเยอะ
- ❌ ไม่ scalable (router มี interface จำกัด)
- ❌ แพงและไม่มีประสิทธิภาพ

**ข้อดี:**

- ✅ เข้าใจง่าย
- ✅ Configure ง่าย
- ✅ แยกกันชัดเจน

### 1.3 Router-on-a-Stick Overview

**แนวทางที่ดีกว่า:**

```
Topology:
         [Router]
            │
          Gi0/0 (Trunk)
            │
    ┌───────┴───────┐
    │    Switch     │
    ├───────┬───────┤
  VLAN 10  VLAN 20

- ใช้ interface เดียวของ router
- Configure เป็น trunk link
- ใช้ subinterfaces สำหรับแต่ละ VLAN
- ใช้สายเส้นเดียว
```

**ข้อดี:**

- ✅ ประหยัด router interfaces
- ✅ ประหยัด switch ports
- ✅ ใช้สายเส้นเดียว
- ✅ Scalable (รองรับหลาย VLANs)
- ✅ Cost-effective

**ข้อเสีย:**

- ❌ Trunk link อาจเป็น bottleneck
- ❌ ซับซ้อนกว่า legacy
- ❌ ไม่เหมาะกับ high-traffic networks

### 1.4 Layer 3 Switch Overview

**แนวทางที่ดีที่สุด:**

```
[Layer 3 Switch]
├── VLAN 10 (SVI: 192.168.10.1)
├── VLAN 20 (SVI: 192.168.20.1)
└── VLAN 30 (SVI: 192.168.30.1)

- Routing ทำงานภายใน switch
- ใช้ SVIs (Switch Virtual Interfaces)
- Wire-speed routing
- ไม่ต้องใช้ external router
```

**ข้อดี:**

- ✅ ประสิทธิภาพสูงสุด (wire-speed)
- ✅ ไม่มี bottleneck
- ✅ Low latency
- ✅ Scalable
- ✅ ไม่ต้องใช้ external router

**ข้อเสีย:**

- ❌ แพงกว่า Layer 2 switch
- ❌ ซับซ้อนกว่าในการ configure
- ❌ ต้อง enable routing

---

## 2. Router-on-a-Stick Inter-VLAN Routing

### การกำหนดเส้นทางด้วย Router-on-a-Stick

### 2.1 Subinterface Concept

**Subinterface คืออะไร:**

- Interface เสมือนที่สร้างจาก physical interface
- แต่ละ subinterface แทน VLAN หนึ่ง
- ใช้ 802.1Q tagging
- อยู่ใน subnet ต่างกัน

**Naming Convention:**

```
Interface.subinterface-number

ตัวอย่าง:
- GigabitEthernet0/0.10   (VLAN 10)
- GigabitEthernet0/0.20   (VLAN 20)
- GigabitEthernet0/0.30   (VLAN 30)

Best Practice:
ใช้ subinterface number = VLAN ID
เช่น .10 สำหรับ VLAN 10, .20 สำหรับ VLAN 20
```

### 2.2 Router-on-a-Stick Configuration

**Topology:**

```
PC-A            PC-B
VLAN 10         VLAN 20
192.168.10.10   192.168.20.10
    │              │
    └──[Switch]────┘
           │
       Trunk (Fa0/5)
           │
       Gi0/0 (Router)

Default Gateways:
- VLAN 10: 192.168.10.1
- VLAN 20: 192.168.20.1
```

**Step 1: Switch Configuration**

```cisco
! สร้าง VLANs
Switch(config)# vlan 10
Switch(config-vlan)# name Sales
Switch(config-vlan)# exit

Switch(config)# vlan 20
Switch(config-vlan)# name Engineering
Switch(config-vlan)# exit

! กำหนด access ports
Switch(config)# interface fa0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config-if)# no shutdown

Switch(config)# interface fa0/2
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 20
Switch(config-if)# no shutdown

! Configure trunk ไป router
Switch(config)# interface fa0/5
Switch(config-if)# description Trunk to Router
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk native vlan 99
Switch(config-if)# switchport trunk allowed vlan 10,20,99
Switch(config-if)# no shutdown
```

**Step 2: Router Configuration**

```cisco
! เข้าสู่ physical interface
Router(config)# interface gigabitethernet 0/0
Router(config-if)# description Trunk link to Switch
Router(config-if)# no shutdown
Router(config-if)# exit

! สร้าง subinterface สำหรับ VLAN 10
Router(config)# interface gigabitethernet 0/0.10
Router(config-subif)# description Sales VLAN 10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0
Router(config-subif)# exit

! สร้าง subinterface สำหรับ VLAN 20
Router(config)# interface gigabitethernet 0/0.20
Router(config-subif)# description Engineering VLAN 20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
Router(config-subif)# exit

! สร้าง subinterface สำหรับ native VLAN (optional แต่แนะนำ)
Router(config)# interface gigabitethernet 0/0.99
Router(config-subif)# description Native VLAN 99
Router(config-subif)# encapsulation dot1Q 99 native
Router(config-subif)# exit
```

**คำอธิบายคำสั่ง:**

```cisco
encapsulation dot1Q <vlan-id>
- ระบุว่า subinterface นี้ใช้ 802.1Q tagging
- ระบุ VLAN ID

encapsulation dot1Q <vlan-id> native
- ระบุว่าเป็น native VLAN
- untagged traffic

ip address <ip> <mask>
- กำหนด IP address (จะเป็น default gateway ของ VLAN นั้น)
```

### 2.3 Complete Configuration Example

**ตัวอย่างสมบูรณ์:**

```cisco
!═══════════════════════════════════════════════════
! SWITCH CONFIGURATION
!═══════════════════════════════════════════════════

hostname S1

! สร้าง VLANs
vlan 10
 name Sales
vlan 20
 name Engineering
vlan 30
 name HR
vlan 99
 name Management

! Management Interface
interface vlan 99
 description Management SVI
 ip address 192.168.99.10 255.255.255.0
 no shutdown

ip default-gateway 192.168.99.1

! Access Ports - VLAN 10
interface range fastethernet 0/1 - 5
 description Sales Department
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
 no shutdown

! Access Ports - VLAN 20
interface range fastethernet 0/6 - 10
 description Engineering Department
 switchport mode access
 switchport access vlan 20
 spanning-tree portfast
 no shutdown

! Access Ports - VLAN 30
interface range fastethernet 0/11 - 15
 description HR Department
 switchport mode access
 switchport access vlan 30
 spanning-tree portfast
 no shutdown

! Trunk to Router
interface gigabitethernet 0/1
 description Trunk to Router R1
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,99
 no shutdown

!═══════════════════════════════════════════════════
! ROUTER CONFIGURATION
!═══════════════════════════════════════════════════

hostname R1

! Enable physical interface
interface gigabitethernet 0/0
 description Trunk link to Switch S1
 no shutdown

! Subinterface for VLAN 10
interface gigabitethernet 0/0.10
 description Sales VLAN 10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

! Subinterface for VLAN 20
interface gigabitethernet 0/0.20
 description Engineering VLAN 20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0

! Subinterface for VLAN 30
interface gigabitethernet 0/0.30
 description HR VLAN 30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0

! Subinterface for Native VLAN
interface gigabitethernet 0/0.99
 description Native VLAN 99
 encapsulation dot1Q 99 native
 ip address 192.168.99.1 255.255.255.0

! Connection to Internet (example)
interface gigabitethernet 0/1
 description Connection to ISP
 ip address 203.0.113.2 255.255.255.252
 no shutdown
```

### 2.4 Verify Router-on-a-Stick

**คำสั่งตรวจสอบ:**

**1. Verify Subinterfaces:**

```cisco
Router# show ip interface brief

Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0     unassigned      YES unset  up                    up
GigabitEthernet0/0.10  192.168.10.1    YES manual up                    up
GigabitEthernet0/0.20  192.168.20.1    YES manual up                    up
GigabitEthernet0/0.30  192.168.30.1    YES manual up                    up
GigabitEthernet0/0.99  192.168.99.1    YES manual up                    up
GigabitEthernet0/1     203.0.113.2     YES manual up                    up
```

**2. Verify Subinterface Details:**

```cisco
Router# show interfaces gigabitethernet 0/0.10

GigabitEthernet0/0.10 is up, line protocol is up
  Hardware is iGbE, address is 0cd9.96d2.4000
  Description: Sales VLAN 10
  Internet address is 192.168.10.1/24
  MTU 1500 bytes, BW 1000000 Kbit/sec
  Encapsulation 802.1Q Virtual LAN, Vlan ID  10
  ...
```

**3. Verify Routing Table:**

```cisco
Router# show ip route

Codes: C - connected, S - static, ...

Gateway of last resort is 203.0.113.1 to network 0.0.0.0

C    192.168.10.0/24 is directly connected, GigabitEthernet0/0.10
C    192.168.20.0/24 is directly connected, GigabitEthernet0/0.20
C    192.168.30.0/24 is directly connected, GigabitEthernet0/0.30
C    192.168.99.0/24 is directly connected, GigabitEthernet0/0.99
C    203.0.113.0/30 is directly connected, GigabitEthernet0/1
S*   0.0.0.0/0 [1/0] via 203.0.113.1
```

**4. Verify Switch Trunk:**

```cisco
Switch# show interfaces trunk

Port        Mode             Encapsulation  Status        Native vlan
Gi0/1       on               802.1q         trunking      99

Port        Vlans allowed on trunk
Gi0/1       10,20,30,99

Port        Vlans allowed and active in management domain
Gi0/1       10,20,30,99
```

**5. Test Connectivity:**

```cisco
! จาก PC ใน VLAN 10
PC-A> ping 192.168.10.1     (Test default gateway)
PC-A> ping 192.168.20.10    (Test inter-VLAN routing)

! จาก Router
Router# ping 192.168.10.10  (Test VLAN 10)
Router# ping 192.168.20.10  (Test VLAN 20)
```

### 2.5 IPv6 Router-on-a-Stick

**Configuration Example:**

```cisco
! Router Configuration
Router(config)# ipv6 unicast-routing

! Physical Interface
Router(config)# interface gigabitethernet 0/0
Router(config-if)# no shutdown

! Subinterface VLAN 10
Router(config)# interface gigabitethernet 0/0.10
Router(config-subif)# description Sales VLAN 10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ipv6 address 2001:db8:acad:10::1/64
Router(config-subif)# ipv6 address fe80::1 link-local

! Subinterface VLAN 20
Router(config)# interface gigabitethernet 0/0.20
Router(config-subif)# description Engineering VLAN 20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ipv6 address 2001:db8:acad:20::1/64
Router(config-subif)# ipv6 address fe80::1 link-local
```

**Verify IPv6:**

```cisco
Router# show ipv6 interface brief

GigabitEthernet0/0         [up/up]
    unassigned
GigabitEthernet0/0.10      [up/up]
    FE80::1
    2001:DB8:ACAD:10::1
GigabitEthernet0/0.20      [up/up]
    FE80::1
    2001:DB8:ACAD:20::1
```

---

## 3. Layer 3 Switch Inter-VLAN Routing

### การกำหนดเส้นทางด้วย Layer 3 Switch

### 3.1 Layer 3 Switch Concepts

**Layer 3 Switch คืออะไร:**

- Switch ที่มีความสามารถในการ routing
- ทำงานทั้ง Layer 2 (switching) และ Layer 3 (routing)
- Routing ด้วย hardware (ASIC) = wire-speed
- ไม่ต้องใช้ external router

**ข้อแตกต่าง:**

```
Layer 2 Switch:
- Switching เท่านั้น
- ใช้ MAC addresses
- ไม่มี routing

Layer 3 Switch:
- Switching + Routing
- ใช้ทั้ง MAC addresses และ IP addresses
- Routing ด้วย hardware (เร็วมาก)
- รองรับ routing protocols (OSPF, EIGRP, etc.)
```

### 3.2 Switch Virtual Interface (SVI)

**SVI คืออะไร:**

- Virtual interface ที่แทน VLAN
- กำหนด IP address บน SVI
- ทำหน้าที่เป็น default gateway ของ VLAN
- ไม่ใช่ physical interface

**SVI Configuration:**

```cisco
! เปิดใช้งาน IP routing
Switch(config)# ip routing

! สร้าง VLANs
Switch(config)# vlan 10
Switch(config-vlan)# name Sales

Switch(config)# vlan 20
Switch(config-vlan)# name Engineering

! สร้าง SVI สำหรับ VLAN 10
Switch(config)# interface vlan 10
Switch(config-if)# description Sales Gateway
Switch(config-if)# ip address 192.168.10.1 255.255.255.0
Switch(config-if)# no shutdown

! สร้าง SVI สำหรับ VLAN 20
Switch(config)# interface vlan 20
Switch(config-if)# description Engineering Gateway
Switch(config-if)# ip address 192.168.20.1 255.255.255.0
Switch(config-if)# no shutdown
```

**สิ่งที่ต้องทำให้ SVI ขึ้น (up):**

1. VLAN ต้องมีอยู่ใน VLAN database
2. อย่างน้อย 1 พอร์ตใน VLAN นั้นต้อง up
3. SVI ต้อง no shutdown

### 3.3 Complete Layer 3 Switch Configuration

**Topology:**

```
PC-A (VLAN 10)      PC-B (VLAN 20)      PC-C (VLAN 30)
192.168.10.10       192.168.20.10       192.168.30.10
      │                   │                   │
      └───────[Layer 3 Switch]────────────────┘
                    │
                   WAN
```

**Full Configuration:**

```cisco
!═══════════════════════════════════════════════════
! LAYER 3 SWITCH CONFIGURATION
!═══════════════════════════════════════════════════

hostname MLS1

! เปิดใช้งาน IP routing (สำคัญ!)
ip routing

! สร้าง VLANs
vlan 10
 name Sales
vlan 20
 name Engineering
vlan 30
 name HR
vlan 99
 name Management

! Configure SVIs (Default Gateways)
interface vlan 10
 description Sales Gateway
 ip address 192.168.10.1 255.255.255.0
 no shutdown

interface vlan 20
 description Engineering Gateway
 ip address 192.168.20.1 255.255.255.0
 no shutdown

interface vlan 30
 description HR Gateway
 ip address 192.168.30.1 255.255.255.0
 no shutdown

interface vlan 99
 description Management
 ip address 192.168.99.1 255.255.255.0
 no shutdown

! Access Ports - VLAN 10
interface range gigabitethernet 1/0/1 - 8
 description Sales Department
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
 no shutdown

! Access Ports - VLAN 20
interface range gigabitethernet 1/0/9 - 16
 description Engineering Department
 switchport mode access
 switchport access vlan 20
 spanning-tree portfast
 no shutdown

! Access Ports - VLAN 30
interface range gigabitethernet 1/0/17 - 24
 description HR Department
 switchport mode access
 switchport access vlan 30
 spanning-tree portfast
 no shutdown

! Routed Port to Router/Firewall (Layer 3 interface)
interface gigabitethernet 1/0/28
 description Connection to WAN Router
 no switchport
 ip address 10.1.1.2 255.255.255.252
 no shutdown

! Default route (ถ้าต้องการ)
ip route 0.0.0.0 0.0.0.0 10.1.1.1
```

### 3.4 Routed Ports on Layer 3 Switch

**Routed Port คืออะไร:**

- Physical interface ที่ทำงานเป็น Layer 3 (เหมือน router port)
- ไม่ทำ switching
- กำหนด IP address โดยตรง
- ใช้เชื่อมต่อกับ router หรือ Layer 3 device อื่น

**Configuration:**

```cisco
! เปลี่ยน switchport เป็น routed port
Switch(config)# interface gigabitethernet 1/0/28
Switch(config-if)# no switchport
Switch(config-if)# ip address 10.1.1.2 255.255.255.252
Switch(config-if)# description Link to WAN Router
Switch(config-if)# no shutdown
```

**คำอธิบาย:**

- `no switchport` - เปลี่ยนจาก Layer 2 เป็น Layer 3
- หลังจากนั้นสามารถกำหนด IP address ได้
- ใช้สำหรับ point-to-point links

**ตัวอย่าง Routed Ports:**

```
[Layer 3 Switch] ─── Gi1/0/28 (routed) ─── [Router]
  10.1.1.2/30                                10.1.1.1/30

! Switch
interface gigabitethernet 1/0/28
 no switchport
 ip address 10.1.1.2 255.255.255.252

! Router
interface gigabitethernet 0/0
 ip address 10.1.1.1 255.255.255.252
```

### 3.5 Verify Layer 3 Switching

**1. Verify IP Routing:**

```cisco
Switch# show ip route

Codes: C - connected, S - static, ...

Gateway of last resort is 10.1.1.1 to network 0.0.0.0

C    192.168.10.0/24 is directly connected, Vlan10
L    192.168.10.1/32 is directly connected, Vlan10
C    192.168.20.0/24 is directly connected, Vlan20
L    192.168.20.1/32 is directly connected, Vlan20
C    192.168.30.0/24 is directly connected, Vlan30
L    192.168.30.1/32 is directly connected, Vlan30
C    10.1.1.0/30 is directly connected, GigabitEthernet1/0/28
L    10.1.1.2/32 is directly connected, GigabitEthernet1/0/28
S*   0.0.0.0/0 [1/0] via 10.1.1.1
```

**2. Verify SVIs:**

```cisco
Switch# show ip interface brief

Interface              IP-Address      OK? Method Status                Protocol
Vlan10                 192.168.10.1    YES manual up                    up
Vlan20                 192.168.20.1    YES manual up                    up
Vlan30                 192.168.30.1    YES manual up                    up
Vlan99                 192.168.99.1    YES manual up                    up
GigabitEthernet1/0/28  10.1.1.2        YES manual up                    up
```

**3. Verify Routing is Enabled:**

```cisco
Switch# show ip protocols

*** IP Routing is NSF aware ***

Routing Protocol is "connected"
  ...
```

**4. Verify Interface Details:**

```cisco
Switch# show interfaces vlan 10

Vlan10 is up, line protocol is up
  Hardware is Ethernet SVI, address is 0cd9.96d2.4800
  Description: Sales Gateway
  Internet address is 192.168.10.1/24
  MTU 1500 bytes, BW 1000000 Kbit/sec
  ...
```

**5. Verify Routed Port:**

```cisco
Switch# show interfaces gigabitethernet 1/0/28

GigabitEthernet1/0/28 is up, line protocol is up
  Hardware is iGbE, address is 0cd9.96d2.4828
  Description: Link to WAN Router
  Internet address is 10.1.1.2/30
  MTU 1500 bytes, BW 1000000 Kbit/sec
  ...
```

### 3.6 IPv6 on Layer 3 Switch

**Configuration:**

```cisco
! เปิด IPv6 routing
Switch(config)# ipv6 unicast-routing

! Configure IPv6 บน SVIs
Switch(config)# interface vlan 10
Switch(config-if)# ipv6 address 2001:db8:acad:10::1/64
Switch(config-if)# ipv6 address fe80::1 link-local

Switch(config)# interface vlan 20
Switch(config-if)# ipv6 address 2001:db8:acad:20::1/64
Switch(config-if)# ipv6 address fe80::1 link-local

! Configure IPv6 บน routed port
Switch(config)# interface gigabitethernet 1/0/28
Switch(config-if)# no switchport
Switch(config-if)# ipv6 address 2001:db8:acad:a::2/64
Switch(config-if)# ipv6 address fe80::2 link-local
```

**Verify:**

```cisco
Switch# show ipv6 interface brief

Vlan10                     [up/up]
    FE80::1
    2001:DB8:ACAD:10::1
Vlan20                     [up/up]
    FE80::1
    2001:DB8:ACAD:20::1
GigabitEthernet1/0/28      [up/up]
    FE80::2
    2001:DB8:ACAD:A::2
```

---

## 4. Troubleshoot Inter-VLAN Routing

### การแก้ไขปัญหา Inter-VLAN Routing

### 4.1 Common Issues - Router-on-a-Stick

**Problem 1: Physical Interface Down**

```
Symptom:
- ไม่มี inter-VLAN routing เลย
- All VLANs affected

Cause:
- Physical interface shutdown
- Cable problem
- Port issue

Troubleshooting:
Router# show ip interface brief
Router# show interfaces gigabitethernet 0/0

Solution:
Router(config)# interface gigabitethernet 0/0
Router(config-if)# no shutdown
```

**Problem 2: Subinterface Configuration Error**

```
Symptom:
- VLAN เฉพาะตัวไม่ทำงาน
- VLANs อื่นทำงานปกติ

Cause:
- Wrong VLAN ID ใน encapsulation
- Wrong IP address
- Wrong subnet mask

Troubleshooting:
Router# show interfaces gigabitethernet 0/0.10
Router# show running-config interface gigabitethernet 0/0.10

Solution:
Router(config)# interface gigabitethernet 0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0
```

**Problem 3: Missing Encapsulation**

```
Symptom:
- Subinterface down
- No routing ใน VLAN นั้น

Cause:
- ลืม configure encapsulation dot1Q

Troubleshooting:
Router# show interfaces gigabitethernet 0/0.10
GigabitEthernet0/0.10 is down (encapsulation is not configured)

Solution:
Router(config)# interface gigabitethernet 0/0.10
Router(config-subif)# encapsulation dot1Q 10
```

**Problem 4: Native VLAN Mismatch**

```
Symptom:
- Native VLAN ไม่ทำงาน
- CDP warnings

Cause:
- Router และ Switch native VLAN ไม่ตรงกัน

Troubleshooting:
Router# show interfaces gigabitethernet 0/0.99
Switch# show interfaces trunk

Solution:
ตั้งให้ตรงกันทั้งสองฝั่ง
Router(config-subif)# encapsulation dot1Q 99 native
Switch(config-if)# switchport trunk native vlan 99
```

**Problem 5: Trunk Not Formed**

```
Symptom:
- ไม่มี inter-VLAN routing

Cause:
- Switch port ไม่ใช่ trunk mode
- DTP mismatch
- Allowed VLANs ไม่ถูกต้อง

Troubleshooting:
Switch# show interfaces trunk
Switch# show interfaces fa0/5 switchport

Solution:
Switch(config)# interface fa0/5
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 10,20,30,99
```

### 4.2 Common Issues - Layer 3 Switch

**Problem 1: IP Routing Not Enabled**

```
Symptom:
- ไม่มี inter-VLAN routing เลย
- Routing table ว่าง

Cause:
- ลืม enable ip routing

Troubleshooting:
Switch# show ip route
Default gateway is not set

Switch# show ip protocols
IP Routing is not enabled

Solution:
Switch(config)# ip routing
```

**Problem 2: SVI Down**

```
Symptom:
- VLAN เฉพาะตัวไม่ routing

Cause:
- SVI shutdown
- VLAN ไม่มีใน database
- ไม่มีพอร์ต active ใน VLAN

Troubleshooting:
Switch# show ip interface brief | include Vlan
Vlan10   192.168.10.1    YES manual down     down

Switch# show vlan id 10
Switch# show interfaces status | include connected

Solution:
! เปิด SVI
Switch(config)# interface vlan 10
Switch(config-if)# no shutdown

! สร้าง VLAN (ถ้ายังไม่มี)
Switch(config)# vlan 10

! ตรวจสอบว่ามีพอร์ต active ใน VLAN
```

**Problem 3: Wrong IP Address on SVI**

```
Symptom:
- PC ไม่สามารถ ping default gateway
- Inter-VLAN routing ไม่ทำงาน

Cause:
- IP address ผิด
- Subnet mask ผิด
- Wrong VLAN

Troubleshooting:
Switch# show running-config interface vlan 10
Switch# show ip interface vlan 10

Solution:
Switch(config)# interface vlan 10
Switch(config-if)# ip address 192.168.10.1 255.255.255.0
```

**Problem 4: Routed Port Issues**

```
Symptom:
- ไม่สามารถเข้าถึง external networks

Cause:
- ลืม "no switchport"
- Wrong IP address
- Port down

Troubleshooting:
Switch# show ip interface brief | include Gig1/0/28
Switch# show running-config interface gigabitethernet 1/0/28

Solution:
Switch(config)# interface gigabitethernet 1/0/28
Switch(config-if)# no switchport
Switch(config-if)# ip address 10.1.1.2 255.255.255.252
Switch(config-if)# no shutdown
```

### 4.3 Systematic Troubleshooting Approach

**Step 1: Verify Physical Connectivity**

```cisco
! ตรวจสอบสถานะ interfaces
show ip interface brief
show interfaces status

! ตรวจสอบ trunk
show interfaces trunk

! ตรวจสอบสาย
show interfaces <interface> | include line protocol
```

**Step 2: Verify VLAN Configuration**

```cisco
! ตรวจสอบ VLANs
show vlan brief

! ตรวจสอบว่าพอร์ตอยู่ใน VLAN ถูกต้อง
show vlan id <vlan-id>
```

**Step 3: Verify Inter-VLAN Routing Config**

```cisco
! Router-on-a-Stick
show running-config interface <interface>
show ip interface brief

! Layer 3 Switch
show ip routing
show ip route
show running-config interface vlan <vlan-id>
```

**Step 4: Verify IP Addressing**

```cisco
! ตรวจสอบ IP addresses
show ip interface brief
show running-config | section interface

! ตรวจสอบ default gateway (PC)
ipconfig (Windows)
ip addr show (Linux)
```

**Step 5: Test Connectivity**

```cisco
! Ping default gateway
ping <gateway-ip>

! Ping อุปกรณ์ในอีก VLAN
ping <remote-ip>

! Trace route
traceroute <remote-ip>
```

**Step 6: Verify Routing Table**

```cisco
! ตรวจสอบว่ามี routes
show ip route
show ip route connected

! ตรวจสอบว่า routing protocol ทำงาน (ถ้ามี)
show ip protocols
show ip route ospf (ตัวอย่าง)
```

### 4.4 Troubleshooting Commands Summary

**Router-on-a-Stick:**

```cisco
show ip interface brief
show interfaces <interface>
show interfaces <interface>.<subinterface>
show running-config interface <interface>
show ip route
show vlans
debug ip packet
```

**Layer 3 Switch:**

```cisco
show ip interface brief
show ip route
show ip routing
show interfaces vlan <vlan-id>
show vlan brief
show vlan id <vlan-id>
show running-config interface vlan <vlan-id>
show interfaces trunk
show ip protocols
```

**Common Commands:**

```cisco
ping <ip>
traceroute <ip>
show arp
show mac address-table
show cdp neighbors
show running-config
show startup-config
```

---

## สรุป

### สิ่งที่ได้เรียนรู้ใน Module 4:

✅ **Inter-VLAN Routing Concepts:**

- ทำไมต้องมี inter-VLAN routing
- Legacy inter-VLAN routing (แยก interface)
- Router-on-a-stick (subinterfaces)
- Layer 3 switching (SVIs)

✅ **Router-on-a-Stick:**

- Subinterface configuration
- 802.1Q encapsulation
- Native VLAN handling
- IPv4 และ IPv6 configuration

✅ **Layer 3 Switch:**

- IP routing enablement
- SVI configuration
- Routed ports
- Wire-speed performance

✅ **Troubleshooting:**

- Common issues
- Systematic approach
- Verification commands
- Problem resolution

✅ **Comparison:**

- Router-on-a-Stick vs Layer 3 Switch
- Use cases
- Advantages/disadvantages

---

## การเปรียบเทียบวิธีการ

|Feature|Router-on-a-Stick|Layer 3 Switch|
|---|---|---|
|**Cost**|ถูกกว่า (ถ้ามี router อยู่แล้ว)|แพงกว่า|
|**Performance**|Bandwidth bottleneck possible|Wire-speed (เร็วที่สุด)|
|**Scalability**|จำกัด (bandwidth)|ดีมาก|
|**Complexity**|ปานกลาง|ปานกลาง-สูง|
|**Latency**|สูงกว่า|ต่ำมาก|
|**Use Case**|Small networks, branch offices|Medium-large networks|
|**Configuration**|Subinterfaces|SVIs|
|**Interface Used**|1 physical + subinterfaces|Virtual interfaces|

---

## Best Practices

### Router-on-a-Stick:

```
✅ ใช้ Gigabit links (ลด bottleneck)
✅ Subinterface number = VLAN ID
✅ Native VLAN ต้องตรงกันทั้งสองฝั่ง
✅ เพิ่ม description บน subinterfaces
✅ เหมาะกับ small-medium networks
```

### Layer 3 Switch:

```
✅ Enable ip routing ก่อนเสมอ
✅ Configure SVI สำหรับทุก VLANs ที่ใช้
✅ ใช้ routed ports สำหรับ point-to-point links
✅ เหมาะกับ medium-large networks
✅ Plan IP addressing ให้ดี
```

### General:

```
✅ Document ทุกการตั้งค่า
✅ ใช้ consistent naming
✅ Test connectivity หลัง configure
✅ Backup configuration
✅ เลือกวิธีที่เหมาะกับองค์กร
```

---

## คำสั่งสำคัญสรุป

### Router-on-a-Stick:

```cisco
! Physical Interface
interface <interface>
 no shutdown

! Subinterface
interface <interface>.<subinterface>
 description <text>
 encapsulation dot1Q <vlan-id> [native]
 ip address <ip> <mask>
```

### Layer 3 Switch:

```cisco
! Enable Routing
ip routing

! SVI
interface vlan <vlan-id>
 description <text>
 ip address <ip> <mask>
 no shutdown

! Routed Port
interface <interface>
 no switchport
 ip address <ip> <mask>
 no shutdown
```

### Verification:

```cisco
show ip interface brief
show ip route
show interfaces trunk
show vlan brief
show running-config interface <interface>
```

---

## Lab Activities

### Lab 4.1: Configure Router-on-a-Stick

**วัตถุประสงค์:**

- Configure subinterfaces
- Configure trunk on switch
- Verify inter-VLAN routing
- Test connectivity

### Lab 4.2: Configure Layer 3 Switch

**วัตถุประสงค์:**

- Enable IP routing
- Configure SVIs
- Configure routed ports
- Verify routing table

### Lab 4.3: Troubleshoot Inter-VLAN Routing

**วัตถุประสงค์:**

- Identify problems
- Use troubleshooting commands
- Fix configuration errors
- Verify solutions

---

## Packet Tracer Activities

### PT 4.2.1: Router-on-a-Stick

**Tasks:**

1. Configure VLANs on switch
2. Configure trunk
3. Configure subinterfaces on router
4. Verify inter-VLAN routing
5. Test connectivity between VLANs

### PT 4.3.1: Layer 3 Switch Configuration

**Tasks:**

1. Enable IP routing
2. Create VLANs
3. Configure SVIs
4. Configure routed port
5. Test routing

### PT 4.4.1: Troubleshooting

**Tasks:**

1. Identify problems
2. Fix router-on-a-stick issues
3. Fix Layer 3 switch issues
4. Verify solutions

---

## คำถามทบทวน

1. ทำไม VLANs ต้องมี routing จึงจะสื่อสารกันได้?
2. Router-on-a-Stick ทำงานอย่างไร?
3. Subinterface คืออะไร?
4. Native VLAN มีความสำคัญอย่างไรกับ subinterfaces?
5. Layer 3 switch ต่างจาก router-on-a-stick อย่างไร?
6. SVI คืออะไร?
7. Routed port ต่างจาก switchport อย่างไร?
8. ทำไมต้อง enable ip routing บน Layer 3 switch?
9. วิธีการ troubleshoot inter-VLAN routing problems?
10. เมื่อไหร่ควรใช้ router-on-a-stick และเมื่อไหร่ควรใช้ Layer 3 switch?

---

## เฉลยคำถาม

1. **Routing Required:** VLANs = แยก broadcast domains = แยก subnets, ต้องใช้ Layer 3 routing เพื่อส่งต่อระหว่าง subnets
2. **Router-on-a-Stick:** ใช้ trunk link + subinterfaces, แต่ละ subinterface = 1 VLAN, routing ระหว่าง VLANs
3. **Subinterface:** Virtual interface สร้างจาก physical interface, แต่ละตัวมี IP แยกกัน, ใช้ encapsulation dot1Q
4. **Native VLAN:** untagged traffic บน trunk, ต้องตรงกันทั้ง router และ switch, ใช้ "native" keyword
5. **Layer 3 vs Router-on-a-Stick:** L3 switch = wire-speed, ไม่มี bottleneck, แพงกว่า; Router = ถูกกว่าแต่อาจมี bottleneck
6. **SVI:** Switch Virtual Interface, virtual interface สำหรับ VLAN, กำหนด IP address, ทำหน้าที่เป็น gateway
7. **Routed Port:** Layer 3 interface (no switchport), กำหนด IP ได้, ไม่ใช่ switchport, ใช้กับ point-to-point links
8. **Enable IP Routing:** Layer 3 switch default ทำ switching อย่างเดียว, ต้อง enable routing เพื่อใช้ routing features
9. **Troubleshooting:** ตรวจสอบ physical → VLANs → routing config → IP addressing → routing table → test connectivity
10. **Use Cases:** Router-on-a-Stick = small networks, budget limited; Layer 3 Switch = medium-large, need performance

---

## Configuration Templates

### Template 1: Router-on-a-Stick

```cisco
!═══════════════════════════════════
! ROUTER CONFIGURATION
!═══════════════════════════════════
hostname R1
!
interface GigabitEthernet0/0
 description Trunk to Switch
 no shutdown
!
interface GigabitEthernet0/0.10
 description VLAN 10 Gateway
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
!
interface GigabitEthernet0/0.20
 description VLAN 20 Gateway
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
!
interface GigabitEthernet0/0.99
 description Native VLAN
 encapsulation dot1Q 99 native
!
```

### Template 2: Layer 3 Switch

```cisco
!═══════════════════════════════════
! LAYER 3 SWITCH CONFIGURATION
!═══════════════════════════════════
hostname MLS1
!
ip routing
!
vlan 10
 name Sales
vlan 20
 name Engineering
!
interface Vlan10
 description Sales Gateway
 ip address 192.168.10.1 255.255.255.0
 no shutdown
!
interface Vlan20
 description Engineering Gateway
 ip address 192.168.20.1 255.255.255.0
 no shutdown
!
interface range GigabitEthernet1/0/1-10
 switchport mode access
 switchport access vlan 10
!
interface range GigabitEthernet1/0/11-20
 switchport mode access
 switchport access vlan 20
!
```

---

**หมายเหตุ:** Inter-VLAN routing เป็นทักษะสำคัญในการออกแบบ network การเลือกใช้ router-on-a-stick หรือ Layer 3 switch ขึ้นอยู่กับขนาด network, budget, และความต้องการด้าน performance

---

**เอกสารจัดทำโดย:** Claude (Anthropic AI)  
**Module:** CCNA 2 Module 4 - Inter-VLAN Routing  
**Version:** v7.02 SRWE  
**วันที่อัพเดท:** พฤศจิกายน 2025