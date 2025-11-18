# CCNA Course 3 - Module 9: QoS Concepts

## แนวคิดเกี่ยวกับ Quality of Service

---

## 9.1 Network Transmission Quality

### What is QoS?

**คำจำกัดความ:**

- **Quality of Service (QoS)** = การรับประกันคุณภาพการส่งข้อมูล
- จัดการ bandwidth, delay, jitter, packet loss
- จัดลำดับความสำคัญของ traffic
- รับประกัน performance สำหรับ applications ที่สำคัญ

**Why QoS is Needed:**

```
Problem: Limited Bandwidth + Mixed Traffic Types

Without QoS:
  Voice call (VoIP)    ┐
  Video conference     ├─── Compete equally
  File download        │    for bandwidth
  Web browsing         │
  Email                ┘
  
  → Voice/Video may suffer (delay, choppy)
  → Poor user experience

With QoS:
  Voice call (VoIP)    ─── Priority 1 (highest)
  Video conference     ─── Priority 2
  Web browsing         ─── Priority 3
  Email                ─── Priority 4
  File download        ─── Priority 5 (lowest)
  
  → Voice/Video get priority
  → Good user experience
```

---

### Network Traffic Characteristics

#### Bandwidth

**คำจำกัดความ:**

```
- ความจุของ link (capacity)
- วัดเป็น bits per second (bps)
- จำกัด (finite resource)
```

**Examples:**

```
Application          Typical Bandwidth
-----------------------------------------------------
VoIP call            64-128 Kbps
Video call (HD)      1-4 Mbps
Video streaming 4K   25 Mbps
File download        Varies (best effort)
Web browsing         100 Kbps - 10 Mbps
Email                Low (< 100 Kbps)
-----------------------------------------------------
```

**Issue:**

```
Total demand > Available bandwidth
→ Congestion
→ Packet loss
→ Delays
```

---

#### Delay (Latency)

**คำจำกัดความ:**

```
- เวลาที่ packet ใช้เดินทางจากต้นทางถึงปลายทาง
- วัดเป็น milliseconds (ms)
- ประกอบด้วย delays หลายประเภท
```

**Types of Delay:**

**1. Serialization Delay:**

```
- เวลาที่ใช้ put bits บน wire
- ขึ้นกับ packet size และ link speed

Formula: Serialization Delay = Packet Size / Link Speed

Example:
  1500-byte packet on 10 Mbps link
  = (1500 × 8 bits) / 10,000,000 bps
  = 0.0012 seconds = 1.2 ms
  
  Same packet on 1 Gbps link
  = 0.000012 seconds = 0.012 ms (100× faster!)
```

**2. Propagation Delay:**

```
- เวลาที่สัญญาณเดินทางบน media
- ขึ้นกับระยะทางและ speed of light

Formula: Propagation Delay = Distance / Speed of Light

Example:
  1000 km fiber link
  Speed in fiber ≈ 200,000 km/s (2/3 speed of light)
  = 1000 / 200,000 = 0.005 seconds = 5 ms
```

**3. Processing Delay:**

```
- เวลาที่ router ใช้ process packet
- Routing table lookup, ACL checking, QoS policies
- Typically: 1-10 microseconds (modern routers)
```

**4. Queuing Delay:**

```
- เวลารอใน queue ก่อนส่ง
- ขึ้นกับ congestion
- Variable (แปรผัน)
- Main target of QoS!

Example:
  No congestion: 0 ms
  Heavy congestion: 100+ ms
```

**Total Delay:**

```
End-to-End Delay = Serialization + Propagation + Processing + Queuing

Example (VoIP):
  Acceptable: < 150 ms
  Noticeable: 150-300 ms
  Unacceptable: > 300 ms
```

---

#### Jitter

**คำจำกัดความ:**

```
- Variation in delay (ความแปรปรวนของ delay)
- Packets arrive at irregular intervals
- วัดเป็น ms
- ปัญหาสำคัญสำหรับ real-time applications
```

**Example:**

```
Without Jitter (Good):
  Packet 1: arrives at 0 ms
  Packet 2: arrives at 20 ms (20 ms later)
  Packet 3: arrives at 40 ms (20 ms later)
  Packet 4: arrives at 60 ms (20 ms later)
  → Consistent intervals

With Jitter (Bad):
  Packet 1: arrives at 0 ms
  Packet 2: arrives at 25 ms (25 ms later)
  Packet 3: arrives at 35 ms (10 ms later)
  Packet 4: arrives at 70 ms (35 ms later)
  → Inconsistent intervals
  → Voice/video choppy, garbled
```

**Jitter Buffer:**

```
- Solution: Buffer at receiver
- Store packets, play out at constant rate
- Smooths out jitter
- Adds delay (trade-off)

Example:
  Jitter buffer: 50 ms
  → Adds 50 ms delay
  → But smooth playback
```

