# CCNA Course 3 - Module 14: Network Automation

## การทำงานอัตโนมัติของเครือข่าย

---

## 14.1 Automation Overview

### What is Network Automation?

**คำจำกัดความ:**

- **Network Automation** = การใช้ software เพื่อจัดการ network tasks โดยอัตโนมัติ
- ลดการทำงานด้วยมือ (manual)
- เพิ่มความเร็ว, ความแม่นยำ
- Consistent configuration
- Reduce human errors

**Traditional vs Automated:**

```
Traditional (Manual):
  - SSH to each device
  - Type commands manually
  - Copy/paste configurations
  - Time-consuming
  - Error-prone
  - Not scalable

Automated:
  - Script runs on all devices
  - Consistent configuration
  - Fast (seconds vs hours)
  - Reliable
  - Scalable (10 or 1000 devices, same effort)
```

---

### Why Automate?

**1. Speed:**

```
Manual: 10 minutes per device × 100 devices = 16.7 hours
Automated: 10 seconds per device × 100 devices = 16.7 minutes

100x faster!
```

**2. Consistency:**

```
Manual: Configuration variations (typos, different commands)
Automated: Identical configuration every time
```

**3. Reduce Errors:**

```
Manual: Human errors (typos, wrong device, forgot steps)
Automated: No typos, always follows script
```

**4. Scalability:**

```
Manual: Effort increases linearly (more devices = more time)
Automated: Same effort (10 or 10,000 devices)
```

**5. Documentation:**

```
Manual: May forget to document
Automated: Script is documentation (what was done)
```

**6. Compliance:**

```
Manual: Hard to verify all devices comply
Automated: Enforce standards automatically
```

---

### What Can Be Automated?

**Configuration Tasks:**

```
✅ Initial device configuration
✅ Configuration changes (VLAN, ACL, routing)
✅ Backup configurations
✅ Firmware upgrades
✅ Configuration compliance checking
```

**Monitoring Tasks:**

```
✅ Health checks (CPU, memory, interface status)
✅ Log collection
✅ Performance monitoring
✅ Alert generation
```

**Troubleshooting Tasks:**

```
✅ Run diagnostic commands
✅ Collect show command outputs
✅ Compare configurations
✅ Find configuration differences
```

**Reporting Tasks:**

```
✅ Generate reports (inventory, utilization)
✅ Compliance reports
✅ Audit trails
```

---

## 14.2 Data Formats

### JSON (JavaScript Object Notation)

**คำจำกัดความ:**

```
- Lightweight data format
- Human-readable
- Easy for machines to parse
- Most popular for APIs
```

**Structure:**

```json
{
  "hostname": "Router1",
  "ip_address": "192.168.1.1",
  "interfaces": [
    {
      "name": "GigabitEthernet0/0",
      "ip": "10.0.0.1",
      "mask": "255.255.255.0",
      "status": "up"
    },
    {
      "name": "GigabitEthernet0/1",
      "ip": "192.168.1.1",
      "mask": "255.255.255.0",
      "status": "up"
    }
  ],
  "vlans": [10, 20, 30],
  "enabled": true
}
```

**Data Types:**

```
Object:   { "key": "value" }          ← Curly braces
Array:    [ "item1", "item2" ]        ← Square brackets
String:   "text"                      ← Quotes
Number:   123, 45.67                  ← No quotes
Boolean:  true, false                 ← No quotes
Null:     null                        ← No quotes
```

**Characteristics:**

```
✅ Easy to read
✅ Widely supported (all languages)
✅ Compact
✅ Popular for REST APIs
❌ No comments allowed
```

---

### XML (Extensible Markup Language)

**คำจำกัดความ:**

```
- Markup language
- Tags to define data
- Verbose (more text)
- Used in SOAP APIs, older systems
```

**Structure:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<router>
  <hostname>Router1</hostname>
  <ip_address>192.168.1.1</ip_address>
  <interfaces>
    <interface>
      <name>GigabitEthernet0/0</name>
      <ip>10.0.0.1</ip>
      <mask>255.255.255.0</mask>
      <status>up</status>
    </interface>
    <interface>
      <name>GigabitEthernet0/1</name>
      <ip>192.168.1.1</ip>
      <mask>255.255.255.0</mask>
      <status>up</status>
    </interface>
  </interfaces>
  <vlans>
    <vlan>10</vlan>
    <vlan>20</vlan>
    <vlan>30</vlan>
  </vlans>
  <enabled>true</enabled>
