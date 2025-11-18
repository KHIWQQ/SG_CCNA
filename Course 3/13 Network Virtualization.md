# CCNA Course 3 - Module 13: Network Virtualization

## การสร้างเครือข่าย Virtual

---

## 13.1 Cloud Computing

### What is Cloud Computing?

**คำจำกัดความ:**

- **Cloud Computing** = การใช้ computing resources ผ่าน Internet
- On-demand self-service
- Broad network access
- Resource pooling
- Rapid elasticity
- Measured service (pay-per-use)

**Traditional vs Cloud:**

```
Traditional IT:
  - Buy servers, install in datacenter
  - Maintain hardware, software
  - Fixed capacity
  - High upfront cost
  - Long deployment time

Cloud Computing:
  - Rent resources from provider
  - Provider maintains infrastructure
  - Scale up/down as needed
  - Pay only for usage
  - Deploy in minutes
```

---

### Cloud Service Models

#### IaaS (Infrastructure as a Service)

**คำจำกัดความ:**

```
- เช่า infrastructure (servers, storage, network)
- ผู้ใช้ติดตั้ง OS, applications
- ผู้ใช้จัดการ OS, middleware, applications
- Provider จัดการ physical infrastructure
```

**Examples:**

```
- Amazon EC2 (Elastic Compute Cloud)
- Microsoft Azure Virtual Machines
- Google Compute Engine
- DigitalOcean Droplets
```

**Use Cases:**

```
✅ Full control over OS and applications
✅ Migration from on-premises (lift-and-shift)
✅ Development/testing environments
✅ High-performance computing
```

**Responsibilities:**

```
Customer:
  - Applications
  - Data
  - Runtime
  - Middleware
  - Operating System

Provider:
  - Virtualization
  - Servers
  - Storage
  - Networking
```

---

#### PaaS (Platform as a Service)

**คำจำกัดความ:**

```
- Platform for developing and deploying applications
- Provider จัดการ OS, middleware, runtime
- ผู้ใช้จัดการ applications และ data เท่านั้น
```

**Examples:**

```
- AWS Elastic Beanstalk
- Microsoft Azure App Service
- Google App Engine
- Heroku
```

**Use Cases:**

```
✅ Application development
✅ API development
✅ Focus on code, not infrastructure
✅ Faster time to market
```

**Responsibilities:**

```
Customer:
  - Applications
  - Data

Provider:
  - Runtime
  - Middleware
  - Operating System
  - Virtualization
  - Servers
  - Storage
  - Networking
```

---

#### SaaS (Software as a Service)

**คำจำกัดความ:**

```
- Ready-to-use software applications
- Access via web browser
- Provider จัดการทุกอย่าง
- ผู้ใช้แค่ใช้งาน
```

**Examples:**

```
- Gmail (email)
- Office 365 (productivity)
- Salesforce (CRM)
- Dropbox (file storage)
- Zoom (video conferencing)
- Slack (collaboration)
```

**Use Cases:**

```
✅ End-user applications
✅ No IT management needed
✅ Accessible anywhere
✅ Automatic updates
```

**Responsibilities:**

```
Customer:
  - Data
  - User access

Provider:
  - Applications
  - Data (storage/backup)
  - Runtime
  - Middleware
  - Operating System
  - Virtualization
  - Servers
  - Storage
  - Networking
```

---

### Service Model Comparison

```
Feature      IaaS               PaaS               SaaS
------------------------------------------------------------------------
Control      Most control       Medium control     Least control
Flexibility  Very flexible      Flexible           Limited
Manage       OS, Apps           Apps only          Nothing
Use Case     Custom infra       App development    End-user apps
Examples     AWS EC2            Heroku             Gmail, Office 365
Cost         Medium             Medium-Low         Lowest
Complexity   Highest            Medium             Lowest
------------------------------------------------------------------------
```

**Analogy (Pizza):**

```
On-Premises: Make everything yourself
  (Buy oven, ingredients, make pizza, clean up)

IaaS: Rent kitchen, you cook
  (Kitchen provided, you make pizza)

PaaS: Take-and-bake
  (Pizza made, you bake it)

SaaS: Delivery pizza
  (Order, eat, done)
```

