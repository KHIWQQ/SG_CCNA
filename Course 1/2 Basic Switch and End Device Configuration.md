# CCNA Course 1 - Module 2: Basic Switch and End Device Configuration

## การตั้งค่าเบื้องต้นของสวิตช์และอุปกรณ์ปลายทาง

---

## 2.1 Cisco IOS Access (การเข้าถึง Cisco IOS)

### IOS (Internetwork Operating System)

**คำจำกัดความ:**

- ระบบปฏิบัติการของอุปกรณ์ Cisco (Router, Switch)
- ใช้สำหรับการตั้งค่าและจัดการอุปกรณ์
- Command-line interface (CLI)

### Access Methods (วิธีการเข้าถึง)

#### 1. Console Access

**ลักษณะ:**

- เชื่อมต่อโดยตรงด้วยสาย Console cable
- ใช้ RJ-45 to DB-9 หรือ USB
- Out-of-band management
- ใช้เมื่อไม่มี network access

**การเชื่อมต่อ:**

```
Settings:
Baud rate: 9600
Data bits: 8
Parity: None
Stop bits: 1
Flow control: None
```

**Terminal Software:**

- **Windows:** PuTTY, Tera Term, SecureCRT
- **macOS/Linux:** Screen, Minicom

#### 2. SSH (Secure Shell)

**ลักษณะ:**

- เชื่อมต่อผ่านเครือข่ายแบบปลอดภัย
- ข้อมูลเข้ารหัส
- Remote management
- Port 22 (TCP)
- แนะนำให้ใช้แทน Telnet

**ข้อดี:**

- ปลอดภัย (Encrypted)
- Authentication ที่แข็งแรง
- Support public key authentication

#### 3. Telnet

**ลักษณะ:**

- เชื่อมต่อผ่านเครือข่าย
- ข้อมูลไม่เข้ารหัส (Plain text)
- Port 23 (TCP)
- **ไม่แนะนำ** - ไม่ปลอดภัย

**ข้อเสีย:**

- Password และข้อมูลเป็น plain text
- ง่ายต่อการดักฟัง
- ไม่มี encryption

---

## 2.2 IOS Navigation (การใช้งาน IOS)

### User Modes (โหมดผู้ใช้)

#### User EXEC Mode

```
Router>
```

**ลักษณะ:**

- โหมดพื้นฐาน Limited commands
- ดูข้อมูลได้เท่านั้น ไม่สามารถเปลี่ยนแปลงการตั้งค่า
- Prompt: `>`

**คำสั่งที่ใช้ได้:**

```
Router> show version
Router> show ip interface brief
Router> ping 192.168.1.1
Router> traceroute 8.8.8.8
Router> show history
```

#### Privileged EXEC Mode

```
Router#
```

**เข้าโหมด:**

```
Router> enable
Router#
```

**ลักษณะ:**

- โหมดผู้ดูแลระบบ
- สามารถดูการตั้งค่าทั้งหมด
- สามารถใช้คำสั่งขั้นสูง
- Prompt: `#`

**คำสั่งที่ใช้ได้:**

```
Router# show running-config
Router# show startup-config
Router# copy running-config startup-config
Router# reload
Router# debug
Router# configure terminal
```

#### Global Configuration Mode

```
Router(config)#
```

**เข้าโหมด:**

```
Router# configure terminal
Router(config)#
```

**ลักษณะ:**

- ใช้สำหรับตั้งค่าที่ส่งผลต่ออุปกรณ์ทั้งหมด
- Changes affect entire device
- Prompt: `(config)#`

**คำสั่งทั่วไป:**

```
Router(config)# hostname R1
Router(config)# enable secret cisco123
Router(config)# no ip domain-lookup
Router(config)# banner motd #Message#
Router(config)# line console 0
Router(config)# interface gigabitEthernet 0/0
```

#### Specific Configuration Modes

**Interface Configuration Mode:**

