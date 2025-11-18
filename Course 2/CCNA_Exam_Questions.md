# CCNA 2 v7 - Switching, Routing, and Wireless Essentials
## Final Exam Questions with Answers

---

### Question 1
Refer to the exhibit. What will router R1 do with a packet that has a destination IPv6 address of 2001:db8:cafe:5::1?

![](image_001.png)

- [ ] forward the packet out GigabitEthernet0/0
- [ ] drop the packet
- [ ] forward the packet out GigabitEthernet0/1
- [x] forward the packet out Serial0/0/0

<details>
<summary>💡 Show Answer</summary>

**Answer:** forward the packet out Serial0/0/0

**Explanation:**
The route ::/0 is the compressed form of the 0000:0000:0000:0000:0000:0000:0000:0000/0 default route. The default route is used if a more specific route is not found in the routing table.
</details>

---

### Question 2
Refer to the exhibit. Currently router R1 uses an EIGRP route learned from Branch2 to reach the 10.10.0.0/16 network. Which floating static route would create a backup route to the 10.10.0.0/16 network in the event that the link between R1 and Branch2 goes down?

![](image_002.png)

- [ ] ip route 10.10.0.0 255.255.0.0 Serial 0/0/0 100
- [ ] ip route 10.10.0.0 255.255.0.0 209.165.200.226 100
- [x] ip route 10.10.0.0 255.255.0.0 209.165.200.225 100
- [ ] ip route 10.10.0.0 255.255.0.0 209.165.200.225 50

<details>
<summary>💡 Show Answer</summary>

**Answer:** ip route 10.10.0.0 255.255.0.0 209.165.200.225 100

**Explanation:**
A floating static route needs to have an administrative distance that is greater than the administrative distance of the active route in the routing table. Router R1 is using an EIGRP route which has an administrative distance of 90 to reach the 10.10.0.0/16 network. To be a backup route the floating static route must have an administrative distance greater than 90 and have a next hop address corresponding to the serial interface IP address of Branch1.
</details>

---

### Question 3
Refer to the exhibit. R1 was configured with the static route command `ip route 209.165.200.224 255.255.255.224 S0/0/0` and consequently users on network 172.16.0.0/16 are unable to reach resources on the Internet. How should this static route be changed to allow user traffic from the LAN to reach the Internet?

![](image_003.png)

- [ ] Add an administrative distance of 254
- [x] Change the destination network and mask to 0.0.0.0 0.0.0.0
- [ ] Change the exit interface to S0/0/1
- [ ] Add the next-hop neighbor address of 209.165.200.226

<details>
<summary>💡 Show Answer</summary>

**Answer:** Change the destination network and mask to 0.0.0.0 0.0.0.0

**Explanation:**
The static route on R1 has been incorrectly configured with the wrong destination network and mask. The correct destination network and mask is 0.0.0.0 0.0.0.0.
</details>

---

### Question 4
Which option shows a correctly configured IPv4 default static route?

- [ ] ip route 0.0.0.0 255.255.255.0 S0/0/0
- [x] ip route 0.0.0.0 0.0.0.0 S0/0/0
- [ ] ip route 0.0.0.0 255.255.255.255 S0/0/0
- [ ] ip route 0.0.0.0 255.0.0.0 S0/0/0

<details>
<summary>💡 Show Answer</summary>

**Answer:** ip route 0.0.0.0 0.0.0.0 S0/0/0

**Explanation:**
The static route `ip route 0.0.0.0 0.0.0.0 S0/0/0` is considered a default static route and will match all destination networks.
</details>

---

### Question 5
Refer to the exhibit. Which static route command can be entered on R1 to forward traffic to the LAN connected to R2?

![](image_005.png)

- [ ] ipv6 route 2001:db8:12:10::/64 S0/0/0
- [ ] ipv6 route 2001:db8:12:10::/64 S0/0/1 fe80::2
- [x] ipv6 route 2001:db8:12:10::/64 S0/0/0 fe80::2
- [ ] ipv6 route 2001:db8:12:10::/64 S0/0/1 2001:db8:12:10::1