**Acceptable Jitter:**

```
VoIP:              < 30 ms
Video conference:  < 50 ms
Video streaming:   More tolerant (larger buffer)
```

---

#### Packet Loss

**คำจำกัดความ:**

```
- Packets dropped (ไม่ถึงปลายทาง)
- วัดเป็น percentage (%)
- เกิดจาก congestion, errors, buffer overflow
```

**Causes:**

```
1. Queue Overflow:
   - Queue full
   - New packets dropped (tail drop)
   - Most common cause

2. Link Errors:
   - Corruption on wire
   - CRC errors
   - Less common on modern networks

3. TTL Expiration:
   - Routing loops
   - Rare
```

**Impact:**

```
Data (TCP):
  - Retransmission
  - Slower, but recoverable
  - Acceptable: < 1%

Voice (VoIP):
  - No retransmission (real-time)
  - Gaps, clicks, choppy audio
  - Acceptable: < 1%

Video:
  - Pixelation, freezing
  - Acceptable: < 1%
```

---

### Traffic Types and Requirements

#### Voice (VoIP)

**Characteristics:**

```
- Real-time
- Two-way (interactive)
- Constant bit rate
- Small packets (160-200 bytes)
- Frequent (50 packets/second)
```

**Requirements:**

```
Bandwidth:     64-128 Kbps per call
Delay:         < 150 ms (one-way)
Jitter:        < 30 ms
Packet Loss:   < 1%
Priority:      Highest
```

**Sensitivity:**

```
Bandwidth:     Medium (predictable)
Delay:         Very sensitive
Jitter:        Very sensitive
Packet Loss:   Very sensitive
```

---

#### Video

**Characteristics:**

```
- Real-time (live) or streaming (buffered)
- One-way or two-way
- Variable bit rate (VBR) or constant (CBR)
- Large packets
- Frequent
```

**Types:**

**Video Conferencing (Interactive):**

```
Bandwidth:     384 Kbps - 10 Mbps
Delay:         < 150 ms
Jitter:        < 50 ms
Packet Loss:   < 1%
Priority:      High
```

**Video Streaming (One-way):**

```
Bandwidth:     1-25+ Mbps (depends on quality)
Delay:         More tolerant (buffering)
Jitter:        More tolerant (buffering)
Packet Loss:   < 1-5%
Priority:      Medium-High
```

---

#### Data

**Characteristics:**

```
- Non-real-time (usually)
- Bursty (irregular traffic patterns)
- Variable packet sizes
- Uses TCP (retransmission)
```

**Types:**

**Mission-Critical Data:**

```
- ERP, databases, financial transactions
Priority:      Medium-High
Examples:      SAP, Oracle, banking systems
```

**Transactional Data:**

```
- Email, web browsing
Priority:      Medium
Examples:      HTTP, SMTP, POP3
```

**Bulk Data:**

```
- File transfers, backups
Priority:      Low
Examples:      FTP, backup traffic
Tolerates:     Delay, jitter (uses TCP)
```

---

### QoS Requirements Summary

```
Application      Bandwidth  Delay      Jitter    Loss   Priority
------------------------------------------------------------------------
VoIP             Low        < 150 ms   < 30 ms   < 1%   Highest
Video Conf       Medium-Hi  < 150 ms   < 50 ms   < 1%   High
Video Stream     High       Tolerant   Tolerant  < 5%   Med-High
Mission Data     Variable   < 1 sec    Tolerant  0%     Medium
Web/Email        Variable   < 5 sec    Tolerant  0%     Low-Med
File Transfer    High       Tolerant   Tolerant  0%     Low
------------------------------------------------------------------------
```

---

## 9.2 Traffic Characteristics

### Best-Effort Delivery

**คำจำกัดความ:**

```
- Default behavior (no QoS)
- First-Come, First-Served (FCFS)
- No guarantees
- All traffic treated equally
```

**How it Works:**

```
All packets → Single queue → FIFO (First In, First Out)

[Packet 1] [Packet 2] [Packet 3] [Packet 4] → Output
    ↓          ↓          ↓          ↓
All packets wait in order, regardless of type
```

**Problems:**

```
❌ Voice packets wait behind large file transfers
❌ No priority for critical applications
❌ Congestion affects all traffic equally
❌ Poor performance for real-time apps
```

**When Acceptable:**

```
✅ Sufficient bandwidth (no congestion)
✅ No real-time applications
✅ All applications equally important
```

---

### Bandwidth vs Throughput vs Goodput

**Bandwidth:**

```
- Maximum capacity of link
- Theoretical maximum
- Example: 100 Mbps Ethernet

Analogy: Highway with 4 lanes (capacity)
```

**Throughput:**