---

### Cloud Deployment Models

#### Public Cloud

**คำจำกัดความ:**

```
- Resources owned and operated by third-party provider
- Shared infrastructure (multi-tenant)
- Access over Internet
- Pay-per-use
```

**Examples:**

```
- AWS (Amazon Web Services)
- Microsoft Azure
- Google Cloud Platform (GCP)
- IBM Cloud
```

**Characteristics:**

```
✅ No upfront cost
✅ Scalable
✅ No maintenance
✅ Global availability
❌ Less control
❌ Shared resources
❌ Security concerns (some industries)
```

---

#### Private Cloud

**คำจำกัดความ:**

```
- Resources used by one organization only
- Can be on-premises or hosted
- Dedicated infrastructure (single-tenant)
- More control, security
```

**Examples:**

```
- VMware vCloud
- Microsoft Azure Stack
- OpenStack (on-premises)
- AWS Outposts (on-premises)
```

**Characteristics:**

```
✅ Full control
✅ Better security/compliance
✅ Customizable
❌ Higher cost
❌ Requires IT staff
❌ Limited scalability
```

---

#### Hybrid Cloud

**คำจำกัดความ:**

```
- Combination of public and private clouds
- Data and applications shared between them
- Flexibility to move workloads
```

**Use Cases:**

```
✅ Sensitive data in private cloud
✅ Public-facing apps in public cloud
✅ Burst to public cloud during peak times (cloud bursting)
✅ Disaster recovery (backup to public cloud)

Example:
  Database (private cloud - sensitive)
  Web frontend (public cloud - scalable)
```

**Characteristics:**

```
✅ Flexibility
✅ Balance security and scalability
✅ Optimize costs
❌ Complex to manage
❌ Integration challenges
```

---

#### Community Cloud

**คำจำกัดความ:**

```
- Shared by multiple organizations with common interests
- Shared costs
- Specific compliance requirements
```

**Examples:**

```
- Government cloud (GovCloud)
- Healthcare cloud (HIPAA compliance)
- Education cloud
```

---

### Cloud Deployment Model Comparison

```
Model        Control    Cost      Security   Scalability  Use Case
------------------------------------------------------------------------
Public       Low        Lowest    Medium     Highest      General
Private      High       Highest   Highest    Limited      Sensitive
Hybrid       Medium     Medium    High       High         Best of both
Community    Medium     Shared    High       Medium       Common needs
------------------------------------------------------------------------
```

---

## 13.2 Virtualization

### What is Virtualization?

**คำจำกัดความ:**

```
- สร้าง virtual version ของ physical resource
- Multiple virtual instances บน physical hardware เดียว
- Maximize resource utilization
- Isolation between virtual instances
```

**Types of Virtualization:**

```
1. Server Virtualization (Virtual Machines)
2. Network Virtualization (SDN, NFV)
3. Storage Virtualization
4. Desktop Virtualization (VDI)
```

---

### Server Virtualization

**Benefits:**

```
✅ Consolidation (many VMs on one server)
✅ Cost savings (less hardware)
✅ Better utilization (70-80% vs 10-15%)
✅ Faster provisioning (minutes vs days)
✅ Easy backup/restore (snapshot)
✅ Isolation (VMs independent)
✅ Testing/development (quick sandbox)
✅ Disaster recovery (migrate VMs)
```

---

### Hypervisors

**คำจำกัดความ:**

```
- Software that creates and runs virtual machines
- Manages virtual hardware resources
- Provides isolation between VMs
```

---

#### Type 1 Hypervisor (Bare Metal)

**คำจำกัดความ:**

```
- Runs directly on physical hardware
- No host OS needed
- Better performance
- Enterprise/production use
```

**Architecture:**

```
┌─────────┐ ┌─────────┐ ┌─────────┐
│  VM 1   │ │  VM 2   │ │  VM 3   │
│ (Linux) │ │(Windows)│ │ (Linux) │
└─────────┘ └─────────┘ └─────────┘
─────────────────────────────────────
        Type 1 Hypervisor
─────────────────────────────────────
      Physical Hardware
      (CPU, RAM, Disk, Network)
```