</router>
```

**Characteristics:**

```
✅ Self-documenting (tag names describe data)
✅ Comments allowed
✅ Strict validation (schemas)
❌ Verbose (more bytes)
❌ Harder to read
❌ Less popular (compared to JSON)
```

---

### YAML (YAML Ain't Markup Language)

**คำจำกัดความ:**

```
- Human-friendly data format
- Minimal syntax
- Indentation-based (like Python)
- Used in Ansible, Kubernetes
```

**Structure:**

```yaml
hostname: Router1
ip_address: 192.168.1.1
interfaces:
  - name: GigabitEthernet0/0
    ip: 10.0.0.1
    mask: 255.255.255.0
    status: up
  - name: GigabitEthernet0/1
    ip: 192.168.1.1
    mask: 255.255.255.0
    status: up
vlans:
  - 10
  - 20
  - 30
enabled: true
```

**Syntax Rules:**

```
Key-Value:     key: value
List:          - item
Indentation:   2 spaces (or 4, consistent)
Comments:      # This is a comment
Multi-line:    |
               Line 1
               Line 2
```

**Characteristics:**

```
✅ Most human-readable
✅ Minimal syntax
✅ Comments allowed
✅ Popular for configuration files
❌ Indentation-sensitive (strict)
❌ Harder to parse (than JSON)
```

---

### Data Format Comparison

```
Feature      JSON               XML                YAML
------------------------------------------------------------------------
Readability  Good               Medium             Best
Size         Compact            Verbose            Compact
Comments     No                 Yes                Yes
Syntax       Brackets/braces    Tags               Indentation
Use Case     APIs (REST)        Legacy, SOAP       Config files
Popularity   Very high          Medium             High
Examples     REST APIs          SOAP, NETCONF      Ansible, K8s
------------------------------------------------------------------------
```

**Same Data in All Formats:**

```
JSON:
{"name": "Router1", "ip": "192.168.1.1"}

XML:
<device>
  <name>Router1</name>
  <ip>192.168.1.1</ip>
</device>

YAML:
name: Router1
ip: 192.168.1.1
```

---

## 14.3 APIs (Application Programming Interfaces)

### What is an API?

**คำจำกัดความ:**

```
- Interface สำหรับ software ติดต่อกัน
- Defined methods to request/send data
- No GUI needed (programmatic access)
- Standard way to interact
```

**Analogy:**

```
Restaurant:
  Customer (you) ↔ Waiter (API) ↔ Kitchen (system)
  
  You don't go to kitchen directly
  You tell waiter (API) what you want
  Waiter communicates with kitchen
  Kitchen prepares food
  Waiter brings food back to you
  
Network:
  Script ↔ API ↔ Network Device
  
  Script doesn't configure CLI directly
  Script calls API methods
  API translates to device commands
  API returns results to script
```

---

### REST API (Representational State Transfer)

**คำจำกัดความ:**

```
- Most popular API style
- Uses HTTP methods
- Stateless (no session)
- Resource-based (URLs)
- JSON format (typically)
```

**HTTP Methods:**

```
GET:     Retrieve data (read)
         Example: Get interface status

POST:    Create new resource
         Example: Create new VLAN

PUT:     Update entire resource
         Example: Update interface configuration

PATCH:   Update part of resource
         Example: Change interface description

DELETE:  Delete resource
         Example: Delete VLAN
```

**REST API Request:**

```
GET /api/v1/interfaces/GigabitEthernet0/0 HTTP/1.1
Host: 192.168.1.1
Authorization: Bearer eyJhbGc...
Content-Type: application/json

(Get info about Gi0/0)
```

**REST API Response:**

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
  "name": "GigabitEthernet0/0",
  "ip_address": "10.0.0.1",
  "subnet_mask": "255.255.255.0",
  "status": "up",
  "speed": "1000",
  "duplex": "full"
}
```

**REST Characteristics:**