<details>
<summary>💡 Show Answer</summary>

**Answer:** ipv6 route 2001:db8:12:10::/64 S0/0/0 fe80::2
</details>

---

### Question 6
What is a method to launch a VLAN hopping attack?

- [x] introducing a rogue switch and enabling trunking
- [ ] sending spoofed native VLAN information
- [ ] sending spoofed IP addresses from the attacking host
- [ ] flooding the switch with MAC addresses

<details>
<summary>💡 Show Answer</summary>

**Answer:** introducing a rogue switch and enabling trunking
</details>

---

### Question 7
A cybersecurity analyst is using the macof tool to evaluate configurations of switches deployed in the backbone network of an organization. Which type of LAN attack is the analyst targeting during this evaluation?

- [ ] VLAN hopping
- [ ] DHCP spoofing
- [x] MAC address table overflow
- [ ] VLAN double-tagging

<details>
<summary>💡 Show Answer</summary>

**Answer:** MAC address table overflow

**Explanation:**
Macof is a network attack tool and is mainly used to flood LAN switches with MAC addresses.
</details>

---

### Question 8
Refer to the exhibit. A network administrator is configuring a router as a DHCPv6 server. The administrator issues a `show ipv6 dhcp pool` command to verify the configuration. Which statement explains the reason that the number of active clients is 0?

![](exam_images/image_008.png)

- [ ] The default gateway address is not provided in the pool
- [ ] No clients have communicated with the DHCPv6 server yet
- [ ] The IPv6 DHCP pool configuration has no IPv6 address range specified
- [x] The state is not maintained by the DHCPv6 server under stateless DHCPv6 operation

<details>
<summary>💡 Show Answer</summary>

**Answer:** The state is not maintained by the DHCPv6 server under stateless DHCPv6 operation

**Explanation:**
Under the stateless DHCPv6 configuration, indicated by the command `ipv6 nd other-config-flag`, the DHCPv6 server does not maintain the state information, because client IPv6 addresses are not managed by the DHCP server. Because the clients will configure their IPv6 addresses by combining the prefix/prefix-length and a self-generated interface ID, the ipv6 dhcp pool configuration does not need to specify the valid IPv6 address range.
</details>

---

### Question 9
Refer to the exhibit. A network administrator configured routers R1 and R2 as part of HSRP group 1. After the routers have been reloaded, a user on Host1 complained of lack of connectivity to the Internet. The network administrator issued the `show standby brief` command on both routers to verify the HSRP operations. In addition, the administrator observed the ARP table on Host1. Which entry should be seen in the ARP table on Host1 in order to gain connectivity to the Internet?

![](exam_images/image_009.png)

- [x] the virtual IP address and the virtual MAC address for the HSRP group 1
- [ ] the virtual IP address of the HSRP group 1 and the MAC address of R1
- [ ] the virtual IP address of the HSRP group 1 and the MAC address of R2
- [ ] the IP address and the MAC address of R1

<details>
<summary>💡 Show Answer</summary>

**Answer:** the virtual IP address and the virtual MAC address for the HSRP group 1

**Explanation:**
Hosts will send an ARP request to the default gateway which is the virtual IP address. ARP replies from the HSRP routers contain the virtual MAC address. The host ARP tables will contain a mapping of the virtual IP to the virtual MAC.
</details>

---

### Question 10
Match the forwarding characteristic to its type. (Not all options are used.)

![](exam_images/image_010.png)

<details>
<summary>💡 Show Answer</summary>

**Answer:** Matching exercise - refer to exhibit
</details>

---

### Question 11
Which statement is correct about how a Layer 2 switch determines how to forward frames?

- [x] Frame forwarding decisions are based on MAC address and port mappings in the CAM table
- [ ] Only frames with a broadcast destination address are forwarded out all active switch ports
- [ ] Unicast frames are always forwarded regardless of the destination MAC address
- [ ] Cut-through frame forwarding ensures that invalid frames are always dropped

