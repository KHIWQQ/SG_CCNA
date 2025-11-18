# CCNA Course 3 - Module 5: ACLs for IPv4 Configuration

## การกำหนดค่า ACLs สำหรับ IPv4

---

## 5.1 Configure Standard IPv4 ACLs

### Standard ACL Overview

**คำจำกัดความ:**

- กรองตาม **source IP address** เท่านั้น
- ไม่สามารถระบุ destination, protocol, port
- Number range: **1-99** และ **1300-1999**

**Syntax:**

```
Router(config)# access-list <number> {permit|deny} <source> [<wildcard>] [log]
```

**Components:**

```
access-list    = คำสั่งสร้าง ACL
<number>       = 1-99 หรือ 1300-1999
permit|deny    = อนุญาตหรือปฏิเสธ
<source>       = Source IP address
<wildcard>     = Wildcard mask
log            = บันทึก log (optional)
```

---

### Standard ACL Configuration

#### Step 1: Create ACL

**Example 1: Permit Single Host**

```
Router(config)# access-list 10 permit host 192.168.1.10

หรือ:
Router(config)# access-list 10 permit 192.168.1.10 0.0.0.0
```

**Example 2: Permit Network**

```
Router(config)# access-list 10 permit 192.168.1.0 0.0.0.255
```

**Example 3: Deny Network, Permit Others**

```
Router(config)# access-list 10 deny 192.168.1.0 0.0.0.255
Router(config)# access-list 10 permit any
```

**Example 4: Permit Specific Range**

```
! Permit 10.1.1.32 - 10.1.1.63
Router(config)# access-list 10 permit 10.1.1.32 0.0.0.31
Router(config)# access-list 10 deny any
```

#### Step 2: Apply ACL to Interface

**Syntax:**

```
Router(config)# interface <interface-id>
Router(config-if)# ip access-group <acl-number> {in|out}
```

**Example:**

```
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip access-group 10 in
```

---

### Standard ACL Complete Example

**Scenario:**

```
Topology:
[PC1: 192.168.1.10] ─┐
[PC2: 192.168.1.20] ─┼─ [Gi0/0 - R1 - Gi0/1] ─── [Server: 10.1.1.100]
[PC3: 192.168.1.30] ─┘
       (LAN)                                           (Server)

Requirements:
  - PC1 (192.168.1.10): Full access to Server
  - PC2 (192.168.1.20): No access to Server
  - PC3 and others: Full access to Server
```

**Configuration:**

```
! Create ACL
R1(config)# access-list 10 remark === Server Access Control ===
R1(config)# access-list 10 permit host 192.168.1.10
R1(config)# access-list 10 deny host 192.168.1.20
R1(config)# access-list 10 permit any

! Apply ACL (Outbound on Gi0/1 - near destination)
R1(config)# interface gigabitEthernet 0/1
R1(config-if)# ip access-group 10 out
R1(config-if)# end
```

**Verification:**

```
R1# show access-lists
Standard IP access list 10
    10 permit 192.168.1.10 (5 matches)
    20 deny   192.168.1.20 (3 matches)
    30 permit any (50 matches)

R1# show ip interface gigabitEthernet 0/1
GigabitEthernet0/1 is up, line protocol is up
  ...
  Outgoing access list is 10
  Inbound  access list is not set
```

**Testing:**

```
PC1# ping 10.1.1.100
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.1.1.100, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5)  ← PC1 สามารถ ping ได้

PC2# ping 10.1.1.100
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.1.1.100, timeout is 2 seconds:
.....
Success rate is 0 percent (0/5)  ← PC2 ถูก deny
```

---

### Named Standard ACL

**ข้อดี:**

```
✅ ชื่อมีความหมาย
✅ แก้ไข ACEs ได้
✅ Insert/Delete specific ACEs
✅ Resequence ACEs
```

**Syntax:**

```
Router(config)# ip access-list standard <name>
Router(config-std-nacl)# [sequence] {permit|deny} <source> [<wildcard>] [log]
```

**Example:**

```
R1(config)# ip access-list standard BRANCH-FILTER
R1(config-std-nacl)# remark === Allow only Branch network ===
R1(config-std-nacl)# permit 10.1.1.0 0.0.0.255
R1(config-std-nacl)# deny any
R1(config-std-nacl)# exit

R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip access-group BRANCH-FILTER in
```

---

### Editing Named ACLs

**Show Sequence Numbers:**

```
R1# show ip access-lists BRANCH-FILTER
Standard IP access list BRANCH-FILTER
    10 permit 10.1.1.0, wildcard bits 0.0.0.255
    20 deny   any
```

**Delete Specific ACE:**