```
✅ Simple (uses HTTP)
✅ Stateless (independent requests)
✅ Cacheable
✅ Widely supported
✅ JSON format
```

---

### NETCONF (Network Configuration Protocol)

**คำจำกัดความ:**

```
- IETF standard (RFC 6241)
- XML-based
- Specialized for network device management
- Replaces CLI/SNMP for configuration
```

**Features:**

```
✅ Separate configuration from operational data
✅ Configuration validation
✅ Transactions (commit/rollback)
✅ Filtering (get specific data)
✅ Multiple datastores (candidate, running, startup)
```

**NETCONF Datastores:**

```
Running:    Current active configuration
Candidate:  Draft configuration (not active yet)
            Can edit, validate, then commit
Startup:    Configuration loaded on boot

Workflow:
  1. Edit candidate datastore
  2. Validate changes
  3. Commit to running (or discard if errors)
  4. Save to startup (persistent)
```

**NETCONF Operations:**

```
<get>:           Retrieve data
<get-config>:    Retrieve configuration
<edit-config>:   Modify configuration
<copy-config>:   Copy configuration between datastores
<delete-config>: Delete configuration
<lock>:          Lock datastore (exclusive access)
<unlock>:        Unlock datastore
<commit>:        Apply candidate to running
<validate>:      Validate configuration
```

**NETCONF Example:**

```xml
<!-- Get interface configuration -->
<rpc message-id="101" xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <get-config>
    <source>
      <running/>
    </source>
    <filter>
      <interfaces xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
        <interface>
          <name>GigabitEthernet0/0</name>
        </interface>
      </interfaces>
    </filter>
  </get-config>
</rpc>
```

**NETCONF Advantages:**

```
✅ Transactional (all-or-nothing)
✅ Validation before apply
✅ Rollback capability
✅ Better than CLI (structured, not text parsing)
✅ Standard (multi-vendor)
```

---

### RESTCONF

**คำจำกัดความ:**

```
- Combines REST + NETCONF
- HTTP-based (like REST)
- YANG data models (like NETCONF)
- JSON or XML format
- Easier than NETCONF (uses HTTP)
```

**Characteristics:**

```
✅ RESTful (HTTP methods: GET, POST, PUT, PATCH, DELETE)
✅ YANG data models
✅ JSON or XML
✅ Simpler than NETCONF (no SSH)
✅ Uses HTTPS (port 443)
```

**RESTCONF vs NETCONF:**

```
Feature          NETCONF         RESTCONF
------------------------------------------------------------------------
Protocol         SSH             HTTP/HTTPS
Format           XML only        JSON or XML
Complexity       Higher          Lower
Data Models      YANG            YANG
Transactions     Yes             Limited
Port             830             443 (HTTPS)
------------------------------------------------------------------------
```

---

## 14.4 Configuration Management Tools

### Ansible

**คำจำกัดความ:**

```
- Open-source automation tool
- Agentless (no software on managed devices)
- Uses SSH (for network devices)
- YAML playbooks
- Idempotent (safe to run multiple times)
```

**Architecture:**

```
┌─────────────────┐
│ Control Node    │  ← Ansible installed here
│ (Ansible)       │
└────────┬────────┘
         │
    ┌────┴────────────────┐
    │                     │
┌───▼────┐           ┌────▼───┐
│ Router1│           │Router2 │  ← No agent needed
└────────┘           └────────┘
```

**Key Concepts:**

**1. Inventory:**

```ini
# hosts file (inventory)
[routers]
router1 ansible_host=192.168.1.1
router2 ansible_host=192.168.1.2

[switches]
switch1 ansible_host=192.168.1.10
switch2 ansible_host=192.168.1.11

[all:vars]
ansible_user=admin
ansible_password=cisco123
ansible_network_os=ios
```

**2. Playbook:**

```yaml
---
# Configure VLANs on switches
- name: Configure VLANs
  hosts: switches
  gather_facts: no
  
  tasks:
    - name: Create VLAN 10
      ios_vlan:
        vlan_id: 10
        name: Users
        state: present
    
    - name: Create VLAN 20
      ios_vlan:
        vlan_id: 20
        name: Servers
        state: present
    
    - name: Save configuration
      ios_command:
        commands: write memory
```