<details>
<summary>💡 Show Answer</summary>

**Answer:** Frame forwarding decisions are based on MAC address and port mappings in the CAM table

**Explanation:**
Cut-through frame forwarding reads up to only the first 22 bytes of a frame, which excludes the frame check sequence and thus invalid frames may be forwarded. In addition to broadcast frames, frames with a destination MAC address that is not in the CAM are also flooded out all active ports. Unicast frames are not always forwarded.
</details>

---

### Question 12
Which statement describes a result after multiple Cisco LAN switches are interconnected?

- [x] The broadcast domain expands to all switches
- [ ] One collision domain exists per switch
- [ ] There is one broadcast domain and one collision domain per switch
- [ ] Frame collisions increase on the segments connecting the switches
- [ ] Unicast frames are always forwarded regardless of the destination MAC address

<details>
<summary>💡 Show Answer</summary>

**Answer:** The broadcast domain expands to all switches

**Explanation:**
In Cisco LAN switches, the microsegmentation makes it possible for each port to represent a separate segment and thus each switch port represents a separate collision domain. This fact will not change when multiple switches are interconnected. However, LAN switches do not filter broadcast frames. A broadcast frame is flooded to all ports. Interconnected switches form one big broadcast domain.
</details>

---

### Question 13
Match the link state to the interface and protocol status. (Not all options are used.)

![](image_013.png)

<details>
<summary>💡 Show Answer</summary>

**Answer:**
- **Layer 1 problem:** down/down
- **Layer 2 problem:** up/down
- **Disabled:** administratively down
- **Operational:** up/up
</details>

---

### Question 14
Refer to the exhibit. How is a frame sent from PCA forwarded to PCC if the MAC address table on switch SW1 is empty?

![](image_014.png)

- [ ] SW1 forwards the frame directly to SW2. SW2 floods the frame to all ports connected to SW2, excluding the port through which the frame entered the switch
- [ ] SW1 floods the frame on all ports on the switch, excluding the interconnected port to switch SW2 and the port through which the frame entered the switch
- [x] SW1 floods the frame on all ports on SW1, excluding the port through which the frame entered the switch
- [ ] SW1 drops the frame because it does not know the destination MAC address

<details>
<summary>💡 Show Answer</summary>

**Answer:** SW1 floods the frame on all ports on SW1, excluding the port through which the frame entered the switch

**Explanation:**
When a switch powers on, the MAC address table is empty. The switch builds the MAC address table by examining the source MAC address of incoming frames. The switch forwards based on the destination MAC address found in the frame header. If a switch has no entries in the MAC address table or if the destination MAC address is not in the switch table, the switch will forward the frame out all ports except the port that brought the frame into the switch.
</details>

---

### Question 15
An administrator is trying to remove configurations from a switch. After using the command `erase startup-config` and reloading the switch, the administrator finds that VLANs 10 and 100 still exist on the switch. Why were these VLANs not removed?

- [x] Because these VLANs are stored in a file that is called vlan.dat that is located in flash memory, this file must be manually deleted
- [ ] These VLANs cannot be deleted unless the switch is in VTP client mode
- [ ] These VLANs are default VLANs that cannot be removed
- [ ] These VLANs can only be removed from the switch by using the no vlan 10 and no vlan 100 commands

<details>
<summary>💡 Show Answer</summary>

**Answer:** Because these VLANs are stored in a file that is called vlan.dat that is located in flash memory, this file must be manually deleted

**Explanation:**
Standard range VLANs (1-1005) are stored in a file that is called vlan.dat that is located in flash memory. Erasing the startup configuration and reloading a switch does not automatically remove these VLANs. The vlan.dat file must be manually deleted from flash memory and then the switch must be reloaded.
</details>

---

### Question 16
Match the description to the correct VLAN type. (Not all options are used.)

![](image_016.png)

<details>
<summary>💡 Show Answer</summary>