```
- Actual data transfer rate
- Always ≤ Bandwidth
- Includes all overhead (headers, retransmissions)
- Example: 95 Mbps actual

Analogy: Cars actually moving on highway (with traffic)
```

**Goodput:**

```
- Useful data only (application data)
- Excludes all overhead
- Always ≤ Throughput
- Example: 85 Mbps useful data

Analogy: Cargo only (not cars/trucks themselves)
```

**Relationship:**

```
Goodput < Throughput ≤ Bandwidth

Example:
  Bandwidth:    100 Mbps (link capacity)
  Throughput:   95 Mbps (actual transfer with headers)
  Goodput:      85 Mbps (useful data only)
  
  Overhead:     15 Mbps (headers, retransmissions, etc.)
```

---

### Network Congestion

**What is Congestion?**

```
- Demand > Available bandwidth
- Queues fill up
- Packets delayed or dropped
```

**Causes:**

```
1. Insufficient bandwidth
   - Link too slow for traffic

2. Traffic bursts
   - Sudden spike in traffic

3. Bottleneck
   - Slower link downstream

4. Device limitations
   - CPU, memory, buffer space
```

**Effects:**

```
1. Increased delay
   - Queuing delay grows

2. Increased jitter
   - Variable delays

3. Packet loss
   - Queue overflow (tail drop)

4. Reduced throughput
   - TCP backs off (congestion avoidance)
```

**Example:**

```
Scenario:
  LAN: 1 Gbps (fast)
  WAN link: 10 Mbps (slow)
  Traffic: 100 Mbps burst from LAN → WAN
  
Result:
  → Bottleneck at WAN link
  → Queue fills (can only send 10 Mbps)
  → Packets delayed
  → Eventually packets dropped
```

---

### Tail Drop

**คำจำกัดความ:**

```
- Default behavior when queue full
- Drop new arriving packets
- Simple but problematic
```

**How it Works:**

```
Queue state:
  [P1][P2][P3][P4][P5][P6][P7][P8] ← FULL!
  
New packet arrives → DROPPED
  
Simple rule: Queue full? Drop new packet.
```

**Problems:**

```
1. TCP Synchronization:
   - Multiple TCP flows drop packets simultaneously
   - All TCP flows back off together
   - All ramp up together
   - Traffic oscillates (waves)
   - Inefficient link utilization

2. No differentiation:
   - Important packets dropped same as unimportant
   - VoIP dropped same as file transfer

3. Bursty drops:
   - Many packets dropped at once
   - Poor for applications
```

**TCP Global Synchronization:**

```
Time
  ↑
  │     ┌─────┐        ┌─────┐        ┌─────┐
  │    ╱       ╲      ╱       ╲      ╱       ╲
  │   ╱         ╲    ╱         ╲    ╱         ╲
  │  ╱           ╲  ╱           ╲  ╱           ╲
  │ ╱             ╲╱             ╲╱             ╲
  └────────────────────────────────────────────────→
  
All TCP flows drop, back off, ramp up together
→ Inefficient (link underutilized during backoff)
```

---

## 9.3 Queuing Algorithms

### FIFO (First In, First Out)

**คำจำกัดความ:**

```
- Simplest queuing
- Single queue
- First packet in → First packet out
- No QoS
```

**How it Works:**

```
Input:         Queue:                    Output:
[VoIP] ───┐   [P1][P2][P3][P4][P5] ───→ [P1]
[Video]───┤   
[File] ───┘   FIFO order

All packets treated equally
```

**Characteristics:**

```
✅ Simple
✅ Low overhead
✅ Fast
❌ No QoS
❌ No priority
❌ Subject to tail drop
```

---

### Weighted Fair Queuing (WFQ)

**คำจำกัดความ:**

```
- Automatic QoS (no configuration needed)
- Multiple queues (per flow)
- Bandwidth shared fairly
- Favors low-volume traffic (e.g., interactive)
```

**How it Works:**

```
1. Classify traffic into flows
   - Based on: Src IP, Dst IP, Src Port, Dst Port, Protocol

2. Create queue per flow
   - Flow 1 (VoIP):   [Q1]
   - Flow 2 (Video):  [Q2]
   - Flow 3 (FTP):    [Q3]

3. Schedule fairly
   - Low-volume flows get priority
   - High-volume flows get remaining bandwidth
```

**Fairness:**

```
Example:
  Link: 10 Mbps
  
  Flow 1 (VoIP):        100 Kbps needed
  Flow 2 (Video):       2 Mbps needed
  Flow 3 (File):        50 Mbps needed
  
WFQ allocation:
  VoIP:   100 Kbps (gets what it needs)
  Video:  2 Mbps (gets what it needs)
  File:   7.9 Mbps (gets remaining)
  
All satisfied within bandwidth!
```

**Characteristics:**