```
Router(config)# interface gigabitEthernet 0/0
Router(config-if)#
```

**Line Configuration Mode:**

```
Router(config)# line console 0
Router(config-line)#

Router(config)# line vty 0 4
Router(config-line)#
```

**Router Configuration Mode:**

```
Router(config)# router ospf 1
Router(config-router)#
```

### Navigation Commands (คำสั่งเดินทางระหว่างโหมด)

#### Moving Between Modes

```
Router> enable                    (User EXEC → Privileged EXEC)
Router# configure terminal        (Privileged → Global Config)
Router(config)# interface gi0/0   (Global → Interface Config)
Router(config-if)# exit           (ออกไปโหมดก่อนหน้า)
Router(config)# end               (กลับไป Privileged EXEC ทันที)
Router# disable                   (Privileged → User EXEC)
```

**Shortcut:**

- `Ctrl+Z` = เหมือน `end` (กลับไป Privileged EXEC)
- `exit` = ออกจากโหมดปัจจุบัน ไปโหมดก่อนหน้า

### Help Features (คุณสมบัติช่วยเหลือ)

#### Context-Sensitive Help

```
Router# ?                         (แสดงคำสั่งทั้งหมดในโหมดนี้)
Router# show ?                    (แสดงคำสั่ง show ทั้งหมด)
Router# show ip ?                 (แสดงคำสั่ง show ip ทั้งหมด)
```

#### Command Completion

```
Router# conf<Tab>                 (Auto-complete เป็น configure)
Router# sh<Tab>                   (Auto-complete เป็น show)
```

#### Abbreviated Commands

```
Router# conf t                    (แทน configure terminal)
Router# sh ip int br              (แทน show ip interface brief)
Router# en                        (แทน enable)
```

### Command Structure (โครงสร้างคำสั่ง)

**รูปแบบ:**

```
command [keyword] [argument]
```

**ตัวอย่าง:**

```
show ip interface brief
│    │  │         │
│    │  │         └─ keyword
│    │  └─ keyword
│    └─ keyword
└─ command

ping 192.168.1.1
│    │
│    └─ argument
└─ command

interface gigabitEthernet 0/0
│         │                │
│         └─ keyword       └─ argument
└─ command
```

---

## 2.3 Basic Device Configuration (การตั้งค่าเบื้องต้น)

### 1. Configure Hostname

**คำสั่ง:**

```
Router> enable
Router# configure terminal
Router(config)# hostname R1
R1(config)#
```

**วัตถุประสงค์:**

- ตั้งชื่ออุปกรณ์เพื่อให้ง่ายต่อการจดจำ
- แสดงใน prompt
- ใช้ในการจัดการและ documentation

**Best Practices:**

- ใช้ชื่อที่บอกถึงหน้าที่หรือตำแหน่ง
- ไม่มีช่องว่าง
- ตัวอย่าง: R1, SW-Core-1, BR-Bangkok

### 2. Password Configuration (ตั้งค่ารหัสผ่าน)

#### Enable Password (รหัสผ่าน Privileged Mode)

**Enable Secret (แนะนำ):**

```
R1(config)# enable secret Str0ng!P@ssw0rd
```

- ใช้ MD5 encryption (Type 5)
- แข็งแรง ปลอดภัย
- แนะนำให้ใช้

**Enable Password (ล้าสมัย):**

```
R1(config)# enable password cisco123
```

- ไม่เข้ารหัส หรือใช้ Type 7 (อ่อนแอ)
- ไม่แนะนำ
- ถ้ามีทั้ง secret และ password จะใช้ secret

#### Console Password

**ตั้งค่า:**

```
R1(config)# line console 0
R1(config-line)# password Cons0le!P@ss
R1(config-line)# login
R1(config-line)# logging synchronous
R1(config-line)# exec-timeout 5 0
R1(config-line)# exit
```

**คำอธิบาย:**