```
R1(config)# ip access-list standard BRANCH-FILTER
R1(config-std-nacl)# no 20
```

**Insert ACE:**

```
R1(config)# ip access-list standard BRANCH-FILTER
R1(config-std-nacl)# 15 permit 10.2.2.0 0.0.0.255
R1(config-std-nacl)# 25 deny any
```

**Result:**

```
R1# show ip access-lists BRANCH-FILTER
Standard IP access list BRANCH-FILTER
    10 permit 10.1.1.0, wildcard bits 0.0.0.255
    15 permit 10.2.2.0, wildcard bits 0.0.0.255
    25 deny   any
```

**Resequence ACL:**

```
R1(config)# ip access-list resequence BRANCH-FILTER 10 10

Result:
Standard IP access list BRANCH-FILTER
    10 permit 10.1.1.0, wildcard bits 0.0.0.255
    20 permit 10.2.2.0, wildcard bits 0.0.0.255
    30 deny   any
```

---

### Securing VTY Lines with Standard ACL

**Use Case: Restrict Telnet/SSH access to router**

**Scenario:**

```
Management Network: 192.168.1.0/24 เท่านั้นสามารถ SSH เข้า router
```

**Configuration:**

```
! Create ACL
R1(config)# access-list 15 remark === VTY Access ===
R1(config)# access-list 15 permit 192.168.1.0 0.0.0.255
R1(config)# access-list 15 deny any

! Apply to VTY lines
R1(config)# line vty 0 4
R1(config-line)# access-class 15 in
R1(config-line)# transport input ssh
R1(config-line)# exit
```

**หมายเหตุ:**

```
⚠️ ใช้ "access-class" สำหรับ VTY (ไม่ใช่ "ip access-group")
⚠️ Inbound only (ไม่มี outbound บน VTY)
```

**Verification:**

```
R1# show line vty 0 4
     Tty Line Typ     Tx/Rx    A Roty AccO AccI   Uses   Noise  Overruns   Int
*    0    0 VTY              -    -    -   15     -      0       0     0/0    -
     1    1 VTY              -    -    -   15     -      0       0     0/0    -
     2    2 VTY              -    -    -   15     -      0       0     0/0    -
     3    3 VTY              -    -    -   15     -      0       0     0/0    -
     4    4 VTY              -    -    -   15     -      0       0     0/0    -
                                          ↑↑
                                    access-class 15
```

---

### Standard ACL Best Practices

**1. Placement:**

```
✅ วางใกล้ destination
❌ อย่าวางใกล้ source (block traffic มากเกินไป)

Topology:
[Branch] ─── [R1] ─── [R2] ─── [Server]

Goal: Block Branch จาก Server

Correct: ACL บน R2 (ใกล้ Server)
Wrong: ACL บน R1 (block traffic ไป R2 ด้วย)
```

**2. Order:**

```
✅ Specific hosts/networks ก่อน
✅ General rules ทีหลัง

Example:
access-list 10 permit host 10.1.1.1       ← Specific
access-list 10 deny 10.1.1.0 0.0.0.255    ← Less specific
access-list 10 permit any                  ← General
```

**3. Documentation:**

```
✅ ใช้ remark
✅ มีความคิดเห็นใน configuration

access-list 10 remark === Production Servers ===
access-list 10 permit 10.1.1.0 0.0.0.255
access-list 10 remark === Development Servers ===
access-list 10 permit 10.2.2.0 0.0.0.255
```

**4. Implicit Deny:**

```
✅ อย่าลืมว่ามี implicit deny any ท้าย ACL
✅ ถ้าต้องการ allow อื่นๆ ต้อง permit any

Example:
access-list 10 deny host 10.1.1.50
! ต้องมี permit any มิฉะนั้น deny ทั้งหมด!
access-list 10 permit any
```

---

## 5.2 Modify IPv4 ACLs

### Modify Numbered ACLs

**ปัญหา:**

```
❌ ไม่สามารถลบ ACE เฉพาะได้
❌ ไม่สามารถ insert ACE ระหว่างได้
→ ต้องลบทั้ง ACL แล้วสร้างใหม่
```

**การแก้ไข Numbered ACL:**

#### Method 1: Delete and Recreate

```
! Show current ACL
R1# show access-lists 10
Standard IP access list 10
    10 permit 192.168.1.0, wildcard bits 0.0.0.255
    20 deny   192.168.2.0, wildcard bits 0.0.0.255
    30 permit any

! Delete entire ACL
R1(config)# no access-list 10

! Recreate with changes
R1(config)# access-list 10 permit 192.168.1.0 0.0.0.255
R1(config)# access-list 10 permit 192.168.3.0 0.0.0.255
R1(config)# access-list 10 deny 192.168.2.0 0.0.0.255
R1(config)# access-list 10 permit any
```