**Answer:**
- **Native VLAN:** Carries untagged traffic
- **Management VLAN:** An IP address and subnet mask are assigned to this VLAN, allowing the switch to be accessed by HTTP, Telnet, SSH, or SNMP
- **Default VLAN:** All switch ports are assigned to this VLAN after initial bootup of the switch
- **Data VLANs:** Configured to carry user generated traffic
</details>

---

### Question 17
Refer to the exhibit. A network administrator has connected two switches together using EtherChannel technology. If STP is running, what will be the end result?

![](image_017.png)

- [x] STP will block one of the redundant links
- [ ] The switches will load balance and utilize both EtherChannels to forward packets
- [ ] The resulting loop will create a broadcast storm
- [ ] Both port channels will shutdown

<details>
<summary>💡 Show Answer</summary>

**Answer:** STP will block one of the redundant links

**Explanation:**
Cisco switches support two protocols for negotiating a channel between two switches: LACP and PAgP. PAgP is Cisco-proprietary. In the topology shown, the switches are connected to each other using redundant links. By default, STP is enabled on switch devices. STP will block redundant links to prevent loops.
</details>

---

### Question 18
What is a secure configuration option for remote access to a network device?

- [ ] Configure an ACL and apply it to the VTY lines
- [ ] Configure 802.1x
- [x] Configure SSH
- [ ] Configure Telnet

<details>
<summary>💡 Show Answer</summary>

**Answer:** Configure SSH
</details>

---

### Question 19
Which wireless encryption method is the most secure?

- [x] WPA2 with AES
- [ ] WPA2 with TKIP
- [ ] WEP
- [ ] WPA

<details>
<summary>💡 Show Answer</summary>

**Answer:** WPA2 with AES
</details>

---

### Question 20
After attaching four PCs to the switch ports, configuring the SSID and setting authentication properties for a small office network, a technician successfully tests the connectivity of all PCs that are connected to the switch and WLAN. A firewall is then configured on the device prior to connecting it to the Internet. What type of network device includes all of the described features?

- [ ] firewall appliance
- [x] wireless router
- [ ] switch
- [ ] standalone wireless access point

<details>
<summary>💡 Show Answer</summary>

**Answer:** wireless router
</details>

---

### Question 21
Refer to the exhibit. Host A has sent a packet to host B. What will be the source MAC and IP addresses on the packet when it arrives at host B?

![](image_021.png)

- [x] Source MAC: 00E0.FE91.7799, Source IP: 10.1.1.10
- [ ] Source MAC: 00E0.FE10.17A3, Source IP: 10.1.1.10
- [ ] Source MAC: 00E0.FE10.17A3, Source IP: 192.168.1.1
- [ ] Source MAC: 00E0.FE91.7799, Source IP: 10.1.1.1
- [ ] Source MAC: 00E0.FE91.7799, Source IP: 192.168.1.1

<details>
<summary>💡 Show Answer</summary>

**Answer:** Source MAC: 00E0.FE91.7799, Source IP: 10.1.1.10

**Explanation:**
As a packet traverses the network, the Layer 2 addresses will change at every hop as the packet is de-encapsulated and re-encapsulated, but the Layer 3 addresses will remain the same.
</details>

---

### Question 23
Refer to the exhibit. In addition to static routes directing traffic to networks 10.10.0.0/16 and 10.20.0.0/16, Router HQ is also configured with the following command: `ip route 0.0.0.0 0.0.0.0 serial 0/1/1`. What is the purpose of this command?

![](exam_images/image_023.png)

- [ ] Packets that are received from the Internet will be forwarded to one of the LANs connected to R1 or R2
- [x] Packets with a destination network that is not 10.10.0.0/16 or is not 10.20.0.0/16 or is not a directly connected network will be forwarded to the Internet
- [ ] Packets from the 10.10.0.0/16 network will be forwarded to network 10.20.0.0/16, and packets from the 10.20.0.0/16 network will be forwarded to network 10.10.0.0/16
- [ ] Packets that are destined for networks that are not in the routing table of HQ will be dropped