```
✅ Automatic (no config)
✅ Fair sharing
✅ Favors interactive traffic
✅ Prevents large flows from starving small flows
❌ No explicit priority control
❌ Limited customization
❌ Not ideal for voice (needs guaranteed priority)
```

---

### Class-Based Weighted Fair Queuing (CBWFQ)

**คำจำกัดความ:**

```
- Extension of WFQ
- User-defined classes
- Bandwidth guarantees per class
- More control than WFQ
```

**How it Works:**

```
1. Define classes
   - Voice class
   - Video class
   - Data class
   - Default class

2. Assign bandwidth to each class
   - Voice: 1 Mbps
   - Video: 3 Mbps
   - Data: 2 Mbps
   - Default: Remaining

3. Queue per class
   - Within class: FIFO
   - Between classes: Weighted scheduling
```

**Example:**

```
Link: 10 Mbps

Class         Bandwidth    Queue
---------------------------------------
Voice         1 Mbps       [Q1]
Video         3 Mbps       [Q2]
Data          2 Mbps       [Q3]
Default       4 Mbps       [Q4] (remaining)

Guarantees: Each class gets at least minimum bandwidth
If class not using full allocation → others can use
```

**Characteristics:**

```
✅ User-defined classes
✅ Bandwidth guarantees
✅ Flexible
✅ Predictable
❌ Doesn't handle delay-sensitive traffic optimally
❌ Doesn't prevent delay (packets wait in queue)
```

---

### Low Latency Queuing (LLQ)

**คำจำกัดความ:**

```
- CBWFQ + Priority Queue
- Strict priority for delay-sensitive traffic
- Best for VoIP, video conferencing
- Most commonly used in production
```

**How it Works:**

```
1. Priority Queue (PQ)
   - Voice, video conferencing
   - Always serviced first
   - Strict priority

2. CBWFQ for other classes
   - Bandwidth guarantees
   - Served after PQ

        ┌─────────────┐
        │Priority Q   │ ← Voice (always first)
        │   (LLQ)     │
        └─────────────┘
             ↓
        ┌─────────────┐
        │Video (CBWFQ)│ ← After PQ
        └─────────────┘
        ┌─────────────┐
        │Data (CBWFQ) │
        └─────────────┘
        ┌─────────────┐
        │Default      │
        └─────────────┘
```

**Example:**

```
Link: 10 Mbps

Queue         Type    Bandwidth   Behavior
----------------------------------------------------------
Voice         LLQ     1 Mbps      Strict priority (first)
Video         CBWFQ   3 Mbps      Guaranteed (after voice)
Data          CBWFQ   2 Mbps      Guaranteed (after voice)
Default       CBWFQ   4 Mbps      Remaining
----------------------------------------------------------

Voice: ✅ Low delay, low jitter
Video: ✅ Guaranteed bandwidth
Data:  ✅ Guaranteed bandwidth
```

**Characteristics:**

```
✅ Low latency for priority traffic
✅ Prevents delay for voice/video
✅ Bandwidth guarantees for other classes
✅ Industry standard
⚠️ Priority queue can starve other queues (policing needed)
```

**Policing Priority Queue:**

```
Problem: Priority queue could use all bandwidth
Solution: Limit priority queue (police)

Example:
  Voice LLQ: 1 Mbps maximum
  → Prevents starvation of other queues
  → If voice exceeds 1 Mbps → excess dropped
```

---

## 9.4 QoS Mechanisms

### Classification and Marking

**Classification:**

```
- Identify traffic types
- First step in QoS
- Based on various criteria
```

**Classification Criteria:**

```
Layer 2/3:
  - Source/Destination MAC
  - Source/Destination IP
  - IP Precedence
  - DSCP (Differentiated Services Code Point)
  - CoS (Class of Service)

Layer 4:
  - Source/Destination Port
  - Protocol (TCP, UDP, ICMP)

Layer 7:
  - Application (HTTP, VoIP, FTP)
  - NBAR (Network-Based Application Recognition)
```

---

**Marking:**

```
- Tag packets with QoS value
- Devices downstream use marking for QoS
- Multiple marking fields available
```

**Marking Fields:**

#### Layer 2 - CoS (Class of Service)

```
- 802.1Q VLAN tag
- 3 bits (0-7)
- 8 values
- Layer 2 only (doesn't cross routers)

CoS Values:
  0 = Best Effort (default)
  1 = Background
  2 = Standard
  3 = Excellent Effort
  4 = Controlled Load
  5 = Voice (EF - Expedited Forwarding)
  6 = Internetwork Control
  7 = Network Control

Recommendation:
  5 = Voice
  4 = Video
  0-3 = Data (various priorities)
```

---

#### Layer 3 - IP Precedence (Legacy)