**3. Modules:**

```
Cisco IOS modules:
  ios_command:     Run commands
  ios_config:      Configure device
  ios_vlan:        Manage VLANs
  ios_interface:   Manage interfaces
  ios_static_route: Manage routes
  ios_banner:      Manage banners
```

**Running Playbook:**

```bash
# Run playbook
ansible-playbook configure-vlans.yml

# Run on specific hosts
ansible-playbook -i inventory configure-vlans.yml --limit router1

# Dry run (check mode)
ansible-playbook configure-vlans.yml --check
```

**Ansible Advantages:**

```
✅ Agentless (SSH only)
✅ Easy to learn (YAML)
✅ Idempotent (safe to re-run)
✅ Large community
✅ Many modules (network, cloud, etc.)
✅ Free (open source)
```

---

### Puppet

**คำจำกัดความ:**

```
- Configuration management tool
- Agent-based (software on managed devices)
- Declarative (describe desired state)
- Ruby-based
- Enterprise-focused
```

**Architecture:**

```
┌─────────────────┐
│ Puppet Master   │  ← Central server
└────────┬────────┘
         │
    ┌────┴────────────────┐
    │                     │
┌───▼────┐           ┌────▼───┐
│ Agent  │           │ Agent  │  ← Puppet agent installed
│Router1 │           │Router2 │
└────────┘           └────────┘
```

**Characteristics:**

```
✅ Declarative (what, not how)
✅ Model-driven
✅ Scales well (enterprise)
❌ Agent required
❌ More complex than Ansible
❌ Primarily for servers (limited network support)
```

---

### Chef

**คำจำกัดความ:**

```
- Configuration management tool
- Agent-based
- Ruby DSL (domain-specific language)
- "Recipes" and "Cookbooks"
- Enterprise-focused
```

**Characteristics:**

```
✅ Powerful (programmable)
✅ Enterprise features
❌ Agent required
❌ Steep learning curve (Ruby)
❌ Primarily for servers
```

---

### Tool Comparison

```
Feature      Ansible          Puppet           Chef
------------------------------------------------------------------------
Agent        No (agentless)   Yes              Yes
Language     YAML             Ruby DSL         Ruby DSL
Learning     Easy             Medium           Hard
Approach     Procedural       Declarative      Procedural
Network      Excellent        Limited          Limited
Use Case     Network/Cloud    Enterprise       Enterprise
Cost         Free             Free/Paid        Paid
------------------------------------------------------------------------

Recommendation for Networks: Ansible (agentless, easy, network-focused)
```

---

## 14.5 Programmability

### Python for Network Automation

**Why Python?**

```
✅ Easy to learn
✅ Readable syntax
✅ Large ecosystem (libraries)
✅ Multi-platform
✅ Industry standard for automation
✅ Extensive network libraries
```

**Popular Python Libraries:**

**1. Netmiko:**

```python
# SSH to network devices
from netmiko import ConnectHandler

device = {
    'device_type': 'cisco_ios',
    'host': '192.168.1.1',
    'username': 'admin',
    'password': 'cisco123',
}

# Connect
connection = ConnectHandler(**device)

# Send command
output = connection.send_command('show ip interface brief')
print(output)

# Send configuration
config_commands = [
    'interface GigabitEthernet0/1',
    'description Connected to Switch1',
    'no shutdown'
]
connection.send_config_set(config_commands)

# Save
connection.send_command('write memory')

# Disconnect
connection.disconnect()
```

**2. NAPALM (Network Automation and Programmability Abstraction Layer with Multivendor support):**

```python
# Vendor-neutral library
from napalm import get_network_driver

driver = get_network_driver('ios')
device = driver(
    hostname='192.168.1.1',
    username='admin',
    password='cisco123'
)

# Connect
device.open()

# Get facts
facts = device.get_facts()
print(f"Hostname: {facts['hostname']}")
print(f"Model: {facts['model']}")
print(f"Uptime: {facts['uptime']}")

# Get interfaces
interfaces = device.get_interfaces()
for interface, data in interfaces.items():
    print(f"{interface}: {data['is_up']}")

# Load configuration (dry-run)
device.load_merge_candidate(filename='config.txt')
diff = device.compare_config()
print(diff)

# Commit or discard
# device.commit_config()  # Apply
device.discard_config()   # Cancel

# Close
device.close()
```