<details>
<summary>💡 Show Answer</summary>

**Answer:** Packets with a destination network that is not 10.10.0.0/16 or is not 10.20.0.0/16 or is not a directly connected network will be forwarded to the Internet
</details>

---

### Question 24
What protocol or technology disables redundant paths to eliminate Layer 2 loops?

- [ ] VTP
- [x] STP
- [ ] EtherChannel
- [ ] DTP

<details>
<summary>💡 Show Answer</summary>

**Answer:** STP
</details>

---

### Question 25
Refer to the exhibit. Based on the exhibited configuration and output, why is VLAN 99 missing?

![](image_025.png)

- [ ] because VLAN 99 is not a valid management VLAN
- [ ] because there is a cabling problem on VLAN 99
- [ ] because VLAN 1 is up and there can only be one management VLAN on the switch
- [x] because VLAN 99 has not yet been created

<details>
<summary>💡 Show Answer</summary>

**Answer:** because VLAN 99 has not yet been created
</details>

---

### Question 26
Which two VTP modes allow for the creation, modification, and deletion of VLANs on the local switch? (Choose two.)

- [ ] client
- [ ] master
- [ ] distribution
- [ ] slave
- [x] server
- [x] transparent

<details>
<summary>💡 Show Answer</summary>

**Answer:** server, transparent
</details>

---

### Question 27
Which three steps should be taken before moving a Cisco switch to a new VTP management domain? (Choose three.)

- [x] Configure the switch with the name of the new management domain
- [ ] Reset the VTP counters to allow the switch to synchronize with the other switches in the domain
- [ ] Configure the VTP server in the domain to recognize the BID of the new switch
- [ ] Download the VTP database from the VTP server in the new domain
- [x] Select the correct VTP mode and version
- [x] Reboot the switch

<details>
<summary>💡 Show Answer</summary>

**Answer:** Configure the switch with the name of the new management domain, Select the correct VTP mode and version, Reboot the switch

**Explanation:**
When adding a new switch to a VTP domain, it is critical to configure the switch with a new domain name, the correct VTP mode, VTP version number, and password. A switch with a higher revision number can propagate invalid VLANs and erase valid VLANs thus preventing connectivity for multiple devices on the valid VLANs.
</details>

---

### Question 28
A network administrator is preparing the implementation of Rapid PVST+ on a production network. How are the Rapid PVST+ link types determined on the switch interfaces?

- [ ] Link types can only be configured on access ports configured with a single VLAN
- [ ] Link types can only be determined if PortFast has been configured
- [x] Link types are determined automatically
- [ ] Link types must be configured with specific port configuration commands

<details>
<summary>💡 Show Answer</summary>

**Answer:** Link types are determined automatically

**Explanation:**
When Rapid PVST+ is being implemented, link types are automatically determined but can be specified manually. Link types can be either point-to-point, shared, or edge.
</details>

---

### Question 29
Refer to the exhibit. All the displayed switches are Cisco 2960 switches with the same default priority and operating at the same bandwidth. Which three ports will be STP designated ports? (Choose three.)

![](image_029.png)

- [x] fa0/9
- [ ] fa0/13
- [x] fa0/10
- [ ] fa0/20
- [ ] fa0/21
- [x] fa0/11

<details>
<summary>💡 Show Answer</summary>

**Answer:** fa0/9, fa0/10, fa0/11

**Explanation:**
Given that all the switches have the same default priority and are operating at the same bandwidth, the switch with the lowest MAC address will become the root bridge. This would be SW3 and all its ports would be designated ports. SW1 has a lower MAC address than SW2 has and therefore port fa0/10 will become the designated port on that link.
</details>

---

### Question 30
How will a router handle static routing differently if Cisco Express Forwarding is disabled?

- [ ] It will not perform recursive lookups
- [x] Ethernet multiaccess interfaces will require fully specified static routes to avoid routing inconsistencies
- [ ] Static routes that use an exit interface will be unnecessary
- [ ] Serial point-to-point interfaces will require fully specified static routes to avoid routing inconsistencies