- `line console 0` - เข้าโหมดตั้งค่า console
- `password` - กำหนดรหัสผ่าน
- `login` - เปิดใช้งานการตรวจสอบรหัสผ่าน
- `logging synchronous` - ป้องกัน log messages ขัดจังหวะการพิมพ์
- `exec-timeout 5 0` - Timeout 5 นาที 0 วินาที (0 0 = ไม่ timeout)

#### VTY Lines Password (Remote Access)

**ตั้งค่า:**

```
R1(config)# line vty 0 4
R1(config-line)# password VTY!P@ssw0rd
R1(config-line)# login
R1(config-line)# transport input ssh
R1(config-line)# exec-timeout 5 0
R1(config-line)# exit
```

**คำอธิบาย:**

- `line vty 0 4` - VTY lines 0-4 (5 lines รวม)
- `transport input ssh` - อนุญาตเฉพาะ SSH (ปิด Telnet)
- `transport input telnet ssh` - อนุญาตทั้ง Telnet และ SSH
- `transport input none` - ปิดการเข้าถึงทั้งหมด

**หมายเหตุ:**

- VTY lines มี 16 lines (0-15)
- line vty 0 4 = 5 concurrent connections
- line vty 0 15 = 16 concurrent connections

### 3. Password Encryption (เข้ารหัสรหัสผ่าน)

**Service Password Encryption:**

```
R1(config)# service password-encryption
```

**ลักษณะ:**

- เข้ารหัสรหัสผ่านทั้งหมดใน running-config
- ใช้ Type 7 encryption (Vigenere cipher)
- อ่อนแอ ถอดรหัสได้ง่าย
- **ดีกว่าไม่มี** แต่ไม่ปลอดภัยจริง

**ก่อนใช้:**

```
line console 0
 password cisco123
```

**หลังใช้:**

```
line console 0
 password 7 070C285F4D06
```

**Password Types:**

- **Type 0** - Plain text (ไม่เข้ารหัส)
- **Type 5** - MD5 hash (แข็งแรง) - enable secret
- **Type 7** - Vigenere (อ่อนแอ) - service password-encryption
- **Type 8** - PBKDF2 (SHA-256) - ใหม่ แข็งแรงมาก
- **Type 9** - Scrypt (ใหม่ล่าสุด แข็งแรงที่สุด)

### 4. Banner Configuration (ตั้งค่าข้อความแจ้งเตือน)

#### MOTD Banner (Message of the Day)

**คำสั่ง:**

```
R1(config)# banner motd #
Enter TEXT message. End with the character '#'.
******************************************
* UNAUTHORIZED ACCESS IS PROHIBITED!    *
* All activities are monitored!         *
* Violators will be prosecuted!         *
******************************************
#
```

**ลักษณะ:**