**3. Requests (for REST APIs):**

```python
import requests
import json

# REST API call
url = 'https://192.168.1.1/restconf/data/ietf-interfaces:interfaces'
headers = {
    'Content-Type': 'application/yang-data+json',
    'Accept': 'application/yang-data+json'
}
auth = ('admin', 'cisco123')

# GET request
response = requests.get(url, headers=headers, auth=auth, verify=False)

if response.status_code == 200:
    data = response.json()
    print(json.dumps(data, indent=2))
else:
    print(f"Error: {response.status_code}")
```

**4. Paramiko (low-level SSH):**

```python
import paramiko

# SSH connection
ssh = paramiko.SSHClient()
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
ssh.connect('192.168.1.1', username='admin', password='cisco123')

# Execute command
stdin, stdout, stderr = ssh.exec_command('show version')
output = stdout.read().decode()
print(output)

# Close
ssh.close()
```

---

### Example: Backup Configurations

**Python Script:**

```python
#!/usr/bin/env python3
"""
Backup router configurations
"""
from netmiko import ConnectHandler
from datetime import datetime
import os

# Device list
devices = [
    {
        'device_type': 'cisco_ios',
        'host': '192.168.1.1',
        'username': 'admin',
        'password': 'cisco123',
        'secret': 'enable_pass',
    },
    {
        'device_type': 'cisco_ios',
        'host': '192.168.1.2',
        'username': 'admin',
        'password': 'cisco123',
        'secret': 'enable_pass',
    },
]

# Backup directory
backup_dir = 'backups'
if not os.path.exists(backup_dir):
    os.makedirs(backup_dir)

# Get current date/time
timestamp = datetime.now().strftime('%Y%m%d-%H%M%S')

# Backup each device
for device in devices:
    try:
        print(f"Connecting to {device['host']}...")
        
        # Connect
        connection = ConnectHandler(**device)
        connection.enable()
        
        # Get hostname
        hostname = connection.send_command('show run | include hostname')
        hostname = hostname.split()[1]
        
        # Get configuration
        config = connection.send_command('show running-config')
        
        # Save to file
        filename = f"{backup_dir}/{hostname}_{timestamp}.txt"
        with open(filename, 'w') as f:
            f.write(config)
        
        print(f"✓ Backed up {hostname} to {filename}")
        
        # Disconnect
        connection.disconnect()
        
    except Exception as e:
        print(f"✗ Error with {device['host']}: {str(e)}")

print("\nBackup complete!")
```

**Running Script:**

```bash
$ python backup_configs.py

Connecting to 192.168.1.1...
✓ Backed up Router1 to backups/Router1_20241118-120000.txt
Connecting to 192.168.1.2...
✓ Backed up Router2 to backups/Router2_20241118-120005.txt

Backup complete!
```

---

### Example: Configure Multiple Devices

**Python Script:**

```python
#!/usr/bin/env python3
"""
Configure VLANs on multiple switches
"""
from netmiko import ConnectHandler

# Switch list
switches = [
    '192.168.1.10',
    '192.168.1.11',
    '192.168.1.12',
]

# VLANs to create
vlans = {
    10: 'Users',
    20: 'Servers',
    30: 'Guests',
    99: 'Management',
}

# Device template
device_template = {
    'device_type': 'cisco_ios',
    'username': 'admin',
    'password': 'cisco123',
}

# Configure each switch
for switch_ip in switches:
    try:
        print(f"\nConfiguring {switch_ip}...")
        
        # Connect
        device = device_template.copy()
        device['host'] = switch_ip
        connection = ConnectHandler(**device)
        
        # Generate VLAN commands
        commands = []
        for vlan_id, vlan_name in vlans.items():
            commands.extend([
                f'vlan {vlan_id}',
                f'name {vlan_name}',
            ])
        
        # Send commands
        output = connection.send_config_set(commands)
        
        # Save configuration
        connection.send_command('write memory')
        
        print(f"✓ Configured {switch_ip}")
        print(output)
        
        # Disconnect
        connection.disconnect()
        
    except Exception as e:
        print(f"✗ Error with {switch_ip}: {str(e)}")

print("\nConfiguration complete!")
```