**⚠️ ข้อระวัง:**

```
- ลบ ACL แล้ว → Traffic ทั้งหมด denied จนกว่าจะสร้าง ACL ใหม่
- ควรทำในช่วง maintenance window
- หรือใช้ Named ACL แทน (แนะนำ)
```

#### Method 2: Convert to Named ACL

```
! Delete numbered ACL
R1(config)# no access-list 10

! Create named ACL
R1(config)# ip access-list standard NETWORK-FILTER
R1(config-std-nacl)# 10 permit 192.168.1.0 0.0.0.255
R1(config-std-nacl)# 20 permit 192.168.3.0 0.0.0.255
R1(config-std-nacl)# 30 deny 192.168.2.0 0.0.0.255
R1(config-std-nacl)# 40 permit any
R1(config-std-nacl)# exit

! Update interface
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip access-group NETWORK-FILTER in
```

---

### Modify Named ACLs

**ข้อดี:**

```
✅ ลบ ACE เฉพาะได้
✅ Insert ACE ได้
✅ Resequence ACEs ได้
✅ ไม่ต้องลบทั้ง ACL
```

#### Add ACE

```
R1(config)# ip access-list standard NETWORK-FILTER
R1(config-std-nacl)# 25 permit 192.168.4.0 0.0.0.255
R1(config-std-nacl)# exit
```

#### Delete ACE

```
R1(config)# ip access-list standard NETWORK-FILTER
R1(config-std-nacl)# no 20
R1(config-std-nacl)# exit
```

#### Resequence

```
! Before resequence
R1# show ip access-lists NETWORK-FILTER
Standard IP access list NETWORK-FILTER
    10 permit 192.168.1.0, wildcard bits 0.0.0.255
    25 permit 192.168.4.0, wildcard bits 0.0.0.255
    30 deny   192.168.2.0, wildcard bits 0.0.0.255
    40 permit any

! Resequence: start at 10, increment by 10
R1(config)# ip access-list resequence NETWORK-FILTER 10 10

! After resequence
R1# show ip access-lists NETWORK-FILTER
Standard IP access list NETWORK-FILTER
    10 permit 192.168.1.0, wildcard bits 0.0.0.255
    20 permit 192.168.4.0, wildcard bits 0.0.0.255
    30 deny   192.168.2.0, wildcard bits 0.0.0.255
    40 permit any
```

---

### ACL Statistics

**View Matches:**

```
R1# show access-lists
Standard IP access list 10
    10 permit 192.168.1.0, wildcard bits 0.0.0.255 (150 matches)
    20 deny   192.168.2.0, wildcard bits 0.0.0.255 (25 matches)
    30 permit any (300 matches)
```

**Clear Counters:**

```
R1# clear access-list counters 10

หรือ clear ทุก ACLs:
R1# clear access-list counters
```

**Enable Logging:**

```
R1(config)# access-list 10 deny 192.168.2.0 0.0.0.255 log
R1(config)# access-list 10 permit any

Syslog messages:
%SEC-6-IPACCESSLOGP: list 10 denied 192.168.2.10 1 packet
```

---

## 5.3 Configure Extended IPv4 ACLs

### Extended ACL Overview

**คำจำกัดความ:**

- กรองตาม: **source, destination, protocol, port**
- ละเอียดกว่า Standard ACL มาก
- Number range: **100-199** และ **2000-2699**

**Syntax:**

```
Router(config)# access-list <number> {permit|deny} <protocol> 
                <source> [<src-port>] <destination> [<dst-port>] [options]
```

**Parameters:**

```
<number>       = 100-199 หรือ 2000-2699
<protocol>     = ip, tcp, udp, icmp, eigrp, ospf, gre, ...
<source>       = Source IP + wildcard
<src-port>     = Source port (optional)
<destination>  = Destination IP + wildcard
<dst-port>     = Destination port (optional)
[options]      = established, log, dscp, precedence, ...
```

---

### Extended ACL Protocols

**Common Protocols:**

```
ip      = All IP protocols
tcp     = TCP
udp     = UDP
icmp    = ICMP (ping, traceroute)
eigrp   = EIGRP routing protocol
ospf    = OSPF routing protocol
gre     = GRE tunneling
esp     = IPsec ESP
ah      = IPsec AH
```

---

### Port Numbers

**Well-Known Ports:**

