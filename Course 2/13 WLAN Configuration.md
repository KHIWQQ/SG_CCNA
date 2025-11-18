# CCNA 2: Module 13 - WLAN Configuration

## การ Configure Wireless LAN

---

## สารบัญ

1. [วัตถุประสงค์ของ Module](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#%E0%B8%A7%E0%B8%B1%E0%B8%95%E0%B8%96%E0%B8%B8%E0%B8%9B%E0%B8%A3%E0%B8%B0%E0%B8%AA%E0%B8%87%E0%B8%84%E0%B9%8C%E0%B8%82%E0%B8%AD%E0%B8%87-module)
2. [Remote Site WLAN Configuration](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#1-remote-site-wlan-configuration)
3. [Configure a Basic WLAN on the WLC](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#2-configure-a-basic-wlan-on-the-wlc)
4. [Configure a WPA2 Enterprise WLAN](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#3-configure-a-wpa2-enterprise-wlan)
5. [Troubleshoot WLAN Issues](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#4-troubleshoot-wlan-issues)
6. [สรุป](https://claude.ai/chat/a47cd23c-c8e5-4c7b-8ac2-e8ed06927782#%E0%B8%AA%E0%B8%A3%E0%B8%B8%E0%B8%9B)

---

## วัตถุประสงค์ของ Module

เมื่อจบ Module นี้ คุณจะสามารถ:

- ✅ Configure wireless router สำหรับ remote access
- ✅ Configure basic WLAN บน WLC
- ✅ Configure WPA2-PSK security
- ✅ Configure WPA2-Enterprise security with RADIUS
- ✅ Troubleshoot common WLAN issues
- ✅ Verify WLAN connectivity

---

## 1. Remote Site WLAN Configuration

### การ Configure WLAN สำหรับ Remote Site

### 1.1 Wireless Router Overview

**Wireless Router (SOHO) คืออะไร:**

```
Combined Device ที่รวม:
✓ Router (Layer 3 routing)
✓ Switch (4-port typically)
✓ Access Point (wireless)
✓ DHCP Server
✓ NAT/PAT
✓ Basic Firewall

Use Case:
- Home networks
- Small offices
- Branch offices
- Retail stores

Popular Brands:
- Linksys
- Netgear
- D-Link
- TP-Link
- ASUS
```

**Typical Wireless Router Features:**

```
Wireless:
- 802.11n/ac/ax
- 2.4 GHz and/or 5 GHz
- WPA2/WPA3 security
- Guest network
- Parental controls

Wired:
- 4-5 Ethernet ports (1 WAN, 4 LAN)
- Gigabit speeds typical

Services:
- DHCP server
- DNS relay
- NAT/PAT
- Port forwarding
- DMZ
- QoS
- VPN passthrough
```

### 1.2 Initial Setup - Physical Connections

**Wireless Router Setup:**

```
Topology:
[Internet] ─ [Modem] ─ [Wireless Router]
                            │
                    ┌───────┼───────┐
                    │       │       │
                 [PC via  [Laptop] [Phone]
                  cable]  wireless wireless

Connections:
1. WAN port → Modem
2. LAN port → PC (for initial setup)
3. Power → Power adapter

Initial Access:
- Connect PC to LAN port
- PC gets DHCP address (usually 192.168.1.x)
- Access web interface via browser
```

**Default Settings (Typical):**

```
IP Address: 192.168.1.1 or 192.168.0.1
Subnet Mask: 255.255.255.0
DHCP Pool: 192.168.1.100-200
Username: admin (or blank)
Password: admin (or printed on label)
SSID: Default_SSID or Brand_Model
Security: Open or WPA2-PSK with default password
```

### 1.3 Basic Router Configuration

**Access Web Interface:**

```
Step 1: Connect PC to router LAN port

Step 2: Open web browser
URL: http://192.168.1.1
or http://192.168.0.1

Step 3: Login
Username: admin
Password: admin (or check router label)

Step 4: Basic Setup Wizard (usually appears)
- Set admin password
- Set timezone
- Configure internet connection
- Configure wireless settings
```

**Change Admin Password (FIRST STEP!):**

```
Location: Administration or System
Old Password: admin
New Password: Strong_Password_123!

⚠️ CRITICAL: Change default password immediately!
Default credentials = major security risk

Best Practice:
- 12+ characters
- Mix of upper/lower/numbers/symbols
- Unique (not used elsewhere)
- Store securely
```

**Configure Internet Connection:**

```
WAN Settings:

Connection Type Options:
1. DHCP (Automatic IP)
   - Most common for home
   - ISP provides IP automatically
   - Just select "Automatic/DHCP"

2. Static IP
   - ISP provides specific IP
   - Enter: IP, Subnet, Gateway, DNS

3. PPPoE
   - DSL connections
   - Enter: Username, Password from ISP

Example (DHCP):
Connection Type: Automatic Configuration - DHCP
→ Save/Apply

Router will get IP from ISP modem
```

**Configure LAN Settings:**

```
LAN IP Address:
Default: 192.168.1.1
Change if needed: 192.168.10.1 (example)
Subnet Mask: 255.255.255.0

DHCP Server:
Status: Enabled
Starting IP: 192.168.10.100
Maximum Users: 50
Lease Time: 86400 seconds (24 hours)

DNS:
Primary: 8.8.8.8 (Google)
Secondary: 8.8.4.4 (Google)
or use ISP's DNS

Save/Apply
```

### 1.4 Wireless Configuration

**Basic Wireless Settings:**

```
Frequency Band Selection:
Option 1: 2.4 GHz only
- Better range, slower speed
- More interference
- Older devices

Option 2: 5 GHz only
- Shorter range, faster speed
- Less interference
- Modern devices

Option 3: Dual-band (Both)
- Best of both
- Two separate SSIDs or Smart Connect
- Recommended
```

**2.4 GHz Configuration:**

```
Network Mode: Mixed (802.11b/g/n) or n-only
Channel Width: 20 MHz (recommended)
Channel: Auto or 1, 6, 11 (manual)

Network Name (SSID): MyHomeNetwork_2.4G
Visibility: Broadcast SSID (recommended)

Settings:
Wireless Network Mode: Mixed
Channel Width: 20MHz
Channel: 6 (or Auto)
SSID: Home_2G
SSID Broadcast: Enabled

Save/Apply
```

**5 GHz Configuration:**

```
Network Mode: Mixed (802.11a/n/ac) or ac-only
Channel Width: 40 MHz or 80 MHz
Channel: Auto (or 36, 40, 44, 48, 149, 153, 157, 161)

Network Name (SSID): MyHomeNetwork_5G
Visibility: Broadcast SSID

Settings:
Wireless Network Mode: Mixed (a/n/ac)
Channel Width: 80MHz
Channel: 149 (or Auto)
SSID: Home_5G
SSID Broadcast: Enabled

Save/Apply
```

### 1.5 Wireless Security Configuration

**WPA2-Personal (PSK) Setup:**

```
Security Mode: WPA2-Personal
Encryption: AES
Passphrase: Strong_WiFi_Password_2024!

Requirements:
- 8-63 characters
- Mix of upper/lower/numbers/symbols
- Easy to remember but hard to guess

Example Configuration:
Security Mode: WPA2-Personal
Encryption: AES
Pre-Shared Key: MySecureWiFi2024!

Save/Apply

⚠️ Everyone uses same password
✓ Simple for home/small office
✓ No additional server needed
```

**WPA3-Personal (if available):**

```
Security Mode: WPA3-Personal
Encryption: AES
Passphrase: Strong_WiFi_Password_2024!

Benefits over WPA2:
✓ Better password protection
✓ Forward secrecy
✓ Protection against offline attacks
✓ Easier password requirements

Compatibility:
- Newer devices only
- May need WPA2/WPA3 mixed mode
```

**Complete Wireless Security Settings:**

```
2.4 GHz Wireless:
SSID: Home_2G
Security: WPA2-Personal
Encryption: AES
Passphrase: HomeWiFi2024_2GHz!

5 GHz Wireless:
SSID: Home_5G
Security: WPA2-Personal
Encryption: AES
Passphrase: HomeWiFi2024_5GHz!

Or use same passphrase for both
```

### 1.6 Guest Network Configuration

**Guest Network Setup:**

```
Purpose:
- Provide WiFi to visitors
- Isolate from main network
- No access to internal resources

Configuration:
Guest Network: Enabled
SSID: Guest_WiFi
Security: WPA2-Personal
Passphrase: GuestAccess2024
Guest Access: Enabled
Access to Local Network: Disabled

Settings:
✓ Enable Guest Network
✓ Different SSID from main
✓ WPA2 security (or Open with captive portal)
✓ Isolation from LAN
✓ Optional: Time limits, bandwidth limits
```

### 1.7 Additional Router Features

**Port Forwarding:**

```
Purpose:
- Allow external access to internal server
- Forward specific ports to specific device

Example - Web Server:
Service: HTTP
External Port: 80
Internal IP: 192.168.10.100
Internal Port: 80
Protocol: TCP

Example - Gaming:
Service: Xbox Live
External Port: 3074
Internal IP: 192.168.10.50
Internal Port: 3074
Protocol: Both (TCP/UDP)

Configuration:
Applications & Gaming → Port Forwarding
Add new rule
Save/Apply
```

**DMZ (Demilitarized Zone):**

```
Purpose:
- Forward ALL ports to one device
- Expose device directly to internet
- Use for gaming consoles, servers

⚠️ Security Risk!
- Device fully exposed
- Use only if necessary
- Better: Use specific port forwarding

Configuration:
DMZ Host IP: 192.168.10.100
Enable DMZ

Use Case:
- Gaming console with NAT issues
- Temporary testing
- When port forwarding doesn't work
```

**QoS (Quality of Service):**

```
Purpose:
- Prioritize traffic
- Ensure important traffic gets bandwidth
- Reduce lag for gaming/video

Configuration:
QoS: Enabled

Priority Levels:
High: Gaming, Video Calls
Medium: Streaming Video
Normal: Web Browsing
Low: Downloads, Updates

Example:
Device: Gaming PC (192.168.10.50)
Priority: High

Application: Netflix
Priority: Medium

Save/Apply
```

**Parental Controls:**

```
Features:
- Block websites
- Time restrictions
- Content filtering

Example:
Device: Kid's Laptop
MAC: xx:xx:xx:xx:xx:xx
Allowed Times: 6pm-9pm weekdays
Blocked Categories: Adult, Gambling
```

### 1.8 Wireless Router Best Practices

**Security Best Practices:**

```
✅ Change default admin password
✅ Use WPA2 or WPA3
✅ Strong WiFi password (20+ characters)
✅ Disable WPS (vulnerable)
✅ Disable remote management (if not needed)
✅ Update firmware regularly
✅ Enable firewall
✅ Use guest network for visitors
✅ Disable UPnP (unless needed)
✅ Review connected devices regularly
```

**Performance Best Practices:**

```
✅ Position router centrally
✅ Elevate (shelf/mount)
✅ Away from obstacles
✅ Away from interference sources
✅ Use 5 GHz for speed
✅ Use 2.4 GHz for range
✅ Select best channel (1, 6, 11 for 2.4 GHz)
✅ Update firmware
✅ Reboot periodically
```

---

## 2. Configure a Basic WLAN on the WLC

### การ Configure WLAN บน Wireless LAN Controller

### 2.1 WLC Overview

**Wireless LAN Controller (WLC) Components:**

```
Enterprise Deployment:
[WLC] ← CAPWAP tunnels → [LAP1] [LAP2] [LAP3] [LAP4]
  │                         ↓      ↓      ↓      ↓
  │                      Wireless Clients
  │
[Network/Internet]

WLC manages:
- Multiple APs (up to 500+ depending on model)
- Centralized configuration
- RF management
- Security policies
- Client mobility
- Load balancing
```

**Cisco WLC Models (Examples):**

```
Cisco 3504 WLC:
- 150 APs support
- 4096 VLANs
- 5 physical ports
- Small to medium enterprise

Cisco 5520 WLC:
- 1500 APs support
- Enterprise scale
- High availability
- Large deployments

Cisco 9800 WLC:
- Next-generation
- IOS XE based
- High performance
- Latest features
```

### 2.2 Access WLC GUI

**Initial Access:**

```
Connection:
1. Connect PC to WLC management port or switch
2. Ensure PC has IP in same subnet as WLC

Default Settings (varies by model):
Management IP: 192.168.1.1 or configured during setup
Username: admin
Password: admin or configured
Protocol: HTTPS

Access Method:
1. Open web browser
2. Enter: https://192.168.100.254 (example WLC IP)
3. Accept security certificate (self-signed)
4. Enter credentials

Note: Initial setup wizard may run on first boot
```

**WLC GUI Interface:**

```
Main Areas:
1. Monitor - Dashboard and statistics
2. WLANs - Wireless network configuration
3. Controller - System settings
4. Wireless - RF and AP management
5. Security - Authentication and policies
6. Management - Admin and maintenance

Dashboard Shows:
- System information
- Number of APs
- Number of clients
- Top WLANs
- Rogue APs
- Alarms
```

### 2.3 WLC Ports and Interfaces

**Physical Ports vs Virtual Interfaces:**

**Ports (Physical):**

```
Physical connections to network

Example - Cisco 3504 WLC:
- 5 Gigabit Ethernet ports
- Console port (RJ-45)
- Service port (out-of-band management)

Port Functions:
- Service Port: Out-of-band management
- Distribution System Ports: Connect to network
- LAG (Link Aggregation): Combine ports

Configuration:
Ports are typically left as default
Connect to switch trunk ports
```

**Interfaces (Virtual):**

```
Logical interfaces for traffic separation

Interface Types:
1. Management Interface
2. AP-Manager Interface
3. Virtual Interface
4. Dynamic Interface (WLAN-specific)
5. Service Port Interface

Each interface can be on different VLAN
```

**Interface Types Explained:**

**1. Management Interface:**

```
Purpose:
- WLC management traffic
- Web GUI access
- SSH/Telnet access
- SNMP
- NTP, Syslog

Configuration:
IP Address: 192.168.100.254
Netmask: 255.255.255.0
Gateway: 192.168.100.1
VLAN: 100 (Management VLAN)
```

**2. AP-Manager Interface:**

```
Purpose:
- CAPWAP tunnel endpoint
- AP management traffic
- Layer 3 communication with APs

Configuration:
IP Address: 192.168.100.253
Netmask: 255.255.255.0
Gateway: 192.168.100.1
VLAN: 100

Note: Can use same interface as Management in small deployments
```

**3. Virtual Interface:**

```
Purpose:
- DHCP relay
- Guest web authentication
- Mobility management
- Not tied to physical port

Configuration:
IP Address: 1.1.1.1 (dummy IP, commonly used)
No gateway needed
No physical association
```

**4. Dynamic Interface:**

```
Purpose:
- WLAN-specific interface
- Map WLAN to VLAN
- Each WLAN needs own interface

Example:
Interface Name: employee-interface
IP Address: 192.168.10.254
Netmask: 255.255.255.0
Gateway: 192.168.10.1
VLAN: 10
Primary DHCP: 192.168.10.1

WLAN "Employee" maps to this interface
```

### 2.4 Configure a New Interface

**Create Dynamic Interface for WLAN:**

```
Step 1: Navigate to Interfaces
Controller → Interfaces → New

Step 2: Basic Configuration
Interface Name: guest-interface
VLAN Identifier: 20
Port Number: 1 (or LAG)

Step 3: IP Configuration
IP Address: 192.168.20.254
Netmask: 255.255.255.0
Gateway: 192.168.20.1

Step 4: DHCP
Primary DHCP Server: 192.168.20.1
(typically the gateway)

Step 5: Apply
Click Apply

Result:
Interface created for Guest WLAN
```

**Complete Example - Create Interface:**

```
Interface Configuration:

Name: employee-interface
VLAN ID: 10
Physical Port: 1

IP Settings:
IP: 192.168.10.254
Mask: 255.255.255.0
Gateway: 192.168.10.1

DHCP:
Primary: 192.168.10.1
Secondary: (optional)

DNS Domain: company.local (optional)

Apply Configuration
```

### 2.5 Create a New WLAN

**WLAN Creation Process:**

```
Step 1: Navigate to WLANs
WLANs → New

Step 2: Basic WLAN Information
Type: WLAN
Profile Name: Employee-WLAN
SSID: Employee-WiFi
ID: 2 (auto-assigned or choose 1-16)

Step 3: Apply to create base WLAN

Step 4: Configure WLAN
```

**WLAN General Tab:**

```
General Configuration:

Profile Name: Employee-WLAN
SSID: Employee-WiFi
Status: Enabled
Broadcast SSID: Enabled
Radio Policy: All (2.4 GHz + 5 GHz)

Interface/VLAN:
Interface: employee-interface
(maps to VLAN 10)

FlexConnect:
Local Switching: Enabled (if using FlexConnect)
Local Auth: (as needed)

Apply
```

**WLAN Advanced Tab:**

```
Advanced Settings:

Allow AAA Override: Disabled (default)
NAC State: None
Web Auth: Disabled
Security: (configure in Security tab)

Coverage Hole Detection: Enabled
Peer-to-Peer Blocking: Disabled (or enable to block client-to-client)

Quality of Service (QoS):
QoS: Silver (default)
or select: Platinum, Gold, Silver, Bronze

Apply
```

**Complete Example - Basic WLAN:**

```
Create Employee WLAN:

WLANs → New
Type: WLAN
Profile Name: Employee
SSID: Employee-WiFi
ID: 2
Apply

General Tab:
Status: Enabled
SSID Broadcast: Enabled
Radio Policy: All
Interface: employee-interface

Security Tab:
Layer 2 Security: WPA+WPA2
WPA2 Policy: Enabled
Auth Key Mgmt: PSK
PSK Format: ASCII
Pre-Shared Key: EmployeePass2024!

Apply

Result: WLAN created, not yet secured
```

### 2.6 Configure WLAN Security (WPA2-PSK)

**WPA2-Personal Configuration:**

```
Navigate to: WLANs → Select WLAN → Security Tab

Layer 2 Security:
Security: WPA+WPA2
(allows both WPA and WPA2 clients)

WPA2 Policy: Enabled
WPA Policy: Enabled (optional, for older devices)

Authentication Key Management:
PSK: Enabled

PSK Format: ASCII
Pre-Shared Key: Strong_Password_2024!
(8-63 characters)

Encryption:
AES Encryption (CCMP): Enabled
TKIP: Disabled (weak, deprecated)

Apply
```

**Step-by-Step WPA2-PSK:**

```
Step 1: Navigate to WLAN Security
WLANs → Select WLAN → Security → Layer 2

Step 2: Select Security Type
Layer 2 Security: WPA+WPA2

Step 3: Enable WPA2
☑ WPA2 Policy

Step 4: Select Authentication
Auth Key Management:
☑ PSK

Step 5: Enter Passphrase
PSK Format: ASCII
Pre-Shared Key: SecureWiFi2024!

Step 6: Verify Encryption
☑ AES Encryption

Step 7: Apply Configuration
Click Apply

Step 8: Verify
WLAN should show "WPA2-PSK" in security column
```

**Security Best Practices:**

```
✅ Use WPA2 (minimum)
✅ Disable WPA if not needed
✅ Disable TKIP (use AES only)
✅ Strong passphrase (20+ characters)
✅ Avoid dictionary words
✅ Change passphrase periodically
✅ Different passphrase per WLAN
```

### 2.7 Verify WLAN Configuration

**Verification Steps:**

```
Step 1: Check WLAN Status
WLANs → View WLANs
Verify: Status = Enabled

Step 2: Check WLAN Details
Click on WLAN name
Verify:
- SSID correct
- Interface mapped
- Security configured
- Broadcast enabled

Step 3: Check APs
Wireless → Access Points
Verify:
- APs registered
- APs associated with WLC
- Operating state: Registered

Step 4: Test Client Connection
From client device:
- Scan for SSID
- Connect with passphrase
- Verify IP address (from DHCP)
- Ping gateway
- Ping internet (8.8.8.8)
```

**WLC Monitoring:**

```
Monitor Dashboard:

System Information:
- WLC uptime
- Software version
- IP address

Access Points:
- Total APs: X
- Associated APs: X
- Registered APs: X

Clients:
- Total clients: X
- Clients per WLAN: breakdown

Top WLANs:
- WLAN name
- Number of clients
- Traffic volume
```

**Show Commands (CLI):**

```
If using CLI:

> show wlan summary
Lists all WLANs with ID, SSID, status

> show wlan <id>
Detailed WLAN configuration

> show ap summary
List all APs with status

> show client summary
List all connected clients

> show interface summary
List all interfaces
```

---

## 3. Configure a WPA2 Enterprise WLAN

### การ Configure WPA2 Enterprise

### 3.1 WPA2-Enterprise Overview

**WPA2-Enterprise vs WPA2-Personal:**

```
WPA2-Personal (PSK):
- Single shared password
- Everyone uses same key
- Simple
- Good for: Home, small office

WPA2-Enterprise (802.1X):
- Individual user credentials
- Username/password per user
- RADIUS authentication
- Complex but scalable
- Good for: Enterprises, schools

Components Needed:
- WLC with WPA2-Enterprise config
- RADIUS server
- User database (in RADIUS)
- 802.1X supplicant (client)
```

### 3.2 RADIUS Server Overview

**RADIUS Server Role:**

```
Purpose:
- Authenticate users
- Authorize access
- Account for usage (AAA)

Architecture:
[Client] ←→ [WLC/AP] ←→ [RADIUS Server]
Supplicant  Authenticator  Auth Server

Process:
1. Client provides credentials
2. WLC forwards to RADIUS
3. RADIUS validates credentials
4. RADIUS returns Accept/Reject
5. WLC allows/denies network access
```

**RADIUS Server Software:**

```
Options:
1. Cisco ISE (Identity Services Engine)
   - Full-featured
   - Policy management
   - Posture assessment
   - Enterprise-grade

2. Microsoft NPS (Network Policy Server)
   - Built into Windows Server
   - Active Directory integration
   - Free with Windows Server

3. FreeRADIUS
   - Open source
   - Linux-based
   - Free but requires expertise

4. ClearPass (Aruba/HPE)
   - Enterprise solution
   - Multi-vendor support
```

### 3.3 Configure RADIUS on WLC

**Add RADIUS Server:**

```
Step 1: Navigate to RADIUS Servers
Security → RADIUS → Authentication

Step 2: Click New

Step 3: Configure RADIUS Server
Server IP Address: 10.1.1.100
Shared Secret: RadiusSecret123!
(must match RADIUS server config)

Port: 1812 (default authentication port)
Server Status: Enabled
Support for RFC 3576: Disabled (or enable if needed)
Server Timeout: 2 seconds
Network User: Enabled
Management: Disabled

Step 4: Apply

Step 5: Verify Server Added
Should appear in RADIUS servers list
```

**Complete RADIUS Configuration:**

```
RADIUS Authentication Server:

Index: 1
Server Address: 10.1.1.100
Shared Secret: RadiusSecret123!
Port: 1812
Status: Enabled
Timeout: 2
Network User: Enabled

RADIUS Accounting Server (Optional):
Server Address: 10.1.1.100
Shared Secret: RadiusSecret123!
Port: 1813
Status: Enabled

Apply

Note: Can have multiple RADIUS servers (primary, backup)
```

### 3.4 Configure WPA2-Enterprise WLAN

**Create Enterprise WLAN:**

```
Step 1: Create New WLAN
WLANs → New

Profile Name: Enterprise-WLAN
SSID: Enterprise-WiFi
ID: 3
Apply

Step 2: General Configuration
Status: Enabled
Broadcast SSID: Enabled
Radio Policy: All
Interface: employee-interface (appropriate interface)
```

**Configure Security:**

```
Step 3: Security Tab → Layer 2
Layer 2 Security: WPA+WPA2

Enable Checkboxes:
☑ WPA2 Policy
☐ WPA Policy (optional)

Authentication Key Management:
☑ 802.1X

Encryption:
☑ AES Encryption
☐ TKIP (disable)

Step 4: RADIUS Servers
AAA Servers:
Select RADIUS servers from dropdown
- Server 1: 10.1.1.100 (primary)
- Server 2: 10.1.1.101 (backup, if available)

Step 5: Apply Configuration
Click Apply

Result: WPA2-Enterprise WLAN created
```

**Advanced Security Settings:**

```
Layer 2 Security Options:

Fast Transition (802.11r):
- Enable for fast roaming
- Mobility Domain ID: specify
- Over-the-DS: Enable

Management Frame Protection (802.11w):
- Required (for WPA3)
- Optional
- Disabled

Session Timeout:
- Time after which client must re-authenticate
- 0 = disabled
- Typical: 3600-28800 seconds

Idle Timeout:
- Disconnect idle clients
- 0 = disabled
- Typical: 300-900 seconds
```

### 3.5 RADIUS Server Configuration (Concepts)

**User Database Configuration:**

```
RADIUS Server must have:

Users/Groups:
Username: john.doe@company.com
Password: UserPassword123
Group: Employees
VLAN: 10 (optional, dynamic)

Username: jane.smith@company.com
Password: UserPassword456
Group: Employees
VLAN: 10

Username: guest1@company.com
Password: GuestPass789
Group: Guests
VLAN: 20

Policies:
- Group "Employees" → VLAN 10, Full access
- Group "Guests" → VLAN 20, Internet only
```

**RADIUS Client Configuration:**

```
On RADIUS Server:
Add WLC as RADIUS client

Client Configuration:
Name: WLC-1
IP Address: 192.168.100.254 (WLC management IP)
Shared Secret: RadiusSecret123!
(must match WLC config)

Allow:
- Authentication
- Accounting
- CoA (Change of Authorization, optional)

Apply

Note: Each WLC = separate RADIUS client
```

### 3.6 EAP Method Selection

**EAP Types for 802.1X:**

```
Common EAP Methods:

PEAP-MSCHAPv2:
- Most common
- Username/password
- Server certificate only
- Microsoft compatible
- Easy to deploy

EAP-TLS:
- Most secure
- Client and server certificates
- No password
- Complex deployment
- Best security

EAP-FAST (Cisco):
- Cisco proprietary
- No certificates needed
- PAC (Protected Access Credential)
- Easier than EAP-TLS

Configuration on WLC:
Usually accept multiple methods
RADIUS server determines allowed methods
```

### 3.7 Client Configuration

**Windows Client (PEAP):**

```
Step 1: Connect to SSID
- Scan for "Enterprise-WiFi"
- Select network

Step 2: Authentication Prompt
- Enter username: john.doe@company.com
- Enter password: UserPassword123

Step 3: Certificate Verification
- Verify server certificate
- Trust certificate (first time)

Step 4: Connect
- Client authenticates via RADIUS
- Receives IP via DHCP
- Connected!

Automatic Configuration (Optional):
- Group Policy (Windows AD)
- Configuration profile (Mac/iOS)
- Pre-configure certificate trust
```

**Manual Configuration (Windows):**

```
Network Properties → Security:

Security type: WPA2-Enterprise
Encryption: AES
Authentication: Microsoft: Protected EAP (PEAP)

Settings (PEAP):
☑ Validate server certificate
Connect to these servers: radius.company.com
Trusted Root CA: (select certificate)
Authentication Method: EAP-MSCHAPv2

☐ Enable Fast Reconnect

Remember credentials: (optional)

OK → Connect
```

### 3.8 Verify WPA2-Enterprise

**Verification Steps:**

```
1. Check WLAN Status:
WLANs → Verify "Enterprise-WiFi" enabled

2. Check RADIUS Status:
Security → RADIUS → Authentication
Verify: Server Active, reachable

3. Test Client Connection:
Client → Connect to SSID
Enter username/password
Verify: Connected, IP received

4. Check Active Clients:
Monitor → Clients
View connected clients:
- Username
- SSID
- IP address
- Auth method: 802.1X

5. RADIUS Server Logs:
Check authentication logs
Verify: Access-Accept sent
```

**Troubleshooting Commands:**

```
WLC CLI:

> show client detail <mac>
Shows client auth status, VLAN, IP

> debug client <mac>
Real-time client debug
Shows auth process

> show radius summary
RADIUS server status

> show radius statistics
Authentication attempts, success/fail

RADIUS Server:
Check authentication logs
Verify shared secret match
Check user database
```

---

## 4. Troubleshoot WLAN Issues

### การแก้ปัญหา WLAN

### 4.1 Common WLAN Issues

**Issue Categories:**

```
1. Connectivity Issues
   - Can't connect
   - Frequent disconnections
   - No IP address

2. Performance Issues
   - Slow speeds
   - High latency
   - Packet loss

3. Coverage Issues
   - Weak signal
   - Dead zones
   - Roaming problems

4. Security Issues
   - Authentication failures
   - Wrong credentials
   - Certificate errors

5. Configuration Issues
   - Wrong VLAN
   - DHCP problems
   - AP not joining WLC
```

### 4.2 Troubleshooting Methodology

**Systematic Approach:**

```
Step 1: Define the Problem
- What is not working?
- When did it start?
- How many users affected?
- Consistent or intermittent?

Step 2: Gather Information
- Check user device
- Check AP status
- Check WLC logs
- Check network connectivity

Step 3: Analyze Information
- Identify potential causes
- Prioritize by likelihood

Step 4: Implement Solution
- Make one change at a time
- Document changes

Step 5: Verify Solution
- Test connectivity
- Monitor for recurrence

Step 6: Document
- Record problem and solution
- Update knowledge base
```

### 4.3 Cannot Connect to WLAN

**Problem: Client Cannot See SSID**

```
Possible Causes:

1. SSID not broadcasting
   Check: WLAN → General → Broadcast SSID
   Solution: Enable broadcast

2. WLAN disabled
   Check: WLAN status
   Solution: Enable WLAN

3. AP not registered
   Check: Wireless → Access Points
   Solution: Troubleshoot AP connectivity

4. Wrong frequency
   Check: Client supports 2.4/5 GHz?
   Check: WLAN Radio Policy
   Solution: Enable appropriate radio

5. Out of range
   Check: Signal strength at location
   Solution: Add AP or adjust power

Verification:
- WLAN enabled: ✓
- SSID broadcast: ✓
- APs registered: ✓
- Client in range: ✓
```

**Problem: Client Cannot Authenticate**

```
WPA2-Personal Issues:

1. Wrong password
   Symptom: "Incorrect password" error
   Solution: Verify passphrase
   - Check caps lock
   - Check for typos
   - Verify with known-good device

2. Security mismatch
   Symptom: Can't connect, no error
   Check: Client supports WPA2?
   Solution: Check WLAN security settings

3. MAC filtering
   Check: Is MAC filtering enabled?
   Solution: Add client MAC to allowed list

WPA2-Enterprise Issues:

1. Wrong credentials
   Check: Username/password correct?
   Check: Account not expired/locked?
   Solution: Reset password on RADIUS

2. RADIUS server down
   Check: WLC can reach RADIUS?
   Command: ping <radius-ip>
   Solution: Fix RADIUS server

3. Shared secret mismatch
   Symptom: RADIUS rejects all users
   Check: Shared secret matches on WLC and RADIUS
   Solution: Update shared secret

4. Certificate error
   Symptom: Certificate warning
   Solution: Install trusted certificate
   Or: Configure client to trust cert

5. EAP method mismatch
   Check: Client and RADIUS support same EAP?
   Solution: Configure compatible EAP method
```

### 4.4 Connectivity but No Network Access

**Problem: Connected but No IP**

```
Possible Causes:

1. DHCP server not configured
   Check: Interface → DHCP Server setting
   Solution: Configure DHCP server IP

2. DHCP server not responding
   Check: DHCP server reachable?
   Test: Ping DHCP server from WLC
   Solution: Fix DHCP server

3. DHCP pool exhausted
   Check: DHCP scope size vs clients
   Solution: Expand DHCP pool

4. Interface/VLAN misconfigured
   Check: WLAN → General → Interface
   Check: Interface → VLAN ID
   Solution: Correct VLAN mapping

5. Client DHCP client disabled
   Check: Client network settings
   Solution: Enable "Obtain IP automatically"

Troubleshooting:
1. Check client: ipconfig /all (Windows)
   IP: 169.254.x.x = No DHCP (APIPA)
   
2. Manual IP test:
   Assign static IP in correct subnet
   If works → DHCP issue
   If not → routing/VLAN issue

3. Verify DHCP on WLC:
   Monitor → Clients → Select client
   Check: IP address assignment
```

**Problem: Has IP but No Internet**

```
Possible Causes:

1. No default gateway
   Check: Client ipconfig
   Solution: Configure gateway in DHCP/interface

2. DNS not configured
   Check: Client DNS settings
   Solution: Configure DNS in DHCP scope

3. ACL blocking traffic
   Check: WLAN → Advanced → ACL
   Solution: Modify or remove restrictive ACL

4. Firewall blocking
   Check: Security policies
   Solution: Adjust firewall rules

5. Upstream network issue
   Check: WLC can reach internet?
   Test: Ping 8.8.8.8 from WLC
   Solution: Fix upstream routing

Troubleshooting:
1. Test DNS:
   nslookup google.com
   If fails → DNS issue

2. Test gateway:
   ping <gateway>
   If fails → gateway/routing issue

3. Test internet:
   ping 8.8.8.8
   If fails → upstream issue
   
4. Test internal:
   ping <internal server>
   Isolate problem to internet or all connectivity
```

### 4.5 Performance Issues

**Problem: Slow WiFi Speeds**

```
Possible Causes:

1. Interference
   Check: Channel utilization
   Tool: WLC RF monitoring
   Solution: Change channel

2. Too many clients on AP
   Check: Clients per AP
   Typical max: 25-50 per AP
   Solution: Add APs, enable load balancing

3. Client far from AP
   Check: RSSI (signal strength)
   Good: -50 dBm or better
   Marginal: -60 to -70 dBm
   Poor: -70 dBm or worse
   Solution: Move client or add AP

4. Client on 2.4 GHz
   Check: Client radio
   Solution: Move client to 5 GHz

5. Bandwidth congestion
   Check: AP throughput
   Solution: Add APs, implement QoS

6. Old client hardware
   Check: Client supports 802.11ac/ax?
   Solution: Upgrade client adapter

Troubleshooting:
1. Speed test from client
2. Compare with wired connection
3. Test at different times
4. Test different clients
5. Check RF environment
```

**Problem: Frequent Disconnections**

```
Possible Causes:

1. Weak signal
   Check: Signal strength
   Solution: Improve coverage

2. Interference
   Check: Spectrum analysis
   Solution: Change channel or frequency

3. Roaming issues
   Check: Cell overlap
   Solution: Adjust AP power, add APs

4. Session timeout
   Check: WLAN → Security → Session Timeout
   Solution: Increase timeout

5. Client power saving
   Check: Client power settings
   Solution: Disable aggressive power saving

6. AP capacity
   Check: Clients per AP
   Solution: Load balance or add APs

Troubleshooting:
1. Check client logs
2. Check WLC client detail:
   show client detail <mac>
3. Check disconnect reason codes
4. Monitor over time
```

### 4.6 AP Troubleshooting

**Problem: AP Not Joining WLC**

```
Possible Causes:

1. No network connectivity
   Check: AP has link light?
   Check: Switch port enabled?
   Solution: Fix physical connection

2. AP can't reach WLC
   Check: AP and WLC on same network?
   Or: Routing configured?
   Solution: Fix network connectivity

3. AP can't discover WLC
   Check: DHCP option 43 configured?
   Or: DNS configured for WLC?
   Solution: Configure DHCP option or DNS

4. Wrong AP mode
   Check: AP in lightweight mode?
   Solution: Convert to lightweight mode if needed

5. WLC at capacity
   Check: WLC AP capacity
   Check: WLC license
   Solution: Upgrade WLC or add WLC

6. AP and WLC version mismatch
   Check: Software compatibility
   Solution: Upgrade AP or WLC

Troubleshooting:
1. Check AP status lights
   - Solid green = connected
   - Flashing = discovering/downloading
   - Red = error

2. Console into AP:
   Check IP address
   Ping WLC
   Check CAPWAP status

3. Check WLC:
   Wireless → Access Points
   Look for AP (MAC or name)

4. Debug on WLC:
   debug capwap events enable
   debug capwap errors enable
```

### 4.7 WLC Troubleshooting Tools

**WLC Monitoring:**

```
Monitor → Summary:
- System info
- AP count
- Client count
- Rogue APs

Monitor → Clients:
- List all clients
- Click client for details:
  - SSID, VLAN, IP
  - RSSI, SNR
  - Data rates
  - Auth method

Monitor → Access Points:
- List all APs
- Click AP for details:
  - Operating state
  - Clients per AP
  - Channel, power
  - Utilization
```

**CLI Commands:**

```
Useful Commands:

> show wlan summary
Lists all WLANs

> show ap summary  
Lists all APs

> show client summary
Lists all clients

> show client detail <mac>
Detailed client info

> show interface summary
Lists interfaces

> show radius summary
RADIUS server status

> debug client <mac>
Real-time client debug
(Use: undebug all to stop)

> show ap config general <ap-name>
AP configuration

> show wireless stats
Wireless statistics
```

**Logs and Traps:**

```
Management → Logs:
- View system logs
- Filter by severity
- Search for events

Management → SNMP → Trap Logs:
- SNMP traps received
- AP join/leave
- Client events
- Rogue AP detected

Export Logs:
- Download for analysis
- Send to syslog server
```

### 4.8 Quick Troubleshooting Checklist

**Client Cannot Connect:**

```
☑ WLAN enabled?
☑ SSID broadcasting?
☑ Correct passphrase?
☑ Client in range?
☑ APs registered?
☑ Compatible security?
☑ RADIUS reachable? (Enterprise)
```

**Client Has No Internet:**

```
☑ IP address assigned?
☑ Gateway configured?
☑ DNS configured?
☑ Can ping gateway?
☑ Can ping 8.8.8.8?
☑ VLAN correct?
☑ Routing configured?
```

**Performance Issues:**

```
☑ Signal strength adequate? (>-65 dBm)
☑ Too many clients per AP?
☑ Interference on channel?
☑ Client on 5 GHz? (for speed)
☑ QoS configured?
☑ Client hardware capable?
```

**AP Issues:**

```
☑ AP powered on?
☑ Network connectivity?
☑ Can reach WLC?
☑ DHCP option 43 configured?
☑ WLC has capacity?
☑ Software compatible?
```

---

## สรุป

### สิ่งที่ได้เรียนรู้ใน Module 13:

✅ **Remote Site WLAN Configuration:**

- Wireless router setup
- Admin access and security
- WAN/LAN configuration
- Wireless settings (SSID, channels)
- WPA2-Personal security
- Guest networks
- Advanced features (port forwarding, QoS, DMZ)

✅ **Basic WLAN on WLC:**

- WLC components and interface
- Ports vs Interfaces
- Create dynamic interfaces
- Create and configure WLANs
- WPA2-PSK security
- WLAN verification

✅ **WPA2-Enterprise Configuration:**

- 802.1X architecture
- RADIUS server integration
- Add RADIUS server to WLC
- Configure WPA2-Enterprise WLAN
- EAP methods
- Client configuration
- Verification

✅ **Troubleshooting:**

- Systematic approach
- Common connectivity issues
- Authentication problems
- Performance issues
- AP issues
- WLC monitoring tools
- Troubleshooting checklists

---

## คำสั่งและขั้นตอนสำคัญ

### Create WLAN on WLC:

```
1. WLANs → New
2. Profile Name, SSID, ID
3. Apply
4. General → Enable, Interface
5. Security → Layer 2 Security
6. Apply
```

### WPA2-PSK Security:

```
Security Tab:
- Layer 2 Security: WPA+WPA2
- WPA2 Policy: Enabled
- Auth Key Mgmt: PSK
- Pre-Shared Key: <password>
- AES Encryption: Enabled
```

### WPA2-Enterprise Security:

```
1. Add RADIUS Server:
   Security → RADIUS → Authentication → New
   Server IP, Shared Secret

2. Configure WLAN Security:
   Security Tab:
   - Layer 2 Security: WPA+WPA2
   - WPA2 Policy: Enabled
   - Auth Key Mgmt: 802.1X
   - RADIUS Servers: Select server
```

### Create Interface:

```
Controller → Interfaces → New
- Name, VLAN ID, Port
- IP Address, Mask, Gateway
- Primary DHCP Server
- Apply
```

---

## Best Practices

### Wireless Router (SOHO):

```
✅ Change default admin password immediately
✅ Use WPA2/WPA3
✅ Strong WiFi password (20+ characters)
✅ Disable WPS
✅ Update firmware regularly
✅ Enable firewall
✅ Use guest network
✅ Disable remote management
```

### WLC Deployment:

```
✅ Plan VLAN structure
✅ Create interface per WLAN
✅ Consistent naming conventions
✅ Document configuration
✅ Test before production
✅ Monitor client connectivity
✅ Regular backups
✅ Firmware updates (planned)
```

### Security:

```
✅ WPA2-Personal: SOHO only
✅ WPA2-Enterprise: Business
✅ Strong passwords always
✅ RADIUS redundancy
✅ Monitor authentication failures
✅ Regular security audits
✅ Least privilege access
```

---

## Common Issues Solutions

|Issue|Quick Fix|
|---|---|
|Can't see SSID|Enable WLAN, enable broadcast|
|Wrong password|Verify passphrase, check caps lock|
|No IP address|Check DHCP server, verify interface|
|No internet|Check gateway, DNS, routing|
|Slow speeds|Change channel, add APs, use 5 GHz|
|AP not joining|Check connectivity, DHCP option 43|
|Auth failing|Check RADIUS, shared secret, credentials|

---

## Lab Activities

### Lab 13.1: Configure Wireless Router

- Basic router configuration
- Wireless settings
- WPA2-PSK security
- Guest network
- Verify connectivity

### Lab 13.2: Configure Basic WLAN on WLC (Packet Tracer)

- Access WLC GUI
- Create interface
- Create WLAN
- Configure WPA2-PSK
- Connect clients

### Lab 13.3: Configure WPA2-Enterprise

- Configure RADIUS server
- Add RADIUS to WLC
- Create Enterprise WLAN
- Test authentication

### Lab 13.4: Troubleshooting WLAN

- Identify issues
- Use monitoring tools
- Resolve problems
- Document solutions

---

## คำถามทบทวน

1. ต่างระหว่าง Port และ Interface ใน WLC อย่างไร?
2. Dynamic Interface ใช้ทำอะไร?
3. WPA2-Personal ต่างจาก WPA2-Enterprise อย่างไร?
4. RADIUS Server ทำหน้าที่อะไรใน WPA2-Enterprise?
5. ถ้า client ไม่เห็น SSID ต้องเช็คอะไรบ้าง?
6. ถ้า client ได้ IP 169.254.x.x แสดงว่าอะไร?
7. RSSI คืออะไร? ค่าเท่าไหร่ถือว่าดี?
8. Channel 1, 6, 11 สำคัญอย่างไร?
9. AP ไม่ join WLC อาจเพราะอะไร?
10. Guest Network ควร configure อย่างไร?

---

## เฉลยคำถาม

1. **Port vs Interface:** Port = physical connection; Interface = virtual (VLAN-based)
2. **Dynamic Interface:** Map WLAN to specific VLAN, ให้ IP/gateway สำหรับ WLAN
3. **Personal vs Enterprise:** Personal = shared password; Enterprise = individual credentials + RADIUS
4. **RADIUS:** Authenticate users, validate credentials, return accept/reject
5. **ไม่เห็น SSID:** Check WLAN enabled, broadcast enabled, AP registered, in range
6. **169.254.x.x:** APIPA address = No DHCP server responding
7. **RSSI:** Received Signal Strength; Good: -50 dBm or better; Poor: -70 dBm or worse
8. **1, 6, 11:** Non-overlapping channels ใน 2.4 GHz, หลีกเลี่ยง interference
9. **AP ไม่ join:** No network connectivity, can't reach WLC, wrong mode, capacity limit
10. **Guest Network:** Separate SSID/VLAN, isolated from LAN, internet only, optional time limit

---

**หมายเหตุ:** WLAN Configuration เป็นทักษะสำคัญสำหรับ network administrators การเข้าใจทั้ง SOHO wireless routers และ enterprise WLC deployments จะทำให้สามารถรองรับทั้งองค์กรขนาดเล็กและขนาดใหญ่ได้

---

**เอกสารจัดทำโดย:** Claude (Anthropic AI)  
**Module:** CCNA 2 Module 13 - WLAN Configuration  
**Version:** v7.02 SRWE  
**วันที่อัพเดท:** พฤศจิกายน 2025