<details>
<summary>💡 Show Answer</summary>

**Answer:** Ethernet multiaccess interfaces will require fully specified static routes to avoid routing inconsistencies

**Explanation:**
In most platforms running IOS 12.0 or later, Cisco Express Forwarding is enabled by default. Cisco Express Forwarding eliminates the need for the recursive lookup. If Cisco Express Forwarding is disabled, multiaccess network interfaces require fully specified static routes in order to avoid inconsistencies in their routing tables.
</details>

---

### Question 31
Compared with dynamic routes, what are two advantages of using static routes on a router? (Choose two.)

- [x] They improve network security
- [ ] They take less time to converge when the network topology changes
- [ ] They improve the efficiency of discovering neighboring networks
- [x] They use fewer router resources

<details>
<summary>💡 Show Answer</summary>

**Answer:** They improve network security, They use fewer router resources

**Explanation:**
Static routes are manually configured on a router. Static routes are not automatically updated and must be manually reconfigured if the network topology changes. Thus static routing improves network security because it does not make route updates among neighboring routers. Static routes also improve resource efficiency by using less bandwidth, and no CPU cycles are used to calculate and communicate routes.
</details>

---

### Question 32
Refer to the exhibit. Which route was configured as a static route to a specific network using the next-hop address?

![](exam_images/image_032.png)

- [x] S 10.17.2.0/24 [1/0] via 10.16.2.2
- [ ] S 0.0.0.0/0 [1/0] via 10.16.2.2
- [ ] S 10.17.2.0/24 is directly connected, Serial 0/0/0
- [ ] C 10.16.2.0/24 is directly connected, Serial0/0/0

<details>
<summary>💡 Show Answer</summary>

**Answer:** S 10.17.2.0/24 [1/0] via 10.16.2.2

**Explanation:**
The C in a routing table indicates an interface that is up and has an IP address assigned. The S in a routing table signifies that a route was installed using the ip route command. The entry that has the S denoting a static route and [1/0] was configured using the next-hop address.
</details>

---

### Question 33
What is the effect of entering the `spanning-tree portfast` configuration command on a switch?

- [ ] It disables an unused port
- [ ] It disables all trunk ports
- [x] It enables portfast on a specific switch interface
- [ ] It checks the source L2 address in the Ethernet header against the sender L2 address in the ARP body

<details>
<summary>💡 Show Answer</summary>

**Answer:** It enables portfast on a specific switch interface
</details>

---

### Question 34
What is the IPv6 prefix that is used for link-local addresses?

- [ ] FF01::/8
- [ ] 2001::/3
- [ ] FC00::/7
- [x] FE80::/10

<details>
<summary>💡 Show Answer</summary>

**Answer:** FE80::/10

**Explanation:**
The IPv6 link-local prefix is FE80::/10 and is used to create a link-local IPv6 address on an interface.
</details>

---

### Question 35
Which two statements are characteristics of routed ports on a multilayer switch? (Choose two.)

- [x] In a switched network, they are mostly configured between switches at the core and distribution layers
- [ ] The interface vlan command has to be entered to create a VLAN on routed ports
- [ ] They support subinterfaces, like interfaces on the Cisco IOS routers
- [ ] They are used for point-to-multipoint links
- [x] They are not associated with a particular VLAN

<details>
<summary>💡 Show Answer</summary>

**Answer:** In a switched network, they are mostly configured between switches at the core and distribution layers, They are not associated with a particular VLAN

**Explanation:**
Routed ports are physical ports that act similarly to a router interface. They are not associated with a particular VLAN, they do not support subinterfaces, and they are used for point-to-point links. In a switched network, they are mostly configured between switches at the core and distribution layers.
</details>

---

