# CCNA 2: Module 11 - Switch Security Configuration

## การ Configure ความปลอดภัย Switch

---

## สารบัญ

1. [วัตถุประสงค์ของ Module](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#%E0%B8%A7%E0%B8%B1%E0%B8%95%E0%B8%96%E0%B8%B8%E0%B8%9B%E0%B8%A3%E0%B8%B0%E0%B8%AA%E0%B8%87%E0%B8%84%E0%B9%8C%E0%B8%82%E0%B8%AD%E0%B8%87-module)
2. [Implement Port Security](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#1-implement-port-security)
3. [Mitigate VLAN Attacks](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#2-mitigate-vlan-attacks)
4. [Mitigate DHCP Attacks](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#3-mitigate-dhcp-attacks)
5. [Mitigate ARP Attacks](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#4-mitigate-arp-attacks)
6. [Mitigate STP Attacks](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#5-mitigate-stp-attacks)
7. [สรุป](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#%E0%B8%AA%E0%B8%A3%E0%B8%B8%E0%B8%9B)

---

## วัตถุประสงค์ของ Module

เมื่อจบ Module นี้ คุณจะสามารถ:

- ✅ Configure port security เพื่อป้องกัน MAC address table attacks
- ✅ Configure switch เพื่อป้องกัน VLAN attacks
- ✅ Configure DHCP snooping เพื่อป้องกัน DHCP attacks
- ✅ Configure Dynamic ARP Inspection เพื่อป้องกัน ARP attacks
- ✅ Configure PortFast และ BPDU Guard เพื่อป้องกัน STP attacks
- ✅ Verify และ troubleshoot security configurations

---

## 1. Implement Port Security

### การ Configure Port Security

### 1.1 Secure Unused Ports

**ความสำคัญของการรักษาความปลอดภัย Unused Ports:**

```
ปัญหา:
- Switch ports เปิดใช้งาน by default
- Unused ports = ช่องทางสำหรับ unauthorized access
- Attacker อาจเสียบ cable เข้า wall jack
- Gain network access ได้ง่าย

Solution:
Shutdown unused ports!
```

**Configuration:**

```cisco
! Shutdown unused port เดียว
Switch(config)# interface FastEthernet0/5
Switch(config-if)# shutdown
Switch(config-if)# description "Unused Port"

! Shutdown multiple ports ด้วย range
Switch(config)# interface range FastEthernet0/10-24
Switch(config-if-range)# shutdown
Switch(config-if-range)# description "Unused Ports"

! Shutdown Gigabit ports
Switch(config)# interface range GigabitEthernet0/1-2
Switch(config-if-range)# shutdown

! Verify
Switch# show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol
FastEthernet0/1        unassigned      YES unset  up                    up
FastEthernet0/5        unassigned      YES unset  administratively down down
FastEthernet0/10       unassigned      YES unset  administratively down down
...
```

**Best Practice for Unused Ports:**

```cisco
! Complete configuration for unused ports
Switch(config)# interface range FastEthernet0/10-24
Switch(config-if-range)# description "Unused - Secured"

! Shutdown
Switch(config-if-range)# shutdown

! Assign to unused VLAN
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 999
! VLAN 999 = unused/black hole VLAN

! Disable DTP
Switch(config-if-range)# switchport nonegotiate
```

### 1.2 Port Security Overview

**Port Security คืออะไร:**

```
Security feature ที่ควบคุมว่า MAC addresses ใดสามารถใช้ port ได้

Benefits:
✅ Prevent MAC flooding attacks
✅ Prevent unauthorized access
✅ Control device connectivity
✅ Track violations

Limitations:
❌ ต้อง configure manually
❌ ไม่ scale ดีกับ dynamic environments
❌ ต้องมี maintenance
```

**Port Security Requirements:**

```
Port Security ทำงานได้เฉพาะ:
✓ Access ports
✓ Manually configured trunk ports

ไม่ทำงานกับ:
✗ Dynamic auto/desirable ports (default)
✗ DTP-enabled ports

ดังนั้นต้อง configure:
switchport mode access
ก่อนเสมอ!
```

### 1.3 Enable Port Security

**Basic Configuration:**

```cisco
! Step 1: Configure as access port
Switch(config)# interface FastEthernet0/1
Switch(config-if)# switchport mode access

! Step 2: Enable port security
Switch(config-if)# switchport port-security

! Verify
Switch# show port-security interface FastEthernet0/1

Port Security              : Enabled
Port Status                : Secure-up
Violation Mode             : Shutdown
Aging Time                 : 0 mins
Aging Type                 : Absolute
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 1
Total MAC Addresses        : 0
Configured MAC Addresses   : 0
Sticky MAC Addresses       : 0
Last Source Address:Vlan   : 0000.0000.0000:0
Security Violation Count   : 0
```

**Default Port Security Settings:**

```
เมื่อ enable port security (ไม่ระบุ parameters):

Maximum MAC addresses: 1
Violation mode: Shutdown
MAC learning: Dynamic
Aging: Disabled

หมายความว่า:
- อนุญาตแค่ 1 MAC address
- ถ้า violation → shutdown port
- เรียนรู้ MAC dynamically
- MAC ไม่ expire
```

### 1.4 Secure MAC Addresses

**Three Types of Secure MAC Addresses:**

**1. Static Secure MAC Addresses**

```cisco
! Manually configured
Switch(config-if)# switchport port-security mac-address 0011.2233.4455

Characteristics:
✓ Saved in running-config
✓ เพิ่มใน MAC table
✓ Persist across reboots ถ้า save config
✓ Most secure
✗ Manual configuration required
```

**Example:**

```cisco
Switch(config)# interface FastEthernet0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security mac-address 0011.2233.4455

! Add multiple MACs
Switch(config-if)# switchport port-security maximum 3
Switch(config-if)# switchport port-security mac-address 0011.2233.4455
Switch(config-if)# switchport port-security mac-address 0022.3344.5566
Switch(config-if)# switchport port-security mac-address 0033.4455.6677

! Verify
Switch# show port-security address

Secure Mac Address Table
-------------------------------------------------------------------
Vlan    Mac Address       Type                Ports   Remaining Time
                                                          (mins)
----    -----------       ----                -----   --------------
   1    0011.2233.4455    SecureConfigured    Fa0/1        -
   1    0022.3344.5566    SecureConfigured    Fa0/1        -
   1    0033.4455.6677    SecureConfigured    Fa0/1        -
-------------------------------------------------------------------
Total Addresses in System (excluding one mac per port)     : 0
Max Addresses limit in System (excluding one mac per port) : 8320
```

**2. Dynamic Secure MAC Addresses**

```cisco
! Learned automatically
! Default behavior when port security enabled

Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
! ไม่ระบุ MAC address

Characteristics:
✓ Learned automatically
✓ เพิ่มใน MAC table
✗ NOT saved in running-config
✗ Lost when switch reboots
✗ Lost when interface goes down
```

**Example:**

```cisco
! Enable port security with dynamic learning
Switch(config)# interface FastEthernet0/2
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security

! After traffic
Switch# show port-security address

Secure Mac Address Table
-------------------------------------------------------------------
Vlan    Mac Address       Type                Ports   Remaining Time
                                                          (mins)
----    -----------       ----                -----   --------------
   1    0044.5566.7788    SecureDynamic       Fa0/2        -
-------------------------------------------------------------------
```

**3. Sticky Secure MAC Addresses**

```cisco
! Learned automatically AND saved to config
Switch(config-if)# switchport port-security mac-address sticky

Characteristics:
✓ Learned automatically
✓ Converted to "sticky" immediately
✓ Saved in running-config
✓ Persist across reboots (if saved)
✓ Best of both worlds!
✗ Need to save config
```

**Example:**

```cisco
! Configure sticky MAC learning
Switch(config)# interface FastEthernet0/3
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security mac-address sticky

! After traffic, check running-config
Switch# show running-config interface FastEthernet0/3

interface FastEthernet0/3
 switchport mode access
 switchport port-security maximum 1
 switchport port-security
 switchport port-security mac-address sticky
 switchport port-security mac-address sticky 0055.6677.8899
!

! Verify
Switch# show port-security address

Secure Mac Address Table
-------------------------------------------------------------------
Vlan    Mac Address       Type                Ports   Remaining Time
                                                          (mins)
----    -----------       ----                -----   --------------
   1    0055.6677.8899    SecureSticky        Fa0/3        -
-------------------------------------------------------------------

! Save to NVRAM
Switch# copy running-config startup-config
```

**Sticky MAC Workflow:**

```
1. Enable sticky learning
   switchport port-security mac-address sticky

2. Device connects
   
3. Switch learns MAC address
   
4. MAC automatically added to running-config
   Type: SecureSticky

5. Save configuration
   copy run start

6. MAC persists across reboot
```

**Use Cases:**

```
Static MACs:
- Critical devices (servers, routers)
- Known, fixed infrastructure
- High-security requirements
- Small number of devices

Dynamic MACs:
- Guest ports
- Temporary connections
- Testing environments
- Low-security requirements

Sticky MACs: (Most Common)
- Office desktops
- IP phones
- Printers
- IoT devices
- Large deployments
- Balance of security and convenience
```

### 1.5 Port Security Maximum

**Configure Maximum MAC Addresses:**

```cisco
! Set maximum
Switch(config-if)# switchport port-security maximum <1-8192>

! Default: 1
! Range: 1-8192 (varies by platform)
```

**Examples:**

```cisco
! Single device (default)
Switch(config-if)# switchport port-security maximum 1

! Desktop + IP Phone
Switch(config-if)# switchport port-security maximum 2

! Desktop + VMs
Switch(config-if)# switchport port-security maximum 3

! Small hub/switch (not recommended!)
Switch(config-if)# switchport port-security maximum 10
```

**Complete Configuration Example:**

```cisco
! Desktop with IP Phone
Switch(config)# interface FastEthernet0/4
Switch(config-if)# description "User Desk - IP Phone"
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10

! Port security
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximum 2
Switch(config-if)# switchport port-security mac-address sticky

! Voice VLAN (for IP Phone)
Switch(config-if)# switchport voice vlan 20
```

**Mixed Configuration:**

```cisco
! Manual + Sticky + Maximum
Switch(config)# interface FastEthernet0/5
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security

! Manually add printer MAC
Switch(config-if)# switchport port-security mac-address 00AA.BBCC.DDEE

! Allow 1 more device (sticky)
Switch(config-if)# switchport port-security maximum 2
Switch(config-if)# switchport port-security mac-address sticky

Result:
- 1 MAC: Manual (printer)
- 1 MAC: Sticky (learned)
- Total: 2 MACs allowed
```

### 1.6 Port Security Violation Modes

**Three Violation Modes:**

```
ถ้า MAC ที่ unauthorized พยายามใช้ port:

1. Shutdown (default)
2. Restrict
3. Protect
```

**Configuration:**

```cisco
Switch(config-if)# switchport port-security violation {shutdown | restrict | protect}
```

**Mode 1: Shutdown (Default)**

```
Action เมื่อ violation:
✓ Port → err-disabled state
✓ LED ปิด
✓ Log syslog message
✓ SNMP trap
✓ Increment violation counter

Result:
- Port down completely
- ต้อง manual recovery
- Most secure
- Common for production
```

**Example:**

```cisco
Switch(config)# interface FastEthernet0/6
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximum 1
Switch(config-if)# switchport port-security violation shutdown

! After violation
Switch# show port-security interface FastEthernet0/6

Port Security              : Enabled
Port Status                : Secure-shutdown  ← err-disabled
Violation Mode             : Shutdown
Aging Time                 : 0 mins
Aging Type                 : Absolute
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 1
Total MAC Addresses        : 1
Configured MAC Addresses   : 0
Sticky MAC Addresses       : 0
Last Source Address:Vlan   : 0066.7788.9900:1
Security Violation Count   : 1  ← Incremented

! Check interface status
Switch# show interface FastEthernet0/6
FastEthernet0/6 is down, line protocol is down (err-disabled)
  Hardware is Lance, address is 0001.c79a.4506
  (output omitted)

! Recovery
Switch(config)# interface FastEthernet0/6
Switch(config-if)# shutdown
Switch(config-if)# no shutdown
```

**Mode 2: Restrict**

```
Action เมื่อ violation:
✓ Drop frames จาก unauthorized MAC
✓ Port ยัง up
✓ Authorized traffic ยังทำงาน
✓ Log syslog message
✓ SNMP trap
✓ Increment violation counter

Result:
- Port ยังใช้งานได้
- Drop เฉพาะ bad traffic
- Moderate security
- Good for monitoring
```

**Example:**

```cisco
Switch(config)# interface FastEthernet0/7
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximum 2
Switch(config-if)# switchport port-security violation restrict

! After violation (3rd MAC appears)
Switch# show port-security interface FastEthernet0/7

Port Security              : Enabled
Port Status                : Secure-up  ← Still up!
Violation Mode             : Restrict
Maximum MAC Addresses      : 2
Total MAC Addresses        : 2
Security Violation Count   : 5  ← Counting violations

! Syslog message
%PORT_SECURITY-2-PSECURE_VIOLATION: Security violation occurred, 
caused by MAC address 0077.8899.aabb on port FastEthernet0/7.
```

**Mode 3: Protect**

```
Action เมื่อ violation:
✓ Drop frames จาก unauthorized MAC
✓ Port ยัง up
✓ Authorized traffic ยังทำงาน
✗ NO syslog message
✗ NO SNMP trap
✗ NO violation counter

Result:
- Silent drop
- Port ยังใช้งานได้
- No alerts
- Lowest security
- Performance-focused
```

**Example:**

```cisco
Switch(config)# interface FastEthernet0/8
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximum 2
Switch(config-if)# switchport port-security violation protect

! After violation
Switch# show port-security interface FastEthernet0/8

Port Security              : Enabled
Port Status                : Secure-up
Violation Mode             : Protect
Maximum MAC Addresses      : 2
Total MAC Addresses        : 2
Security Violation Count   : 0  ← NOT incremented

! No log messages
! Traffic from 3rd MAC silently dropped
```

**Comparison Table:**

```
┌─────────────┬──────────┬──────────┬─────────┐
│ Action      │ Shutdown │ Restrict │ Protect │
├─────────────┼──────────┼──────────┼─────────┤
│ Drop Traffic│ Yes      │ Yes      │ Yes     │
│ Shutdown    │ Yes      │ No       │ No      │
│ Log Message │ Yes      │ Yes      │ No      │
│ SNMP Trap   │ Yes      │ Yes      │ No      │
│ Counter++   │ Yes      │ Yes      │ No      │
│ Security    │ High     │ Medium   │ Low     │
│ Common Use  │ Prod     │ Monitor  │ Special │
└─────────────┴──────────┴──────────┴─────────┘
```

### 1.7 Port Security Aging

**MAC Address Aging:**

```
Purpose:
- Remove old/unused MAC addresses automatically
- Allow new devices without manual intervention
- Flexibility vs Security trade-off
```

**Aging Configuration:**

```cisco
! Enable aging
Switch(config-if)# switchport port-security aging time <minutes>
Switch(config-if)# switchport port-security aging type {absolute | inactivity}
Switch(config-if)# switchport port-security aging static
```

**Aging Types:**

**1. Absolute Aging**

```
MAC removed after X minutes
ไม่ว่าจะมี traffic หรือไม่

Example:
Aging time: 60 minutes

Timeline:
0:00  - MAC learned
0:30  - Traffic (still active)
1:00  - 60 minutes passed
      → MAC removed automatically
```

**Configuration:**

```cisco
Switch(config-if)# switchport port-security aging time 60
Switch(config-if)# switchport port-security aging type absolute
```

**2. Inactivity Aging (Recommended)**

```
MAC removed after X minutes of NO traffic
ถ้ามี traffic → reset timer

Example:
Aging time: 120 minutes

Timeline:
0:00  - MAC learned
1:00  - Traffic → timer reset to 0
2:00  - Traffic → timer reset to 0
2:30  - No traffic...
4:30  - 120 minutes of inactivity
      → MAC removed

Active device: Never removed
Inactive device: Removed after 120 min
```

**Configuration:**

```cisco
Switch(config-if)# switchport port-security aging time 120
Switch(config-if)# switchport port-security aging type inactivity
```

**Aging Static Addresses:**

```
By default: Static MACs ไม่ age out

Enable aging for static MACs:
Switch(config-if)# switchport port-security aging static
```

**Complete Aging Example:**

```cisco
! Guest port with aging
Switch(config)# interface FastEthernet0/9
Switch(config-if)# description "Guest Port"
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 50

! Port security with aging
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximum 1
Switch(config-if)# switchport port-security mac-address sticky
Switch(config-if)# switchport port-security violation restrict

! Inactivity aging - 2 hours
Switch(config-if)# switchport port-security aging time 120
Switch(config-if)# switchport port-security aging type inactivity

! Verify
Switch# show port-security interface FastEthernet0/9

Port Security              : Enabled
Port Status                : Secure-up
Violation Mode             : Restrict
Aging Time                 : 120 mins
Aging Type                 : Inactivity
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 1
Total MAC Addresses        : 1
Configured MAC Addresses   : 0
Sticky MAC Addresses       : 1
Last Source Address:Vlan   : 0088.9900.aabb:50
Security Violation Count   : 0
```

**Use Cases:**

```
No Aging:
- Office desktops
- Permanent devices
- High security areas

Absolute Aging:
- Time-limited access
- Rental spaces
- Temporary workstations

Inactivity Aging: (Most Common)
- Guest ports
- Conference rooms
- Hot-desking areas
- Flexible workspaces
```

### 1.8 Err-Disabled Recovery

**Err-Disabled State:**

```
เมื่อ port shutdown จาก violation:
- Port status: err-disabled
- Port down
- LED ปิด
- ต้อง manual recovery (by default)
```

**Manual Recovery:**

```cisco
! Check err-disabled port
Switch# show interface status err-disabled

Port      Name               Status       Reason
Fa0/6                        err-disabled psecure-violation

! Manual recovery
Switch(config)# interface FastEthernet0/6
Switch(config-if)# shutdown
Switch(config-if)# no shutdown

! Must use BOTH commands!
! "no shutdown" alone is not enough!
```

**Automatic Recovery:**

```cisco
! Enable auto-recovery
Switch(config)# errdisable recovery cause psecure-violation

! Set recovery interval (default: 300 seconds)
Switch(config)# errdisable recovery interval 600
! 600 seconds = 10 minutes

! Verify
Switch# show errdisable recovery

ErrDisable Reason            Timer Status
-----------------            --------------
psecure-violation            Enabled
...

Timer interval: 600 seconds

Interfaces that will be enabled at the next timeout:
Interface    Errdisable reason    Time left(sec)
---------    -----------------    --------------
Fa0/6        psecure-violation         425
```

**Complete Err-Disable Configuration:**

```cisco
! Global configuration
Switch(config)# errdisable recovery cause psecure-violation
Switch(config)# errdisable recovery interval 300

! Per-interface
Switch(config)# interface FastEthernet0/10
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximum 1
Switch(config-if)# switchport port-security violation shutdown

Scenario:
1. Violation occurs → Port err-disabled
2. Wait 300 seconds (5 minutes)
3. Port automatically re-enabled
4. Ready for authorized device
```

**Best Practices:**

```
Manual Recovery:
✅ High-security environments
✅ Investigate each violation
✅ Controlled changes
✅ Small networks

Auto Recovery:
✅ Large networks
✅ Self-service areas
✅ Minimize help desk calls
✅ Known-good recovery time
```

### 1.9 Verify Port Security

**Verification Commands:**

**1. Show Port Security (All Interfaces)**

```cisco
Switch# show port-security

Secure Port  MaxSecureAddr  CurrentAddr  SecurityViolation  Security Action
                (Count)       (Count)          (Count)
---------------------------------------------------------------------------
    Fa0/1              1              1                 0            Shutdown
    Fa0/2              2              2                 3            Restrict
    Fa0/3              1              1                 0            Shutdown
    Fa0/4              2              1                 0            Shutdown
---------------------------------------------------------------------------
Total Addresses in System (excluding one mac per port)     : 3
Max Addresses limit in System (excluding one mac per port) : 8320
```

**2. Show Port Security Interface (Specific)**

```cisco
Switch# show port-security interface FastEthernet0/1

Port Security              : Enabled
Port Status                : Secure-up
Violation Mode             : Shutdown
Aging Time                 : 120 mins
Aging Type                 : Inactivity
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 2
Total MAC Addresses        : 1
Configured MAC Addresses   : 0
Sticky MAC Addresses       : 1
Last Source Address:Vlan   : 0011.2233.4455:1
Security Violation Count   : 0
```

**3. Show Port Security Address (MAC Addresses)**

```cisco
Switch# show port-security address

Secure Mac Address Table
-------------------------------------------------------------------
Vlan    Mac Address       Type                Ports   Remaining Time
                                                          (mins)
----    -----------       ----                -----   --------------
   1    0011.2233.4455    SecureSticky        Fa0/1        -
   1    0022.3344.5566    SecureConfigured    Fa0/2        -
   1    0033.4455.6677    SecureDynamic       Fa0/2        -
  10    0044.5566.7788    SecureSticky        Fa0/3       85
-------------------------------------------------------------------
Total Addresses in System (excluding one mac per port)     : 3
Max Addresses limit in System (excluding one mac per port) : 8320

! Filter by interface
Switch# show port-security address interface FastEthernet0/1
```

**4. Show Running-Config (Configuration)**

```cisco
Switch# show running-config interface FastEthernet0/1

interface FastEthernet0/1
 description "User Port - Sticky MAC"
 switchport mode access
 switchport access vlan 10
 switchport port-security maximum 2
 switchport port-security
 switchport port-security violation restrict
 switchport port-security aging time 120
 switchport port-security aging type inactivity
 switchport port-security mac-address sticky
 switchport port-security mac-address sticky 0011.2233.4455
!
```

**5. Show Interface Status**

```cisco
Switch# show interface status

Port      Name               Status       Vlan       Duplex  Speed Type
Fa0/1     User Port          connected    10         a-full  a-100 10/100BaseTX
Fa0/6     Violation Port     err-disabled 1          auto    auto  10/100BaseTX
Fa0/10    Unused             disabled     1          auto    auto  10/100BaseTX

Switch# show interface status err-disabled

Port      Name               Status       Reason
Fa0/6     Violation Port     err-disabled psecure-violation
```

### 1.10 Troubleshooting Port Security

**Common Issues:**

**Issue 1: Port Security Command Rejected**

```
Symptom:
Switch(config-if)# switchport port-security
Command rejected: An interface whose trunk...

Cause:
Port is in dynamic auto/desirable mode

Solution:
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
```

**Issue 2: Port Immediately Err-Disabled**

```
Symptom:
Port goes err-disabled หลัง enable

Cause:
- มี devices มากกว่า maximum
- MAC address ไม่ตรงกับ configured

Troubleshooting:
1. Check current devices
   show mac address-table interface <int>

2. Check port security
   show port-security interface <int>

3. Check violation count
   Security Violation Count: X

Solution:
- Disconnect extra devices
- Increase maximum
- Update MAC addresses
- Recover port:
  shutdown / no shutdown
```

**Issue 3: Sticky MACs Not Saving**

```
Symptom:
Sticky MACs lost after reload

Cause:
Running-config not saved to startup

Solution:
Switch# copy running-config startup-config
Or
Switch# write memory
```

**Issue 4: Port Security Not Learning MAC**

```
Symptom:
Total MAC Addresses: 0

Cause:
- No traffic from device
- Interface down
- Maximum reached

Solution:
1. Generate traffic (ping)
2. Check interface status
   show interface <int>
3. Check maximum
   show port-security interface <int>
```

**Issue 5: Aging Not Working**

```
Symptom:
MACs not aging out

Check:
1. Aging enabled?
   show port-security interface <int>
   Aging Time: 0 mins ← Disabled!

2. Type correct?
   Aging Type: Absolute or Inactivity

3. Timer running?
   show port-security address
   Remaining Time column

Solution:
Configure aging properly:
switchport port-security aging time <min>
switchport port-security aging type inactivity
```

**Debug Commands:**

```cisco
! Enable debugging (use carefully!)
Switch# debug port-security

Port Security Port Security-6-PSECURE_STICKY_MAC_UPDATE: 
Security MAC address 0011.2233.4455 was updated on FastEthernet0/1

! Disable debugging
Switch# no debug all
Or
Switch# undebug all
```

---

## 2. Mitigate VLAN Attacks

### การป้องกัน VLAN Attacks

### 2.1 VLAN Attack Review

**VLAN Attacks ประเภทหลัก:**

```
1. Switch Spoofing (DTP Attack)
   - Attacker ปลอมตัวเป็น switch
   - สร้าง trunk link
   - เข้าถึงทุก VLAN

2. Double Tagging
   - ส่ง frames ที่มี 2 VLAN tags
   - Bypass VLAN segmentation
   - One-way attack

3. VLAN Hopping
   - กระโดดข้าม VLAN boundaries
   - Unauthorized VLAN access
```

### 2.2 Mitigate Switch Spoofing

**DTP (Dynamic Trunking Protocol) Review:**

```
DTP:
- Cisco proprietary
- Auto-negotiate trunk links
- Default: enabled

Modes:
- Dynamic Auto (default)
- Dynamic Desirable
- Trunk
- Access

Problem:
Attacker sends DTP → Creates trunk → Access all VLANs
```

**Mitigation Strategy:**

```
1. Disable DTP on access ports
2. Manually configure trunk ports
3. Disable DTP on trunk ports (optional)
```

**Configuration:**

**Access Ports (End Devices):**

```cisco
! Disable DTP and force access mode
Switch(config)# interface FastEthernet0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport nonegotiate

! Or using range
Switch(config)# interface range FastEthernet0/1-24
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport nonegotiate
```

**Trunk Ports (Switch-to-Switch):**

```cisco
! Manually configure trunk
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# description "Trunk to SW2"
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport nonegotiate

! Restrict allowed VLANs
Switch(config-if)# switchport trunk allowed vlan 10,20,30,40

! Set native VLAN to unused VLAN
Switch(config-if)# switchport trunk native vlan 999
```

**Verification:**

```cisco
! Check DTP status
Switch# show dtp interface FastEthernet0/1

DTP information for FastEthernet0/1:
  TOS/TAS/TNS:                              ACCESS/OFF/NONEGOTIATE
  TOT/TAT/TNT:                              NATIVE/OFF/NONEGOTIATE
  Neighbor address 1:                       000000000000
  Neighbor address 2:                       000000000000

! nonegotiate = DTP disabled ✓

! Check all interfaces
Switch# show interface status

Port      Name               Status       Vlan       Duplex  Speed Type
Fa0/1     Access Port        connected    10         a-full  a-100 10/100BaseTX
Gi0/1     Trunk to SW2       connected    trunk      a-full  a-1000 1000BaseSX

! Show trunks
Switch# show interface trunk

Port        Mode             Encapsulation  Status        Native vlan
Gi0/1       on               802.1q         trunking      999

Port        Vlans allowed on trunk
Gi0/1       10,20,30,40

Port        Vlans allowed and active in management domain
Gi0/1       10,20,30,40

Port        Vlans in spanning tree forwarding state and not pruned
Gi0/1       10,20,30,40
```

### 2.3 Mitigate Double Tagging

**Double Tagging Attack Review:**

```
Requirements:
1. Attacker in native VLAN
2. Native VLAN tagged packets
3. Target VLAN exists

Attack:
[Outer: VLAN 1][Inner: VLAN 20][Data]
     ↓
First switch strips outer tag (native)
     ↓
[Inner: VLAN 20][Data]
     ↓
Second switch delivers to VLAN 20
```

**Mitigation Strategy:**

```
1. Change native VLAN to unused VLAN
2. Don't use VLAN 1
3. Tag native VLAN (optional)
4. Prune unused VLANs from trunks
```

**Configuration:**

```cisco
! Configure trunk with unused native VLAN
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport nonegotiate

! Use VLAN 999 as native (unused)
Switch(config-if)# switchport trunk native vlan 999

! Restrict allowed VLANs
Switch(config-if)# switchport trunk allowed vlan 10,20,30,40,999

! Optional: Tag native VLAN
Switch(config)# vlan dot1q tag native

! Create unused VLAN
Switch(config)# vlan 999
Switch(config-vlan)# name UNUSED
Switch(config-vlan)# exit

! Don't assign any ports to VLAN 999
! Don't put any hosts in VLAN 999
```

**Best Practices:**

```
Native VLAN:
✅ Use high number (900+)
✅ Use consistently across all trunks
✅ Never assign access ports to it
✅ Monitor for violations

VLAN 1:
✅ Never use as native
✅ Consider removing from trunks
✅ Use for management (optional)

Trunk Configuration:
✅ Manual mode only
✅ Explicit allowed VLANs
✅ Document native VLAN
✅ Consistent configuration
```

### 2.4 Complete VLAN Security Template

**Access Port Template:**

```cisco
! User access port
interface FastEthernet0/5
 description "User Access - VLAN 10"
 
 ! Basic config
 switchport mode access
 switchport access vlan 10
 
 ! Disable DTP
 switchport nonegotiate
 
 ! Port security
 switchport port-security
 switchport port-security maximum 2
 switchport port-security mac-address sticky
 switchport port-security violation restrict
 
 ! STP
 spanning-tree portfast
 spanning-tree bpduguard enable
 
 ! Enable
 no shutdown
!
```

**Trunk Port Template:**

```cisco
! Trunk to another switch
interface GigabitEthernet0/1
 description "Trunk to Distribution Switch"
 
 ! Manual trunk
 switchport mode trunk
 switchport nonegotiate
 
 ! Native VLAN
 switchport trunk native vlan 999
 
 ! Allowed VLANs
 switchport trunk allowed vlan 10,20,30,40,999
 
 ! STP
 spanning-tree guard root
 
 ! Enable
 no shutdown
!
```

**Unused Port Template:**

```cisco
! Unused/Disabled ports
interface range FastEthernet0/20-24
 description "Unused Ports"
 
 ! Access mode
 switchport mode access
 switchport access vlan 999
 
 ! Disable DTP
 switchport nonegotiate
 
 ! Shutdown
 shutdown
!
```

---

## 3. Mitigate DHCP Attacks

### การป้องกัน DHCP Attacks

### 3.1 DHCP Snooping Configuration

**DHCP Snooping Review:**

```
Purpose:
- Validate DHCP messages
- Build binding table
- Trusted vs Untrusted ports

Prevents:
- DHCP starvation
- DHCP spoofing
- Rogue DHCP servers
```

**Configuration Steps:**

```
Step 1: Enable DHCP snooping globally
Step 2: Enable DHCP snooping on VLANs
Step 3: Configure trusted ports
Step 4: (Optional) Configure rate limiting
Step 5: Verify operation
```

**Step 1: Enable Globally**

```cisco
! Enable DHCP snooping
Switch(config)# ip dhcp snooping

! Verify
Switch# show ip dhcp snooping

Switch DHCP snooping is enabled
DHCP snooping is configured on following VLANs:
none
Insertion of option 82 is enabled
   circuit-id default format: vlan-mod-port
   remote-id: 0cd9.96d2.3f80 (MAC)
Option 82 on untrusted port is not allowed
Verification of hwaddr field is enabled
Verification of giaddr field is enabled
DHCP snooping trust/rate is configured on the following Interfaces:

Interface                  Trusted    Allow option    Rate limit (pps)
-----------------------    -------    ------------    ----------------
```

**Step 2: Enable on VLANs**

```cisco
! Enable for specific VLANs
Switch(config)# ip dhcp snooping vlan 10
Switch(config)# ip dhcp snooping vlan 20,30,40

! Or range
Switch(config)# ip dhcp snooping vlan 10-50

! Verify
Switch# show ip dhcp snooping

Switch DHCP snooping is enabled
DHCP snooping is configured on following VLANs:
10,20,30,40
Insertion of option 82 is enabled
...
```

**Step 3: Configure Trusted Ports**

```cisco
! Uplink to DHCP server/router
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# description "Uplink to Router/DHCP Server"
Switch(config-if)# ip dhcp snooping trust

! Uplink to another switch
Switch(config)# interface GigabitEthernet0/2
Switch(config-if)# description "Uplink to Distribution Switch"
Switch(config-if)# ip dhcp snooping trust

! Verify
Switch# show ip dhcp snooping

Interface                  Trusted    Allow option    Rate limit (pps)
-----------------------    -------    ------------    ----------------
GigabitEthernet0/1         yes        yes             unlimited
GigabitEthernet0/2         yes        yes             unlimited
```

**Default Behavior:**

```
Trusted Ports:
- Allow ALL DHCP messages
- Server messages OK
- Client messages OK
- Forward to other ports

Untrusted Ports (default = all ports):
- Allow client messages (Discover, Request)
- Block server messages (Offer, ACK)
- Rate limit possible
```

**Step 4: Rate Limiting (Optional)**

```cisco
! Limit DHCP packets on untrusted ports
Switch(config)# interface range FastEthernet0/1-24
Switch(config-if-range)# ip dhcp snooping limit rate 10

! 10 packets per second
! ถ้าเกิน → port err-disabled

! Verify
Switch# show ip dhcp snooping

Interface                  Trusted    Allow option    Rate limit (pps)
-----------------------    -------    ------------    ----------------
GigabitEthernet0/1         yes        yes             unlimited
GigabitEthernet0/2         yes        yes             unlimited
FastEthernet0/1            no         no              10
FastEthernet0/2            no         no              10
...
FastEthernet0/24           no         no              10
```

**Step 5: Verify Operation**

```cisco
! Show DHCP snooping status
Switch# show ip dhcp snooping

! Show binding table
Switch# show ip dhcp snooping binding

MacAddress          IpAddress        Lease(sec)  Type           VLAN  Interface
------------------  ---------------  ----------  -------------  ----  --------------------
00:11:22:33:44:55   192.168.10.10      86400    dhcp-snooping   10   FastEthernet0/1
00:55:66:77:88:99   192.168.10.11      86400    dhcp-snooping   10   FastEthernet0/2
00:AA:BB:CC:DD:EE   192.168.20.10      43200    dhcp-snooping   20   FastEthernet0/5
Total number of bindings: 3

! Show statistics
Switch# show ip dhcp snooping statistics

Packets received from untrusted sources    10542
Packets forwarded to DHCP server           1245
Packets forwarded to DHCP clients          1245
Packets dropped from untrusted ports       12
Packets dropped due to rate limit          5
Packets dropped due to Option 82 failure   0
```

### 3.2 DHCP Snooping Option 82

**Option 82 คืออะไร:**

```
DHCP Relay Information Option
เพิ่มข้อมูลเกี่ยวกับ:
- Switch port
- VLAN
- Switch MAC

Purpose:
- Track DHCP requests
- Security
- Troubleshooting
```

**Configuration:**

```cisco
! Enable Option 82 (enabled by default)
Switch(config)# ip dhcp snooping information option

! Disable Option 82
Switch(config)# no ip dhcp snooping information option

! Configure custom format
Switch(config)# ip dhcp snooping information option format remote-id hostname
```

**Issue with Option 82:**

```
Problem:
Untrusted port receives DHCP message with Option 82
→ Switch drops it (security)

Scenario:
[PC] → [Access Switch] → [Distribution Switch] → [DHCP Server]
         (untrusted)        (inserts Option 82)

Access switch sees Option 82 from untrusted port (Distribution)
→ Drops packet!

Solution:
Trust the uplink port:
Switch(config-if)# ip dhcp snooping trust
```

### 3.3 Complete DHCP Snooping Example

```cisco
! === Access Switch Configuration ===

! Enable DHCP snooping
Switch(config)# ip dhcp snooping

! Enable for VLANs
Switch(config)# ip dhcp snooping vlan 10,20,30

! Trust uplink
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# description "Uplink to Distribution"
Switch(config-if)# ip dhcp snooping trust
Switch(config-if)# exit

! Rate limit access ports
Switch(config)# interface range FastEthernet0/1-24
Switch(config-if-range)# ip dhcp snooping limit rate 15
Switch(config-if-range)# exit

! Save config
Switch# copy running-config startup-config
```

**Verification Workflow:**

```cisco
! 1. Check snooping status
Switch# show ip dhcp snooping

Switch DHCP snooping is enabled
DHCP snooping is configured on following VLANs:
10,20,30

! 2. Check trusted ports
Interface                  Trusted    Allow option    Rate limit (pps)
GigabitEthernet0/1         yes        yes             unlimited
FastEthernet0/1-24         no         no              15

! 3. Generate DHCP traffic (from PC)
PC> ipconfig /release
PC> ipconfig /renew

! 4. Check binding table
Switch# show ip dhcp snooping binding

MacAddress          IpAddress        Lease(sec)  Type           VLAN  Interface
00:11:22:33:44:55   192.168.10.100     86400    dhcp-snooping   10   Fa0/5

! 5. Check statistics
Switch# show ip dhcp snooping statistics

Packets forwarded to DHCP server           4
Packets forwarded to DHCP clients          4
Packets dropped from untrusted ports       0
```

---

## 4. Mitigate ARP Attacks

### การป้องกัน ARP Attacks

### 4.1 Dynamic ARP Inspection (DAI) Configuration

**DAI Review:**

```
Purpose:
- Validate ARP packets
- Use DHCP snooping binding table
- Trusted vs Untrusted ports

Prevents:
- ARP spoofing
- ARP poisoning
- Man-in-the-middle attacks
```

**Prerequisites:**

```
DHCP Snooping MUST be configured first!

Why?
DAI ใช้ DHCP snooping binding table
เพื่อ validate ARP packets

No DHCP snooping = No DAI validation
```

**Configuration Steps:**

```
Step 1: Configure DHCP snooping (prerequisite)
Step 2: Enable DAI on VLANs
Step 3: Configure trusted ports
Step 4: (Optional) Configure validation
Step 5: (Optional) Configure rate limiting
Step 6: Verify operation
```

**Step 1: DHCP Snooping (Already Done)**

```cisco
! From previous section
Switch(config)# ip dhcp snooping
Switch(config)# ip dhcp snooping vlan 10,20,30
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# ip dhcp snooping trust
```

**Step 2: Enable DAI**

```cisco
! Enable DAI on VLANs
Switch(config)# ip arp inspection vlan 10
Switch(config)# ip arp inspection vlan 20,30

! Or range
Switch(config)# ip arp inspection vlan 10-50

! Verify
Switch# show ip arp inspection

Source Mac Validation      : Disabled
Destination Mac Validation : Disabled
IP Address Validation      : Disabled

 Vlan     Configuration    Operation   ACL Match          Static ACL
 ----     -------------    ---------   ---------          ----------
   10     Enabled          Active                         
   20     Enabled          Active                         
   30     Enabled          Active                         

 Vlan     ACL Logging      DHCP Logging      Probe Logging
 ----     -----------      ------------      -------------
   10     Deny             Deny              Off          
   20     Deny             Deny              Off          
   30     Deny             Deny              Off
```

**Step 3: Configure Trusted Ports**

```cisco
! Trust uplink (to router/switches)
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# description "Uplink - Trusted"
Switch(config-if)# ip arp inspection trust

! Trust inter-switch link
Switch(config)# interface GigabitEthernet0/2
Switch(config-if)# description "ISL - Trusted"
Switch(config-if)# ip arp inspection trust

! Verify
Switch# show ip arp inspection interfaces

 Interface        Trust State     Rate (pps)    Burst Interval
 ---------------  -----------     ----------    --------------
 Gi0/1            Trusted         None          N/A
 Gi0/2            Trusted         None          N/A
 Fa0/1            Untrusted       15            1
 Fa0/2            Untrusted       15            1
 ...
```

**Default Behavior:**

```
Trusted Ports:
- NO validation
- Forward all ARPs
- Typically: uplinks, server ports

Untrusted Ports (default):
- Full validation
- Check against binding table
- Drop invalid ARPs
- Typically: access ports
```

**Step 4: Configure Validation (Optional)**

```cisco
! Enable validation checks
Switch(config)# ip arp inspection validate src-mac
Switch(config)# ip arp inspection validate dst-mac
Switch(config)# ip arp inspection validate ip

! Or all at once
Switch(config)# ip arp inspection validate src-mac dst-mac ip

! Verify
Switch# show ip arp inspection

Source Mac Validation      : Enabled
Destination Mac Validation : Enabled
IP Address Validation      : Enabled
```

**Validation Checks:**

```
src-mac:
- Ethernet source MAC = ARP sender MAC
- Detect MAC spoofing

dst-mac:
- Ethernet destination MAC = ARP target MAC
- For ARP replies only
- Detect MAC spoofing

ip:
- Validate IP addresses
- Check for 0.0.0.0, 255.255.255.255
- Check for multicast IPs
- Detect IP spoofing
```

**Step 5: Rate Limiting (Optional)**

```cisco
! Limit ARP packets on untrusted ports
Switch(config)# interface range FastEthernet0/1-24
Switch(config-if-range)# ip arp inspection limit rate 15
! 15 ARP packets per second

! Burst interval (optional)
Switch(config-if-range)# ip arp inspection limit rate 15 burst interval 1

! ถ้าเกิน rate → port err-disabled

! Verify
Switch# show ip arp inspection interfaces

 Interface        Trust State     Rate (pps)    Burst Interval
 ---------------  -----------     ----------    --------------
 Fa0/1            Untrusted       15            1
 Fa0/2            Untrusted       15            1
```

**Default Rate Limits:**

```
Untrusted ports: 15 pps (packets per second)
Trusted ports: No limit

Recommended:
Normal port: 10-15 pps
Busy port: 20-30 pps
```

**Step 6: Verify Operation**

```cisco
! Show DAI status
Switch# show ip arp inspection

! Show statistics
Switch# show ip arp inspection statistics

 Vlan      Forwarded        Dropped     DHCP Drops     ACL Drops
 ----      ---------        -------     ----------     ---------
   10           1250             15             15             0
   20            850              3              3             0
   30            420              0              0             0

! Show per-VLAN
Switch# show ip arp inspection statistics vlan 10

 Vlan      Forwarded        Dropped     DHCP Drops     ACL Drops
 ----      ---------        -------     ----------     ---------
   10           1250             15             15             0

! Show logs
Switch# show ip arp inspection log

Syslog rate : 1 entries per 300 seconds.

Tue Nov 18 14:30:45 2025

Interface  VLAN Sender MAC       Sender IP      Num PKT  Reason
---------- ---- ----------------- -------------- ------- -----------
Fa0/5       10  0066.7788.9900    192.168.10.50       1  DHCP Deny
```

### 4.2 DAI with Static IPs

**Problem:**

```
DAI uses DHCP snooping binding table
ถ้า device ใช้ static IP → ไม่มี binding
→ ARP validation fails
→ Packets dropped!
```

**Solution 1: ARP Access Control List (ACL)**

```cisco
! Create ARP ACL
Switch(config)# arp access-list STATIC-HOSTS
Switch(config-arp-nacl)# permit ip host 192.168.10.100 mac host 00AA.BBCC.DDEE
Switch(config-arp-nacl)# permit ip host 192.168.10.101 mac host 00BB.CCDD.EEFF
Switch(config-arp-nacl)# exit

! Apply to VLAN
Switch(config)# ip arp inspection filter STATIC-HOSTS vlan 10

! Verify
Switch# show ip arp inspection

 Vlan     Configuration    Operation   ACL Match          Static ACL
 ----     -------------    ---------   ---------          ----------
   10     Enabled          Active      STATIC-HOSTS       

Switch# show arp access-list

ARP access list STATIC-HOSTS
    permit ip host 192.168.10.100 mac host 00aa.bbcc.ddee
    permit ip host 192.168.10.101 mac host 00bb.ccdd.eeff
```

**Solution 2: Trust the Port**

```cisco
! Trust port ที่มี static IP device
Switch(config)# interface FastEthernet0/10
Switch(config-if)# description "Server - Static IP"
Switch(config-if)# ip arp inspection trust

! Not recommended for access ports!
! Use only for servers/infrastructure
```

**Solution 3: Manual IP Source Binding**

```cisco
! Create static binding
Switch(config)# ip source binding 00AA.BBCC.DDEE vlan 10 192.168.10.100 interface FastEthernet0/10

! Verify
Switch# show ip source binding

MacAddress          IpAddress        Lease(sec)  Type           VLAN  Interface
------------------  ---------------  ----------  -------------  ----  --------------------
00:AA:BB:CC:DD:EE   192.168.10.100     infinite   static          10   FastEthernet0/10
00:11:22:33:44:55   192.168.10.50        86400   dhcp-snooping   10   FastEthernet0/5
```

### 4.3 Complete DAI Example

```cisco
! === Complete Configuration ===

! Step 1: DHCP Snooping
Switch(config)# ip dhcp snooping
Switch(config)# ip dhcp snooping vlan 10,20
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# ip dhcp snooping trust
Switch(config-if)# exit

! Step 2: Enable DAI
Switch(config)# ip arp inspection vlan 10,20

! Step 3: Trust uplinks
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# ip arp inspection trust
Switch(config-if)# exit

! Step 4: Validation
Switch(config)# ip arp inspection validate src-mac dst-mac ip

! Step 5: Rate limiting
Switch(config)# interface range FastEthernet0/1-24
Switch(config-if-range)# ip arp inspection limit rate 15
Switch(config-if-range)# exit

! Step 6: Static bindings (if needed)
Switch(config)# arp access-list SERVERS
Switch(config-arp-nacl)# permit ip host 192.168.10.100 mac host 00AA.BBCC.DDEE
Switch(config-arp-nacl)# exit
Switch(config)# ip arp inspection filter SERVERS vlan 10

! Save
Switch# copy running-config startup-config
```

---

## 5. Mitigate STP Attacks

### การป้องกัน STP Attacks

### 5.1 STP Attack Review

**STP Attacks:**

```
1. BPDU Manipulation
   - Attacker sends fake BPDUs
   - Claims to be root bridge
   - Disrupts topology

2. Root Bridge Hijacking
   - Priority 0 BPDUs
   - Becomes root
   - Controls traffic flow

3. Topology Manipulation
   - Force recalculations
   - Cause loops
   - DoS attacks
```

**Mitigation Features:**

```
1. PortFast - Skip listening/learning states
2. BPDU Guard - Shutdown on BPDU receipt
3. Root Guard - Prevent unauthorized root
4. Loop Guard - Detect unidirectional links
```

### 5.2 PortFast Configuration

**PortFast Overview:**

```
Purpose:
- เร่ง port transition เป็น forwarding
- Skip listening/learning states
- Immediate forwarding (for end devices)

Normal STP: 30-50 seconds
With PortFast: <2 seconds

⚠️ ใช้กับ access ports เท่านั้น!
⚠️ ห้ามใช้กับ trunks/uplinks!
```

**Configuration:**

**Per-Interface:**

```cisco
! Enable PortFast on interface
Switch(config)# interface FastEthernet0/1
Switch(config-if)# description "User Access Port"
Switch(config-if)# switchport mode access
Switch(config-if)# spanning-tree portfast

%Warning: portfast should only be enabled on ports connected to a single
host. Connecting hubs, concentrators, switches, bridges, etc... to this
interface when portfast is enabled, can cause temporary bridging loops.
Use with CAUTION

%Portfast has been configured on FastEthernet0/1 but will only
have effect when the interface is in a non-trunking mode.

! Verify
Switch# show spanning-tree interface FastEthernet0/1 portfast

VLAN0010         enabled
```

**Global (All Access Ports):**

```cisco
! Enable PortFast globally
Switch(config)# spanning-tree portfast default

! Applies to all access ports automatically
! Does NOT apply to trunks
! Recommended for enterprise

! Verify
Switch# show spanning-tree summary

Switch is in pvst mode
Root bridge for: none
Extended system ID           is enabled
Portfast Default             is enabled  ← Enabled globally
PortFast BPDU Guard Default  is disabled
Portfast BPDU Filter Default is disabled
```

**Disable PortFast:**

```cisco
! Disable on specific interface
Switch(config)# interface FastEthernet0/1
Switch(config-if)# spanning-tree portfast disable

! Or globally
Switch(config)# no spanning-tree portfast default
```

### 5.3 BPDU Guard Configuration

**BPDU Guard Overview:**

```
Purpose:
- Protect PortFast ports
- Detect rogue switches
- ถ้าได้รับ BPDU → shutdown port immediately

When to use:
✅ Access ports (end devices)
✅ PortFast-enabled ports
✅ User-facing ports

When NOT to use:
❌ Trunk ports
❌ Uplinks
❌ Switch-to-switch links
```

**Configuration:**

**Global (Recommended):**

```cisco
! Enable BPDU Guard globally
! Affects ALL PortFast ports
Switch(config)# spanning-tree portfast bpduguard default

! Verify
Switch# show spanning-tree summary

Switch is in pvst mode
Root bridge for: none
Extended system ID           is enabled
Portfast Default             is enabled
PortFast BPDU Guard Default  is enabled  ← Enabled
Portfast BPDU Filter Default is disabled
```

**Per-Interface:**

```cisco
! Enable on specific interface
Switch(config)# interface FastEthernet0/1
Switch(config-if)# switchport mode access
Switch(config-if)# spanning-tree portfast
Switch(config-if)# spanning-tree bpduguard enable

! Verify
Switch# show spanning-tree interface FastEthernet0/1 detail

Port 1 (FastEthernet0/1) of VLAN0010 is forwarding
   Port path cost 19, Port priority 128, Port Identifier 128.1.
   Designated root has priority 32778, address 0cd9.96d2.3f80
   ...
   Bpdu guard is enabled  ← Enabled
```

**When BPDU Received:**

```
1. Switch receives BPDU on BPDU Guard port
2. Port immediately goes err-disabled
3. LED turns off
4. Syslog message generated
5. SNMP trap sent

Example:
%SPANTREE-2-BPDUGUARD: Received BPDU on port FastEthernet0/1 
with BPDU Guard enabled. Putting port in err-disable state.

%PM-4-ERR_DISABLE: bpduguard error detected on Fa0/1, 
putting Fa0/1 in err-disable state
```

**Verify BPDU Guard:**

```cisco
! Check err-disabled ports
Switch# show interface status err-disabled

Port      Name               Status       Reason
Fa0/1     User Port          err-disabled bpduguard

! Show spanning-tree
Switch# show spanning-tree interface FastEthernet0/1

Port 1 (FastEthernet0/1) of VLAN0010 is broken (BPDU Guard)
   Port path cost 19, Port priority 128, Port Identifier 128.1.
   ...
```

**Recovery from BPDU Guard:**

**Manual Recovery:**

```cisco
! Remove rogue device first!

! Then recover port
Switch(config)# interface FastEthernet0/1
Switch(config-if)# shutdown
Switch(config-if)# no shutdown

! Verify
Switch# show interface FastEthernet0/1
FastEthernet0/1 is up, line protocol is up
```

**Automatic Recovery:**

```cisco
! Enable auto-recovery
Switch(config)# errdisable recovery cause bpduguard

! Set interval (default 300 seconds)
Switch(config)# errdisable recovery interval 600

! Verify
Switch# show errdisable recovery

ErrDisable Reason            Timer Status
-----------------            --------------
bpduguard                    Enabled

Timer interval: 600 seconds

Interfaces that will be enabled at the next timeout:
Interface    Errdisable reason    Time left(sec)
---------    -----------------    --------------
Fa0/1        bpduguard                 435
```

### 5.4 Root Guard Configuration

**Root Guard Overview:**

```
Purpose:
- Prevent unauthorized root bridge
- Protect STP topology
- Block superior BPDUs

When to use:
✅ Ports where root bridge should NOT be
✅ Ports to access/distribution layers
✅ Customer/guest connections

When NOT to use:
❌ Ports toward legitimate root bridge
❌ Ports where root might appear
```

**Configuration:**

```cisco
! Enable Root Guard
Switch(config)# interface GigabitEthernet0/2
Switch(config-if)# description "Downlink to Access Layer"
Switch(config-if)# spanning-tree guard root

! Verify
Switch# show spanning-tree interface GigabitEthernet0/2 detail

Port 26 (GigabitEthernet0/2) of VLAN0010 is forwarding
   Port path cost 4, Port priority 128, Port Identifier 128.26.
   ...
   Root guard is enabled  ← Enabled
```

**When Superior BPDU Received:**

```
1. Superior BPDU arrives (better than root)
2. Port enters root-inconsistent state
3. Port stops forwarding
4. Syslog message generated
5. When superior BPDUs stop:
   - Port automatically recovers
   - Returns to forwarding

Example:
%SPANTREE-2-ROOTGUARD_BLOCK: Root guard blocking port 
GigabitEthernet0/2 on VLAN 10.

%SPANTREE-2-ROOTGUARD_UNBLOCK: Root guard unblocking port 
GigabitEthernet0/2 on VLAN 10.
```

**Verify Root Guard:**

```cisco
! Check root-inconsistent ports
Switch# show spanning-tree inconsistentports

Name                 Interface              Inconsistency
-------------------- ---------------------- ------------------
VLAN0010             GigabitEthernet0/2     Root Inconsistent

Number of inconsistent ports (segments) in the system : 1

! Show interface detail
Switch# show spanning-tree interface GigabitEthernet0/2

Port 26 (GigabitEthernet0/2) of VLAN0010 is broken (Root Inconsistent)
   Port path cost 4, Port priority 128, Port Identifier 128.26.
   ...
```

### 5.5 Complete STP Security Configuration

**Access Port Template:**

```cisco
! User access port with full STP protection
interface FastEthernet0/5
 description "User Access Port"
 
 ! Basic config
 switchport mode access
 switchport access vlan 10
 switchport nonegotiate
 
 ! Port security
 switchport port-security
 switchport port-security maximum 2
 switchport port-security mac-address sticky
 switchport port-security violation restrict
 
 ! STP security
 spanning-tree portfast
 spanning-tree bpduguard enable
 
 ! Enable
 no shutdown
!
```

**Uplink Template (to Distribution):**

```cisco
! Uplink with Root Guard
interface GigabitEthernet0/1
 description "Uplink to Distribution Switch"
 
 ! Trunk config
 switchport mode trunk
 switchport nonegotiate
 switchport trunk native vlan 999
 switchport trunk allowed vlan 10,20,30,40,999
 
 ! Security features
 ip dhcp snooping trust
 ip arp inspection trust
 
 ! STP protection
 spanning-tree guard root
 
 ! Enable
 no shutdown
!
```

**Global STP Security:**

```cisco
! === Global STP Configuration ===

! Set priority (if root bridge)
Switch(config)# spanning-tree vlan 1-4094 priority 0

! Enable PortFast globally
Switch(config)# spanning-tree portfast default

! Enable BPDU Guard globally
Switch(config)# spanning-tree portfast bpduguard default

! Enable auto-recovery
Switch(config)# errdisable recovery cause bpduguard
Switch(config)# errdisable recovery cause psecure-violation
Switch(config)# errdisable recovery interval 300
```

**Verification Checklist:**

```cisco
! 1. Check PortFast status
Switch# show spanning-tree summary
Portfast Default             is enabled
PortFast BPDU Guard Default  is enabled

! 2. Check per-interface
Switch# show spanning-tree interface FastEthernet0/1 portfast
VLAN0010         enabled

! 3. Check BPDU Guard
Switch# show spanning-tree interface FastEthernet0/1 detail
Bpdu guard is enabled

! 4. Check Root Guard
Switch# show spanning-tree interface GigabitEthernet0/1 detail
Root guard is enabled

! 5. Check for inconsistencies
Switch# show spanning-tree inconsistentports
(Should be empty)

! 6. Check err-disabled ports
Switch# show interface status err-disabled
(Check for issues)
```

---

## สรุป

### สิ่งที่ได้เรียนรู้ใน Module 11:

✅ **Port Security:**

- Secure unused ports
- Static, Dynamic, Sticky MAC addresses
- Maximum MAC configuration
- Violation modes (Shutdown, Restrict, Protect)
- MAC aging
- Err-disabled recovery
- Complete configuration และ verification

✅ **VLAN Attack Mitigation:**

- Disable DTP on access ports
- Manual trunk configuration
- Native VLAN best practices
- VLAN security templates

✅ **DHCP Attack Mitigation:**

- DHCP Snooping configuration
- Trusted ports
- Rate limiting
- Binding table
- Option 82

✅ **ARP Attack Mitigation:**

- Dynamic ARP Inspection (DAI)
- Validation options
- Rate limiting
- Static IP handling
- ARP ACLs

✅ **STP Attack Mitigation:**

- PortFast configuration
- BPDU Guard
- Root Guard
- Auto-recovery
- Complete STP security

---

## คำสั่งสำคัญสรุป

### Port Security:

```cisco
! Enable port security
interface <interface>
 switchport mode access
 switchport port-security
 switchport port-security maximum <number>
 switchport port-security mac-address {<mac> | sticky}
 switchport port-security violation {shutdown | restrict | protect}
 switchport port-security aging time <minutes>
 switchport port-security aging type {absolute | inactivity}

! Recovery
errdisable recovery cause psecure-violation
errdisable recovery interval <seconds>
```

### VLAN Security:

```cisco
! Access port
interface <interface>
 switchport mode access
 switchport nonegotiate

! Trunk port
interface <interface>
 switchport mode trunk
 switchport nonegotiate
 switchport trunk native vlan <unused-vlan>
 switchport trunk allowed vlan <vlan-list>
```

### DHCP Snooping:

```cisco
! Global
ip dhcp snooping
ip dhcp snooping vlan <vlan-list>

! Trusted port
interface <interface>
 ip dhcp snooping trust

! Rate limit
interface <interface>
 ip dhcp snooping limit rate <pps>
```

### Dynamic ARP Inspection:

```cisco
! Enable DAI
ip arp inspection vlan <vlan-list>
ip arp inspection validate {src-mac | dst-mac | ip}

! Trusted port
interface <interface>
 ip arp inspection trust

! Rate limit
interface <interface>
 ip arp inspection limit rate <pps>
```

### STP Security:

```cisco
! PortFast
spanning-tree portfast default
interface <interface>
 spanning-tree portfast

! BPDU Guard
spanning-tree portfast bpduguard default
interface <interface>
 spanning-tree bpduguard enable

! Root Guard
interface <interface>
 spanning-tree guard root

! Recovery
errdisable recovery cause bpduguard
errdisable recovery interval <seconds>
```

### Verification:

```cisco
show port-security [interface <int>]
show port-security address
show ip dhcp snooping
show ip dhcp snooping binding
show ip arp inspection
show ip arp inspection statistics
show spanning-tree summary
show spanning-tree interface <int> detail
show interface status err-disabled
show errdisable recovery
```

---

## Best Practices

### Port Security:

```
✅ Use sticky MAC for production
✅ Set appropriate maximum
✅ Use restrict mode for monitoring
✅ Enable aging for guest ports
✅ Document MAC addresses
✅ Enable auto-recovery
✅ Regular audits
```

### VLAN Security:

```
✅ Disable DTP on all access ports
✅ Manual trunk configuration
✅ Use unused VLAN as native
✅ Never use VLAN 1 as native
✅ Restrict allowed VLANs on trunks
✅ Shutdown unused ports
```

### DHCP Snooping:

```
✅ Enable on all VLANs with DHCP
✅ Trust uplinks only
✅ Appropriate rate limits
✅ Monitor binding table
✅ Regular verification
```

### DAI:

```
✅ Enable after DHCP snooping
✅ Trust uplinks and servers
✅ Enable all validations
✅ Appropriate rate limits
✅ ARP ACLs for static IPs
```

### STP Security:

```
✅ PortFast on access ports only
✅ BPDU Guard on all access ports
✅ Root Guard on distribution links
✅ Auto-recovery enabled
✅ Regular inconsistency checks
```

---

## Security Configuration Template

```cisco
! === Complete Secure Access Port ===
interface FastEthernet0/X
 description "Secure Access Port"
 
 ! Basic
 switchport mode access
 switchport access vlan <vlan-id>
 switchport nonegotiate
 
 ! Port Security
 switchport port-security
 switchport port-security maximum 2
 switchport port-security mac-address sticky
 switchport port-security violation restrict
 
 ! DHCP/ARP
 ip dhcp snooping limit rate 15
 ip arp inspection limit rate 15
 
 ! STP
 spanning-tree portfast
 spanning-tree bpduguard enable
 
 ! Enable
 no shutdown
!

! === Complete Secure Trunk Port ===
interface GigabitEthernet0/X
 description "Secure Trunk"
 
 ! Trunk
 switchport mode trunk
 switchport nonegotiate
 switchport trunk native vlan 999
 switchport trunk allowed vlan <vlan-list>
 
 ! Security
 ip dhcp snooping trust
 ip arp inspection trust
 spanning-tree guard root
 
 ! Enable
 no shutdown
!

! === Global Security ===
! Port Security
errdisable recovery cause psecure-violation
errdisable recovery interval 300

! DHCP Snooping
ip dhcp snooping
ip dhcp snooping vlan <vlan-list>

! DAI
ip arp inspection vlan <vlan-list>
ip arp inspection validate src-mac dst-mac ip

! STP
spanning-tree portfast default
spanning-tree portfast bpduguard default
errdisable recovery cause bpduguard
```

---

## Common Issues and Solutions

**Issue 1: Port Security Not Working**

```
Problem: switchport port-security rejected

Solution:
1. Check port mode:
   switchport mode access
2. Disable DTP:
   switchport nonegotiate
3. Then enable port security
```

**Issue 2: DHCP Clients Not Getting IPs**

```
Problem: No DHCP after enabling snooping

Solution:
1. Trust uplink to DHCP server:
   ip dhcp snooping trust
2. Check VLAN configuration
3. Verify Option 82 settings
```

**Issue 3: DAI Dropping Valid ARPs**

```
Problem: Connectivity issues with DAI

Solution:
1. Check DHCP snooping first
2. Trust server/uplink ports
3. Create ARP ACL for static IPs
4. Check rate limits
```

**Issue 4: PortFast/BPDU Guard Conflicts**

```
Problem: Ports keep going err-disabled

Solution:
1. Verify PortFast only on access ports
2. Check for loops/switches
3. Adjust recovery timer
4. Document violations
```

---

## Lab Activities

### Lab 11.1: Configure Port Security

- Enable port security
- Configure sticky MACs
- Test violations
- Recovery procedures

### Lab 11.2: DHCP Snooping and DAI

- Configure DHCP snooping
- Enable DAI
- Verify bindings
- Test validation

### Lab 11.3: Complete Switch Security

- Port security
- VLAN security
- DHCP snooping
- DAI
- STP protection

---

## คำถามทบทวน

1. Port Security มี MAC address types อะไรบ้าง?
2. Sticky MAC addresses ต่างจาก dynamic อย่างไร?
3. Port Security violation modes มีอะไรบ้าง?
4. ทำไมต้อง configure DHCP Snooping ก่อน DAI?
5. DAI validation options มีอะไรบ้าง?
6. PortFast ควรใช้กับ port ประเภทใด?
7. BPDU Guard ทำงานอย่างไร?
8. Root Guard ต่างจาก BPDU Guard อย่างไร?
9. Err-disabled recovery ทำงานอย่างไร?
10. Native VLAN ควรเป็น VLAN ใด? ทำไม?

---

## เฉลยคำถาม

1. **MAC Types:** Static (manual), Dynamic (learned, not saved), Sticky (learned, saved)
2. **Sticky vs Dynamic:** Sticky saved in running-config, persist across reboots; Dynamic lost
3. **Violation Modes:** Shutdown (err-disable), Restrict (drop + log), Protect (drop only)
4. **DHCP → DAI:** DAI uses DHCP snooping binding table for validation
5. **DAI Validation:** src-mac, dst-mac, ip
6. **PortFast:** Access ports เท่านั้น (end devices), ห้ามใช้ trunk/uplink
7. **BPDU Guard:** รับ BPDU → shutdown port immediately (err-disabled)
8. **Root vs BPDU Guard:** Root Guard = block superior BPDUs; BPDU Guard = block ANY BPDU
9. **Err-disabled Recovery:** Auto re-enable port after configured interval
10. **Native VLAN:** Unused VLAN (999), never VLAN 1, consistent across trunks

---

**หมายเหตุ:** Switch Security Configuration เป็นการนำ concepts จาก Module 10 มาใช้จริง การ configure อย่างถูกต้องและครบถ้วนจะช่วยป้องกัน Layer 2 attacks ได้อย่างมีประสิทธิภาพ

---

**เอกสารจัดทำโดย:** Claude (Anthropic AI)  
**Module:** CCNA 2 Module 11 - Switch Security Configuration  
**Version:** v7.02 SRWE  
**วันที่อัพเดท:** พฤศจิกายน 2025