- แสดงก่อน login prompt
- ใช้ delimiting character (ตัวคั่น) ใดก็ได้ (แต่ # นิยม)
- ไม่ควรมีคำว่า "Welcome" (เพราะเป็นการต้อนรับผู้บุกรุก)

**Best Practices:**

- ระบุว่าเป็นระบบส่วนตัว
- เตือนเรื่องกฎหมาย
- ไม่เปิดเผยข้อมูลอุปกรณ์
- ไม่ใช้คำว่า Welcome, Hello

#### ประเภท Banner อื่นๆ

**Login Banner:**

```
R1(config)# banner login #
Please enter your credentials
#
```

- แสดงหลัง MOTD แต่ก่อน login prompt

**Exec Banner:**

```
R1(config)# banner exec #
You are now in privileged mode
#
```

- แสดงหลัง login เข้า EXEC mode

---

## 2.4 Save Configuration (บันทึกการตั้งค่า)

### Configuration Files

#### Running Configuration

**คำจำกัดความ:**

- Configuration ที่ใช้งานอยู่ปัจจุบัน
- เก็บใน **RAM** (Volatile memory)
- **หายเมื่อปิดเครื่องหรือ reload**
- เปลี่ยนแปลงได้ทันที

**ดูการตั้งค่า:**

```
R1# show running-config
R1# show run               (ย่อ)
```

#### Startup Configuration

**คำจำกัดความ:**

- Configuration ที่บันทึกถาวร
- เก็บใน **NVRAM** (Non-volatile memory)
- **ไม่หายเมื่อปิดเครื่อง**
- ใช้เมื่อเปิดเครื่องใหม่

**ดูการตั้งค่า:**

```
R1# show startup-config
R1# show start             (ย่อ)
```

### Save Commands (คำสั่งบันทึก)

**วิธีที่ 1:**

```
R1# copy running-config startup-config
Destination filename [startup-config]? 
Building configuration...
[OK]
```

**วิธีที่ 2:**

```
R1# write memory
Building configuration...
[OK]
```

**วิธีที่ 3:**

```
R1# wr
Building configuration...
[OK]
```

**ทั้ง 3 วิธีทำงานเหมือนกัน** - บันทึก running-config ไปยัง startup-config

### Backup Configuration (สำรองการตั้งค่า)

#### Backup to TFTP Server

```
R1# copy running-config tftp:
Address or name of remote host []? 192.168.1.100
Destination filename [r1-confg]? R1-backup-2024-11-18
!!
3847 bytes copied in 0.253 secs (15205 bytes/sec)
```

#### Restore from TFTP Server

```
R1# copy tftp: running-config
Address or name of remote host []? 192.168.1.100
Source filename []? R1-backup-2024-11-18
Destination filename [running-config]? 
Accessing tftp://192.168.1.100/R1-backup-2024-11-18...
Loading R1-backup-2024-11-18 from 192.168.1.100: !
[OK - 3847 bytes]
3847 bytes copied in 0.398 secs (9666 bytes/sec)
```

#### Backup to USB

```
R1# copy running-config usbflash0:
Destination filename [running-config]? R1-backup
```

### Erase Configuration (ลบการตั้งค่า)

**ลบ Startup Configuration:**

```
R1# erase startup-config
Erasing the nvram filesystem will remove all configuration files! Continue? [confirm]
[OK]
Erase of nvram: complete
```

**ลบและ Reload:**

```
R1# write erase
R1# reload
```

---

## 2.5 Ports and Addresses (พอร์ตและที่อยู่)

### IP Address Configuration (ตั้งค่า IP Address)

#### Router Interface Configuration

**คำสั่ง:**

```
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# description Link to LAN
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit
```

**คำอธิบาย:**

- `interface gigabitEthernet 0/0` - เลือก interface
- `description` - คำอธิบาย interface
- `ip address [IP] [Subnet Mask]` - กำหนด IP
- `no shutdown` - เปิดใช้งาน interface (สำคัญมาก!)
- Router interfaces มี **administrative shutdown** โดย default

**ตรวจสอบ:**

```
R1# show ip interface brief

Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0     192.168.1.1     YES manual up                    up
GigabitEthernet0/1     unassigned      YES unset  administratively down down
```

#### Switch Virtual Interface (SVI)

**คำสั่ง:**

```
Switch(config)# interface vlan 1
Switch(config-if)# ip address 192.168.1.10 255.255.255.0
Switch(config-if)# no shutdown
Switch(config-if)# exit
Switch(config)# ip default-gateway 192.168.1.1
```

**คำอธิบาย:**

- Switch ใช้ SVI สำหรับ management
- ไม่ใช้สำหรับ switching traffic
- ต้องตั้ง default-gateway

### Interface Status (สถานะ Interface)

**Command:**

```
R1# show ip interface brief
```

**Status Meanings:**

|Status|Protocol|Meaning|
|---|---|---|
|up|up|**ทำงานปกติ** - Layer 1 และ 2 ทำงานดี|
|up|down|**Layer 1 ทำงาน แต่ Layer 2 มีปัญหา** - ไม่มี keepalive, encapsulation ผิด|
|down|down|**ไม่มีสาย หรือปลายทางปิด** - Layer 1 ไม่ทำงาน|
|administratively down|down|**ใช้ shutdown command** - ปิดด้วยคำสั่ง|

### Verification Commands (คำสั่งตรวจสอบ)

#### Show IP Interface Brief

```
R1# show ip interface brief
R1# sh ip int br              (ย่อ)
```

- แสดงสรุป interfaces ทั้งหมด
- แสดง IP address และ status

#### Show Interfaces

```
R1# show interfaces
R1# show interfaces gigabitEthernet 0/0
```

- แสดงรายละเอียดทั้งหมด
- MAC address, Speed, Duplex, Statistics

#### Show IP Interface

```
R1# show ip interface gigabitEthernet 0/0
```

- แสดงข้อมูล Layer 3
- IP address, ACLs, Helper address

#### Show Running-Config Interface

```
R1# show running-config interface gigabitEthernet 0/0
```

- แสดงการตั้งค่าของ interface เฉพาะ

---

## 2.6 Configure SSH (ตั้งค่า SSH)

### SSH Configuration Steps

**Step 1: Set Hostname and Domain Name**

```
R1(config)# hostname R1
R1(config)# ip domain-name example.com
```

- จำเป็นสำหรับการสร้าง RSA key

**Step 2: Generate RSA Key Pair**

```
R1(config)# crypto key generate rsa
The name for the keys will be: R1.example.com
Choose the size of the key modulus in the range of 360 to 4096 for your
  General Purpose Keys. Choosing a key modulus greater than 512 may take
  a few minutes.

How many bits in the modulus [512]: 2048
% Generating 2048 bit RSA keys, keys will be non-exportable...
[OK] (elapsed time was 1 seconds)
```

- แนะนำ 2048 bits ขึ้นไป
- 1024 bits = minimum for security

**Step 3: Configure SSH Version**

```
R1(config)# ip ssh version 2
```

- SSH version 2 ปลอดภัยกว่า version 1

**Step 4: Create Local User Account**

```
R1(config)# username admin privilege 15 secret Adm!n!P@ss
```

- privilege 15 = full administrative access

**Step 5: Configure VTY Lines**

```
R1(config)# line vty 0 4
R1(config-line)# transport input ssh
R1(config-line)# login local
R1(config-line)# exec-timeout 5 0
R1(config-line)# exit
```

- `transport input ssh` - อนุญาตเฉพาะ SSH
- `login local` - ใช้ local username/password

**Step 6: Configure Enable Secret**

```
R1(config)# enable secret En@ble!P@ss
```

### Verify SSH Configuration

**Show SSH Status:**

```
R1# show ip ssh
SSH Enabled - version 2.0
Authentication timeout: 120 secs; Authentication retries: 3
```

**Show SSH Sessions:**

```
R1# show ssh
Connection Version Mode Encryption  Hmac         State              Username
0          2.0     IN   aes256-cbc  hmac-sha1    Session started    admin
1          2.0     OUT  aes256-cbc  hmac-sha1    Session started    admin
```

**Show Crypto Key:**

```
R1# show crypto key mypubkey rsa
```

### Connect Using SSH

**From PC:**

```
C:\> ssh admin@192.168.1.1
Password: 
R1>
```

**From Another Cisco Device:**

```
R2# ssh -l admin 192.168.1.1
Password:
R1>
```

---

## 2.7 Basic Troubleshooting (การแก้ปัญหาเบื้องต้น)

### Common Problems and Solutions

#### Problem 1: Cannot Enter Privileged Mode

**Symptom:**

```
Router> enable
Password: 
Password: 
Password: 
% Bad secrets
```

**Solutions:**

- ตรวจสอบ Caps Lock
- ตรวจสอบ enable secret ที่ตั้งไว้
- ใช้ password recovery procedure

#### Problem 2: Interface Down

**Symptom:**

```
GigabitEthernet0/0     unassigned      YES unset  administratively down down
```

**Solution:**

```
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# no shutdown
```

#### Problem 3: Cannot Access via SSH

**Possible Causes:**

1. SSH not configured
2. No IP address on interface
3. VTY lines not configured
4. Firewall blocking
5. Wrong username/password

**Verification:**

```
R1# show ip ssh
R1# show ssh
R1# show line vty 0 4
```

#### Problem 4: Configuration Not Saved

**Symptom:**

- Configuration lost after reload

**Solution:**

```
R1# copy running-config startup-config
```

### Useful Troubleshooting Commands

```
show version                    (IOS version, uptime, hardware)
show running-config             (Current configuration)
show startup-config             (Saved configuration)
show ip interface brief         (Interface summary)
show interfaces                 (Interface details)
show controllers               (Physical layer information)
show history                    (Command history)
show clock                      (System clock)
show users                      (Active users)
show sessions                   (Outgoing connections)
debug                          (Real-time troubleshooting)
```

---

## Key Configuration Examples (ตัวอย่างการตั้งค่าสำคัญ)

### Complete Basic Router Configuration

```
Router> enable
Router# configure terminal
Router(config)# hostname R1
R1(config)# enable secret Str0ng!P@ssw0rd
R1(config)# service password-encryption

! Console
R1(config)# line console 0
R1(config-line)# password Cons0le!P@ss
R1(config-line)# login
R1(config-line)# logging synchronous
R1(config-line)# exec-timeout 5 0
R1(config-line)# exit

! VTY Lines (SSH Only)
R1(config)# ip domain-name example.com
R1(config)# crypto key generate rsa modulus 2048
R1(config)# ip ssh version 2
R1(config)# username admin privilege 15 secret Adm!n!P@ss
R1(config)# line vty 0 4
R1(config-line)# transport input ssh
R1(config-line)# login local
R1(config-line)# exec-timeout 5 0
R1(config-line)# exit

! Banner
R1(config)# banner motd #
******************************************
* UNAUTHORIZED ACCESS PROHIBITED!       *
******************************************
#

! Interface
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# description Link to LAN
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit

! Save
R1(config)# end
R1# copy running-config startup-config
```

### Complete Basic Switch Configuration

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname SW1
SW1(config)# enable secret Str0ng!P@ssw0rd
SW1(config)# service password-encryption

! Console
SW1(config)# line console 0
SW1(config-line)# password Cons0le!P@ss
SW1(config-line)# login
SW1(config-line)# logging synchronous
SW1(config-line)# exit

! VTY Lines
SW1(config)# line vty 0 15
SW1(config-line)# password VTY!P@ssw0rd
SW1(config-line)# login
SW1(config-line)# transport input ssh
SW1(config-line)# exit

! Management Interface
SW1(config)# interface vlan 1
SW1(config-if)# description Management Interface
SW1(config-if)# ip address 192.168.1.10 255.255.255.0
SW1(config-if)# no shutdown
SW1(config-if)# exit
SW1(config)# ip default-gateway 192.168.1.1

! Banner
SW1(config)# banner motd #Authorized Access Only#

! Save
SW1(config)# end
SW1# write memory
```

---

## Summary (สรุป)

Module 2 นี้เราได้เรียนรู้:

1. ✅ **การเข้าถึง Cisco IOS** - Console, SSH, Telnet
2. ✅ **User Modes** - User EXEC, Privileged EXEC, Configuration Modes
3. ✅ **การตั้งค่าพื้นฐาน** - Hostname, Passwords, Banner
4. ✅ **การบันทึกการตั้งค่า** - Running-config vs Startup-config
5. ✅ **การตั้งค่า Interface** - IP address, Status
6. ✅ **การตั้งค่า SSH** - Secure remote access
7. ✅ **การแก้ปัญหาเบื้องต้น** - Troubleshooting commands

**Next Module:** Module 3 - Protocols and Models (OSI, TCP/IP)