**Examples:**

```
- VMware ESXi (vSphere)
- Microsoft Hyper-V (standalone)
- Citrix XenServer
- KVM (Kernel-based Virtual Machine)
```

**Characteristics:**

```
✅ High performance (direct hardware access)
✅ Better security (smaller attack surface)
✅ Scalable (hundreds of VMs)
✅ Enterprise features (live migration, HA)
❌ Requires dedicated server
❌ More complex setup
```

---

#### Type 2 Hypervisor (Hosted)

**คำจำกัดความ:**

```
- Runs on top of host OS
- Host OS manages hardware
- Lower performance (overhead)
- Desktop/testing use
```

**Architecture:**

```
┌─────────┐ ┌─────────┐
│  VM 1   │ │  VM 2   │
│ (Linux) │ │(Windows)│
└─────────┘ └─────────┘
─────────────────────────
  Type 2 Hypervisor
─────────────────────────
      Host OS
    (Windows/Linux)
─────────────────────────
   Physical Hardware
```

**Examples:**

```
- VMware Workstation
- VMware Fusion (Mac)
- Oracle VirtualBox
- Parallels Desktop (Mac)
```

**Characteristics:**

```
✅ Easy to install (on existing OS)
✅ Good for desktop/testing
✅ Free options (VirtualBox)
❌ Lower performance (host OS overhead)
❌ Less scalable
❌ Not for production servers
```

---

#### Type 1 vs Type 2 Comparison

```
Feature          Type 1 (Bare Metal)    Type 2 (Hosted)
------------------------------------------------------------------------
Installation     Dedicated server       On existing OS
Performance      High                   Lower
Overhead         Low                    Higher
Use Case         Production servers     Desktop, testing
Scalability      Hundreds of VMs        Few VMs
Examples         ESXi, Hyper-V          VirtualBox, VMware WS
Cost             Enterprise license     Free or low cost
Management       Complex                Simple
------------------------------------------------------------------------
```

---

## 13.3 Virtual Machines

### VM Components

**Virtual Hardware:**

```
- Virtual CPU (vCPU)
- Virtual RAM
- Virtual disk (VMDK, VHD, QCOW2)
- Virtual NIC (network interface)
- Virtual graphics
```

**VM Files:**

```
- Configuration file (.vmx, .xml)
- Virtual disk file (.vmdk, .vhd)
- Snapshot files
- Log files
```

---

### VM Benefits

**1. Server Consolidation:**

```
Before:
  10 physical servers × 10% utilization = 90% waste

After:
  1 physical server with 10 VMs × 70% utilization
  
Savings:
  - 9 servers (hardware cost)
  - 9 × power/cooling/space
  - 9 × maintenance
```

**2. Rapid Provisioning:**

```
Physical server: Days/weeks
  - Order hardware
  - Receive, rack, cable
  - Install OS
  - Configure

Virtual machine: Minutes
  - Clone template
  - Customize (hostname, IP)
  - Power on
  - Done
```

**3. Testing and Development:**

```
✅ Create sandbox quickly
✅ Test configurations safely
✅ Snapshot before changes
✅ Rollback if needed
✅ Delete when done (no waste)
```

**4. Disaster Recovery:**

```
✅ Easy backup (snapshot entire VM)
✅ Fast restore (from snapshot)
✅ Live migration (move VM to another host)
✅ High availability (auto-restart on failure)
```

---

### VM Limitations

**1. Performance Overhead:**

```
❌ Hypervisor adds ~5-10% overhead
❌ I/O intensive apps may suffer
❌ Not suitable for all workloads
```

**2. Resource Contention:**

```
❌ VMs compete for resources
❌ "Noisy neighbor" problem
❌ Need proper resource allocation
```

**3. Licensing:**

```
❌ OS licenses per VM
❌ Application licenses per VM
❌ Can be expensive
```

**4. Single Point of Failure:**

```
❌ If physical host fails → All VMs down
❌ Need HA clustering
❌ Need redundancy
```