### Question 36
Successful inter-VLAN routing has been operating on a network with multiple VLANs across multiple switches for some time. When an inter-switch trunk link fails and Spanning Tree Protocol brings up a backup trunk link, it is reported that hosts on two VLANs can access some, but not all the network resources that could be accessed previously. Hosts on all other VLANs do not have this problem. What is the most likely cause of this problem?

- [ ] The protected edge port function on the backup trunk interfaces has been disabled
- [x] The allowed VLANs on the backup link were not configured correctly
- [ ] Dynamic Trunking Protocol on the link has failed
- [ ] Inter-VLAN routing also failed when the trunk link failed

<details>
<summary>💡 Show Answer</summary>

**Answer:** The allowed VLANs on the backup link were not configured correctly
</details>

---

### Question 37
Which command will start the process to bundle two physical interfaces to create an EtherChannel group via LACP?

- [ ] interface port-channel 2
- [ ] channel-group 1 mode desirable
- [x] interface range GigabitEthernet 0/4 – 5
- [ ] channel-group 2 mode auto

<details>
<summary>💡 Show Answer</summary>

**Answer:** interface range GigabitEthernet 0/4 – 5
</details>

---

### Question 38
What action takes place when a frame entering a switch has a multicast destination MAC address?

- [x] The switch will forward the frame out all ports except the incoming port
- [ ] The switch forwards the frame out of the specified port
- [ ] The switch adds a MAC address table entry mapping for the destination MAC address and the ingress port
- [ ] The switch replaces the old entry and uses the more current port

<details>
<summary>💡 Show Answer</summary>

**Answer:** The switch will forward the frame out all ports except the incoming port

**Explanation:**
If the destination MAC address is a broadcast or a multicast, the frame is also flooded out all ports except the incoming port.
</details>

---

### Question 39
A junior technician was adding a route to a LAN router. A traceroute to a device on the new network revealed a wrong path and unreachable status. What should be done or checked?

- [ ] Verify that there is not a default route in any of the edge router routing tables
- [ ] Check the configuration on the floating static route and adjust the AD
- [ ] Create a floating static route to that network
- [x] Check the configuration of the exit interface on the new static route

<details>
<summary>💡 Show Answer</summary>

**Answer:** Check the configuration of the exit interface on the new static route
</details>

---

### Question 40
Select the three PAgP channel establishment modes. (Choose three.)

- [x] auto
- [ ] default
- [ ] passive
- [x] desirable
- [ ] extended
- [x] on

<details>
<summary>💡 Show Answer</summary>

**Answer:** auto, desirable, on
</details>

---

### Question 41
A static route has been configured on a router. However, the destination network no longer exists. What should an administrator do to remove the static route from the routing table?

- [x] Remove the route using the no ip route command
- [ ] Change the administrative distance for that route
- [ ] Change the routing metric for that route
- [ ] Nothing. The static route will go away on its own

<details>
<summary>💡 Show Answer</summary>

**Answer:** Remove the route using the no ip route command

**Explanation:**
When the destination network specified in a static route does not exist anymore, the static route stays in the routing table until it is manually removed by using the no ip route command.
</details>

---

### Question 42
Refer to the exhibit. What can be concluded about the configuration shown on R1?

![](image_042.png)

- [ ] R1 is configured as a DHCPv4 relay agent
- [x] R1 is operating as a DHCPv4 server
- [ ] R1 will broadcast DHCPv4 requests on behalf of local DHCPv4 clients
- [ ] R1 will send a message to a local DHCPv4 client to contact a DHCPv4 server at 10.10.10.8

<details>
<summary>💡 Show Answer</summary>

**Answer:** R1 is operating as a DHCPv4 server
</details>

---

### Question 43
Match the step to each switch boot sequence description. (Not all options are used.)

![](exam_images/image_043.png)

<details>
<summary>💡 Show Answer</summary>

**Answer:**
1. execute POST
2. load the boot loader from ROM
3. CPU register initializations
4. flash file system initialization
5. load the IOS
6. transfer switch control to the IOS
</details>

---

### Question 44
Refer to the exhibit. R1 has been configured as shown. However, PC1 is not able to receive an IPv4 address. What is the problem?