```
- Top 3 bits of ToS byte in IP header
- 3 bits (0-7)
- 8 values
- Legacy (replaced by DSCP)

IP Precedence Values:
  0 = Routine (Best Effort)
  1 = Priority
  2 = Immediate
  3 = Flash
  4 = Flash Override
  5 = Critical (Voice)
  6 = Internetwork Control
  7 = Network Control
```

---

#### Layer 3 - DSCP (Differentiated Services Code Point)

```
- Modern standard
- Top 6 bits of ToS byte in IP header
- 64 possible values (0-63)
- Replaces IP Precedence

IP Header ToS Byte:
  ┌───────────────────────────────────┐
  │ DSCP (6 bits) │ ECN (2 bits)      │
  └───────────────────────────────────┘
      0-63            Congestion notify
```

**Common DSCP Values:**

```
DSCP Name           Decimal   Binary    Use
------------------------------------------------------------------------
Default (BE)        0         000000    Best Effort (default)
CS1                 8         001000    Scavenger (bulk)
AF11                10        001010    Low priority data
AF21                18        010010    Medium priority data
AF31                26        011010    High priority data
AF41                34        100010    Video
EF (Expedited Fwd)  46        101110    Voice
CS6                 48        110000    Network control
CS7                 56        111000    Network control
------------------------------------------------------------------------

EF = Expedited Forwarding (Voice)
AF = Assured Forwarding (Data with priority)
CS = Class Selector (compatibility with IP Prec)
```

**DSCP Recommendations (RFC 4594):**

```
Application             DSCP Value      Name
------------------------------------------------------------------------
Voice (VoIP)            46 (EF)         Expedited Forwarding
Interactive Video       34 (AF41)       Assured Forwarding 41
Streaming Video         32 (CS4)        Class Selector 4
Mission-Critical Data   26 (AF31)       Assured Forwarding 31
Transactional Data      18 (AF21)       Assured Forwarding 21
Bulk Data               10 (AF11)       Assured Forwarding 11
Best Effort             0  (BE)         Default
Scavenger               8  (CS1)        Class Selector 1
------------------------------------------------------------------------
```

---

#### CoS vs DSCP

```
Feature          CoS                 DSCP
------------------------------------------------------------------------
Layer            2 (Ethernet)        3 (IP)
Bits             3 (0-7)             6 (0-63)
Values           8                   64
Scope            LAN (L2)            Routed networks (L3)
Standard         802.1Q/p            RFC 2474
Trust boundary   Access switch       Distribution/Core
------------------------------------------------------------------------

Recommendation:
  - Mark at Layer 3 (DSCP) for end-to-end QoS
  - Use CoS only for LAN segments
  - Map CoS ↔ DSCP at trust boundary
```

---

### Trust Boundary

**คำจำกัดความ:**

```
- จุดที่ network trust QoS markings จาก device
- Typically: Access switch or router
- Outside trust boundary: Remark or classify
```

**Trust Boundary Placement:**

```
Scenario 1: Trust end device (IP Phone)
  [IP Phone] ──TRUST──→ [Switch] ─→ [Router]
     DSCP=46            (keeps DSCP=46)
  
  ✅ Phone marks voice (DSCP=46)
  ✅ Network trusts marking

Scenario 2: Don't trust end device (PC)
  [PC] ──NO TRUST──→ [Switch] ─→ [Router]
    DSCP=46          (remarked to DSCP=0)
  
  ❌ PC marks traffic (DSCP=46) - trying to cheat!
  ✅ Switch remarks to default (DSCP=0)
  ✅ Prevents abuse
```

**Best Practices:**

```
Trust:
  ✅ IP Phones (802.1X authenticated)
  ✅ Trusted servers
  ✅ Network devices (routers, switches)

Don't Trust:
  ❌ End-user PCs
  ❌ Guest devices
  ❌ Unmanaged devices
  
Remark at trust boundary (access switch)
```

---

### Congestion Avoidance

**Problem: Tail Drop Issues**

```
- TCP global synchronization
- Inefficient link utilization
- No differentiation
```

**Solution: Random Early Detection (RED)**

**How RED Works:**

```
1. Monitor queue depth
2. When queue reaches threshold → Start dropping randomly
3. Before queue full → Prevents tail drop
4. Random drops → Prevents TCP synchronization

Queue States:
  Empty ────────────── Min ───────── Max ──── Full
  
  < Min:              No drops
  Min - Max:          Random drops (probability increases)
  > Max:              Drop all (like tail drop)
```

**Benefits:**

```
✅ Prevents TCP global synchronization
✅ Better link utilization
✅ Smoother traffic flow
✅ Proactive (drops before queue full)
```

---

**Weighted RED (WRED):**

```
- Extension of RED
- Different thresholds per class
- Drop low-priority traffic first

Example:
  High-priority traffic:  Min=40, Max=60
  Low-priority traffic:   Min=20, Max=40
  
  → Low-priority drops first (at 20)
  → High-priority protected until 40
```