```
Port    Protocol    Service
------------------------------------------------------------------------
20      TCP         FTP Data
21      TCP         FTP Control
22      TCP         SSH
23      TCP         Telnet
25      TCP         SMTP (Email)
53      TCP/UDP     DNS
67      UDP         DHCP Server
68      UDP         DHCP Client
69      UDP         TFTP
80      TCP         HTTP
110     TCP         POP3
143     TCP         IMAP
161     UDP         SNMP
443     TCP         HTTPS
3389    TCP         RDP
------------------------------------------------------------------------
```

---

### Port Operators

**Operators:**

```
eq      = Equal (เท่ากับ)
        access-list 100 permit tcp any any eq 80

ne      = Not equal (ไม่เท่ากับ)
        access-list 100 deny tcp any any ne 80

gt      = Greater than (มากกว่า)
        access-list 100 permit tcp any any gt 1023

lt      = Less than (น้อยกว่า)
        access-list 100 permit tcp any any lt 1024

range   = Range (ช่วง)
        access-list 100 permit tcp any any range 20 21
```

**ตัวอย่าง:**

```
! Allow HTTP (port 80)
access-list 100 permit tcp any any eq 80
access-list 100 permit tcp any any eq www

! Allow FTP (ports 20-21)
access-list 100 permit tcp any any range 20 21
access-list 100 permit tcp any any range ftp-data ftp

! Allow ephemeral ports (1024-65535)
access-list 100 permit tcp any gt 1023 any

! Deny Telnet
access-list 100 deny tcp any any eq 23
access-list 100 deny tcp any any eq telnet
```

---

### Extended ACL Configuration

#### Example 1: Block Telnet to Specific Server

**Scenario:**

```
Goal: Block Telnet (port 23) จาก 192.168.1.0/24 ไป Server 10.1.1.100
```

**Configuration:**

```
R1(config)# access-list 100 remark === Block Telnet to Server ===
R1(config)# access-list 100 deny tcp 192.168.1.0 0.0.0.255 host 10.1.1.100 eq 23
R1(config)# access-list 100 permit ip any any

R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip access-group 100 in
```

#### Example 2: Allow Only Specific Services

**Scenario:**

```
Goal: Allow เฉพาะ HTTP (80), HTTPS (443), DNS (53) จาก LAN ไป Internet
```

**Configuration:**

```
R1(config)# access-list 110 remark === Internet Access ===
R1(config)# access-list 110 permit tcp 192.168.1.0 0.0.0.255 any eq 80
R1(config)# access-list 110 permit tcp 192.168.1.0 0.0.0.255 any eq 443
R1(config)# access-list 110 permit udp 192.168.1.0 0.0.0.255 any eq 53
R1(config)# access-list 110 deny ip any any

R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip access-group 110 in
```

#### Example 3: Block ICMP Ping

**Scenario:**

```
Goal: Block ping จาก Internet เข้า LAN แต่อนุญาต ping ออก
```

**Configuration:**

```
! Block inbound ping
R1(config)# access-list 120 deny icmp any 192.168.1.0 0.0.0.255 echo
R1(config)# access-list 120 permit icmp any 192.168.1.0 0.0.0.255 echo-reply
R1(config)# access-list 120 permit ip any any

R1(config)# interface gigabitEthernet 0/1
R1(config-if)# ip access-group 120 in
```

**ICMP Types:**

```
echo            = Ping request
echo-reply      = Ping reply
unreachable     = Destination unreachable
time-exceeded   = TTL exceeded
redirect        = ICMP redirect
```

---

### Named Extended ACL

**Syntax:**

```
Router(config)# ip access-list extended <name>
Router(config-ext-nacl)# [sequence] {permit|deny} <protocol> 
                         <source> [<src-port>] <destination> [<dst-port>] [options]
```

**Example:**

```
R1(config)# ip access-list extended WEBSERVER-FILTER
R1(config-ext-nacl)# remark === Allow HTTP and HTTPS only ===
R1(config-ext-nacl)# permit tcp any host 10.1.1.100 eq 80
R1(config-ext-nacl)# permit tcp any host 10.1.1.100 eq 443
R1(config-ext-nacl)# deny ip any host 10.1.1.100
R1(config-ext-nacl)# permit ip any any
R1(config-ext-nacl)# exit

R1(config)# interface gigabitEthernet 0/1
R1(config-if)# ip access-group WEBSERVER-FILTER out
```

---

### Extended ACL with Established

**คำจำกัดความ:**

- **established** keyword = อนุญาต TCP traffic ที่เป็น part ของ established connection
- ตรวจสอบ ACK หรือ RST flag = 1

**Use Case: Allow return traffic only**

**Scenario:**