![](exam_images/image_044.png)

- [x] The ip helper-address command was applied on the wrong interface
- [ ] R1 is not configured as a DHCPv4 server
- [ ] A DHCP server must be installed on the same LAN as the host that is receiving the IP address
- [ ] The ip address dhcp command was not issued on the interface Gi0/1

<details>
<summary>💡 Show Answer</summary>

**Answer:** The ip helper-address command was applied on the wrong interface

**Explanation:**
The ip helper-address command has to be applied on interface Gi0/0. This command must be present on the interface of the LAN that contains the DHCPv4 client PC1 and must be directed to the correct DHCPv4 server.
</details>

---

### Question 45
What two default wireless router settings can affect network security? (Choose two.)

- [x] The SSID is broadcast
- [ ] MAC address filtering is enabled
- [ ] WEP encryption is enabled
- [ ] The wireless channel is automatically selected
- [x] A well-known administrator password is set

<details>
<summary>💡 Show Answer</summary>

**Answer:** The SSID is broadcast, A well-known administrator password is set

**Explanation:**
Default settings on wireless routers often include broadcasting the SSID and using a well-known administrative password. Both of these pose a security risk to wireless networks. WEP encryption and MAC address filtering are not set by default.
</details>

---

### Question 46
What is the common term given to SNMP log messages that are generated by network devices and sent to the SNMP server?

- [x] traps
- [ ] acknowledgments
- [ ] auditing
- [ ] warnings

<details>
<summary>💡 Show Answer</summary>

**Answer:** traps
</details>

---

### Question 47
A network administrator is adding a new WLAN on a Cisco 3500 series WLC. Which tab should the administrator use to create a new VLAN interface to be used for the new WLAN?

- [ ] WIRELESS
- [ ] MANAGEMENT
- [x] CONTROLLER
- [ ] WLANs

<details>
<summary>💡 Show Answer</summary>

**Answer:** CONTROLLER
</details>

---

### Question 48
A network administrator is configuring a WLAN. Why would the administrator change the default DHCP IPv4 addresses on an AP?

- [ ] to restrict access to the WLAN by authorized, authenticated users only
- [ ] to monitor the operation of the wireless network
- [x] to reduce outsiders intercepting data or accessing the wireless network by using a well-known address range
- [ ] to reduce the risk of interference by external devices such as microwave ovens

<details>
<summary>💡 Show Answer</summary>

**Answer:** to reduce outsiders intercepting data or accessing the wireless network by using a well-known address range
</details>

---

### Question 49
Which two functions are performed by a WLC when using split media access control (MAC)? (Choose two.)

- [ ] packet acknowledgments and retransmissions
- [x] frame queuing and packet prioritization
- [ ] beacons and probe responses
- [ ] frame translation to other protocols
- [x] association and re-association of roaming clients

<details>
<summary>💡 Show Answer</summary>

**Answer:** frame queuing and packet prioritization, association and re-association of roaming clients
</details>

---

### Question 50
On what switch ports should BPDU guard be enabled to enhance STP stability?

- [x] all PortFast-enabled ports
- [ ] only ports that are elected as designated ports
- [ ] only ports that attach to a neighboring switch
- [ ] all trunk ports that are not root ports

<details>
<summary>💡 Show Answer</summary>

**Answer:** all PortFast-enabled ports
</details>

---

_Continue for remaining 124 questions..._

---

## 📚 Study Resources

### Topics Covered
- IPv4 and IPv6 Routing
- Static and Dynamic Routing
- VLAN Configuration
- Inter-VLAN Routing
- Spanning Tree Protocol
- EtherChannel
- DHCP Configuration
- Wireless Networking
- Network Security
- Switch Operations

### Study Tips
1. Practice with Packet Tracer
2. Understand the "why" behind each answer
3. Focus on command syntax
4. Review network diagrams carefully
5. Take timed practice tests

---

**Good luck with your CCNA exam! 🎓**