**WRED Profile:**

```
Drop      │
Probability│         ┌─────────
(%)        │        ╱
100%───────│       ╱
           │      ╱
50%────────│     ╱
           │    ╱
0%─────────┴───┴──────────────
           Min Max  Full
           Threshold

Min: Start dropping randomly
Max: Drop all
```

---

### Policing and Shaping

**Purpose:**

```
- Control traffic rate
- Enforce bandwidth limits
- Prevent oversubscription
```

---

#### Policing

**คำจำกัดความ:**

```
- Enforce bandwidth limit
- Drop or remark excess traffic
- Immediate action
- Typically on ingress
```

**How it Works:**

```
Configured rate: 10 Mbps

Traffic rate > 10 Mbps → Excess packets dropped/remarked
Traffic rate ≤ 10 Mbps → All packets allowed

Example:
  Traffic: 15 Mbps
  Policy: 10 Mbps
  Result: 10 Mbps forwarded, 5 Mbps dropped
```

**Characteristics:**

```
✅ Simple
✅ Immediate enforcement
✅ Low memory (no buffering)
❌ Drops packets (may cause retransmissions)
❌ Can hurt TCP performance
```

**Use Cases:**

```
- ISP enforcing customer bandwidth
- Limit traffic entering network (ingress)
- Prevent DOS attacks
- Enforce service level agreements (SLAs)
```

---

#### Shaping

**คำจำกัดความ:**

```
- Enforce bandwidth limit
- Buffer excess traffic
- Smooth traffic flow
- Typically on egress
```

**How it Works:**

```
Configured rate: 10 Mbps

Traffic rate > 10 Mbps → Excess packets buffered (queued)
Traffic rate ≤ 10 Mbps → All packets forwarded

Buffer sends at shaped rate (10 Mbps)

Example:
  Traffic: 15 Mbps burst
  Shaping: 10 Mbps
  Result: 10 Mbps sent, 5 Mbps buffered → sent later
```

**Characteristics:**

```
✅ Smooth traffic
✅ No packet loss (buffered)
✅ Better for TCP
❌ Adds delay (buffering)
❌ Requires memory (buffer)
```

**Use Cases:**

```
- Shape traffic to WAN speed (before bottleneck)
- Ensure traffic fits provider's rate limit
- Smooth bursty traffic
- Video streaming (constant rate)
```

---

#### Policing vs Shaping

```
Feature          Policing             Shaping
------------------------------------------------------------------------
Action           Drop/Remark excess   Buffer excess
Buffer           No                   Yes (queues)
Packet Loss      Yes (excess)         No (buffered)
Delay            No additional        Yes (buffering)
Memory           Low                  Higher
Direction        Typically ingress    Typically egress
Use Case         Enforce limits       Smooth traffic
TCP-friendly     Less                 More
------------------------------------------------------------------------

Analogy:
  Policing = Strict bouncer (over limit → rejected)
  Shaping = Patient queue (over limit → wait in line)
```

---

### Link Efficiency Mechanisms

#### Compression

**Header Compression (cRTP - Compressed RTP):**

```
Purpose: Reduce overhead for VoIP packets

Without compression:
  IP header:   20 bytes
  UDP header:  8 bytes
  RTP header:  12 bytes
  Voice data:  20-160 bytes
  Total:       60-200 bytes
  Overhead:    40 bytes (20-66% overhead!)

With cRTP:
  Compressed:  2-4 bytes
  Voice data:  20-160 bytes
  Total:       22-164 bytes
  Overhead:    2-4 bytes (1-20% overhead)
  
Savings: 36-38 bytes per packet
Benefit: More bandwidth for voice calls on slow links
```

**Payload Compression:**

```
- Compress actual data (not just headers)
- CPU intensive
- Adds latency
- Use only on slow links (< 2 Mbps)

Example: LZS, STAC
```

---

#### Link Fragmentation and Interleaving (LFI)

**Problem:**

```
Slow link: 64 Kbps

Large packet: 1500 bytes
Serialization delay = (1500 × 8) / 64000 = 187.5 ms

Voice packet arrives → Must wait 187.5 ms!
→ Unacceptable delay (> 150 ms requirement)
```

**Solution: LFI**

```
1. Fragment large packets into smaller pieces
2. Interleave small packets (voice) between fragments

Without LFI:
  [Large packet ─────────────────] [Voice] 
  ← 187.5 ms wait →

With LFI:
  [Frag1][Voice][Frag2][Voice][Frag3][Voice]
  ← 10ms→ Low delay!
```

**Fragment Size Calculation:**

```
Goal: < 10 ms serialization delay

Formula: Fragment size = (Link speed × 10 ms) / 8

Example (64 Kbps link):
  = (64000 × 0.01) / 8
  = 80 bytes per fragment
  
1500-byte packet → 19 fragments (80 bytes each)
Voice packets interleaved between fragments
```