---

## 13.4 Network Virtualization

### Virtual Switches (vSwitches)

**คำจำกัดความ:**

```
- Software-based switch in hypervisor
- Connects VMs to each other and physical network
- Layer 2 switching
```

**Architecture:**

```
┌────────┐  ┌────────┐  ┌────────┐
│  VM1   │  │  VM2   │  │  VM3   │
└───┬────┘  └───┬────┘  └───┬────┘
    │           │           │
    └───────────┴───────────┘
          Virtual Switch
              │
         ┌────┴────┐
         │ Physical│
         │   NIC   │
         └─────────┘
              │
         Physical Network
```

**Examples:**

```
- VMware vSwitch
- Cisco Nexus 1000V
- Open vSwitch (OVS)
```

**Features:**

```
✅ VLANs (tag traffic)
✅ Port groups (group ports with common config)
✅ Traffic shaping (limit bandwidth)
✅ Security policies
✅ Port mirroring (SPAN)
✅ Load balancing
```

---

### Virtual NICs (vNICs)

**คำจำกัดความ:**

```
- Virtual network adapter in VM
- Multiple vNICs per VM possible
- Connects to vSwitch
```

**Types:**

```
E1000: Emulated Intel NIC
       - Widely compatible
       - Lower performance

VMXNET3: Paravirtualized NIC
         - VMware optimized
         - Higher performance
         - Requires VMware Tools

VirtIO: Paravirtualized NIC (KVM)
        - High performance
```

---

### SDN (Software-Defined Networking)

**คำจำกัดความ:**

```
- Separate control plane from data plane
- Centralized control (SDN controller)
- Programmable network
- API-driven
```

**Traditional Network vs SDN:**

```
Traditional:
  Control Plane + Data Plane in each device
  Distributed intelligence
  Manual configuration per device

SDN:
  Control Plane: Centralized controller
  Data Plane: Simple forwarding devices
  Centralized intelligence
  Programmatic configuration
```

**Architecture:**

```
┌─────────────────────────────────────┐
│    Application Layer                │
│  (Network Apps, Orchestration)      │
└──────────────┬──────────────────────┘
               │ Northbound API (REST, etc.)
┌──────────────┴──────────────────────┐
│    Control Layer (SDN Controller)   │
│  (Network Intelligence, Policies)   │
└──────────────┬──────────────────────┘
               │ Southbound API (OpenFlow, etc.)
┌──────────────┴──────────────────────┐
│    Data Layer (Switches, Routers)   │
│  (Packet Forwarding)                │
└─────────────────────────────────────┘
```

**SDN Controllers:**

```
- Cisco DNA Center
- Cisco ACI (Application Centric Infrastructure)
- VMware NSX
- OpenDaylight (open source)
```

**Benefits:**

```
✅ Centralized management
✅ Programmable (automation)
✅ Agile (quick changes)
✅ Vendor-neutral (standards-based)
✅ Network-wide visibility
✅ Dynamic path selection
```

**Use Cases:**

```
- Data center networks
- Campus networks
- WAN optimization (SD-WAN)
- Network automation
- Cloud networking
```

---

### NFV (Network Functions Virtualization)

**คำจำกัดความ:**

```
- Network functions as software (not hardware)
- Run on standard servers (VMs/containers)
- Decouple functions from hardware
```

**Traditional vs NFV:**

```
Traditional:
  Network functions = Proprietary hardware
  Router = Physical router
  Firewall = Physical firewall appliance
  Load balancer = Physical appliance

NFV:
  Network functions = Software (VMs)
  Router = Virtual router (vRouter)
  Firewall = Virtual firewall (vFirewall)
  Load balancer = Virtual load balancer
  
  All run on standard x86 servers
```

**Examples of VNFs (Virtual Network Functions):**

```
- Virtual router
- Virtual firewall
- Virtual load balancer
- Virtual IDS/IPS
- Virtual WAN optimizer
- Virtual NAT
```

**Benefits:**

```
✅ Reduce hardware costs
✅ Faster deployment (software)
✅ Scale easily (add more VMs)
✅ Flexibility (change functions)
✅ Automation-friendly
```