```
Goal: อนุญาตให้ LAN เริ่ม connection ออก Internet
      แต่ไม่อนุญาต Internet เริ่ม connection เข้า LAN
```

**Configuration:**

```
! Allow outbound connections
R1(config)# access-list 130 permit tcp 192.168.1.0 0.0.0.255 any

! Allow return traffic (established connections)
R1(config)# access-list 131 permit tcp any 192.168.1.0 0.0.0.255 established
R1(config)# access-list 131 deny ip any any

! Apply ACLs
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip access-group 130 in     (LAN side)

R1(config)# interface gigabitEthernet 0/1
R1(config-if)# ip access-group 131 in     (Internet side)
```

**การทำงาน:**

```
LAN → Internet:
  - SYN (new connection) → Permitted (ACL 130)
  - Data (ACK flag set) → Permitted (ACL 130)

Internet → LAN:
  - SYN (new connection) → Denied (ACL 131 - no established)
  - SYN-ACK (reply, ACK flag set) → Permitted (ACL 131 - established)
  - Data (ACK flag set) → Permitted (ACL 131 - established)
```

---

### Extended ACL Complete Example

**Scenario:**

```
Topology:
[LAN: 192.168.1.0/24] ─── [R1] ─── [DMZ: 10.1.1.0/24]
   Gi0/0                           Gi0/1
                                   |
                              [Web: 10.1.1.100]
                              [DNS: 10.1.1.200]

Requirements:
  1. LAN สามารถ access Web server (HTTP/HTTPS)
  2. LAN สามารถ query DNS server
  3. LAN ไม่สามารถ Telnet/SSH ไป DMZ
  4. DMZ ไม่สามารถเริ่ม connection เข้า LAN
  5. Allow return traffic
```

**Configuration:**

```
! ACL สำหรับ LAN → DMZ (Gi0/0 inbound)
R1(config)# ip access-list extended LAN-TO-DMZ
R1(config-ext-nacl)# remark === Allow HTTP/HTTPS to Web ===
R1(config-ext-nacl)# permit tcp 192.168.1.0 0.0.0.255 host 10.1.1.100 eq 80
R1(config-ext-nacl)# permit tcp 192.168.1.0 0.0.0.255 host 10.1.1.100 eq 443
R1(config-ext-nacl)# remark === Allow DNS queries ===
R1(config-ext-nacl)# permit udp 192.168.1.0 0.0.0.255 host 10.1.1.200 eq 53
R1(config-ext-nacl)# remark === Block Telnet/SSH ===
R1(config-ext-nacl)# deny tcp 192.168.1.0 0.0.0.255 10.1.1.0 0.0.0.255 eq 22
R1(config-ext-nacl)# deny tcp 192.168.1.0 0.0.0.255 10.1.1.0 0.0.0.255 eq 23
R1(config-ext-nacl)# remark === Allow other traffic ===
R1(config-ext-nacl)# permit ip 192.168.1.0 0.0.0.255 any
R1(config-ext-nacl)# exit

! ACL สำหรับ DMZ → LAN (Gi0/1 inbound)
R1(config)# ip access-list extended DMZ-TO-LAN
R1(config-ext-nacl)# remark === Allow return traffic only ===
R1(config-ext-nacl)# permit tcp 10.1.1.0 0.0.0.255 192.168.1.0 0.0.0.255 established
R1(config-ext-nacl)# permit udp host 10.1.1.200 eq 53 192.168.1.0 0.0.0.255
R1(config-ext-nacl)# deny ip any any
R1(config-ext-nacl)# exit

! Apply ACLs
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip access-group LAN-TO-DMZ in

R1(config)# interface gigabitEthernet 0/1
R1(config-if)# ip access-group DMZ-TO-LAN in
R1(config-if)# end
```

**Verification:**

```
R1# show access-lists
Extended IP access list LAN-TO-DMZ
    10 permit tcp 192.168.1.0 0.0.0.255 host 10.1.1.100 eq www
    20 permit tcp 192.168.1.0 0.0.0.255 host 10.1.1.100 eq 443
    30 permit udp 192.168.1.0 0.0.0.255 host 10.1.1.200 eq domain
    40 deny tcp 192.168.1.0 0.0.0.255 10.1.1.0 0.0.0.255 eq 22
    50 deny tcp 192.168.1.0 0.0.0.255 10.1.1.0 0.0.0.255 eq telnet
    60 permit ip 192.168.1.0 0.0.0.255 any

Extended IP access list DMZ-TO-LAN
    10 permit tcp 10.1.1.0 0.0.0.255 192.168.1.0 0.0.0.255 established
    20 permit udp host 10.1.1.200 eq domain 192.168.1.0 0.0.0.255
    30 deny ip any any

R1# show ip interface gigabitEthernet 0/0
GigabitEthernet0/0 is up, line protocol is up
  ...
  Outgoing access list is not set
  Inbound  access list is LAN-TO-DMZ
```