---

## 9.5 QoS Models

### Best Effort

**คำจำกัดความ:**

```
- No QoS (default)
- First-Come, First-Served
- No guarantees
```

**Characteristics:**

```
✅ Simple (no configuration)
✅ Low overhead
✅ Scalable
❌ No QoS guarantees
❌ All traffic equal priority
❌ Poor for real-time apps
```

**When to Use:**

```
✅ Sufficient bandwidth (no congestion)
✅ No critical applications
✅ All apps equal importance
```

---

### Integrated Services (IntServ)

**คำจำกัดความ:**

```
- Hard QoS guarantees
- Per-flow reservations
- RSVP (Resource Reservation Protocol)
- End-to-end resource reservation
```

**How it Works:**

```
1. Application requests bandwidth via RSVP
2. RSVP messages travel hop-by-hop
3. Each router reserves resources
4. If all routers accept → Reservation successful
5. Application sends traffic (guaranteed QoS)
```

**Characteristics:**

```
✅ Guaranteed QoS (hard guarantees)
✅ Per-flow granularity
❌ Not scalable (per-flow state on every router)
❌ Complex
❌ Overhead (RSVP signaling)
❌ All routers must support RSVP
```

**Use Cases:**

```
- Small networks
- Special applications (lab environments)
- Not used in modern networks (too complex)
```

---

### Differentiated Services (DiffServ)

**คำจำกัดความ:**

```
- Scalable QoS model
- Class-based (not per-flow)
- DSCP marking
- Industry standard
```

**How it Works:**

```
1. Classify traffic at edge (mark with DSCP)
2. Core routers use DSCP for QoS decisions
3. Per-class behavior (PHB - Per-Hop Behavior)
4. No per-flow state in core
```

**PHBs (Per-Hop Behaviors):**

```
Default PHB:
  - Best Effort (DSCP 0)

EF PHB (Expedited Forwarding):
  - DSCP 46
  - Low delay, jitter, loss
  - Voice

AF PHB (Assured Forwarding):
  - AFxy (x=class, y=drop probability)
  - AF11, AF21, AF31, AF41
  - Data with priority

CS PHB (Class Selector):
  - CS1-CS7
  - Backward compatibility
```

**Characteristics:**

```
✅ Scalable (class-based, not per-flow)
✅ Simple in core (just DSCP)
✅ Flexible
✅ Industry standard
✅ Works across different networks
❌ Soft guarantees (not hard like IntServ)
```

**DiffServ Architecture:**

```
Edge Router (Classification):
  - Classify traffic
  - Mark DSCP
  - Police/Shape

Core Router (Forwarding):
  - Read DSCP
  - Apply PHB (queuing, dropping)
  - No classification (simple)
```

---

### QoS Model Comparison

```
Model         Guarantee   Scalability   Complexity   Standard
------------------------------------------------------------------------
Best Effort   None        Excellent     Very Low     N/A
IntServ       Hard        Poor          High         RSVP
DiffServ      Soft        Excellent     Medium       DSCP/PHB
------------------------------------------------------------------------

Recommendation: DiffServ (industry standard)
```

---

## 9.6 QoS Implementation

### QoS Strategy

**End-to-End QoS:**

```
                  Trust
                Boundary
                    ↓
[Phone] → [Access SW] → [Distrib SW] → [Core SW] → [WAN Router] → [WAN]
 Mark       Trust/        Trust           Trust       Shape/        ISP
 DSCP=46    Remark                                   Queue          QoS

Access:     Classify, Mark, Trust boundary
Distribution: Trust markings, Apply policies
Core:       Simple (trust DSCP)
WAN:        Shape to WAN speed, Priority queuing
```

---

### QoS Best Practices

**1. Classification and Marking:**

```
✅ Mark as close to source as possible
✅ Trust IP Phones (authenticated)
✅ Don't trust PCs
✅ Use DSCP (not CoS or IP Precedence)
✅ Consistent marking across network
```

**2. Trust Boundaries:**

```
✅ Access switch or first network device
✅ Remark untrusted traffic
✅ Use 802.1X for authentication
```

**3. Queuing:**

```
✅ Use LLQ for voice (strict priority)
✅ Limit priority queue (prevent starvation)
✅ CBWFQ for data classes
✅ WRED for congestion avoidance
```

**4. Link Efficiency:**

```
✅ cRTP for slow links (< 2 Mbps)
✅ LFI for slow links (< 768 Kbps)
✅ Compression where beneficial
```

**5. Bandwidth Allocation:**