**Use Cases:**

```
- Service provider networks
- Enterprise edge (branch routers)
- Security (virtual firewalls)
- SD-WAN (virtual WAN routers)
```

---

## 13.5 Containers

### What are Containers?

**คำจำกัดความ:**

```
- Lightweight virtualization
- Package application + dependencies
- Share host OS kernel
- Faster, lighter than VMs
```

**VMs vs Containers:**

```
Virtual Machines:
  ┌──────────┐ ┌──────────┐
  │   App    │ │   App    │
  ├──────────┤ ├──────────┤
  │  Bins/   │ │  Bins/   │
  │  Libs    │ │  Libs    │
  ├──────────┤ ├──────────┤
  │ Guest OS │ │ Guest OS │  ← Full OS per VM
  └──────────┘ └──────────┘
  ─────────────────────────
      Hypervisor
  ─────────────────────────
        Host OS
  ─────────────────────────
       Hardware

Containers:
  ┌──────────┐ ┌──────────┐
  │   App    │ │   App    │
  ├──────────┤ ├──────────┤
  │  Bins/   │ │  Bins/   │
  │  Libs    │ │  Libs    │  ← No guest OS!
  └──────────┘ └──────────┘
  ─────────────────────────
    Container Runtime
  ─────────────────────────
        Host OS             ← Shared kernel
  ─────────────────────────
       Hardware
```

**Key Differences:**

```
Feature          VMs                 Containers
------------------------------------------------------------------------
Size             GBs                 MBs
Startup Time     Minutes             Seconds
Resource Use     Heavy               Lightweight
Isolation        Strong (hypervisor) Process-level
Portability      Medium              High
Density          10s per host        100s per host
OS               Full OS per VM      Shared host OS
Use Case         Full OS needed      Microservices, apps
------------------------------------------------------------------------
```

---

### Docker

**คำจำกัดความ:**

```
- Most popular container platform
- Package app + dependencies into image
- Run anywhere (dev, test, prod)
- "Build once, run anywhere"
```

**Components:**

**1. Docker Image:**

```
- Read-only template
- Contains app + dependencies
- Built in layers
- Stored in registry

Example:
  nginx:latest (web server image)
  mysql:8.0 (database image)
  python:3.9 (Python runtime)
```

**2. Docker Container:**

```
- Running instance of image
- Isolated process
- Temporary (can be deleted)
- Writable layer on top of image
```

**3. Docker Registry:**

```
- Repository for images
- Public: Docker Hub
- Private: Company registry

Example:
  docker pull nginx:latest (download image)
  docker push myapp:v1.0 (upload image)
```

**4. Dockerfile:**

```
- Recipe for building image
- Text file with instructions

Example:
  FROM python:3.9
  COPY app.py /app/
  RUN pip install flask
  CMD ["python", "/app/app.py"]
```

---

### Container Benefits

**1. Portability:**

```
✅ Same image runs everywhere
✅ No "works on my machine" problem
✅ Dev → Test → Prod consistency

Example:
  Developer's laptop (Mac)
  → Test server (Linux)
  → Production cloud (AWS)
  Same container image!
```

**2. Lightweight:**

```
✅ Small size (MBs vs GBs)
✅ Fast startup (seconds)
✅ Low overhead
✅ High density (more containers per host)

Example:
  Server with 64 GB RAM:
    VMs: ~10-20 VMs
    Containers: ~100-200 containers
```

**3. Microservices:**

```
✅ One service per container
✅ Independent scaling
✅ Independent updates
✅ Easier maintenance

Example:
  Monolithic app → One big VM
  
  Microservices → Multiple containers:
    - Web frontend (container 1)
    - API (container 2)
    - Database (container 3)
    - Cache (container 4)
```

**4. CI/CD:**

```
✅ Fast deployment
✅ Automated testing
✅ Easy rollback

Pipeline:
  Code → Build image → Test → Deploy → Done!
```

---

### Container Orchestration

**Problem:**

```
- Managing many containers manually is hard
- Need to:
  • Deploy containers
  • Scale up/down
  • Health monitoring
  • Load balancing
  • Rolling updates
  • Self-healing
```