---

## 14.6 Model-Driven Programmability

### YANG (Yet Another Next Generation)

**คำจำกัดความ:**

```
- Data modeling language
- Defines structure of configuration/operational data
- Used by NETCONF, RESTCONF
- Vendor-neutral
- IETF standard (RFC 6020)
```

**Purpose:**

```
Traditional:
  - CLI output = unstructured text
  - Hard to parse
  - Vendor-specific

YANG:
  - Structured data (XML/JSON)
  - Well-defined schema
  - Easy to parse
  - Consistent across vendors
```

**YANG Model Example:**

```yang
module interface {
  namespace "http://example.com/interface";
  prefix "if";

  container interfaces {
    list interface {
      key "name";
      leaf name {
        type string;
      }
      leaf description {
        type string;
      }
      leaf enabled {
        type boolean;
        default true;
      }
      leaf ip-address {
        type string;
      }
      leaf subnet-mask {
        type string;
      }
    }
  }
}
```

**YANG Benefits:**

```
✅ Structured data (not CLI text)
✅ Data validation
✅ Consistent format
✅ Machine-readable
✅ Version control
✅ Documentation (built-in)
```

---

### Model-Driven vs Traditional

**Traditional (CLI):**

```
Router# show ip interface brief
Interface              IP-Address      OK? Method Status   Protocol
GigabitEthernet0/0     10.0.0.1        YES manual up       up
GigabitEthernet0/1     192.168.1.1     YES manual up       up

Problems:
  - Text parsing (fragile)
  - Format varies by vendor
  - Hard to automate
```

**Model-Driven (YANG):**

```json
{
  "interfaces": {
    "interface": [
      {
        "name": "GigabitEthernet0/0",
        "ip-address": "10.0.0.1",
        "subnet-mask": "255.255.255.0",
        "enabled": true,
        "status": "up"
      },
      {
        "name": "GigabitEthernet0/1",
        "ip-address": "192.168.1.1",
        "subnet-mask": "255.255.255.0",
        "enabled": true,
        "status": "up"
      }
    ]
  }
}

Benefits:
  ✅ Structured (JSON)
  ✅ Easy to parse
  ✅ Consistent format
  ✅ Vendor-neutral
```

---

## 14.7 Cisco Automation Tools

### Cisco DNA Center

**คำจำกัดความ:**

```
- Network management and automation platform
- GUI-based (intent-based networking)
- SDN controller
- Campus and branch networks
```

**Features:**

```
✅ Design (network design tools)
✅ Policy (business policies → network config)
✅ Provision (zero-touch provisioning)
✅ Assurance (AI/ML analytics, troubleshooting)
```

**Use Cases:**

```
- Campus network automation
- Branch provisioning
- SD-Access (software-defined access)
- Application visibility and control
```

---

### Cisco vManage

**คำจำกัดความ:**

```
- SD-WAN management platform
- Centralized management for WAN
- GUI and APIs
```

**Features:**

```
✅ WAN orchestration
✅ Zero-touch provisioning
✅ Application-aware routing
✅ Security policies
✅ Monitoring and troubleshooting
```

---

### Cisco NSO (Network Services Orchestrator)

**คำจำกัดความ:**

```
- Multi-vendor orchestration
- Service lifecycle management
- Model-driven (YANG)
- For service providers and large enterprises
```

**Features:**

```
✅ Multi-vendor support (not just Cisco)
✅ Service templates
✅ Transaction management (rollback)
✅ APIs (REST, NETCONF)
```

---

## 14.8 DevOps and NetOps

### DevOps Principles

**คำจำกัดความ:**

```
DevOps = Development + Operations
  - Collaboration between dev and ops teams
  - Automation
  - Continuous integration/delivery (CI/CD)
  - Infrastructure as Code (IaC)
```

**DevOps Practices:**

```
1. Version Control (Git)
   - Track changes
   - Collaborate
   - Rollback

2. Automation
   - Eliminate manual tasks
   - Consistent, repeatable

3. CI/CD
   - Continuous Integration: Merge code frequently
   - Continuous Delivery: Automated deployment

4. Infrastructure as Code
   - Configuration in files (not manual)
   - Version control for infrastructure
   - Automated deployment

5. Monitoring and Logging
   - Real-time visibility
   - Proactive issue detection

6. Collaboration
   - Break down silos
   - Shared responsibility
```

