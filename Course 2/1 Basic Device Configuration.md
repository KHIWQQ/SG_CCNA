# CCNA 2: Module 1 - Basic Device Configuration

## การตั้งค่าอุปกรณ์เครือข่ายเบื้องต้น

---

## สารบัญ

1. [วัตถุประสงค์ของ Module](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#%E0%B8%A7%E0%B8%B1%E0%B8%95%E0%B8%96%E0%B8%B8%E0%B8%9B%E0%B8%A3%E0%B8%B0%E0%B8%AA%E0%B8%87%E0%B8%84%E0%B9%8C%E0%B8%82%E0%B8%AD%E0%B8%87-module)
2. [Configure a Switch with Initial Settings](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#1-configure-a-switch-with-initial-settings)
3. [Switch Boot Sequence](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#2-switch-boot-sequence)
4. [Configure Switch Ports](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#3-configure-switch-ports)
5. [Secure Remote Access](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#4-secure-remote-access)
6. [Basic Router Configuration](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#5-basic-router-configuration)
7. [สรุป](https://claude.ai/chat/5ad62f10-a325-41c3-9806-4a0a28af590d#%E0%B8%AA%E0%B8%A3%E0%B8%B8%E0%B8%9B)

---

## วัตถุประสงค์ของ Module

เมื่อจบ Module นี้ คุณจะสามารถ:

- ✅ Configure initial settings บน Cisco IOS switch
- ✅ อธิบาย Switch boot sequence และ verify process
- ✅ Configure switch ports including speed, duplex, และ MDIX settings
- ✅ Configure secure remote access โดยใช้ SSH
- ✅ Verify switch configuration และ troubleshoot common issues
- ✅ Configure basic router settings

---

## 1. Configure a Switch with Initial Settings

### การตั้งค่าเริ่มต้นของ Switch

### 1.1 ทำไมต้องตั้งค่า Switch?

Switch ที่ออกจากโรงงานมาจะ:

- ไม่มีชื่ออุปกรณ์ (hostname)
- ไม่มีการตั้งรหัสผ่าน
- ไม่มีการตั้งค่า IP address สำหรับการจัดการ
- ทุกพอร์ตอยู่ใน VLAN 1 (default)
- ไม่มีการรักษาความปลอดภัย

**การตั้งค่าเบื้องต้นที่จำเป็น:**

1. ตั้งชื่ออุปกรณ์ (hostname)
2. กำหนดรหัสผ่านสำหรับ privileged EXEC mode
3. ตั้งค่า IP address สำหรับการจัดการ (Management IP)
4. กำหนด default gateway
5. Configure console และ VTY lines สำหรับ remote access
6. บันทึกการตั้งค่า (save configuration)

### 1.2 Switch Configuration Mode

**User EXEC Mode:**

```
Switch>
```

- โหมดพื้นฐาน จำกัดคำสั่ง
- ใช้สำหรับดูข้อมูลเบื้องต้น

**Privileged EXEC Mode:**

```
Switch> enable
Switch#
```

- เข้าถึงคำสั่งทั้งหมด
- สามารถดูและแก้ไข configuration

**Global Configuration Mode:**

```
Switch# configure terminal
Switch(config)#
```

- ใช้สำหรับตั้งค่าที่ส่งผลต่อทั้ง Switch

### 1.3 Basic Switch Configuration Example

```cisco
! เข้าสู่ Global Configuration Mode
Switch> enable
Switch# configure terminal

! ตั้งชื่อ Switch
Switch(config)# hostname S1
S1(config)#

! กำหนดรหัสผ่าน privileged EXEC mode (encrypted)
S1(config)# enable secret cisco123

! ป้องกันการแสดงรหัสผ่านในรูปแบบ plain text
S1(config)# service password-encryption

! Configure console line
S1(config)# line console 0
S1(config-line)# password consolepass
S1(config-line)# login
S1(config-line)# logging synchronous
S1(config-line)# exit

! Configure VTY lines (for Telnet/SSH - รองรับ 16 connections)
S1(config)# line vty 0 15
S1(config-line)# password vtypass
S1(config-line)# login
S1(config-line)# exit

! ตั้งค่า banner (ข้อความเตือน)
S1(config)# banner motd #
******************************************
*  Unauthorized access is prohibited!  *
*  All activities are monitored.       *
******************************************
#

! บันทึกการตั้งค่า
S1(config)# exit
S1# copy running-config startup-config
```

### 1.4 Configure Management IP Address

Switch ต้องการ IP address เพื่อ:

- Remote management (SSH, Telnet)
- Monitoring และ troubleshooting
- SNMP management

**การตั้งค่า Management IP บน SVI (Switch Virtual Interface):**

```cisco
! เข้าสู่ VLAN 1 interface (default management VLAN)
S1(config)# interface vlan 1

! กำหนด IP address และ subnet mask
S1(config-if)# ip address 192.168.1.10 255.255.255.0

! อธิบาย interface
S1(config-if)# description Management Interface

! เปิดใช้งาน interface
S1(config-if)# no shutdown

! ออกจาก interface configuration mode
S1(config-if)# exit

! กำหนด default gateway
S1(config)# ip default-gateway 192.168.1.1
```

**คำอธิบาย:**

- `interface vlan 1` - VLAN 1 คือ management VLAN เริ่มต้น
- `ip address` - กำหนด IP สำหรับจัดการ Switch
- `no shutdown` - เปิดใช้งาน interface (default คือปิด)
- `ip default-gateway` - เส้นทางสำหรับติดต่อเครือข่ายอื่น

### 1.5 Verify Switch Configuration

**ตรวจสอบการตั้งค่าที่บันทึกแล้ว:**

```cisco
S1# show startup-config
```

**ตรวจสอบการตั้งค่าที่ใช้งานอยู่:**

```cisco
S1# show running-config
```

**ตรวจสอบข้อมูลเบื้องต้นของ Switch:**

```cisco
S1# show version
```

แสดงข้อมูล:

- IOS version
- System uptime
- Serial number
- Configuration register

**ตรวจสอบสถานะ interface:**

```cisco
S1# show ip interface brief
```

แสดงข้อมูล:

- Interface name
- IP address
- Status (up/down)
- Protocol (up/down)

**ตรวจสอบ VLAN:**

```cisco
S1# show vlan brief
```

---

## 2. Switch Boot Sequence

### กระบวนการบูตของ Switch

### 2.1 Switch Boot Process

เมื่อ Switch เริ่มทำงาน จะมีขั้นตอนดังนี้:

**ขั้นตอนที่ 1: Power-On Self Test (POST)**

- ตรวจสอบฮาร์ดแวร์ทั้งหมด
- ตรวจสอบ CPU, RAM, Flash memory
- ไฟ LED จะกะพริบเพื่อแสดงสถานะ

**ขั้นตอนที่ 2: Load Boot Loader**

- โหลดโปรแกรม Boot Loader จาก ROM
- Boot Loader เป็นโปรแกรมเล็กๆ ที่ใช้จัดการการบูต

**ขั้นตอนที่ 3: Initialize Flash File System**

- เริ่มต้นระบบไฟล์ใน Flash memory
- ค้นหาไฟล์ IOS image

**ขั้นตอนที่ 4: Load IOS**

- โหลด Cisco IOS จาก Flash memory เข้า RAM
- ถ้าไม่พบ IOS จะเข้าสู่ ROMMON mode

**ขั้นตอนที่ 5: Load Configuration**

- โหลด startup-config จาก NVRAM เข้า running-config ใน RAM
- ถ้าไม่มี startup-config จะเข้าสู่ setup mode

### 2.2 Boot System Command

คำสั่ง `boot system` ใช้กำหนดว่าจะบูต IOS จากไหน:

```cisco
! บูตจากไฟล์ IOS เฉพาะใน Flash
S1(config)# boot system flash:/c2960-lanbasek9-mz.150-2.SE/c2960-lanbasek9-mz.150-2.SE.bin

! บูตจาก TFTP server (backup)
S1(config)# boot system tftp://192.168.1.100/c2960-lanbasek9-mz.150-2.SE.bin

! บูตจาก ROM (emergency mode)
S1(config)# boot system rom

! ดูคำสั่ง boot system ที่ตั้งค่าไว้
S1# show boot
```

**ลำดับการบูต:**

1. ตามคำสั่ง `boot system` ที่กำหนดไว้
2. ถ้าไม่มีคำสั่ง จะบูตไฟล์ IOS แรกที่เจอใน Flash
3. ถ้าไม่มีใน Flash จะบูตจาก TFTP server
4. ถ้าล้มเหลวจะบูตจาก ROM (limited IOS)

### 2.3 Switch LED Indicators

### สัญญาณไฟ LED บน Switch

Switch Cisco มี LED หลายดวงเพื่อแสดงสถานะ:

**System LED:**

- **สีเขียว** - ระบบทำงานปกติ
- **สีเหลือง/Amber** - ระบบมีปัญหา (POST failed)
- **ดับ** - ไม่มีไฟเลี้ยง

**Port Status LED:**

- **ดับ** - ไม่มีการเชื่อมต่อ หรือ port ถูก shutdown
- **สีเขียว** - มีการเชื่อมต่อและทำงานปกติ
- **กะพริบสีเขียว** - กำลังส่ง/รับข้อมูล
- **สีเหลือง** - Layer 1 มีปัญหา หรือถูก blocked โดย STP
- **สีเขียวและเหลืองสลับกัน** - Link fault error

**Port Speed LED:**

- **ดับ** - 10 Mbps
- **สีเขียว** - 100 Mbps
- **กะพริบสีเขียว** - 1000 Mbps (Gigabit)

**Port Duplex LED:**

- **ดับ** - Half duplex
- **สีเขียว** - Full duplex

**PoE LED (สำหรับ Switch ที่รองรับ PoE):**

- **ดับ** - PoE ปิด
- **สีเขียว** - PoE เปิดและทำงานปกติ
- **กะพริบสีเหลือง** - PoE ถูกปิดเนื่องจาก fault หรือ overload

### 2.4 Recovering from a System Crash

**กรณีที่ Switch ไม่สามารถบูตได้:**

1. **เข้าสู่ ROMMON Mode:**
    
    - กด Mode button ค้างไว้ตอนเปิดเครื่อง
    - หรือ boot fail จะเข้า ROMMON อัตโนมัติ
2. **ตรวจสอบไฟล์ใน Flash:**
    
    ```
    switch: dir flash:
    ```
    
3. **บูตด้วยตนเอง:**
    
    ```
    switch: boot flash:c2960-lanbasek9-mz.150-2.SE.bin
    ```
    
4. **กู้คืนผ่าน TFTP:**
    
    - ตั้งค่า IP address
    - Download IOS ใหม่จาก TFTP server
    
    ```
    switch: set IP_ADDR 192.168.1.10
    switch: set NETMASK 255.255.255.0
    switch: set DEFAULT_ROUTER 192.168.1.1
    switch: set TFTP_SERVER 192.168.1.100
    switch: set TFTP_FILE c2960-lanbasek9-mz.150-2.SE.bin
    switch: tftp
    ```
    
5. **Password Recovery:**
    
    - เข้า ROMMON mode
    - ข้าม startup-config
    
    ```
    switch: flash_init
    switch: load_helper
    switch: dir flash:
    switch: rename flash:config.text flash:config.old
    switch: boot
    ```
    
    - หลังบูตเสร็จ เปลี่ยนรหัสผ่านใหม่
    - Rename config กลับและโหลดใหม่

---

## 3. Configure Switch Ports

### การตั้งค่าพอร์ตของ Switch

### 3.1 Duplex Communication

**Half Duplex:**

- ส่งหรือรับข้อมูลได้ครั้งละทิศทางเดียว
- ใช้ CSMA/CD เพื่อตรวจจับ collision
- ความเร็วต่ำกว่า Full Duplex

**Full Duplex:**

- ส่งและรับข้อมูลได้พร้อมกันทั้งสองทิศทาง
- ไม่มี collision
- ใช้แบนด์วิธได้เต็มประสิทธิภาพ
- **Best Practice:** ตั้งเป็น Full Duplex เสมอถ้าได้

**Duplex Mismatch:**

- ปัญหาที่เกิดเมื่อทั้งสองฝั่งตั้งค่า duplex ไม่ตรงกัน
- ทำให้ประสิทธิภาพต่ำ มี late collisions
- ต้องตั้งให้ตรงกันทั้งสองฝั่ง

### 3.2 Configure Switch Ports at Physical Layer

**การตั้งค่าพอร์ตพื้นฐาน:**

```cisco
! เข้าสู่ interface configuration mode
S1(config)# interface fastethernet 0/1

! หรือใช้รูปแบบย่อ
S1(config)# interface fa0/1

! กำหนด description
S1(config-if)# description Connected to PC1

! ตั้งค่า speed (10, 100, 1000, auto)
S1(config-if)# speed 100

! ตั้งค่า duplex (half, full, auto)
S1(config-if)# duplex full

! เปิดใช้งานพอร์ต
S1(config-if)# no shutdown

! ปิดพอร์ต (administratively down)
S1(config-if)# shutdown

! ออกจาก interface mode
S1(config-if)# exit
```

**ตั้งค่าหลายพอร์ตพร้อมกัน (Interface Range):**

```cisco
! เลือกหลายพอร์ต
S1(config)# interface range fastethernet 0/1 - 24

! หรือ
S1(config)# interface range fa0/1 - 24

! ตั้งค่าพอร์ตทั้งหมด
S1(config-if-range)# description Access Ports
S1(config-if-range)# speed 100
S1(config-if-range)# duplex full
S1(config-if-range)# no shutdown
```

### 3.3 Auto-MDIX

**คืออะไร:**

- Automatic Medium-Dependent Interface Crossover
- ตรวจจับและปรับประเภทของสายอัตโนมัติ
- ไม่ต้องกังวลว่าใช้ Straight-through หรือ Crossover cable

**ประเภทการเชื่อมต่อ:**

- **Straight-through cable:** Switch ↔ Computer, Router ↔ Computer
- **Crossover cable:** Switch ↔ Switch, Router ↔ Router, Computer ↔ Computer

**เปิดใช้งาน Auto-MDIX:**

```cisco
S1(config)# interface fastethernet 0/1

! เปิด Auto-MDIX (ต้องตั้ง speed และ duplex เป็น auto ก่อน)
S1(config-if)# speed auto
S1(config-if)# duplex auto
S1(config-if)# mdix auto
```

**หมายเหตุ:**

- Auto-MDIX ทำงานได้ก็ต่อเมื่อ speed และ duplex เป็น auto
- Switch รุ่นใหม่เปิด Auto-MDIX เป็นค่า default

### 3.4 Switch Verification Commands

**ตรวจสอบสถานะพอร์ต:**

```cisco
! แสดงสรุปสถานะ interface ทั้งหมด
S1# show ip interface brief

! แสดงรายละเอียด interface เฉพาะ
S1# show interface fastethernet 0/1

! แสดงสถานะ interface แบบย่อ
S1# show interface status

! แสดงเฉพาะ interface ที่ up
S1# show interface status | include connected

! แสดงการตั้งค่า running configuration ของ interface
S1# show running-config interface fa0/1
```

**ผลลัพธ์ตัวอย่าง:**

```
S1# show interface fastethernet 0/1
FastEthernet0/1 is up, line protocol is up (connected)
  Hardware is Fast Ethernet, address is 0cd9.96d2.3f01
  Description: Connected to PC1
  MTU 1500 bytes, BW 100000 Kbit/sec, DLY 100 usec
  Encapsulation ARPA, loopback not set
  Keepalive set (10 sec)
  Full-duplex, 100Mb/s, media type is 10/100BaseTX
  Input flow-control is off, output flow-control is unsupported
  ...
```

**การอ่านผลลัพธ์:**

- `FastEthernet0/1 is up` - Physical layer (Layer 1) ทำงานปกติ
- `line protocol is up` - Data link layer (Layer 2) ทำงานปกติ
- `Full-duplex, 100Mb/s` - ความเร็วและโหมด duplex

### 3.5 Network Access Layer Issues

**ปัญหาที่พบบ่อยใน Layer 1 และ Layer 2:**

1. **Port is administratively down:**
    
    - สาเหตุ: ใช้คำสั่ง `shutdown`
    - แก้ไข: `no shutdown`
2. **Port is down:**
    
    - สาเหตุ: ไม่มีการเชื่อมต่อ, สายเคเบิลเสีย
    - แก้ไข: ตรวจสอบสายและการเชื่อมต่อ
3. **Duplex mismatch:**
    
    - สาเหตุ: ทั้งสองฝั่งตั้งค่า duplex ไม่ตรงกัน
    - แก้ไข: ตั้งค่าให้เหมือนกันทั้งสองฝั่ง หรือใช้ auto
4. **Speed mismatch:**
    
    - สาเหตุ: ความเร็วไม่ตรงกัน
    - แก้ไข: ตั้งค่าความเร็วให้เหมือนกัน
5. **Interface errors:**
    
    - CRC errors
    - Runts (packets ที่เล็กเกินไป)
    - Giants (packets ที่ใหญ่เกินไป)

### 3.6 Interface Input and Output Errors

**ตรวจสอบ errors:**

```cisco
S1# show interface fastethernet 0/1
```

**ประเภท errors ที่สำคัญ:**

**Input Errors:**

- **Runts:** Frames ที่มีขนาดเล็กกว่า 64 bytes
- **Giants:** Frames ที่มีขนาดใหญ่กว่า 1518 bytes
- **CRC:** Frame ที่ผิดพลาดใน FCS (Frame Check Sequence)
- **Input errors:** จำนวน errors ทั้งหมดที่ input

**Output Errors:**

- **Collisions:** จำนวนครั้งที่เกิด collision
- **Late collisions:** Collision ที่เกิดหลังส่งไป 64 bytes แล้ว
- **Output errors:** จำนวน errors ทั้งหมดที่ output

**ตัวอย่างผลลัพธ์:**

```
  5 minute input rate 1000 bits/sec, 1 packets/sec
  5 minute output rate 2000 bits/sec, 2 packets/sec
  1234 packets input, 567890 bytes, 0 no buffer
  Received 1200 broadcasts, 34 runts, 0 giants, 0 throttles
  12 input errors, 10 CRC, 2 frame, 0 overrun, 0 ignored
  0 watchdog, 1100 multicast, 0 pause input
  0 input packets with dribble condition detected
  2345 packets output, 678901 bytes, 0 underruns
  0 output errors, 5 collisions, 1 interface resets
```

**การแก้ไข errors:**

1. ตรวจสอบสายเคเบิล
2. ตรวจสอบ NIC card
3. ตรวจสอบ duplex/speed settings
4. เปลี่ยนสายหรือพอร์ต
5. อัพเดท driver

### 3.7 Troubleshooting Network Access Layer

**ขั้นตอนการแก้ไขปัญหา:**

1. **ตรวจสอบ Physical Connection:**
    
    ```cisco
    S1# show interface status
    S1# show interface fa0/1
    ```
    
2. **ตรวจสอบ Configuration:**
    
    ```cisco
    S1# show running-config interface fa0/1
    ```
    
3. **ตรวจสอบ Speed และ Duplex:**
    
    ```cisco
    S1# show interface fa0/1 | include duplex
    ```
    
4. **ตรวจสอบ Errors:**
    
    ```cisco
    S1# show interface fa0/1 | include error
    ```
    
5. **รีเซ็ต Interface:**
    
    ```cisco
    S1(config)# interface fa0/1
    S1(config-if)# shutdown
    S1(config-if)# no shutdown
    ```
    
6. **Clear Counter:**
    
    ```cisco
    S1# clear counters fa0/1
    ```
    

---

## 4. Secure Remote Access

### การตั้งค่าการเข้าถึงระยะไกลอย่างปลอดภัย

### 4.1 Telnet Operation

**Telnet คืออะไร:**

- โปรโตคอลสำหรับ remote access
- ใช้ TCP port 23
- **ไม่ปลอดภัย** - ส่งข้อมูลแบบ plain text
- ควรใช้เฉพาะในเครือข่ายปลอดภัยหรือ lab เท่านั้น

**ข้อเสียของ Telnet:**

- Username และ password ไม่มีการเข้ารหัส
- ข้อมูลถูกส่งแบบ clear text
- เสี่ยงต่อการดักข้อมูล (eavesdropping)

### 4.2 SSH Operation

**SSH (Secure Shell) คืออะไร:**

- โปรโตคอลสำหรับ remote access แบบปลอดภัย
- ใช้ TCP port 22
- เข้ารหัสข้อมูลทั้งหมด (encryption)
- **Best Practice:** ใช้ SSH แทน Telnet เสมอ

**ข้อดีของ SSH:**

- ข้อมูลถูกเข้ารหัส (encrypted)
- Authentication ที่ปลอดภัย
- ป้องกันการดักข้อมูล
- รองรับ SSH version 1 และ 2 (ใช้ v2)

**SSH version:**

- **SSHv1:** รุ่นเก่า มีช่องโหว่ความปลอดภัย
- **SSHv2:** รุ่นปัจจุบัน ปลอดภัยกว่า ควรใช้รุ่นนี้เสมอ

### 4.3 Verify Switch Supports SSH

**ตรวจสอบว่า Switch รองรับ SSH:**

```cisco
S1# show version
```

ตรวจสอบใน output ว่ามี:

- **k9** ใน IOS image name (เช่น c2960-lanbasek9-mz)
- k9 = crypto image = รองรับ SSH

**ตัวอย่าง:**

```
System image file is "flash:c2960-lanbasek9-mz.150-2.SE.bin"
```

**ถ้าไม่มี k9:**

- Switch ไม่รองรับ SSH
- ต้องใช้ Telnet หรืออัพเกรด IOS

### 4.4 Configure SSH

**ขั้นตอนการตั้งค่า SSH:**

```cisco
! ขั้นตอนที่ 1: ตรวจสอบว่า SSH support
S1# show version | include k9

! ขั้นตอนที่ 2: ตั้งค่า hostname และ domain name
S1(config)# hostname S1
S1(config)# ip domain-name cisco.com

! ขั้นตอนที่ 3: สร้าง RSA key pair
S1(config)# crypto key generate rsa
How many bits in the modulus [512]: 1024

! ขั้นตอนที่ 4: สร้าง local username และ password
S1(config)# username admin privilege 15 secret Admin123

! ขั้นตอนที่ 5: Configure VTY lines สำหรับ SSH
S1(config)# line vty 0 15
S1(config-line)# transport input ssh
S1(config-line)# login local
S1(config-line)# exit

! ขั้นตอนที่ 6: ระบุ SSH version 2
S1(config)# ip ssh version 2

! ขั้นตอนที่ 7: ตั้งค่า timeout และ authentication retries
S1(config)# ip ssh time-out 60
S1(config)# ip ssh authentication-retries 3
```

**คำอธิบายคำสั่ง:**

- `ip domain-name` - จำเป็นสำหรับการสร้าง RSA keys
- `crypto key generate rsa` - สร้าง encryption keys
    - 1024 bits = ปลอดภัยพอใช้
    - 2048 bits = ปลอดภัยมากขึ้น
- `username ... privilege 15` - สร้าง user ที่มีสิทธิ์เต็ม
- `transport input ssh` - อนุญาตเฉพาะ SSH (ปิด Telnet)
- `login local` - ใช้ local username database
- `ip ssh version 2` - บังคับใช้ SSH version 2
- `ip ssh time-out 60` - disconnect หลัง idle 60 วินาที
- `ip ssh authentication-retries 3` - ลองล็อกอินได้ 3 ครั้ง

**หมายเหตุสำคัญ:**

- ต้องมี hostname และ domain name ก่อนสร้าง RSA key
- ถ้าเปลี่ยน hostname หรือ domain name ต้องสร้าง key ใหม่
- การสร้าง key ใช้เวลาสักครู่

### 4.5 Verify SSH is Operational

**ตรวจสอบการตั้งค่า SSH:**

```cisco
! ตรวจสอบ SSH version และการตั้งค่า
S1# show ip ssh

! ตรวจสอบ SSH sessions ที่ active
S1# show ssh

! ตรวจสอบ RSA keys
S1# show crypto key mypubkey rsa

! ดูการตั้งค่า VTY line
S1# show running-config | section line vty
```

**ผลลัพธ์ตัวอย่าง:**

```
S1# show ip ssh
SSH Enabled - version 2.0
Authentication timeout: 60 secs; Authentication retries: 3
```

**ทดสอบ SSH connection:**

```cisco
! จากอุปกรณ์อื่นใน network
PC> ssh -l admin 192.168.1.10

! หรือ
PC> ssh admin@192.168.1.10
```

### 4.6 การลบ SSH Configuration

**ลบ RSA keys:**

```cisco
S1(config)# crypto key zeroize rsa
```

**อนุญาตทั้ง SSH และ Telnet:**

```cisco
S1(config)# line vty 0 15
S1(config-line)# transport input ssh telnet
```

**อนุญาตทุกโปรโตคอล:**

```cisco
S1(config-line)# transport input all
```

---

## 5. Basic Router Configuration

### การตั้งค่า Router เบื้องต้น

### 5.1 ความแตกต่างระหว่าง Switch และ Router

**Switch:**

- ทำงานที่ Layer 2 (Data Link)
- ใช้ MAC address ในการส่งต่อ
- Forward frames ภายใน LAN เดียวกัน
- ไม่แบ่ง broadcast domain

**Router:**

- ทำงานที่ Layer 3 (Network)
- ใช้ IP address ในการส่งต่อ
- Forward packets ระหว่าง network
- แบ่ง broadcast domain

### 5.2 Basic Router Configuration

**การตั้งค่า Router คล้ายกับ Switch:**

```cisco
! เข้าสู่ privileged EXEC mode
Router> enable
Router# configure terminal

! ตั้งชื่อ Router
Router(config)# hostname R1

! กำหนดรหัสผ่าน
R1(config)# enable secret Cisco123
R1(config)# service password-encryption

! Configure console line
R1(config)# line console 0
R1(config-line)# password consolepass
R1(config-line)# login
R1(config-line)# logging synchronous
R1(config-line)# exit

! Configure VTY lines
R1(config)# line vty 0 4
R1(config-line)# password vtypass
R1(config-line)# login
R1(config-line)# exit

! ตั้งค่า banner
R1(config)# banner motd #Authorized Access Only!#
```

### 5.3 Configure Router Interfaces

**Router interfaces โดย default จะ shutdown:**

```cisco
! Configure Gigabit Ethernet interface
R1(config)# interface gigabitethernet 0/0

! หรือใช้รูปแบบย่อ
R1(config)# interface g0/0

! กำหนด description
R1(config-if)# description Link to LAN 1

! กำหนด IP address
R1(config-if)# ip address 192.168.1.1 255.255.255.0

! เปิด interface
R1(config-if)# no shutdown

! ตั้งค่า clock rate (สำหรับ serial interface ที่เป็น DCE)
R1(config-if)# clock rate 64000
```

**Configure Serial Interface:**

```cisco
R1(config)# interface serial 0/0/0
R1(config-if)# description Link to R2
R1(config-if)# ip address 10.1.1.1 255.255.255.252
R1(config-if)# clock rate 64000
R1(config-if)# no shutdown
```

**หมายเหตุ:**

- Router interfaces ต้อง `no shutdown` จึงจะทำงาน
- Serial interface ที่เป็น DCE ต้องตั้ง clock rate
- ตรวจสอบว่า interface เป็น DCE หรือ DTE:
    
    ```cisco
    R1# show controllers serial 0/0/0
    ```
    

### 5.4 Configure IPv6 on Router

**เปิดใช้งาน IPv6 routing:**

```cisco
! เปิด IPv6 routing (จำเป็น!)
R1(config)# ipv6 unicast-routing

! Configure IPv6 address บน interface
R1(config)# interface g0/0
R1(config-if)# ipv6 address 2001:db8:acad:1::1/64
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# no shutdown
```

### 5.5 Verify Router Configuration

**คำสั่งตรวจสอบ Router:**

```cisco
! แสดง running configuration
R1# show running-config

! แสดงข้อมูล interface
R1# show ip interface brief
R1# show ipv6 interface brief

! แสดงรายละเอียด interface
R1# show interface g0/0
R1# show interface serial 0/0/0

! แสดง routing table
R1# show ip route
R1# show ipv6 route

! แสดง IP protocols
R1# show ip protocols

! แสดงข้อมูล CDP neighbors
R1# show cdp neighbors
R1# show cdp neighbors detail
```

### 5.6 Default Gateway Configuration

**บน Switch:**

```cisco
S1(config)# ip default-gateway 192.168.1.1
```

**บน Router:**

- ใช้ Static Route หรือ Dynamic Routing
- จะเรียนรู้ใน Module ถัดไป

### 5.7 Save Configuration

**บันทึกการตั้งค่า:**

```cisco
! วิธีที่ 1
R1# copy running-config startup-config

! วิธีที่ 2 (รูปแบบย่อ)
R1# copy run start

! วิธีที่ 3 (สั้นที่สุด)
R1# write

! หรือใช้
R1# wr
```

**ลบการตั้งค่า:**

```cisco
! ลบ startup-config
R1# erase startup-config
R1# reload

! หรือ (รุ่นใหม่)
R1# write erase
R1# reload
```

---

## สรุป

### สิ่งที่ได้เรียนรู้ใน Module 1:

✅ **Switch Configuration:**

- Initial configuration (hostname, passwords, banner)
- Management IP address configuration
- Save และ verify configuration

✅ **Switch Boot Process:**

- POST, Boot Loader, Load IOS, Load Config
- Boot system commands
- LED indicators
- System recovery

✅ **Switch Port Configuration:**

- Speed และ duplex settings
- Auto-MDIX
- Interface range commands
- Troubleshooting interface issues

✅ **Secure Remote Access:**

- Telnet (ไม่ปลอดภัย)
- SSH configuration (ปลอดภัย)
- SSH version 2
- Local authentication

✅ **Router Configuration:**

- Basic router setup
- Interface configuration
- IPv4 และ IPv6 addressing
- Verification commands

---

## Lab Activities

### Lab 1: Basic Switch Configuration

**วัตถุประสงค์:**

- Configure hostname, passwords
- Configure management IP
- Save configuration

### Lab 2: Configure SSH

**วัตถุประสงค์:**

- Generate RSA keys
- Configure SSH version 2
- Test SSH connection

### Lab 3: Basic Router Configuration

**วัตถุประสงค์:**

- Configure router interfaces
- Configure IPv4 และ IPv6 addresses
- Verify connectivity

---

## Packet Tracer Activities

### Packet Tracer 1.3.6: Configure SSH

**สิ่งที่ต้องทำ:**

1. Configure hostname และ domain name
2. Generate RSA key
3. Create local user
4. Configure VTY lines
5. Test SSH connection

---

## คำสั่งสำคัญที่ต้องจำ

### Initial Configuration:

```cisco
hostname <name>
enable secret <password>
service password-encryption
banner motd #<message>#
```

### Interface Configuration:

```cisco
interface <type> <number>
description <text>
ip address <ip> <mask>
speed <10|100|1000|auto>
duplex <half|full|auto>
no shutdown
```

### SSH Configuration:

```cisco
ip domain-name <domain>
crypto key generate rsa
username <name> privilege 15 secret <password>
line vty 0 15
transport input ssh
login local
ip ssh version 2
```

### Verification:

```cisco
show running-config
show startup-config
show ip interface brief
show interface <interface>
show ip ssh
show ssh
```

---

## คำถามทบทวน

1. อะไรคือขั้นตอนทั้งหมดใน Switch Boot Sequence?
2. ต่างระหว่าง `enable password` และ `enable secret` อย่างไร?
3. ทำไมต้องใช้ SSH แทน Telnet?
4. `no shutdown` คำสั่งนี้ทำอะไร?
5. Auto-MDIX คืออะไร?
6. ต่างระหว่าง running-config และ startup-config อย่างไร?
7. คำสั่ง `show ip interface brief` แสดงอะไรบ้าง?
8. จะ configure SSH ต้องทำอะไรบ้าง?
9. Half-duplex และ Full-duplex ต่างกันอย่างไร?
10. จะตรวจสอบ errors บน interface ได้อย่างไร?

---

## เฉลยคำถาม

1. POST → Boot Loader → Initialize Flash → Load IOS → Load Configuration
2. `enable password` เก็บเป็น plain text, `enable secret` เข้ารหัสด้วย MD5
3. SSH เข้ารหัสข้อมูล (secure), Telnet ส่งเป็น plain text (ไม่ปลอดภัย)
4. เปิดใช้งาน interface ที่ถูก shutdown
5. ตรวจจับและปรับใช้สายได้อัตโนมัติ (straight/crossover)
6. running-config = active config ใน RAM, startup-config = saved config ใน NVRAM
7. แสดง interface name, IP address, status (up/down)
8. ตั้ง hostname/domain → สร้าง RSA key → สร้าง user → ตั้งค่า VTY → เปิด SSH v2
9. Half-duplex ส่งทีละทิศทาง, Full-duplex ส่งได้สองทิศทางพร้อมกัน
10. ใช้คำสั่ง `show interface <interface>` หรือ `show interface <interface> | include error`

---

**หมายเหตุ:** Module 1 เป็นพื้นฐานสำคัญสำหรับ Module ถัดไป ควรฝึกฝน Lab และ Packet Tracer ให้คล่อง เพราะจะใช้คำสั่งเหล่านี้ตลอดหลักสูตร

---

**เอกสารจัดทำโดย:** Claude (Anthropic AI)  
**Module:** CCNA 2 Module 1 - Basic Device Configuration  
**Version:** v7.02 SRWE  
**วันที่อัพเดท:** พฤศจิกายน 2025