```
Voice:    Highest priority (LLQ), limit to expected
Video:    High priority, guaranteed bandwidth
Data:     Classes based on business needs
Scavenger: Lowest priority (bulk transfers)

Example (10 Mbps WAN):
  Voice (LLQ):        1 Mbps (max)
  Video (AF41):       3 Mbps (min)
  Critical Data:      2 Mbps (min)
  Default:            4 Mbps (remaining)
```

**6. Monitoring:**

```
✅ Monitor queue depths
✅ Track drops
✅ Measure delay and jitter
✅ Adjust policies based on real traffic
```

---

### DSCP Marking Strategy

**Recommended DSCP Values:**

```
Traffic Type              DSCP      Decimal   Queue Type
------------------------------------------------------------------------
Voice (VoIP)              EF        46        LLQ (priority)
Interactive Video         AF41      34        LLQ or CBWFQ
Streaming Video           CS4       32        CBWFQ
Signaling (SIP, H.323)    CS3       24        CBWFQ
Mission-Critical Data     AF31      26        CBWFQ
Transactional Data        AF21      18        CBWFQ
Bulk Data                 AF11      10        CBWFQ
Best Effort               0         0         Default
Scavenger (P2P, backup)   CS1       8         Lowest priority
------------------------------------------------------------------------
```

---

## Summary (สรุป)

Module 9 นี้เราได้เรียนรู้:

1. ✅ **Network Transmission Quality**:
    
    - **Bandwidth**: Capacity
    - **Delay**: Latency (serialization, propagation, processing, queuing)
    - **Jitter**: Delay variation
    - **Packet Loss**: Dropped packets
2. ✅ **Traffic Characteristics**:
    
    - **VoIP**: Low bandwidth, very delay/jitter sensitive, < 150 ms delay
    - **Video**: Medium-high bandwidth, delay sensitive
    - **Data**: Variable, tolerant of delay (uses TCP)
3. ✅ **Best Effort vs QoS**:
    
    - Best Effort: FIFO, no guarantees
    - QoS: Priority, guarantees
4. ✅ **Queuing Algorithms**:
    
    - **FIFO**: Simple, no QoS
    - **WFQ**: Automatic fairness
    - **CBWFQ**: User-defined classes, bandwidth guarantees
    - **LLQ**: CBWFQ + Priority Queue (best for voice)
5. ✅ **QoS Mechanisms**:
    
    - **Classification**: Identify traffic
    - **Marking**: Tag packets (DSCP, CoS)
    - **Policing**: Drop excess
    - **Shaping**: Buffer excess
    - **WRED**: Congestion avoidance
6. ✅ **Marking Fields**:
    
    - **CoS**: Layer 2, 3 bits (0-7)
    - **DSCP**: Layer 3, 6 bits (0-63), **recommended**
    - EF = 46 (Voice), AF41 = 34 (Video)
7. ✅ **QoS Models**:
    
    - **Best Effort**: No QoS
    - **IntServ**: Hard guarantees, not scalable
    - **DiffServ**: Soft guarantees, scalable, **industry standard**
8. ✅ **Link Efficiency**:
    
    - **cRTP**: Header compression (VoIP)
    - **LFI**: Fragment + interleave (slow links)

**สิ่งสำคัญที่ต้องจำ:**

- QoS = Guarantee quality for important traffic
- VoIP = Most sensitive (delay, jitter, loss)
- LLQ = Best queuing for voice (priority)
- DSCP = Standard marking (Layer 3)
- EF (46) = Voice
- AF41 (34) = Video
- Trust boundary = Access switch/router
- DiffServ = Industry standard model
- Policing = Drop excess
- Shaping = Buffer excess
- WRED = Prevent TCP synchronization
- LFI = For slow links (< 768 Kbps)

**QoS Requirements Quick Reference:**

```
VoIP:
  - Bandwidth: 64-128 Kbps
  - Delay: < 150 ms
  - Jitter: < 30 ms
  - Loss: < 1%
  - DSCP: EF (46)
  - Queue: LLQ (strict priority)

Video Conferencing:
  - Bandwidth: 384 Kbps - 10 Mbps
  - Delay: < 150 ms
  - Jitter: < 50 ms
  - Loss: < 1%
  - DSCP: AF41 (34)
  - Queue: LLQ or high-priority CBWFQ

Data:
  - Tolerates delay
  - Uses TCP (retransmission)
  - DSCP: AF21, AF31 (based on priority)
  - Queue: CBWFQ
```

**Configuration Approach:**

```
1. Classify and mark at network edge (DSCP)
2. Trust markings in core
3. Queue at congestion points (WAN)
4. Use LLQ for voice (limit bandwidth)
5. CBWFQ for other classes
6. WRED for congestion avoidance
7. Shape to WAN speed
8. Monitor and adjust
```

**Next Module:** Module 10 - Network Management

---

**[ไฟล์ Module 9 สมบูรณ์แล้ว!]**