---

## 5.4 Troubleshoot ACLs

### Common ACL Problems

#### Problem 1: Wrong ACL Order

**Symptom:**

```
Traffic ที่ควร permit ถูก deny
```

**Cause:**

```
General rule ก่อน specific rule
```

**Example:**

```
! Wrong order
access-list 100 deny tcp any any eq 23
access-list 100 permit tcp host 10.1.1.1 any eq 23  ← ไม่มีทางถึง
access-list 100 permit ip any any

Result: 10.1.1.1 ไม่สามารถ Telnet (match line 1)
```

**Solution:**

```
! Correct order
access-list 100 permit tcp host 10.1.1.1 any eq 23  ← Specific first
access-list 100 deny tcp any any eq 23
access-list 100 permit ip any any
```

#### Problem 2: Missing Permit Statement

**Symptom:**

```
Traffic ทั้งหมดถูก deny
```

**Cause:**

```
Implicit deny any ท้าย ACL
ลืมใส่ permit statement
```

**Example:**

```
! Missing permit
access-list 100 deny tcp any any eq 23
! Implicit deny any ← Block ทุกอย่าง!

Result: Block Telnet + Block all other traffic
```

**Solution:**

```
! Add permit statement
access-list 100 deny tcp any any eq 23
access-list 100 permit ip any any  ← เพิ่ม
```

#### Problem 3: Wrong Interface or Direction

**Symptom:**

```
ACL ไม่ทำงาน
```

**Cause:**

```
- ACL ใช้ interface ผิด
- ACL ใช้ direction ผิด (in vs out)
```

**Verification:**

```
R1# show ip interface gigabitEthernet 0/0
GigabitEthernet0/0 is up, line protocol is up
  ...
  Outgoing access list is 100  ← ตรวจสอบ
  Inbound  access list is not set
```

**Solution:**

```
! Remove from wrong interface/direction
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# no ip access-group 100 out

! Apply to correct interface/direction
R1(config)# interface gigabitEthernet 0/1
R1(config-if)# ip access-group 100 in
```

#### Problem 4: Wildcard Mask Error

**Symptom:**

```
ACL match addresses ผิด
```

**Cause:**

```
ใช้ subnet mask แทน wildcard mask
```

**Example:**

```
! Wrong: ใช้ subnet mask
access-list 100 permit ip 192.168.1.0 255.255.255.0 any
→ Match ผิด!

! Correct: ใช้ wildcard mask
access-list 100 permit ip 192.168.1.0 0.0.0.255 any
```

**Verification:**

```
R1# show access-lists 100
Extended IP access list 100
    10 permit ip 192.168.1.0, wildcard bits 255.255.255.0 any
                                           ↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑
                                         ผิด! (subnet mask)
```

#### Problem 5: Incomplete ACL

**Symptom:**

```
ACL อนุญาต/ปฏิเสธ traffic ไม่ครบ
```

**Cause:**

```
ลืม ACEs บางตัว
```

**Example:**

```
! Incomplete: อนุญาตเฉพาะ HTTP
access-list 100 permit tcp any host 10.1.1.100 eq 80
access-list 100 permit ip any any

! ลืม HTTPS (443)

Result: HTTPS ถูก permit ด้วย (match line 2)
→ ถ้าต้องการ block services อื่น ต้อง deny ก่อน permit any
```

**Solution:**

```
! Complete: Allow เฉพาะ HTTP/HTTPS
access-list 100 permit tcp any host 10.1.1.100 eq 80
access-list 100 permit tcp any host 10.1.1.100 eq 443
access-list 100 deny ip any host 10.1.1.100  ← Block อื่นๆ
access-list 100 permit ip any any
```

#### Problem 6: TCP vs UDP Confusion

**Symptom:**

```
ACL ไม่ match traffic ที่ต้องการ
```

**Cause:**

```
ใช้ protocol ผิด (TCP vs UDP)
```

**Example:**

```
! Wrong: DNS ใช้ UDP แต่ ACL เป็น TCP
access-list 100 permit tcp any host 10.1.1.200 eq 53
→ DNS queries ถูก deny!

! Correct: DNS ใช้ UDP (queries)
access-list 100 permit udp any host 10.1.1.200 eq 53
! DNS zone transfers ใช้ TCP
access-list 100 permit tcp any host 10.1.1.200 eq 53
```

---

### Troubleshooting Workflow

**1. Verify ACL exists:**

