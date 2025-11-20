# CCNA 2 Quiz - Quick Reference

## 📊 สถิติโดยรวม

- **จำนวนคำถามทั้งหมด**: 173 ข้อ
- **คำถามที่มี Network Diagram**: 39 ข้อ
- **จำนวนรูปภาพทั้งหมด**: 48 รูป

## 📚 หัวข้อหลักที่ครอบคลุม

### 1. Static Routing (55+ mentions)
- การตั้งค่า Static Route บน IPv4 และ IPv6
- Default Route (0.0.0.0/0)
- Floating Static Routes
- Administrative Distance
- Next-hop vs Exit Interface

### 2. VLAN (97+ mentions)
- VLAN Configuration
- Trunk และ Access Ports
- Inter-VLAN Routing (Router-on-a-Stick)
- Native VLAN
- VLAN Hopping Attacks
- DTP (Dynamic Trunking Protocol)

### 3. DHCP (68+ mentions)
- DHCPv4 Configuration
- DHCPv6 (Stateful และ Stateless)
- DHCP Relay Agent
- DHCP Snooping
- Router Advertisement (RA)

### 4. Network Security (48+ mentions)
- VLAN Hopping
- MAC Address Table Overflow
- DHCP Spoofing
- ARP Spoofing
- Port Security
- BPDU Guard
- Root Guard

### 5. IPv6 (38+ mentions)
- IPv6 Addressing
- IPv6 Static Routing
- SLAAC (Stateless Address Autoconfiguration)
- DHCPv6
- IPv6 Neighbor Discovery

### 6. EtherChannel (16+ mentions)
- LACP (Link Aggregation Control Protocol)
- PAgP (Port Aggregation Protocol)
- EtherChannel Configuration
- Load Balancing

### 7. Routing Protocols (7+ mentions)
- EIGRP Basics
- OSPF Basics
- Routing Protocol Comparison

## 🎯 คำสั่งสำคัญที่ควรจำ

### Static Routing
```
ip route [network] [mask] [next-hop | exit-interface] [AD]
ipv6 route [prefix/length] [next-hop | exit-interface]
```

### VLAN
```
vlan [vlan-id]
switchport mode access/trunk
switchport access vlan [vlan-id]
switchport trunk allowed vlan [vlan-list]
```

### DHCP
```
ip dhcp pool [name]
network [network] [mask]
default-router [gateway]
dns-server [dns-ip]
```

### Security
```
switchport port-security
switchport port-security maximum [number]
switchport port-security violation [protect|restrict|shutdown]
ip dhcp snooping
```

## 💡 Tips สำหรับการสอบ

1. **อ่าน Network Diagram ให้ดี**
   - ตรวจสอบ IP Address และ Subnet Mask
   - ดู interface ที่ใช้
   - สังเกต routing table

2. **เข้าใจ Administrative Distance**
   - Connected: 0
   - Static: 1
   - EIGRP: 90
   - OSPF: 110
   - RIP: 120

3. **จำ Port Security Violation Modes**
   - **Protect**: Drop packets, no alert
   - **Restrict**: Drop packets, log alert
   - **Shutdown**: Disable port (default)

4. **เข้าใจความแตกต่าง**
   - Access Port vs Trunk Port
   - Native VLAN vs Tagged VLAN
   - Stateful DHCPv6 vs Stateless DHCPv6
   - LACP vs PAgP

5. **ระวังคำถามแบบ Multiple Choice (Choose multiple)**
   - อ่านให้ละเอียดว่าต้องเลือกกี่ข้อ
   - มักจะเป็น "Choose two" หรือ "Choose three"

## 📖 แหล่งข้อมูลเพิ่มเติม

- Cisco Networking Academy: https://www.netacad.com/
- Cisco Command Reference: https://www.cisco.com/
- Packet Tracer Labs: ฝึกทำ Lab เสริมเพื่อเข้าใจมากขึ้น

---

**อัพเดทล่าสุด**: 2025-11-19  
**จำนวนคำถาม**: 173 ข้อ  
**สถานะ**: ✅ Complete