---

### NetOps (Network Operations)

**คำจำกัดความ:**

```
NetOps = DevOps principles applied to networking
  - Network as Code
  - Automated testing
  - CI/CD for network changes
  - Version control for configurations
```

**Traditional Network Ops vs NetOps:**

```
Traditional:
  - Manual configuration (CLI)
  - Ad-hoc changes
  - Limited testing
  - No version control
  - Slow, error-prone

NetOps:
  - Automated configuration (scripts, APIs)
  - Planned changes (code review)
  - Automated testing (pre-deployment)
  - Version control (Git)
  - Fast, reliable
```

---

### Version Control (Git)

**คำจำกัดความ:**

```
- Track changes to files
- Collaborate with team
- History of all changes
- Rollback to previous versions
```

**Basic Git Workflow:**

```bash
# Initialize repository
git init

# Check status
git status

# Add files
git add config.txt
git add .                # Add all files

# Commit changes
git commit -m "Added VLAN 10 configuration"

# View history
git log

# Create branch
git branch feature-vlan20

# Switch branch
git checkout feature-vlan20

# Merge branch
git checkout main
git merge feature-vlan20

# Push to remote (GitHub, GitLab)
git push origin main

# Pull from remote
git pull origin main
```

**Why Git for Network?**

```
✅ Track configuration changes (who, what, when)
✅ Rollback to working configuration
✅ Collaborate (multiple engineers)
✅ Code review (before deployment)
✅ Backup (remote repositories)
✅ Audit trail (compliance)
```

---

## Summary (สรุป)

Module 14 นี้เราได้เรียนรู้:

1. ✅ **Automation Overview**:
    
    - Why automate: Speed, consistency, reduce errors, scalability
    - What to automate: Configuration, monitoring, troubleshooting, reporting
2. ✅ **Data Formats**:
    
    - **JSON**: Most popular for APIs, human-readable, compact
    - **XML**: Verbose, tags, SOAP/NETCONF
    - **YAML**: Most readable, indentation-based, Ansible/K8s
3. ✅ **APIs**:
    
    - **REST API**: HTTP-based, JSON, stateless, most popular
    - **NETCONF**: XML-based, network-focused, transactions
    - **RESTCONF**: REST + NETCONF, HTTP + YANG
4. ✅ **Configuration Management**:
    
    - **Ansible**: Agentless, YAML, easy, network-focused ✅ Recommended
    - **Puppet**: Agent-based, declarative, enterprise
    - **Chef**: Agent-based, Ruby, complex
5. ✅ **Python Libraries**:
    
    - **Netmiko**: SSH to network devices (simple)
    - **NAPALM**: Vendor-neutral, multi-vendor support
    - **Requests**: REST APIs
    - **Paramiko**: Low-level SSH
6. ✅ **YANG**:
    
    - Data modeling language
    - Structured data (not CLI text)
    - Used by NETCONF/RESTCONF
    - Vendor-neutral
7. ✅ **Cisco Tools**:
    
    - **DNA Center**: Campus/branch automation, intent-based
    - **vManage**: SD-WAN management
    - **NSO**: Multi-vendor orchestration
8. ✅ **DevOps/NetOps**:
    
    - Automation, CI/CD, Infrastructure as Code
    - Version control (Git)
    - Network as Code

**สิ่งสำคัญที่ต้องจำ:**

- **Automation**: Speed, consistency, scalability, reduce errors
- **Data Formats**: JSON (APIs), XML (NETCONF), YAML (Ansible)
- **APIs**: REST (HTTP/JSON), NETCONF (SSH/XML), RESTCONF (HTTP/YANG)
- **Ansible**: Best for network automation (agentless, easy, YAML)
- **Python**: Industry standard for scripting (Netmiko, NAPALM, Requests)
- **YANG**: Data models for structured network data
- **Git**: Version control for configurations
- **NetOps**: DevOps for networking (Network as Code)

**Key Comparisons:**