**Solution: Kubernetes (K8s):**

```
- Container orchestration platform
- Automates deployment, scaling, management
- Open source (Cloud Native Computing Foundation)
- Industry standard
```

**Kubernetes Features:**

```
✅ Auto-scaling (based on load)
✅ Self-healing (restart failed containers)
✅ Rolling updates (zero-downtime)
✅ Service discovery (find containers)
✅ Load balancing (distribute traffic)
✅ Storage orchestration
✅ Secret management
```

**Kubernetes Architecture:**

```
┌────────────────────────────────────┐
│      Control Plane (Master)        │
│  - API Server                      │
│  - Scheduler                       │
│  - Controller Manager              │
│  - etcd (cluster data)             │
└────────────────────────────────────┘
          │       │       │
    ┌─────┴───┐   │   ┌───┴─────┐
    │  Node 1 │   │   │  Node 3 │
    │  (Pod1) │   │   │  (Pod3) │
    └─────────┘   │   └─────────┘
            ┌─────┴─────┐
            │   Node 2  │
            │   (Pod2)  │
            └───────────┘

Node: Worker machine (VM or physical)
Pod: Smallest unit (one or more containers)
```

---

## 13.6 Virtualization in Enterprise

### Use Cases

**1. Server Consolidation:**

```
Before:
  - 50 physical servers (10% utilization)
  - High power/cooling costs
  - Large datacenter footprint

After:
  - 5 physical servers with 10 VMs each
  - 70% utilization
  - Reduced costs (90% reduction in servers)
```

**2. Development and Testing:**

```
✅ Quick sandbox creation
✅ Multiple OS versions testing
✅ Isolated environments
✅ Easy cleanup (delete VM)

Example:
  Test new OS version:
    - Create VM from template
    - Test application
    - If works: Deploy to production
    - If fails: Delete VM, no impact
```

**3. Disaster Recovery:**

```
✅ Backup entire VMs
✅ Replicate to DR site
✅ Fast recovery (start VM)
✅ Test DR without impact

Example:
  Primary site: Production VMs
  DR site: Replicated VMs (off)
  Disaster → Power on DR VMs (minutes, not days)
```

**4. Cloud Migration:**

```
✅ Lift-and-shift (VM to cloud)
✅ Hybrid cloud (some VMs on-prem, some cloud)

Example:
  On-premises VM → Export → Import to AWS/Azure
  Minimal changes needed
```

---

### Best Practices

**1. Resource Allocation:**

```
✅ Don't over-allocate (avoid resource contention)
✅ Leave headroom (not 100% allocation)
✅ Monitor utilization
✅ Right-size VMs (don't over-provision)

Example:
  Physical server: 64 GB RAM
  Don't allocate: 8 VMs × 8 GB = 64 GB (100%)
  Better: 8 VMs × 6 GB = 48 GB (75%)
  Leaves 16 GB for host and overhead
```

**2. High Availability:**

```
✅ Cluster hosts (multiple physical servers)
✅ Shared storage (SAN/NAS)
✅ HA features (auto-restart VMs)
✅ Live migration (move VMs without downtime)

Example:
  Host 1 fails → VMs auto-restart on Host 2
  Or: Live migrate VMs from Host 1 to Host 2 (no downtime)
```

**3. Backup and Recovery:**

```
✅ Regular VM backups (snapshots)
✅ Test restores (verify backups work)
✅ Offsite backups (DR)
✅ Retention policy (30/60/90 days)
```

**4. Security:**

```
✅ VM isolation (prevent VM-to-VM attacks)
✅ Patch management (keep hypervisor updated)
✅ Network segmentation (VLANs, firewalls)
✅ Access control (who can manage VMs?)
```

**5. Performance Monitoring:**

```
✅ Monitor CPU, RAM, disk, network
✅ Set alerts (thresholds)
✅ Capacity planning (when to add resources?)
✅ Identify bottlenecks
```

---

## Summary (สรุป)

Module 13 นี้เราได้เรียนรู้:

1. ✅ **Cloud Computing**:
    
    - **Service Models**:
        - IaaS (Infrastructure): EC2, Azure VMs
        - PaaS (Platform): Heroku, App Engine
        - SaaS (Software): Gmail, Office 365
    - **Deployment Models**:
        - Public Cloud (AWS, Azure, GCP)
        - Private Cloud (on-premises)
        - Hybrid Cloud (combination)
        - Community Cloud (shared)
2. ✅ **Virtualization**:
    
    - Create multiple virtual instances on physical hardware
    - Benefits: Consolidation, cost savings, flexibility
    - **Hypervisors**:
        - Type 1 (Bare Metal): ESXi, Hyper-V - Production
        - Type 2 (Hosted): VirtualBox, VMware WS - Desktop/Testing
3. ✅ **Virtual Machines**:
    
    - Virtual hardware (vCPU, vRAM, vDisk, vNIC)
    - Benefits: Consolidation, rapid provisioning, DR
    - Limitations: Overhead, licensing, resource contention
4. ✅ **Network Virtualization**:
    
    - Virtual switches (vSwitch)
    - Virtual NICs (vNIC)
    - **SDN** (Software-Defined Networking):
        - Centralized control
        - Programmable
        - API-driven
    - **NFV** (Network Functions Virtualization):
        - Network functions as software
        - Virtual router, firewall, load balancer
5. ✅ **Containers**:
    
    - Lightweight virtualization
    - Share host OS kernel
    - **Docker**: Images, containers, registries
    - **VMs vs Containers**:
        - VMs: Full OS, GBs, minutes startup
        - Containers: Shared OS, MBs, seconds startup
    - **Kubernetes**: Container orchestration
        - Auto-scaling, self-healing, rolling updates
6. ✅ **Enterprise Use Cases**:
    
    - Server consolidation (reduce hardware)
    - Dev/test environments
    - Disaster recovery
    - Cloud migration (lift-and-shift)

**สิ่งสำคัญที่ต้องจำ:**

- **Cloud Service Models**: IaaS (infra), PaaS (platform), SaaS (software)
- **Cloud Deployment**: Public (AWS), Private (on-prem), Hybrid (both)
- **Hypervisor Types**: Type 1 (bare metal, production), Type 2 (hosted, desktop)
- **VM Benefits**: Consolidation, rapid provisioning, isolation, DR
- **SDN**: Control plane separated, centralized, programmable
- **NFV**: Network functions as software (VMs)
- **Containers**: Lightweight, portable, fast, microservices
- **Docker**: Image → Container (running instance)
- **Kubernetes**: Container orchestration (manage many containers)
- **VMs vs Containers**: VMs = full OS, Containers = shared OS (lighter)

**Technology Comparison:**

```
Feature          Physical    VMs            Containers
------------------------------------------------------------------------
Size             N/A         GBs            MBs
Boot Time        Minutes     Minutes        Seconds
Density          1           10-20/host     100-200/host
Isolation        Best        Good           Process-level
Portability      Low         Medium         High
Resource Use     Dedicated   Heavy          Lightweight
Use Case         Legacy      Most workloads Microservices
------------------------------------------------------------------------
```

**When to Use:**

```
Physical Servers:
  ✅ Maximum performance needed
  ✅ Hardware-specific apps
  ✅ Licensing restrictions

Virtual Machines:
  ✅ Server consolidation
  ✅ Different OS needed
  ✅ Strong isolation needed
  ✅ Traditional apps

Containers:
  ✅ Microservices
  ✅ Cloud-native apps
  ✅ CI/CD pipelines
  ✅ Portability needed
  ✅ High density needed
```

**Cloud Decision:**

```
IaaS:     Need full control (OS, apps)
PaaS:     Focus on app development (no infra management)
SaaS:     Ready-to-use software (no IT management)

Public:   Low cost, scalability (general use)
Private:  Control, compliance (sensitive data)
Hybrid:   Best of both (flexibility)
```

**Next Module:** Module 14 - Network Automation (Final Module!)

---

**[ไฟล์ Module 13 สมบูรณ์แล้ว!]**