```
R1# show access-lists
R1# show ip access-lists
```

**2. Verify ACL applied to interface:**

```
R1# show ip interface gigabitEthernet 0/0
R1# show running-config interface gigabitEthernet 0/0
```

**3. Check ACL order:**

```
R1# show access-lists 100
! ตรวจสอบว่า specific rules ก่อน general rules
```

**4. Test connectivity:**

```
! From client
PC# ping 10.1.1.100
PC# telnet 10.1.1.100
PC# ssh 10.1.1.100

! Extended ping from router
R1# ping
Protocol [ip]:
Target IP address: 10.1.1.100
Repeat count [5]:
Datagram size [100]:
Timeout in seconds [2]:
Extended commands [n]: y
Source address or interface: 192.168.1.10
...
```

**5. Check ACL matches:**

```
R1# show access-lists 100
Extended IP access list 100
    10 deny tcp any host 10.1.1.100 eq telnet (5 matches)  ← ดูว่า match หรือไม่
    20 permit ip any any (100 matches)

! ถ้า matches = 0 → ACL ไม่ match traffic
```

**6. Enable logging:**

```
R1(config)# access-list 100 deny tcp any host 10.1.1.100 eq 23 log
R1(config)# access-list 100 permit ip any any log

! ดู syslog
R1# show logging
```

**7. Use debug (ระวัง: CPU intensive):**

```
R1# debug ip packet 100
! Test traffic
! ดู debug output
R1# undebug all
```

---

### ACL Testing Tools

#### Extended Ping

```
R1# ping
Protocol [ip]:
Target IP address: 10.1.1.100
Repeat count [5]: 10
Datagram size [100]:
Timeout in seconds [2]:
Extended commands [n]: y
Source address or interface: 192.168.1.10
Type of service [0]:
Set DF bit in IP header? [no]:
Validate reply data? [no]:
Data pattern [0xABCD]:
Loose, Strict, Record, Timestamp, Verbose[none]:
Sweep range of sizes [n]:
Type escape sequence to abort.
Sending 10, 100-byte ICMP Echos to 10.1.1.100, timeout is 2 seconds:
Packet sent with a source address of 192.168.1.10
!!!!!!!!!!
Success rate is 100 percent (10/10), round-trip min/avg/max = 1/2/4 ms
```

#### Telnet/SSH Test

```
! Test Telnet
PC# telnet 10.1.1.100

! Test SSH
PC# ssh -l admin 10.1.1.100

! Test specific port
PC# telnet 10.1.1.100 80
```

#### Packet Tracer Simulation

```
! ใช้ Packet Tracer's Simulation Mode
1. Enter Simulation Mode
2. Add filters (ICMP, TCP, etc.)
3. Send packet
4. ดู packet flow และ ACL processing
```

---

## 5.5 ACL Best Practices Summary

### Design Best Practices

**1. Planning:**

```
✅ วางแผน security policy ก่อน
✅ ระบุ traffic ที่ต้องการ filter
✅ เลือก ACL type ที่เหมาะสม
✅ วางแผน placement (interface, direction)
```

**2. ACL Type Selection:**

```
Standard ACL:
  - Simple filtering (source IP only)
  - Placement: ใกล้ destination

Extended ACL:
  - Complex filtering (src, dst, protocol, port)
  - Placement: ใกล้ source
```

**3. Ordering:**

```
✅ Specific rules ก่อน
✅ General rules ทีหลัง
✅ Permit specific traffic
✅ Deny general traffic
✅ Permit any (ถ้าต้องการ)
```

**4. Documentation:**

```
✅ ใช้ named ACLs (มีชื่อมีความหมาย)
✅ ใช้ remark
✅ มี comments ใน configuration
✅ เก็บ external documentation
```

---

### Implementation Best Practices

**1. Testing:**

```
✅ Test บน lab ก่อน production
✅ ใช้ extended ping
✅ Test ทุก scenarios
✅ Verify ด้วย show commands
```

**2. Deployment:**

```
✅ Deploy ในช่วง maintenance window
✅ มี rollback plan
✅ Backup configuration ก่อน
✅ แจ้ง users ล่วงหน้า
```

**3. Monitoring:**

```
✅ ใช้ logging
✅ Monitor matches
✅ Review logs เป็นประจำ
✅ Adjust ACLs ตามความจำเป็น
```

---

### Configuration Template

**Standard ACL Template:**

```
! Named Standard ACL
ip access-list standard <name>
 remark === Description ===
 [sequence] permit <specific-source> [<wildcard>]
 [sequence] deny <specific-source> [<wildcard>]
 [sequence] permit any
 exit

! Apply to interface
interface <interface-id>
 ip access-group <name> {in|out}

! Apply to VTY
line vty 0 4
 access-class <name> in
```