```
Data Formats:
  JSON    → APIs (compact, widely supported)
  XML     → NETCONF, legacy (verbose)
  YAML    → Configuration files (readable)

APIs:
  REST       → General-purpose, HTTP, JSON
  NETCONF    → Network-focused, SSH, XML, transactions
  RESTCONF   → Network-focused, HTTP, YANG

Tools:
  Ansible    → Network automation (best for networks)
  Puppet     → Configuration management (servers)
  Python     → Scripting (flexible, powerful)

Python Libraries:
  Netmiko    → SSH (simple)
  NAPALM     → Multi-vendor (consistent interface)
  Requests   → REST APIs
```

**Automation Best Practices:**

```
1. Start Small:
   ☐ Automate simple, repetitive tasks first
   ☐ Backup configurations
   ☐ Generate reports

2. Use Version Control:
   ☐ Git for all scripts and configs
   ☐ Track changes
   ☐ Code review

3. Test Before Deploy:
   ☐ Test in lab environment
   ☐ Dry-run mode (Ansible --check)
   ☐ Validate before commit

4. Document:
   ☐ Comment code
   ☐ README files
   ☐ Runbooks

5. Error Handling:
   ☐ Try/except blocks
   ☐ Logging
   ☐ Graceful failures

6. Idempotency:
   ☐ Safe to run multiple times
   ☐ Same result each time
   ☐ No unintended changes

7. Security:
   ☐ Don't hardcode passwords
   ☐ Use vault (Ansible Vault)
   ☐ Least privilege
   ☐ Secure credentials storage
```

**Getting Started with Automation:**

```
Step 1: Learn Python basics
  - Variables, loops, functions
  - File I/O
  - Libraries (import)

Step 2: Learn data formats
  - JSON, YAML
  - Parsing and creating

Step 3: Try simple scripts
  - Backup configurations (Netmiko)
  - Show command collection
  - Basic reporting

Step 4: Learn Ansible
  - Inventory files
  - Playbooks
  - Modules (ios_command, ios_config)

Step 5: Use version control
  - Git basics
  - GitHub/GitLab

Step 6: Expand skills
  - REST APIs
  - More complex automation
  - CI/CD pipelines
```

**Resources:**

```
Learning:
  - Python.org (Python tutorial)
  - Ansible documentation
  - GitHub (examples)
  - Cisco DevNet (learning labs)

Practice:
  - Cisco DevNet Sandbox (free devices)
  - GNS3/EVE-NG (lab environment)
  - Packet Tracer (Cisco simulator)
```

---

## 🎉 Congratulations!

คุณได้เรียนจบ **CCNA Course 3** ทั้ง **14 Modules** แล้ว!

**Modules ที่เรียน:**

1. ✅ Single-Area OSPFv2 Concepts
2. ✅ Single-Area OSPFv2 Configuration
3. ✅ Network Security Concepts
4. ✅ ACL Concepts
5. ✅ ACLs for IPv4 Configuration
6. ✅ NAT for IPv4
7. ✅ WAN Concepts
8. ✅ VPN and IPsec Concepts
9. ✅ QoS Concepts
10. ✅ Network Management
11. ✅ Network Design
12. ✅ Network Troubleshooting
13. ✅ Network Virtualization
14. ✅ Network Automation

**หัวข้อสำคัญที่ครอบคลุม:**

- ✅ Dynamic Routing (OSPF)
- ✅ Network Security (ACLs, Firewalls)
- ✅ Address Translation (NAT, PAT)
- ✅ WAN Technologies (MPLS, Metro Ethernet, SD-WAN)
- ✅ VPN and IPsec
- ✅ Quality of Service (QoS)
- ✅ Network Management (CDP, LLDP, NTP, SNMP, Syslog)
- ✅ Network Design (Hierarchical, Scalability, Redundancy)
- ✅ Troubleshooting Methodology
- ✅ Virtualization (Cloud, VMs, Containers)
- ✅ Network Automation (Ansible, Python, APIs)

**พร้อมสำหรับ CCNA Exam แล้ว!** 🎓

Good luck with your CCNA certification! 🚀

---

**[ไฟล์ Module 14 สมบูรณ์แล้ว - FINAL MODULE!]** 🎊