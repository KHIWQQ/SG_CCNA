# CCNA 2 Quiz Collection

📚 คลังคำถามสำหรับการสอบ CCNA 2 (Routing and Switching Essentials)

## 📋 เนื้อหา

ไฟล์นี้ประกอบด้วยคำถามทั้งหมด **173 ข้อ** ครอบคลุมหัวข้อต่างๆ ดังนี้:

- Static Routing (IPv4 และ IPv6)
- Default Routes
- VLAN และ Inter-VLAN Routing
- Network Security (VLAN Hopping, MAC Flooding, DHCP Spoofing)
- DHCPv4 และ DHCPv6
- Routing Protocols (EIGRP, OSPF)
- Network Troubleshooting

## 📁 โครงสร้างไฟล์

```
.
├── README.md                   # ไฟล์นี้
├── CCNA_2_Quiz.md              # ไฟล์คำถามทั้งหมด (173 ข้อ)
├── CCNA_2_Quiz_ANSWERS.md      # ⭐ เฉลยคำตอบพร้อมคำอธิบาย (174 ข้อ)
├── ANSWERS_README.md           # คู่มือการใช้เฉลย
├── TABLE_OF_CONTENTS.md        # สารบัญคำถาม
├── QUICK_REFERENCE.md          # สรุปเนื้อหาแบบเร็ว
├── GITHUB_UPLOAD_GUIDE.md      # คู่มืออัพโหลด GitHub
└── images/                     # โฟลเดอร์รูปภาพ
    ├── image_1.png             # รูปข้อสอบ (48 รูป)
    ├── image_2.png
    ├── ...
    └── answers/                # โฟลเดอร์รูปเฉลย (49 รูป)
        ├── answer_image_1.png
        └── ...
```

## 🎯 วิธีใช้งาน

1. **อ่านคำถาม**: เปิดไฟล์ [CCNA_2_Quiz.md](CCNA_2_Quiz.md)
2. **ดูรูปภาพประกอบ**: รูปภาพจะแสดงอัตโนมัติใน markdown viewer
3. **ฝึกทำข้อสอบ**: ลองตอบคำถามด้วยตัวเอง แล้วเช็คคำตอบ
4. **⭐ ตรวจเฉลย**: เปิดไฟล์ [CCNA_2_Quiz_ANSWERS.md](CCNA_2_Quiz_ANSWERS.md) เพื่อดูคำตอบที่ถูกต้อง (มี ✅) พร้อมคำอธิบายละเอียด
5. **อ่านคำอธิบาย**: ทุกข้อมีคำอธิบายเหตุผล ช่วยให้เข้าใจลึกซึ้งยิ่งขึ้น

## 🖼️ รูปแบบคำถาม

แต่ละข้อจะประกอบด้วย:
- **หัวข้อคำถาม**: อธิบายสถานการณ์หรือปัญหา
- **รูปภาพ Network Diagram**: (ถ้ามี) แสดง topology และการตั้งค่า
- **ตัวเลือก (Options)**: คำตอบที่เป็นไปได้ 4 ข้อ

## 📝 ตัวอย่างคำถาม

```markdown
## Question 1

**Refer to the exhibit. What will router R1 do with a packet that 
has a destination IPv6 address of 2001:db8:cafe:5::1?**

![Question 1 Exhibit](images/image_1.png)

**Options:**

- forward the packet out GigabitEthernet0/0
- drop the packet
- forward the packet out GigabitEthernet0/1
- forward the packet out Serial0/0/0
```

### 🔍 ตัวอย่างเฉลยคำตอบ

```markdown
## Question 1

**Options:**

- forward the packet out GigabitEthernet0/0
- drop the packet
- forward the packet out GigabitEthernet0/1
- ✅ **forward the packet out Serial0/0/0** _(Correct Answer)_

**Explanation:**

The route ::/0 is the compressed form of the default route. 
The default route is used if a more specific route is not 
found in the routing table.
```

## 🚀 Tips สำหรับการทำข้อสอบ

1. **อ่านโจทย์ให้ละเอียด** - โดยเฉพาะข้อความ "Refer to the exhibit"
2. **วิเคราะห์ Network Diagram** - ดู IP, subnet mask, routing table
3. **ระวังคำถามเชิงลบ** - เช่น "Which is NOT..." หรือ "What will NOT..."
4. **ทบทวน concept** - Static route, floating route, administrative distance

## 📚 หัวข้อที่ควรทบทวน

- ✅ Static Route Configuration
- ✅ IPv6 Routing
- ✅ Default Route (0.0.0.0/0)
- ✅ Floating Static Routes
- ✅ VLAN Security
- ✅ DHCP Configuration
- ✅ Layer 2 Attacks (MAC Flooding, VLAN Hopping)
- ✅ Routing Protocol Basics

## 🔗 ทรัพยากรเพิ่มเติม

- [Cisco CCNA 2 Official Curriculum](https://www.netacad.com/)
- [Cisco IOS Command Reference](https://www.cisco.com/c/en/us/support/docs/ios-nx-os-software/ios-software-releases-121-mainline/26523-routing-command.html)

## 📄 License

เนื้อหานี้จัดทำขึ้นเพื่อการศึกษาเท่านั้น

---

⭐ ขอให้สอบผ่านครับ! Good luck! 🎓