**Extended ACL Template:**

```
! Named Extended ACL
ip access-list extended <name>
 remark === Description ===
 [sequence] permit <protocol> <source> [<src-port>] <destination> [<dst-port>]
 [sequence] deny <protocol> <source> [<src-port>] <destination> [<dst-port>]
 [sequence] permit ip any any
 exit

! Apply to interface
interface <interface-id>
 ip access-group <name> {in|out}
```

---

### Common ACL Patterns

**Pattern 1: Block Specific Host**

```
access-list 10 deny host 192.168.1.50
access-list 10 permit any
```

**Pattern 2: Allow Management Network Only**

```
access-list 15 permit 192.168.1.0 0.0.0.255
access-list 15 deny any
```

**Pattern 3: Block Telnet, Allow Others**

```
access-list 100 deny tcp any any eq 23
access-list 100 permit ip any any
```

**Pattern 4: Allow Specific Services to Server**

```
access-list 110 permit tcp any host 10.1.1.100 eq 80
access-list 110 permit tcp any host 10.1.1.100 eq 443
access-list 110 deny ip any host 10.1.1.100
access-list 110 permit ip any any
```

**Pattern 5: Allow Outbound, Block Inbound (except return traffic)**

```
! Outbound ACL (LAN side inbound)
access-list 120 permit tcp 192.168.1.0 0.0.0.255 any
access-list 120 permit udp 192.168.1.0 0.0.0.255 any
access-list 120 permit icmp 192.168.1.0 0.0.0.255 any

! Inbound ACL (Internet side inbound)
access-list 121 permit tcp any 192.168.1.0 0.0.0.255 established
access-list 121 permit udp any eq 53 192.168.1.0 0.0.0.255
access-list 121 permit icmp any 192.168.1.0 0.0.0.255 echo-reply
access-list 121 deny ip any any
```

---

## Summary (สรุป)

Module 5 นี้เราได้เรียนรู้:

1. ✅ **Standard ACL Configuration**:
    
    - Syntax: `access-list <1-99|1300-1999> {permit|deny} <source> [<wildcard>]`
    - กรอง source IP only
    - Placement: ใกล้ destination
    - VTY access: `access-class`
2. ✅ **Named Standard ACL**:
    
    - `ip access-list standard <name>`
    - Edit, Insert, Delete ACEs
    - Resequence: `ip access-list resequence`
3. ✅ **Extended ACL Configuration**:
    
    - Syntax: `access-list <100-199|2000-2699> {permit|deny} <protocol> <src> <dst>`
    - กรอง src, dst, protocol, port
    - Placement: ใกล้ source
    - Port operators: eq, ne, gt, lt, range
    - Established keyword
4. ✅ **Named Extended ACL**:
    
    - `ip access-list extended <name>`
    - Flexible editing
    - Professional approach
5. ✅ **ACL Modification**:
    
    - Numbered: ลบทั้ง ACL แล้วสร้างใหม่
    - Named: Edit individual ACEs
    - Resequence ACEs
    - Clear counters
6. ✅ **Troubleshooting**:
    
    - Wrong order
    - Missing permit
    - Wrong interface/direction
    - Wildcard mask errors
    - TCP vs UDP confusion
    - Debug และ verification

**สิ่งสำคัญที่ต้องจำ:**

- Standard ACL: 1-99, 1300-1999 → Source only → Near destination
- Extended ACL: 100-199, 2000-2699 → Src/Dst/Port → Near source
- Named ACL แนะนำกว่า Numbered (edit ได้)
- Order: Specific → General
- Implicit deny any ท้ายทุก ACL
- VTY ใช้ `access-class` ไม่ใช่ `ip access-group`
- Established = Allow return traffic (ACK/RST flags)
- Test ก่อน deploy เสมอ!

**Configuration Checklist:**

```
☐ วางแผน security policy
☐ เลือก ACL type (Standard/Extended)
☐ เขียน ACEs (Specific → General)
☐ ใส่ remark/documentation
☐ เลือก placement (interface, direction)
☐ Test บน lab
☐ Backup configuration
☐ Deploy
☐ Verify
☐ Monitor
```

**Verification Commands:**

```
show access-lists [<number|name>]
show ip access-lists [<number|name>]
show ip interface <interface-id>
show running-config | include access-list
show running-config | section access-list
show line vty 0 4
clear access-list counters [<number|name>]
```

**Next Module:** Module 6 - NAT for IPv4

---

**[ไฟล์ Module 5 สมบูรณ์แล้ว!]**