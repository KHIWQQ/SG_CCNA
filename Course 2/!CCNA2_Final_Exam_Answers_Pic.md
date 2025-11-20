# CCNA 2: Switching, Routing, and Wireless Essentials

## Version 7.00 - SRWE Course Final Exam - Complete with Answers and Images

---

### Question 1

**Refer to the exhibit. What will router R1 do with a packet that has a destination IPv6 address of 2001:db8:cafe:5::1?**

![Exhibit](images/image_001.png)

**Options:**

- [ ] forward the packet out GigabitEthernet0/0
- [ ] drop the packet
- [ ] forward the packet out GigabitEthernet0/1
- [x] forward the packet out Serial0/0/0

**Answer:** forward the packet out Serial0/0/0

**Explanation:** The route ::/0 is the compressed form of the 0000:0000:0000:0000:0000:0000:0000:0000/0 default route. The default route is used if a more specific route is not found in the routing table.

---

### Question 2

**Refer to the exhibit. Currently router R1 uses an EIGRP route learned from Branch2 to reach the 10.10.0.0/16 network. Which floating static route would create a backup route to the 10.10.0.0/16 network in the event that the link between R1 and Branch2 goes down?**

![Exhibit](images/image_002.png)

**Options:**

- [ ] ip route 10.10.0.0 255.255.0.0 Serial 0/0/0 100
- [x] ip route 10.10.0.0 255.255.0.0 209.165.200.226 100
- [ ] ip route 10.10.0.0 255.255.0.0 209.165.200.225 100
- [ ] ip route 10.10.0.0 255.255.0.0 209.165.200.225 50

**Answer:** ip route 10.10.0.0 255.255.0.0 209.165.200.226 100

**Explanation:** A floating static route needs to have an administrative distance that is greater than the administrative distance of the active route in the routing table. Router R1 is using an EIGRP route which has an administrative distance of 90 to reach the 10.10.0.0/16 network. To be a backup route the floating static route must have an administrative distance greater than 90 and have a next hop address corresponding to the serial interface IP address of Branch1.

---

### Question 3

**Refer to the exhibit. R1 was configured with the static route command ip route 209.165.200.224 255.255.255.224 S0/0/0 and consequently users on network 172.16.0.0/16 are unable to reach resources on the Internet. How should this static route be changed to allow user traffic from the LAN to reach the Internet?**

![Exhibit](images/image_003.png)

**Options:**

- [ ] Add an administrative distance of 254.
- [x] Change the destination network and mask to 0.0.0.0 0.0.0.0
- [ ] Change the exit interface to S0/0/1.
- [ ] Add the next-hop neighbor address of 209.165.200.226.

**Answer:** Change the destination network and mask to 0.0.0.0 0.0.0.0

**Explanation:** The static route on R1 has been incorrectly configured with the wrong destination network and mask. The correct destination network and mask is 0.0.0.0 0.0.0.0.

---

### Question 4

**Which option shows a correctly configured IPv4 default static route?**

**Options:**

- [ ] ip route 0.0.0.0 255.255.255.0 S0/0/0
- [x] ip route 0.0.0.0 0.0.0.0 S0/0/0
- [ ] ip route 0.0.0.0 255.255.255.255 S0/0/0
- [ ] ip route 0.0.0.0 255.0.0.0 S0/0/0

**Answer:** ip route 0.0.0.0 0.0.0.0 S0/0/0

**Explanation:** The static route ip route 0.0.0.0 0.0.0.0 S0/0/0 is considered a default static route and will match all destination networks.

---

### Question 5

**Refer to the exhibit. Which static route command can be entered on R1 to forward traffic to the LAN connected to R2?**

![Exhibit](images/image_004.png)

**Options:**

- [ ] ipv6 route 2001:db8:12:10::/64 S0/0/0
- [ ] ipv6 route 2001:db8:12:10::/64 S0/0/1 fe80::2
- [x] ipv6 route 2001:db8:12:10::/64 S0/0/0 fe80::2
- [ ] ipv6 route 2001:db8:12:10::/64 S0/0/1 2001:db8:12:10::1

**Answer:** ipv6 route 2001:db8:12:10::/64 S0/0/0 fe80::2

---

### Question 6

**What is a method to launch a VLAN hopping attack?**

**Options:**

- [x] introducing a rogue switch and enabling trunking
- [ ] sending spoofed native VLAN information
- [ ] sending spoofed IP addresses from the attacking host
- [ ] flooding the switch with MAC addresses

**Answer:** introducing a rogue switch and enabling trunking

---

### Question 7

**A cybersecurity analyst is using the macof tool to evaluate configurations of switches deployed in the backbone network of an organization. Which type of LAN attack is the analyst targeting during this evaluation?**

**Options:**

- [ ] VLAN hopping
- [ ] DHCP spoofing
- [x] MAC address table overflow
- [ ] VLAN double-tagging

**Answer:** MAC address table overflow

**Explanation:** Macof is a network attack tool and is mainly used to flood LAN switches with MAC addresses.

---

### Question 8

**Refer to the exhibit. A network administrator is configuring a router as a DHCPv6 server. The administrator issues a show ipv6 dhcp pool command to verify the configuration. Which statement explains the reason that the number of active clients is 0?**

![Exhibit](images/image_005.png)

**Options:**

- [ ] The default gateway address is not provided in the pool.
- [ ] No clients have communicated with the DHCPv6 server yet.
- [ ] The IPv6 DHCP pool configuration has no IPv6 address range specified.
- [x] The state is not maintained by the DHCPv6 server under stateless DHCPv6 operation.

**Answer:** The state is not maintained by the DHCPv6 server under stateless DHCPv6 operation.

**Explanation:** Under the stateless DHCPv6 configuration, indicated by the command ipv6 nd other-config-flag, the DHCPv6 server does not maintain the state information, because client IPv6 addresses are not managed by the DHCP server.

---

### Question 9

**Refer to the exhibit. A network administrator configured routers R1 and R2 as part of HSRP group 1. After the routers have been reloaded, a user on Host1 complained of lack of connectivity to the Internet. Which entry should be seen in the ARP table on Host1 in order to gain connectivity to the Internet?**

![Exhibit](images/image_006.jpeg)

**Options:**

- [x] the virtual IP address and the virtual MAC address for the HSRP group 1
- [ ] the virtual IP address of the HSRP group 1 and the MAC address of R1
- [ ] the virtual IP address of the HSRP group 1 and the MAC address of R2
- [ ] the IP address and the MAC address of R1

**Answer:** the virtual IP address and the virtual MAC address for the HSRP group 1

**Explanation:** Hosts will send an ARP request to the default gateway which is the virtual IP address. ARP replies from the HSRP routers contain the virtual MAC address. The host ARP tables will contain a mapping of the virtual IP to the virtual MAC.

---

### Question 10

**Match the forwarding characteristic to its type. (Not all options are used.)**

![Exhibit](images/image_007.png)

---

### Question 11

**Which statement is correct about how a Layer 2 switch determines how to forward frames?**

**Options:**

- [x] Frame forwarding decisions are based on MAC address and port mappings in the CAM table.
- [ ] Only frames with a broadcast destination address are forwarded out all active switch ports.
- [ ] Unicast frames are always forwarded regardless of the destination MAC address.
- [ ] Cut-through frame forwarding ensures that invalid frames are always dropped.

**Answer:** Frame forwarding decisions are based on MAC address and port mappings in the CAM table.

**Explanation:** Cut-through frame forwarding reads up to only the first 22 bytes of a frame, which excludes the frame check sequence and thus invalid frames may be forwarded.

---

### Question 12

**Which statement describes a result after multiple Cisco LAN switches are interconnected?**

**Options:**

- [x] The broadcast domain expands to all switches.
- [ ] One collision domain exists per switch.
- [ ] There is one broadcast domain and one collision domain per switch.
- [ ] Frame collisions increase on the segments connecting the switches.

**Answer:** The broadcast domain expands to all switches.

**Explanation:** In Cisco LAN switches, the microsegmentation makes it possible for each port to represent a separate segment and thus each switch port represents a separate collision domain. However, LAN switches do not filter broadcast frames. Interconnected switches form one big broadcast domain.

---

### Question 13

**Match the link state to the interface and protocol status. (Not all options are used.)**

---

### Question 14

**Refer to the exhibit. How is a frame sent from PCA forwarded to PCC if the MAC address table on switch SW1 is empty?**

![Exhibit](images/image_008.jpeg)

**Options:**

- [ ] SW1 forwards the frame directly to SW2.
- [ ] SW2 floods the frame to all ports connected to SW2, excluding the port through which the frame entered the switch.
- [ ] SW1 floods the frame on all ports on the switch, excluding the interconnected port to switch SW2 and the port through which the frame entered the switch.
- [x] SW1 floods the frame on all ports on SW1, excluding the port through which the frame entered the switch.
- [ ] SW1 drops the frame because it does not know the destination MAC address.

**Answer:** SW1 floods the frame on all ports on SW1, excluding the port through which the frame entered the switch.

**Explanation:** When a switch powers on, the MAC address table is empty. If a switch has no entries in the MAC address table or if the destination MAC address is not in the switch table, the switch will forward the frame out all ports except the port that brought the frame into the switch.

---

### Question 15

**An administrator is trying to remove configurations from a switch. After using the command erase startup-config and reloading the switch, the administrator finds that VLANs 10 and 100 still exist on the switch. Why were these VLANs not removed?**

**Options:**

- [x] Because these VLANs are stored in a file that is called vlan.dat that is located in flash memory, this file must be manually deleted.
- [ ] These VLANs cannot be deleted unless the switch is in VTP client mode.
- [ ] These VLANs are default VLANs that cannot be removed.
- [ ] These VLANs can only be removed from the switch by using the no vlan 10 and no vlan 100 commands.

**Answer:** Because these VLANs are stored in a file that is called vlan.dat that is located in flash memory, this file must be manually deleted.

**Explanation:** Standard range VLANs (1-1005) are stored in a file that is called vlan.dat that is located in flash memory. The vlan.dat file must be manually deleted from flash memory and then the switch must be reloaded.

---

### Question 16

**Match the description to the correct VLAN type. (Not all options are used.)**

---

### Question 17

**Refer to the exhibit. A network administrator has connected two switches together using EtherChannel technology. If STP is running, what will be the end result?**

![Exhibit](images/image_009.jpeg)

**Options:**

- [x] STP will block one of the redundant links.
- [ ] The switches will load balance and utilize both EtherChannels to forward packets.
- [ ] The resulting loop will create a broadcast storm.
- [ ] Both port channels will shutdown.

**Answer:** STP will block one of the redundant links.

**Explanation:** By default, STP is enabled on switch devices. STP will block redundant links to prevent loops.

---

### Question 18

**What is a secure configuration option for remote access to a network device?**

**Options:**

- [ ] Configure an ACL and apply it to the VTY lines.
- [ ] Configure 802.1x.
- [x] Configure SSH.
- [ ] Configure Telnet.

**Answer:** Configure SSH

---

### Question 19

**Which wireless encryption method is the most secure?**

**Options:**

- [x] WPA2 with AES
- [ ] WPA2 with TKIP
- [ ] WEP
- [ ] WPA

**Answer:** WPA2 with AES

---

### Question 20

**After attaching four PCs to the switch ports, configuring the SSID and setting authentication properties for a small office network, a technician successfully tests the connectivity of all PCs that are connected to the switch and WLAN. A firewall is then configured on the device prior to connecting it to the Internet. What type of network device includes all of the described features?**

**Options:**

- [ ] firewall appliance
- [x] wireless router
- [ ] switch
- [ ] standalone wireless access point

**Answer:** wireless router

---

### Question 21

**Refer to the exhibit. Host A has sent a packet to host B. What will be the source MAC and IP addresses on the packet when it arrives at host B?**

![Exhibit](images/image_010.jpeg)

**Options:**

- [x] Source MAC: 00E0.FE91.7799 Source IP: 10.1.1.10
- [ ] Source MAC: 00E0.FE10.17A3 Source IP: 10.1.1.10
- [ ] Source MAC: 00E0.FE10.17A3 Source IP: 192.168.1.1
- [ ] Source MAC: 00E0.FE91.7799 Source IP: 10.1.1.1
- [ ] Source MAC: 00E0.FE91.7799 Source IP: 192.168.1.1

**Answer:** Source MAC: 00E0.FE91.7799 Source IP: 10.1.1.10

**Explanation:** As a packet traverses the network, the Layer 2 addresses will change at every hop as the packet is de-encapsulated and re-encapsulated, but the Layer 3 addresses will remain the same.

---

### Question 22

**Match the purpose with its DHCP message type. (Not all options are used.)**

---

### Question 23

**Refer to the exhibit. In addition to static routes directing traffic to networks 10.10.0.0/16 and 10.20.0.0/16, Router HQ is also configured with the command: ip route 0.0.0.0 0.0.0.0 serial 0/1/1. What is the purpose of this command?**

**Options:**

- [ ] Packets that are received from the Internet will be forwarded to one of the LANs connected to R1 or R2.
- [x] Packets with a destination network that is not 10.10.0.0/16 or is not 10.20.0.0/16 or is not a directly connected network will be forwarded to the Internet.
- [ ] Packets from the 10.10.0.0/16 network will be forwarded to network 10.20.0.0/16, and packets from the 10.20.0.0/16 network will be forwarded to network 10.10.0.0/16.
- [ ] Packets that are destined for networks that are not in the routing table of HQ will be dropped.

**Answer:** Packets with a destination network that is not 10.10.0.0/16 or is not 10.20.0.0/16 or is not a directly connected network will be forwarded to the Internet.

---

### Question 24

**What protocol or technology disables redundant paths to eliminate Layer 2 loops?**

**Options:**

- [ ] VTP
- [x] STP
- [ ] EtherChannel
- [ ] DTP

**Answer:** STP

---

### Question 25

**Refer to the exhibit. Based on the exhibited configuration and output, why is VLAN 99 missing?**

![Exhibit](images/image_011.jpeg)

**Options:**

- [ ] because VLAN 99 is not a valid management VLAN
- [ ] because there is a cabling problem on VLAN 99
- [ ] because VLAN 1 is up and there can only be one management VLAN on the switch
- [x] because VLAN 99 has not yet been created

**Answer:** because VLAN 99 has not yet been created

---

### Question 26

**Which two VTP modes allow for the creation, modification, and deletion of VLANs on the local switch? (Choose two.)**

**Options:**

- [ ] client
- [ ] master
- [ ] distribution
- [ ] slave
- [x] server
- [x] transparent

**Answer:** server, transparent

---

### Question 27

**Which three steps should be taken before moving a Cisco switch to a new VTP management domain? (Choose three.)**

![Exhibit](images/image_012.png)

**Options:**

- [x] Configure the switch with the name of the new management domain.
- [x] Reset the VTP counters to allow the switch to synchronize with the other switches in the domain.
- [ ] Configure the VTP server in the domain to recognize the BID of the new switch.
- [ ] Download the VTP database from the VTP server in the new domain.
- [x] Select the correct VTP mode and version.
- [ ] Reboot the switch.

**Answer:** Configure the switch with the name of the new management domain, Reset the VTP counters to allow the switch to synchronize with the other switches in the domain, Select the correct VTP mode and version.

**Explanation:** When adding a new switch to a VTP domain, it is critical to configure the switch with a new domain name, the correct VTP mode, VTP version number, and password.

---

### Question 28

**A network administrator is preparing the implementation of Rapid PVST+ on a production network. How are the Rapid PVST+ link types determined on the switch interfaces?**

**Options:**

- [ ] Link types can only be configured on access ports configured with a single VLAN.
- [ ] Link types can only be determined if PortFast has been configured.
- [x] Link types are determined automatically.
- [ ] Link types must be configured with specific port configuration commands.

**Answer:** Link types are determined automatically.

**Explanation:** When Rapid PVST+ is being implemented, link types are automatically determined but can be specified manually.

---

### Question 29

**Refer to the exhibit. All the displayed switches are Cisco 2960 switches with the same default priority and operating at the same bandwidth. Which three ports will be STP designated ports? (Choose three.)**

**Options:**

- [x] fa0/9
- [x] fa0/13
- [x] fa0/10
- [ ] fa0/20
- [ ] fa0/21
- [ ] fa0/11

**Answer:** fa0/9, fa0/13, fa0/10

**Explanation:** Given that all the switches have the same default priority and are operating at the same bandwidth, the switch with the lowest MAC address will become the root bridge. This would be SW3 and all its ports would be designated ports. SW1 has a lower MAC address than SW2 has and therefore port fa0/10 will become the designated port on that link.

---

### Question 30

**How will a router handle static routing differently if Cisco Express Forwarding is disabled?**

**Options:**

- [ ] It will not perform recursive lookups.
- [x] Ethernet multiaccess interfaces will require fully specified static routes to avoid routing inconsistencies.
- [ ] Static routes that use an exit interface will be unnecessary.
- [ ] Serial point-to-point interfaces will require fully specified static routes to avoid routing inconsistencies.

**Answer:** Ethernet multiaccess interfaces will require fully specified static routes to avoid routing inconsistencies.

**Explanation:** If Cisco Express Forwarding is disabled, multiaccess network interfaces require fully specified static routes in order to avoid inconsistencies in their routing tables.

---

### Question 31

**Compared with dynamic routes, what are two advantages of using static routes on a router? (Choose two.)**

![Exhibit](images/image_013.png)

**Options:**

- [x] They improve network security.
- [ ] They take less time to converge when the network topology changes.
- [ ] They improve the efficiency of discovering neighboring networks.
- [x] They use fewer router resources.

**Answer:** They improve network security, They use fewer router resources.

**Explanation:** Static routes improve network security because it does not make route updates among neighboring routers. Static routes also improve resource efficiency by using less bandwidth, and no CPU cycles are used to calculate and communicate routes.

---

### Question 32

**Refer to the exhibit. Which route was configured as a static route to a specific network using the next-hop address?**

**Options:**

- [x] S 10.17.2.0/24 [1/0] via 10.16.2.2
- [ ] S 0.0.0.0/0 [1/0] via 10.16.2.2
- [ ] S 10.17.2.0/24 is directly connected, Serial 0/0/0
- [ ] C 10.16.2.0/24 is directly connected, Serial0/0/0

**Answer:** S 10.17.2.0/24 [1/0] via 10.16.2.2

**Explanation:** The entry that has the S denoting a static route and [1/0] was configured using the next-hop address.

---

### Question 33

**What is the effect of entering the spanning-tree portfast configuration command on a switch?**

**Options:**

- [ ] It disables an unused port.
- [ ] It disables all trunk ports.
- [x] It enables portfast on a specific switch interface.
- [ ] It checks the source L2 address in the Ethernet header against the sender L2 address in the ARP body.

**Answer:** It enables portfast on a specific switch interface.

---

### Question 34

**What is the IPv6 prefix that is used for link-local addresses?**

![Exhibit](images/image_014.png)

**Options:**

- [ ] FF01::/8
- [ ] 2001::/3
- [ ] FC00::/7
- [x] FE80::/10

**Answer:** FE80::/10

**Explanation:** The IPv6 link-local prefix is FE80::/10 and is used to create a link-local IPv6 address on an interface.

---

### Question 35

**Which two statements are characteristics of routed ports on a multilayer switch? (Choose two.)**

**Options:**

- [x] In a switched network, they are mostly configured between switches at the core and distribution layers.
- [ ] The interface vlan command has to be entered to create a VLAN on routed ports.
- [ ] They support subinterfaces, like interfaces on the Cisco IOS routers.
- [ ] They are used for point-to-multipoint links.
- [x] They are not associated with a particular VLAN.

**Answer:** In a switched network, they are mostly configured between switches at the core and distribution layers, They are not associated with a particular VLAN.

**Explanation:** Routed ports are physical ports that act similarly to a router interface. They are not associated with a particular VLAN, they do not support subinterfaces, and they are used for point-to-point links.

---

### Question 36

**Successful inter-VLAN routing has been operating on a network with multiple VLANs across multiple switches for some time. When an inter-switch trunk link fails and Spanning Tree Protocol brings up a backup trunk link, it is reported that hosts on two VLANs can access some, but not all the network resources that could be accessed previously. What is the most likely cause of this problem?**

**Options:**

- [ ] The protected edge port function on the backup trunk interfaces has been disabled.
- [x] The allowed VLANs on the backup link were not configured correctly.
- [ ] Dynamic Trunking Protocol on the link has failed.
- [ ] Inter-VLAN routing also failed when the trunk link failed.

**Answer:** The allowed VLANs on the backup link were not configured correctly.

---

### Question 37

**Which command will start the process to bundle two physical interfaces to create an EtherChannel group via LACP?**

**Options:**

- [ ] interface port-channel 2
- [ ] channel-group 1 mode desirable
- [x] interface range GigabitEthernet 0/4 – 5
- [ ] channel-group 2 mode auto

**Answer:** interface range GigabitEthernet 0/4 – 5

---

### Question 38

**What action takes place when a frame entering a switch has a multicast destination MAC address?**

![Exhibit](images/image_015.png)

**Options:**

- [x] The switch will forward the frame out all ports except the incoming port.
- [ ] The switch forwards the frame out of the specified port.
- [ ] The switch adds a MAC address table entry mapping for the destination MAC address and the ingress port.
- [ ] The switch replaces the old entry and uses the more current port.

**Answer:** The switch will forward the frame out all ports except the incoming port.

**Explanation:** If the destination MAC address is a broadcast or a multicast, the frame is also flooded out all ports except the incoming port.

---

### Question 39

**A junior technician was adding a route to a LAN router. A traceroute to a device on the new network revealed a wrong path and unreachable status. What should be done or checked?**

**Options:**

- [ ] Verify that there is not a default route in any of the edge router routing tables.
- [ ] Check the configuration on the floating static route and adjust the AD.
- [ ] Create a floating static route to that network.
- [x] Check the configuration of the exit interface on the new static route.

**Answer:** Check the configuration of the exit interface on the new static route.

---

### Question 40

**Select the three PAgP channel establishment modes. (Choose three.)**

**Options:**

- [x] auto
- [ ] default
- [ ] passive
- [x] desirable
- [ ] extended
- [x] on

**Answer:** auto, desirable, on

---

### Question 41

**A static route has been configured on a router. However, the destination network no longer exists. What should an administrator do to remove the static route from the routing table?**

**Options:**

- [x] Remove the route using the no ip route command.
- [ ] Change the administrative distance for that route.
- [ ] Change the routing metric for that route.
- [ ] Nothing. The static route will go away on its own.

**Answer:** Remove the route using the no ip route command.

**Explanation:** When the destination network specified in a static route does not exist anymore, the static route stays in the routing table until it is manually removed by using the no ip route command.

---

### Question 42

**Refer to the exhibit. What can be concluded about the configuration shown on R1?**

**Options:**

- [x] R1 is configured as a DHCPv4 relay agent.
- [ ] R1 is operating as a DHCPv4 server.
- [ ] R1 will broadcast DHCPv4 requests on behalf of local DHCPv4 clients.
- [ ] R1 will send a message to a local DHCPv4 client to contact a DHCPv4 server at 10.10.10.8.

**Answer:** R1 is configured as a DHCPv4 relay agent.

---

### Question 43

**Match the step to each switch boot sequence description. (Not all options are used.)**

---

### Question 44

**Refer to the exhibit. R1 has been configured as shown. However, PC1 is not able to receive an IPv4 address. What is the problem?**

![Exhibit](images/image_016.png)

**Options:**

- [x] The ip helper-address command was applied on the wrong interface.
- [ ] R1 is not configured as a DHCPv4 server.
- [ ] A DHCP server must be installed on the same LAN as the host that is receiving the IP address.
- [ ] The ip address dhcp command was not issued on the interface Gi0/1.

**Answer:** The ip helper-address command was applied on the wrong interface.

**Explanation:** The ip helper-address command has to be applied on interface Gi0/0. This command must be present on the interface of the LAN that contains the DHCPv4 client PC1.

---

### Question 45

**What two default wireless router settings can affect network security? (Choose two.)**

![Exhibit](images/image_017.png)

**Options:**

- [x] The SSID is broadcast.
- [ ] MAC address filtering is enabled.
- [ ] WEP encryption is enabled.
- [ ] The wireless channel is automatically selected.
- [x] A well-known administrator password is set.

**Answer:** The SSID is broadcast, A well-known administrator password is set.

**Explanation:** Default settings on wireless routers often include broadcasting the SSID and using a well-known administrative password. Both of these pose a security risk to wireless networks.

---

### Question 46

**What is the common term given to SNMP log messages that are generated by network devices and sent to the SNMP server?**

![Exhibit](images/image_018.jpeg)

**Options:**

- [x] traps
- [ ] acknowledgments
- [ ] auditing
- [ ] warnings

**Answer:** traps

---

### Question 47

**A network administrator is adding a new WLAN on a Cisco 3500 series WLC. Which tab should the administrator use to create a new VLAN interface to be used for the new WLAN?**

**Options:**

- [ ] WIRELESS
- [ ] MANAGEMENT
- [x] CONTROLLER
- [ ] WLANs

**Answer:** CONTROLLER

---

### Question 48

**A network administrator is configuring a WLAN. Why would the administrator change the default DHCP IPv4 addresses on an AP?**

**Options:**

- [ ] to restrict access to the WLAN by authorized, authenticated users only
- [ ] to monitor the operation of the wireless network
- [x] to reduce outsiders intercepting data or accessing the wireless network by using a well-known address range
- [ ] to reduce the risk of interference by external devices such as microwave ovens

**Answer:** to reduce outsiders intercepting data or accessing the wireless network by using a well-known address range

---

### Question 49

**Which two functions are performed by a WLC when using split media access control (MAC)? (Choose two.)**

**Options:**

- [ ] packet acknowledgments and retransmissions
- [x] frame queuing and packet prioritization
- [ ] beacons and probe responses
- [x] frame translation to other protocols
- [ ] association and re-association of roaming clients

**Answer:** frame queuing and packet prioritization, frame translation to other protocols

---

### Question 50

**On what switch ports should BPDU guard be enabled to enhance STP stability?**

**Options:**

- [x] all PortFast-enabled ports
- [ ] only ports that are elected as designated ports
- [ ] only ports that attach to a neighboring switch
- [ ] all trunk ports that are not root ports

**Answer:** all PortFast-enabled ports

---

### Question 51

**Which network attack is mitigated by enabling BPDU guard?**

**Options:**

- [x] rogue switches on a network
- [ ] CAM table overflow attacks
- [ ] MAC address spoofing
- [ ] rogue DHCP servers on a network

**Answer:** rogue switches on a network

**Explanation:** BPDU guard immediately error-disables a port that receives a BPDU. Applied to all end-user ports. The receipt of BPDUs may be part of an unauthorized attempt to add a switch to the network.

---

### Question 52

**Why is DHCP snooping required when using the Dynamic ARP Inspection feature?**

**Options:**

- [ ] It relies on the settings of trusted and untrusted ports set by DHCP snooping.
- [ ] It uses the MAC address table to verify the default gateway IP address.
- [ ] It redirects ARP requests to the DHCP server for verification.
- [x] It uses the MAC-address-to-IP-address binding database to validate an ARP packet.

**Answer:** It uses the MAC-address-to-IP-address binding database to validate an ARP packet.

**Explanation:** DAI relies on DHCP snooping. DHCP snooping listens to DHCP message exchanges and builds a bindings database of valid tuples (MAC address, IP address, VLAN interface).

---

### Question 53

**Refer to the exhibit. Router R1 has an OSPF neighbor relationship with the ISP router over the 192.168.0.32 network. The 192.168.0.36 network link should serve as a backup when the OSPF link goes down. Which change should be made to the static route command so that traffic will only use the OSPF link when it is up?**

**Options:**

- [x] Change the administrative distance to 120.
- [ ] Add the next hop neighbor address of 192.168.0.36.
- [ ] Change the destination network to 192.168.0.34.
- [ ] Change the administrative distance to 1.

**Answer:** Change the administrative distance to 120.

**Explanation:** The administrative distance will need to be higher than that of OSPF, which is 110, so that the router will only use the OSPF link when it is up.

---

### Question 54

**Refer to the exhibit. What is the metric to forward a data packet with the IPv6 destination address 2001:DB8:ACAD:E:240:BFF:FED4:9DD2?**

![Exhibit](images/image_019.png)

**Options:**

- [ ] 90
- [ ] 128
- [ ] 2170112
- [ ] 2681856
- [x] 2682112
- [ ] 3193856

**Answer:** 2682112

**Explanation:** The IPv6 destination address 2001:DB8:ACAD:E:240:BFF:FED4:9DD2 belongs to the network of 2001:DB8:ACAD:E::/64. In the routing table, the route to forward the packet has Serial 0/0/1 as an exit interface and 2682112 as the cost.

---

### Question 55

**A network administrator is configuring a new Cisco switch for remote management access. Which three items must be configured on the switch for the task? (Choose three.)**

**Options:**

- [x] IP address
- [ ] VTP domain
- [x] vty lines
- [ ] default VLAN
- [x] default gateway
- [ ] loopback address

**Answer:** IP address, vty lines, default gateway

**Explanation:** To enable the remote management access, the Cisco switch must be configured with an IP address and a default gateway. In addition, vty lines must configured to enable either Telnet or SSH connections.

---

### Question 56

**Refer to the exhibit. Which statement shown in the output allows router R1 to respond to stateless DHCPv6 requests?**

![Exhibit](images/image_020.png)

**Options:**

- [x] ipv6 nd other-config-flag
- [ ] prefix-delegation 2001:DB8:8::/48 00030001000E84244E70
- [ ] ipv6 dhcp server LAN1
- [ ] ipv6 unicast-routing
- [ ] dns-server 2001:DB8:8::8

**Answer:** ipv6 nd other-config-flag

**Explanation:** The interface command ipv6 nd other-config-flag allows RA messages to be sent on this interface, indicating that additional information is available from a stateless DHCPv6 server.

---

### Question 57

**Refer to the exhibit. A Layer 3 switch routes for three VLANs and connects to a router for Internet connectivity. Which two configurations would be applied to the switch? (Choose two.)**

**Options:**

- [ ] (config)# interface gigabitethernet1/1 (config-if)# switchport mode trunk
- [x] (config)# interface gigabitethernet 1/1 (config-if)# no switchport (config-if)# ip address 192.168.1.2 255.255.255.252
- [ ] (config)# interface vlan 1 (config-if)# ip address 192.168.1.2 255.255.255.0 (config-if)# no shutdown
- [x] (config)# ip routing
- [ ] (config)# interface fastethernet0/4 (config-if)# switchport mode trunk

**Answer:** (config)# interface gigabitethernet 1/1 (config-if)# no switchport (config-if)# ip address 192.168.1.2 255.255.255.252, (config)# ip routing

---

### Question 58

**A technician is troubleshooting a slow WLAN and decides to use the split-the-traffic approach. Which two parameters would have to be configured to do this? (Choose two.)**

![Exhibit](images/image_021.png)

**Options:**

- [x] Configure the 5 GHz band for streaming multimedia and time sensitive traffic.
- [ ] Configure the security mode to WPA Personal TKIP/AES for one network and WPA2 Personal AES for the other network
- [x] Configure the 2.4 GHz band for basic internet traffic that is not time sensitive.
- [ ] Configure the security mode to WPA Personal TKIP/AES for both networks.
- [ ] Configure a common SSID for both split networks.

**Answer:** Configure the 5 GHz band for streaming multimedia and time sensitive traffic, Configure the 2.4 GHz band for basic internet traffic that is not time sensitive.

---

### Question 59

**A company has just switched to a new ISP. The ISP has completed and checked the connection from its site to the company. However, employees at the company are not able to access the internet. What should be done or checked?**

![Exhibit](images/image_022.png)

**Options:**

- [ ] Verify that the static route to the server is present in the routing table.
- [ ] Check the configuration on the floating static route and adjust the AD.
- [x] Ensure that the old default route has been removed from the company edge routers.
- [ ] Create a floating static route to that network.

**Answer:** Ensure that the old default route has been removed from the company edge routers.

---

### Question 60

**Which information does a switch use to populate the MAC address table?**

**Options:**

- [ ] the destination MAC address and the incoming port
- [ ] the destination MAC address and the outgoing port
- [ ] the source and destination MAC addresses and the incoming port
- [ ] the source and destination MAC addresses and the outgoing port
- [x] the source MAC address and the incoming port
- [ ] the source MAC address and the outgoing port

**Answer:** the source MAC address and the incoming port

**Explanation:** To maintain the MAC address table, the switch uses the source MAC address of the incoming packets and the port that the packets enter.

---

### Question 61

**Refer to the exhibit. A network administrator is reviewing the configuration of switch S1. Which protocol has been implemented to group multiple physical ports into one logical link?**

**Options:**

- [x] PAgP
- [ ] DTP
- [ ] LACP
- [ ] STP

**Answer:** PAgP

**Explanation:** The EtherChannel protocol PAgP provides the grouping of physical interfaces and utilizes the modes of auto and desirable.

---

### Question 62

**Which type of static route is configured with a greater administrative distance to provide a backup route to a route learned from a dynamic routing protocol?**

**Options:**

- [x] floating static route
- [ ] default static route
- [ ] summary static route
- [ ] standard static route

**Answer:** floating static route

**Explanation:** Floating static routes are backup routes that are placed into the routing table if a primary route is lost.

---

### Question 63

**What action takes place when a frame entering a switch has a unicast destination MAC address appearing in the MAC address table?**

**Options:**

- [x] The switch updates the refresh timer for the entry.
- [ ] The switch forwards the frame out of the specified port.
- [ ] The switch purges the entire MAC address table.
- [ ] The switch replaces the old entry and uses the more current port.

**Answer:** The switch updates the refresh timer for the entry.

---

### Question 64

**Refer to the exhibit. Which command will create a static route on R2 in order to reach PC B?**

**Options:**

- [x] R2(config)# ip route 172.16.2.0 255.255.255.0 172.16.3.1
- [ ] R2(config)# ip route 172.16.2.1 255.255.255.0 172.16.3.1
- [ ] R2(config)# ip route 172.16.2.0 255.255.255.0 172.16.2.254
- [ ] R2(config)# ip route 172.16.3.0 255.255.255.0 172.16.2.254

**Answer:** R2(config)# ip route 172.16.2.0 255.255.255.0 172.16.3.1

**Explanation:** Because the network to be reached is 172.16.2.0 and the next-hop IP address is 172.16.3.1, the command is R2(config)# ip route 172.16.2.0 255.255.255.0 172.16.3.1

---

### Question 65

**What protocol or technology allows data to transmit over redundant switch links?**

**Options:**

- [x] EtherChannel
- [ ] DTP
- [ ] STP
- [ ] VTP

**Answer:** EtherChannel

---

### Question 66

**Refer to the exhibit. Which three hosts will receive ARP requests from host A, assuming that port Fa0/4 on both switches is configured to carry traffic for multiple VLANs? (Choose three.)**

![Exhibit](images/image_023.jpeg)

**Options:**

- [x] host B
- [x] host C
- [x] host D
- [ ] host E
- [ ] host F
- [ ] host G

**Answer:** host B, host C, host D

**Explanation:** ARP requests are sent out as broadcasts. That means the ARP request is sent only throughout a specific VLAN. VLAN 1 hosts will only hear ARP requests from hosts on VLAN 1.

---

### Question 67

**Refer to the exhibit. The network administrator configures both switches as displayed. However, host C is unable to ping host D and host E is unable to ping host F. What action should the administrator take to enable this communication?**

![Exhibit](images/image_024.png)

**Options:**

- [ ] Associate hosts A and B with VLAN 10 instead of VLAN 1.
- [ ] Configure either trunk port in the dynamic desirable mode.
- [x] Include a router in the topology.
- [ ] Remove the native VLAN from the trunk.
- [ ] Add the switchport nonegotiate command to the configuration of SW2.

**Answer:** Include a router in the topology.

---

### Question 68

**What is the effect of entering the shutdown configuration command on a switch?**

**Options:**

- [ ] It enables BPDU guard on a specific port.
- [x] It disables an unused port.
- [ ] It enables portfast on a specific switch interface.
- [ ] It disables DTP on a non-trunking interface.

**Answer:** It disables an unused port.

---

### Question 69

**What would be the primary reason an attacker would launch a MAC address overflow attack?**

![Exhibit](images/image_025.png)

**Options:**

- [ ] so that the switch stops forwarding traffic
- [ ] so that legitimate hosts cannot obtain a MAC address
- [x] so that the attacker can see frames that are destined for other hosts
- [ ] so that the attacker can execute arbitrary code on the switch

**Answer:** so that the attacker can see frames that are destined for other hosts

---

### Question 70

**During the AAA process, when will authorization be implemented?**

**Options:**

- [x] Immediately after successful authentication against an AAA data source
- [ ] Immediately after AAA accounting and auditing receives detailed reports
- [ ] Immediately after an AAA client sends authentication information to a centralized server
- [ ] Immediately after the determination of which resources a user can access

**Answer:** Immediately after successful authentication against an AAA data source

**Explanation:** AAA authorization is implemented immediately after the user is authenticated against a specific AAA data source.

---

### Question 71

**A company security policy requires that all MAC addressing be dynamically learned and added to both the MAC address table and the running configuration on each switch. Which port security configuration will accomplish this?**

**Options:**

- [ ] auto secure MAC addresses
- [ ] dynamic secure MAC addresses
- [ ] static secure MAC addresses
- [x] sticky secure MAC addresses

**Answer:** sticky secure MAC addresses

**Explanation:** With sticky secure MAC addressing, the MAC addresses can be either dynamically learned or manually configured and then stored in the address table and added to the running configuration file.

---

### Question 72

**Which three Wi-Fi standards operate in the 2.4GHz range of frequencies? (Choose three.)**

**Options:**

- [ ] 802.11a
- [x] 802.11b
- [x] 802.11g
- [x] 802.11n
- [ ] 802.11ac

**Answer:** 802.11b, 802.11g, 802.11n

**Explanation:** 802.11b and 802.11g operate in the 2.4GHz range, and 802.11n can operate in either the 2.4GHz or the 5GHz range. 802.11a and 802.11ac operate only in the 5GHz range.

---

### Question 73

**To obtain an overview of the spanning tree status of a switched network, a network engineer issues the show spanning-tree command on a switch. Which two items of information will this command display? (Choose two.)**

**Options:**

- [x] The root bridge BID.
- [x] The role of the ports in all VLANs.
- [ ] The status of native VLAN ports.
- [ ] The number of broadcasts received on each root port.
- [ ] The IP address of the management VLAN interface.

**Answer:** The root bridge BID, The role of the ports in all VLANs.

---

### Question 74

**Refer to the exhibit. Which trunk link will not forward any traffic after the root bridge election process is complete?**

**Options:**

- [ ] Trunk1
- [ ] Trunk2
- [x] Trunk3
- [ ] Trunk4

**Answer:** Trunk3

---

### Question 75

**Which method of IPv6 prefix assignment relies on the prefix contained in RA messages?**

**Options:**

- [ ] EUI-64
- [x] SLAAC
- [ ] static
- [ ] stateful DHCPv6

**Answer:** SLAAC

**Explanation:** Stateless Address Autoconfiguration (SLAAC) relies on information received in router advertisement (RA) messages in order to automatically create an IPv6 address.

---

### Question 76

**Which two protocols are used to provide server-based AAA authentication? (Choose two.)**

![Exhibit](images/image_026.jpeg)

**Options:**

- [ ] 802.1x
- [ ] SSH
- [ ] SNMP
- [x] TACACS+
- [x] RADIUS

**Answer:** TACACS+, RADIUS

**Explanation:** Server-based AAA authentication uses an external TACACS or RADIUS authentication server to maintain a username and password database.

---

### Question 77

**A network administrator is configuring a WLAN. Why would the administrator disable the broadcast feature for the SSID?**

**Options:**

- [x] to eliminate outsiders scanning for available SSIDs in the area
- [ ] to reduce the risk of interference by external devices such as microwave ovens
- [ ] to reduce the risk of unauthorized APs being added to the network
- [ ] to provide privacy and integrity to wireless traffic by using encryption

**Answer:** to eliminate outsiders scanning for available SSIDs in the area

---

### Question 78

**Which mitigation technique would prevent rogue servers from providing false IP configuration parameters to clients?**

**Options:**

- [ ] implementing port security
- [x] turning on DHCP snooping
- [ ] disabling CDP on edge ports
- [ ] implementing port-security on edge ports

**Answer:** turning on DHCP snooping

**Explanation:** Like Dynamic ARP Inspection (DAI), IP Source Guard (IPSG) needs to determine the validity of MAC-address-to-IP-address bindings. To do this IPSG uses the bindings database built by DHCP snooping.

---

### Question 79

**A network administrator configures the port security feature on a switch. The security policy specifies that each access port should allow up to two MAC addresses. When the maximum number of MAC addresses is reached, a frame with the unknown source MAC address is dropped and a notification is sent to the syslog server. Which security violation mode should be configured for each access port?**

**Options:**

- [ ] shutdown
- [x] restrict
- [ ] warning
- [ ] protect

**Answer:** restrict

**Explanation:** Restrict – a port security violation causes the interface to drop packets with unknown source addresses and to send a notification that a security violation has occurred.

---

### Question 80

**What protocol or technology defines a group of routers, one of them defined as active and another one as standby?**

**Options:**

- [ ] EtherChannel
- [ ] VTP
- [x] HSRP
- [ ] DTP

**Answer:** HSRP

---

### Question 81

**Refer to the exhibit. After attempting to enter the configuration that is shown in router RTA, an administrator receives an error and users on VLAN 20 report that they are unable to reach users on VLAN 30. What is causing the problem?**

**Options:**

- [ ] There is no address on Fa0/0 to use as a default gateway.
- [x] RTA is using the same subnet for VLAN 20 and VLAN 30.
- [ ] Dot1q does not support subinterfaces.
- [ ] The no shutdown command should have been issued on Fa0/0.20 and Fa0/0.30.

**Answer:** RTA is using the same subnet for VLAN 20 and VLAN 30.

---

### Question 82

**Which three pairs of trunking modes will establish a functional trunk link between two Cisco switches? (Choose three.)**

**Options:**

- [ ] dynamic auto - dynamic auto
- [ ] access - trunk
- [x] dynamic desirable - trunk
- [ ] access - dynamic auto
- [x] dynamic desirable - dynamic desirable
- [x] dynamic desirable - dynamic auto

**Answer:** dynamic desirable - trunk, dynamic desirable - dynamic desirable, dynamic desirable - dynamic auto

---

### Question 83

**A technician is configuring a router for a small company with multiple WLANs and doesn't need the complexity of a dynamic routing protocol. What should be done or checked?**

![Exhibit](images/image_027.png)

**Options:**

- [ ] Verify that there is not a default route in any of the edge router routing tables.
- [x] Create static routes to all internal networks and a default route to the internet.
- [ ] Create extra static routes to the same location with an AD of 1.
- [ ] Check the statistics on the default route for oversaturation.

**Answer:** Create static routes to all internal networks and a default route to the internet.

---

### Question 84

**A company is deploying a wireless network in the distribution facility in a Boston suburb. The warehouse is quite large and it requires multiple access points to be used. Because some of the company devices still operate at 2.4GHz, the network administrator decides to deploy the 802.11g standard. Which channel assignments on the multiple access points will make sure that the wireless channels are not overlapping?**

**Options:**

- [ ] channels 1, 5, and 9
- [x] channels 1, 6, and 11
- [ ] channels 1, 7, and 13
- [ ] channels 2, 6, and 10

**Answer:** channels 1, 6, and 11

**Explanation:** In the North America domain, 11 channels are allowed for 2.4GHz wireless networking. Among these 11 channels, the combination of channels 1, 6, and 11 are the only non-overlapping channel combination.

---

### Question 85

**A network administrator of a small advertising company is configuring WLAN security by using the WPA2 PSK method. Which credential do office users need in order to connect their laptops to the WLAN?**

**Options:**

- [ ] the company username and password through Active Directory service
- [x] a key that matches the key on the AP
- [ ] a user passphrase
- [ ] a username and password configured on the AP

**Answer:** a key that matches the key on the AP

**Explanation:** When a WLAN is configured with WPA2 PSK, wireless users must know the pre-shared key to associate and authenticate with the AP.

---

### Question 86

**Refer to the exhibit. What are the possible port roles for ports A, B, C, and D in this RSTP-enabled network?**

**Options:**

- [ ] alternate, designated, root, root
- [x] designated, alternate, root, root
- [ ] alternate, root, designated, root
- [ ] designated, root, alternate, root

**Answer:** designated, alternate, root, root

**Explanation:** Because S1 is the root bridge, B is a designated port, and C and D root ports. RSTP supports a new port type, alternate port in discarding state, that can be port A in this scenario.

---

### Question 87

**Refer to the exhibit. Which static route would an IT technician enter to create a backup route to the 172.16.1.0 network that is only used if the primary RIP learned route fails?**

**Options:**

- [ ] ip route 172.16.1.0 255.255.255.0 s0/0/0
- [x] ip route 172.16.1.0 255.255.255.0 s0/0/0 121
- [ ] ip route 172.16.1.0 255.255.255.0 s0/0/0 111
- [ ] ip route 172.16.1.0 255.255.255.0 s0/0/0 91

**Answer:** ip route 172.16.1.0 255.255.255.0 s0/0/0 121

**Explanation:** A backup static route is called a floating static route. A floating static route has an administrative distance greater than the administrative distance of another static route or dynamic route.

---

### Question 88

**What mitigation plan is best for thwarting a DoS attack that is creating a MAC address table overflow?**

![Exhibit](images/image_028.jpeg)

**Options:**

- [ ] Disable DTP.
- [ ] Disable STP.
- [x] Enable port security.
- [ ] Place unused ports in an unused VLAN.

**Answer:** Enable port security.

**Explanation:** A MAC address (CAM) table overflow attack, buffer overflow, and MAC address spoofing can all be mitigated by configuring port security.

---

### Question 89

**A network engineer is troubleshooting a newly deployed wireless network that is using the latest 802.11 standards. When users access high bandwidth services such as streaming video, the wireless network performance is poor. To improve performance the network engineer decides to configure a 5 Ghz frequency band SSID and train users to use that SSID for streaming media services. Why might this solution improve the wireless network performance for that type of service?**

![Exhibit](images/image_029.png)

**Options:**

- [ ] Requiring the users to switch to the 5 GHz band for streaming media is inconvenient and will result in fewer users accessing these services.
- [x] The 5 GHz band has more channels and is less crowded than the 2.4 GHz band, which makes it more suited to streaming multimedia.
- [ ] The 5 GHz band has a greater range and is therefore likely to be interference-free.
- [ ] The only users that can switch to the 5 GHz band will be those with the latest wireless NICs, which will reduce usage.

**Answer:** The 5 GHz band has more channels and is less crowded than the 2.4 GHz band, which makes it more suited to streaming multimedia.

---

### Question 90

**Which DHCPv4 message will a client send to accept an IPv4 address that is offered by a DHCP server?**

**Options:**

- [ ] broadcast DHCPACK
- [x] broadcast DHCPREQUEST
- [ ] unicast DHCPACK
- [ ] unicast DHCPREQUEST

**Answer:** broadcast DHCPREQUEST

**Explanation:** When a DHCP client receives DHCPOFFER messages, it will send a broadcast DHCPREQUEST message for two purposes. First, it indicates to the offering DHCP server that it would like to accept the offer and bind the IP address.

---

### Question 91

**Refer to the exhibit. Which destination MAC address is used when frames are sent from the workstation to the default gateway?**

**Options:**

- [x] MAC address of the virtual router
- [ ] MAC address of the standby router
- [ ] MAC addresses of both the forwarding and standby routers
- [ ] MAC address of the forwarding router

**Answer:** MAC address of the virtual router

**Explanation:** The IP address of the virtual router acts as the default gateway for all the workstations. Therefore, the MAC address that is returned by the Address Resolution Protocol to the workstation will be the MAC address of the virtual router.

---

### Question 92

**After a host has generated an IPv6 address by using the DHCPv6 or SLAAC process, how does the host verify that the address is unique and therefore usable?**

**Options:**

- [ ] The host sends an ICMPv6 echo request message to the DHCPv6 or SLAAC-learned address and if no reply is returned, the address is considered unique.
- [x] The host sends an ICMPv6 neighbor solicitation message to the DHCP or SLAAC-learned address and if no neighbor advertisement is returned, the address is considered unique.
- [ ] The host checks the local neighbor cache for the learned address and if the address is not cached, it it considered unique.
- [ ] The host sends an ARP broadcast to the local link and if no hosts send a reply, the address is considered unique.

**Answer:** The host sends an ICMPv6 neighbor solicitation message to the DHCP or SLAAC-learned address and if no neighbor advertisement is returned, the address is considered unique.

**Explanation:** To verify that the address is indeed unique, the host sends an ICMPv6 neighbor solicitation to the address. If no neighbor advertisement is returned, the host considers the address to be unique and configures it on the interface.

---

### Question 93

**Match the purpose with its DHCP message type. (Not all options are used.)**

![Exhibit](images/image_030.jpeg)

---

### Question 94

**Which protocol adds security to remote connections?**

**Options:**

- [ ] FTP
- [ ] HTTP
- [ ] NetBEUI
- [ ] POP
- [x] SSH

**Answer:** SSH

**Explanation:** SSH allows a technician to securely connect to a remote network device for monitoring and troubleshooting.

---

### Question 95

**Refer to the exhibit. A network administrator is verifying the configuration of inter-VLAN routing. Users complain that PC2 cannot communicate with PC1. Based on the output, what is the possible cause of the problem?**

**Options:**

- [x] Gi0/0 is not configured as a trunk port.
- [ ] The command interface GigabitEthernet0/0.5 was entered incorrectly.
- [ ] There is no IP address configured on the interface Gi0/0.
- [ ] The no shutdown command is not entered on subinterfaces.
- [ ] The encapsulation dot1Q 5 command contains the wrong VLAN.

**Answer:** Gi0/0 is not configured as a trunk port.

---

### Question 96

**Refer to the exhibit. A network administrator is configuring inter-VLAN routing on a network. For now, only one VLAN is being used, but more will be added soon. What is the missing parameter that is shown as the highlighted question mark in the graphic?**

**Options:**

- [x] It identifies the subinterface.
- [ ] It identifies the VLAN number.
- [ ] It identifies the native VLAN number.
- [ ] It identifies the type of encapsulation that is used.
- [ ] It identifies the number of hosts that are allowed on the interface.

**Answer:** It identifies the subinterface.

---

### Question 97

**Match each DHCP message type with its description. (Not all options are used.)**

---

### Question 98

**What network attack seeks to create a DoS for clients by preventing them from being able to obtain a DHCP lease?**

![Exhibit](images/image_031.png)

**Options:**

- [ ] IP address spoofing
- [x] DHCP starvation
- [ ] CAM table attack
- [ ] DHCP spoofing

**Answer:** DHCP starvation

**Explanation:** DCHP starvation attacks are launched by an attacker with the intent to create a DoS for DHCP clients. The attacker uses a tool that sends many DHCPDISCOVER messages in order to lease the entire pool of available IP addresses, thus denying them to legitimate hosts.

---

### Question 99

**Refer to the exhibit. If the IP addresses of the default gateway router and the DNS server are correct, what is the configuration problem?**

**Options:**

- [ ] The DNS server and the default gateway router should be in the same subnet.
- [x] The IP address of the default gateway router is not contained in the excluded address list.
- [ ] The default-router and dns-server commands need to be configured with subnet masks.
- [ ] The IP address of the DNS server is not contained in the excluded address list.

**Answer:** The IP address of the default gateway router is not contained in the excluded address list.

**Explanation:** In this configuration, the excluded address list should include the address that is assigned to the default gateway router.

---

### Question 100

**Refer to the exhibit. A network administrator has added a new subnet to the network and needs hosts on that subnet to receive IPv4 addresses from the DHCPv4 server. What two commands will allow hosts on the new subnet to receive addresses from the DHCP4 server? (Choose two.)**

**Options:**

- [x] R1(config-if)# ip helper-address 10.2.0.250
- [x] R1(config)# interface G0/1
- [ ] R1(config)# interface G0/0
- [ ] R2(config-if)# ip helper-address 10.2.0.250
- [ ] R2(config)# interface G0/0
- [ ] R1(config-if)# ip helper-address 10.1.0.254

**Answer:** R1(config-if)# ip helper-address 10.2.0.250, R1(config)# interface G0/1

---

### Question 101

**What protocol or technology uses source IP to destination IP as a load-balancing mechanism?**

![Exhibit](images/image_032.jpeg)

**Options:**

- [ ] VTP
- [x] EtherChannel
- [ ] DTP
- [ ] STP

**Answer:** EtherChannel

---

### Question 102

**What protocol should be disabled to help mitigate VLAN attacks?**

**Options:**

- [ ] CDP
- [ ] ARP
- [ ] STP
- [x] DTP

**Answer:** DTP

---

### Question 103

**What protocol or technology requires switches to be in server mode or client mode?**

**Options:**

- [ ] EtherChannel
- [ ] STP
- [x] VTP
- [ ] DTP

**Answer:** VTP

---

### Question 104

**What are two reasons a network administrator would segment a network with a Layer 2 switch? (Choose two.)**

**Options:**

- [ ] to create fewer collision domains
- [x] to enhance user bandwidth
- [ ] to create more broadcast domains
- [ ] to eliminate virtual circuits
- [x] to isolate traffic between segments
- [ ] to isolate ARP request messages from the rest of the network

**Answer:** to enhance user bandwidth, to isolate traffic between segments

**Explanation:** A switch has the ability of creating temporary point-to-point connections between the directly-attached transmitting and receiving network devices. The two devices have full-bandwidth full-duplex connectivity during the transmission.

---

### Question 105

**What command will enable a router to begin sending messages that allow it to configure a link-local address without using an IPv6 DHCP server?**

**Options:**

- [ ] a static route
- [ ] the ipv6 route ::/0 command
- [x] the ipv6 unicast-routing command
- [ ] the ip routing command

**Answer:** the ipv6 unicast-routing command

**Explanation:** To enable IPv6 on a router you must use the ipv6 unicast-routing global configuration command.

---

### Question 106

**A network administrator is using the router-on-a-stick model to configure a switch and a router for inter-VLAN routing. What configuration should be made on the switch port that connects to the router?**

**Options:**

- [ ] Configure it as a trunk port and allow only untagged traffic.
- [ ] Configure the port as an access port and a member of VLAN1.
- [x] Configure the port as an 802.1q trunk port.
- [ ] Configure the port as a trunk port and assign it to VLAN1.

**Answer:** Configure the port as an 802.1q trunk port.

**Explanation:** The port on the switch that connects to the router interface should be configured as a trunk port.

---

### Question 107

**What are three techniques for mitigating VLAN attacks? (Choose three.)**

**Options:**

- [ ] Use private VLANs.
- [ ] Enable BPDU guard.
- [x] Enable trunking manually
- [ ] Enable Source Guard.
- [x] Disable DTP.
- [x] Set the native VLAN to an unused VLAN.

**Answer:** Enable trunking manually, Disable DTP, Set the native VLAN to an unused VLAN.

**Explanation:** Mitigating a VLAN attack can be done by disabling Dynamic Trunking Protocol (DTP), manually setting ports to trunking mode, and by setting the native VLAN of trunk links to VLANs not in use.

---

### Question 108

**Match the DHCP message types to the order of the DHCPv4 process. (Not all options are used.)**

---

### Question 109

**In which situation would a technician use the show interfaces switch command?**

**Options:**

- [ ] to determine if remote access is enabled
- [x] when packets are being dropped from a particular directly attached host
- [ ] when an end device can reach local devices, but not remote devices
- [ ] to determine the MAC address of a directly attached network device on a particular interface

**Answer:** when packets are being dropped from a particular directly attached host

**Explanation:** The show interfaces command is useful to detect media errors, to see if packets are being sent and received, and to determine if any runts, giants, CRCs, interface resets, or other errors have occurred.

---

### Question 110

**What is a drawback of the local database method of securing device access that can be solved by using AAA with centralized servers?**

**Options:**

- [ ] There is no ability to provide accountability.
- [x] User accounts must be configured locally on each device, which is an unscalable authentication solution.
- [ ] It is very susceptible to brute-force attacks because there is no username.
- [ ] The passwords can only be stored in plain text in the running configuration.

**Answer:** User accounts must be configured locally on each device, which is an unscalable authentication solution.

**Explanation:** The account information must be configured on each device where that account should have access, making this solution very difficult to scale.

---

### Question 111

**What action does a DHCPv4 client take if it receives more than one DHCPOFFER from multiple DHCP servers?**

**Options:**

- [x] It sends a DHCPREQUEST that identifies which lease offer the client is accepting.
- [ ] It sends a DHCPNAK and begins the DHCP process over again.
- [ ] It discards both offers and sends a new DHCPDISCOVER.
- [ ] It accepts both DHCPOFFER messages and sends a DHCPACK.

**Answer:** It sends a DHCPREQUEST that identifies which lease offer the client is accepting.

---

### Question 112

**Refer to the exhibit. The network administrator is configuring the port security feature on switch SWC. The administrator issued the command show port-security interface fa 0/2 to verify the configuration. What can be concluded from the output that is shown? (Choose three.)**

![Exhibit](images/image_033.jpeg)

**Options:**

- [ ] Three security violations have been detected on this interface.
- [x] This port is currently up.
- [ ] The port is configured as a trunk link.
- [ ] Security violations will cause this port to shut down immediately.
- [ ] There is no device currently connected to this port.
- [x] The switch port mode for this interface is access mode.

**Answer:** This port is currently up, The switch port mode for this interface is access mode.

**Explanation:** The port is up because of the port status of secure-up. A port must be in access mode in order to activate and use port security.

---

### Question 113

**What method of wireless authentication is dependent on a RADIUS authentication server?**

**Options:**

- [ ] WEP
- [ ] WPA Personal
- [ ] WPA2 Personal
- [x] WPA2 Enterprise

**Answer:** WPA2 Enterprise

---

### Question 114

**A network administrator has found a user sending a double-tagged 802.1Q frame to a switch. What is the best solution to prevent this type of attack?**

**Options:**

- [ ] The native VLAN number used on any trunk should be one of the active data VLANs.
- [x] The VLANs for user access ports should be different VLANs than any native VLANs used on trunk ports.
- [ ] Trunk ports should be configured with port security.
- [ ] Trunk ports should use the default VLAN as the native VLAN number.

**Answer:** The VLANs for user access ports should be different VLANs than any native VLANs used on trunk ports.

---

### Question 115

**Refer to the exhibit. Which two conclusions can be drawn from the output? (Choose two.)**

**Options:**

- [ ] The EtherChannel is down.
- [x] The port channel ID is 2.
- [ ] The port channel is a Layer 3 channel.
- [x] The bundle is fully operational.
- [ ] The load-balancing method used is source port to destination port.

**Answer:** The port channel ID is 2, The bundle is fully operational.

---

### Question 116

**Match the step number to the sequence of stages that occur during the HSRP failover process. (Not all options are used.)**

---

### Question 117

**On a Cisco 3504 WLC Summary page ( Advanced > Summary ), which tab allows a network administrator to configure a particular WLAN with a WPA2 policy?**

![Exhibit](images/image_034.png)

**Options:**

- [x] WLANs
- [ ] SECURITY
- [ ] WIRELESS
- [ ] MANAGEMENT

**Answer:** WLANs

**Explanation:** The WLANs tab in the Cisco 3504 WLC advanced Summary page allows a user to access the configuration of WLANs including security, QoS, and policy-mapping.

---

### Question 118

**Refer to the exhibit. A network engineer is configuring IPv6 routing on the network. Which command issued on router HQ will configure a default route to the Internet to forward packets to an IPv6 destination network that is not listed in the routing table?**

![Exhibit](images/image_035.png)

**Options:**

- [ ] ipv6 route ::/0 serial 0/0/0
- [ ] ip route 0.0.0.0 0.0.0.0 serial 0/1/1
- [ ] ipv6 route ::1/0 serial 0/1/1
- [x] ipv6 route ::/0 serial 0/1/1

**Answer:** ipv6 route ::/0 serial 0/1/1

---

### Question 119

**Users are complaining of sporadic access to the internet every afternoon. What should be done or checked?**

**Options:**

- [ ] Create static routes to all internal networks and a default route to the internet.
- [ ] Verify that there is not a default route in any of the edge router routing tables.
- [ ] Create a floating static route to that network.
- [x] Check the statistics on the default route for oversaturation.

**Answer:** Check the statistics on the default route for oversaturation.

---

### Question 120

**What action takes place when the source MAC address of a frame entering a switch appears in the MAC address table associated with a different port?**

![Exhibit](images/image_036.png)

**Options:**

- [ ] The switch purges the entire MAC address table.
- [x] The switch replaces the old entry and uses the more current port.
- [ ] The switch updates the refresh timer for the entry.
- [ ] The switch forwards the frame out of the specified port.

**Answer:** The switch replaces the old entry and uses the more current port.

---

### Question 121

**A network administrator is configuring a WLAN. Why would the administrator use a WLAN controller?**

**Options:**

- [x] to centralize management of multiple WLANs
- [ ] to provide privacy and integrity to wireless traffic by using encryption
- [ ] to facilitate group configuration and management of multiple WLANs through a WLC
- [ ] to provide prioritized service for time-sensitive applications

**Answer:** to centralize management of multiple WLANs

---

### Question 122

**A new Layer 3 switch is connected to a router and is being configured for interVLAN routing. What are three of the five steps required for the configuration? (Choose three.)**

**Options:**

- [x] creating SVI interfaces
- [ ] adjusting the route metric
- [x] enabling IP routing
- [x] assigning ports to VLANs
- [ ] deleting the default VLAN
- [ ] assigning the ports to the native VLAN

**Answer:** creating SVI interfaces, enabling IP routing, assigning ports to VLANs

**Explanation:** Steps to configure Layer 3 switch to route: (1) Configure the routed port, (2) Enable routing, (3) Configure routing, (4) Verify routing, (5) Verify connectivity.

---

### Question 123

**Which three statements accurately describe duplex and speed settings on Cisco 2960 switches? (Choose three.)**

**Options:**

- [x] An autonegotiation failure can result in connectivity issues.
- [ ] When the speed is set to 1000 Mb/s, the switch ports will operate in full-duplex mode.
- [x] The duplex and speed settings of each switch port can be manually configured.
- [ ] Enabling autonegotiation on a hub will prevent mismatched port speeds when connecting the hub to the switch.
- [ ] By default, the speed is set to 100 Mb/s and the duplex mode is set to autonegotiation.
- [x] By default, the autonegotiation feature is enabled.

**Answer:** An autonegotiation failure can result in connectivity issues, The duplex and speed settings of each switch port can be manually configured, By default, the autonegotiation feature is enabled.

---

### Question 124

**Refer to the exhibit. A network administrator configures R1 for inter-VLAN routing between VLAN 10 and VLAN 20. However, the devices in VLAN 10 and VLAN 20 cannot communicate. Based on the configuration in the exhibit, what is a possible cause for the problem?**

**Options:**

- [x] The port Gi0/0 should be configured as trunk port.
- [ ] The encapsulation is misconfigured on a subinterface.
- [ ] A no shutdown command should be added in each subinterface configuration.
- [ ] The command interface gigabitEthernet 0/0.1 is wrong.

**Answer:** The port Gi0/0 should be configured as trunk port.

---

### Question 125

**A network administrator uses the spanning-tree portfast bpduguard default global configuration command to enable BPDU guard on a switch. However, BPDU guard is not activated on all access ports. What is the cause of the issue?**

**Options:**

- [ ] BPDU guard needs to be activated in the interface configuration command mode.
- [ ] Access ports configured with root guard cannot be configured with BPDU guard.
- [ ] Access ports belong to different VLANs.
- [x] PortFast is not configured on all access ports.

**Answer:** PortFast is not configured on all access ports.

---

### Question 126

**Which two types of spanning tree protocols can cause suboptimal traffic flows because they assume only one spanning-tree instance for the entire bridged network? (Choose two.)**

![Exhibit](images/image_037.png)

**Options:**

- [ ] MSTP
- [x] RSTP
- [ ] Rapid PVST+
- [ ] PVST+
- [x] STP

**Answer:** RSTP, STP

---

### Question 127

**Refer to the exhibit. A network administrator is configuring the router R1 for IPv6 address assignment. Based on the partial configuration, which IPv6 global unicast address assignment scheme does the administrator intend to implement?**

**Options:**

- [ ] stateful
- [x] stateless
- [ ] manual configuration
- [ ] SLAAC

**Answer:** stateless

---

### Question 128

**A WLAN engineer deploys a WLC and five wireless APs using the CAPWAP protocol with the DTLS feature to secure the control plane of the network devices. While testing the wireless network, the WLAN engineer notices that data traffic is being exchanged between the WLC and the APs in plain-text and is not being encrypted. What is the most likely reason for this?**

**Options:**

- [ ] DTLS only provides data security through authentication and does not provide encryption for data moving between a wireless LAN controller (WLC) and an access point (AP).
- [x] Although DTLS is enabled by default to secure the CAPWAP control channel, it is disabled by default for the data channel.
- [ ] DTLS is a protocol that only provides security between the access point (AP) and the wireless client.
- [ ] Data encryption requires a DTLS license to be installed on each access point (AP) prior to being enabled on the wireless LAN controller (WLC).

**Answer:** Although DTLS is enabled by default to secure the CAPWAP control channel, it is disabled by default for the data channel.

**Explanation:** DTLS is enabled by default to secure the CAPWAP control channel but is disabled by default for the data channel.

---

### Question 129

**A network administrator is adding a new WLAN on a Cisco 3500 series WLC. The network administrator does not want the technicians in the remote office to be able to add new VLANs to the switch, but the switch should receive VLAN updates from the VTP domain. Which two steps must be performed to configure VTP on the new switch to meet these conditions? (Choose two.)**

![Exhibit](images/image_038.gif)

**Options:**

- [x] Configure the new switch as a VTP client.
- [x] Configure the existing VTP domain name on the new switch.
- [ ] Configure an IP address on the new switch.
- [ ] Configure all ports of both switches to access mode.
- [ ] Enable VTP pruning.

**Answer:** Configure the new switch as a VTP client, Configure the existing VTP domain name on the new switch.

---

### Question 130

**Refer to the exhibit. Consider that the main power has just been restored. PC3 issues a broadcast IPv4 DHCP request. To which port will SW1 forward this request?**

**Options:**

- [ ] to Fa0/1, Fa0/2, and Fa0/3 only
- [ ] to Fa0/1, Fa0/2, Fa0/3, and Fa0/4
- [ ] to Fa0/1 only
- [x] to Fa0/1, Fa0/2, and Fa0/4 only
- [ ] to Fa0/1 and Fa0/2 only

**Answer:** to Fa0/1, Fa0/2, and Fa0/4 only

---

### Question 131

**What action takes place when the source MAC address of a frame entering a switch is not in the MAC address table?**

**Options:**

- [ ] The switch forwards the frame out of the specified port.
- [ ] The switch will forward the frame out all ports except the incoming port.
- [x] The switch adds the MAC address and incoming port number to the table.
- [ ] The switch adds a MAC address table entry mapping for the destination MAC address and the ingress port.

**Answer:** The switch adds the MAC address and incoming port number to the table.

---

### Question 132

**Employees are unable to connect to servers on one of the internal networks. What should be done or checked?**

![Exhibit](images/image_039.jpeg)

**Options:**

- [x] Use the "show ip interface brief" command to see if an interface is down.
- [ ] Verify that there is not a default route in any of the edge router routing tables.
- [ ] Create static routes to all internal networks and a default route to the internet.
- [ ] Check the statistics on the default route for oversaturation.

**Answer:** Use the "show ip interface brief" command to see if an interface is down.

---

### Question 133

**What is the effect of entering the ip dhcp snooping configuration command on a switch?**

**Options:**

- [x] It enables DHCP snooping globally on a switch.
- [ ] It enables PortFast globally on a switch.
- [ ] It disables DTP negotiations on trunking ports.
- [ ] It manually enables a trunk link.

**Answer:** It enables DHCP snooping globally on a switch.

---

### Question 134

**An administrator notices that large numbers of packets are being dropped on one of the branch routers. What should be done or checked?**

**Options:**

- [ ] Create static routes to all internal networks and a default route to the internet.
- [ ] Create extra static routes to the same location with an AD of 1.
- [x] Check the statistics on the default route for oversaturation.
- [ ] Check the routing table for a missing static route.

**Answer:** Check the statistics on the default route for oversaturation.

---

### Question 135

**What are two switch characteristics that could help alleviate network congestion? (Choose two.)**

**Options:**

- [x] fast internal switching
- [x] large frame buffers
- [ ] store-and-forward switching
- [ ] low port density
- [ ] frame check sequence (FCS) check

**Answer:** fast internal switching, large frame buffers

---

### Question 136

**What is a result of connecting two or more switches together?**

**Options:**

- [ ] The number of broadcast domains is increased.
- [x] The size of the broadcast domain is increased.
- [ ] The number of collision domains is reduced.
- [ ] The size of the collision domain is increased.

**Answer:** The size of the broadcast domain is increased.

**Explanation:** When two or more switches are connected together, the size of the broadcast domain is increased and so is the number of collision domains.

---

### Question 137

**Match the step number to the sequence of stages that occur during the boot process of Cisco IOS routers and switches. (Not all options are used.)**

---

### Question 138

**Branch users were able to access a site in the morning but have had no connectivity with the site since lunch time. What should be done or checked?**

**Options:**

- [x] Verify that the static route to the server is present in the routing table.
- [ ] Use the "show ip interface brief" command to see if an interface is down.
- [ ] Check the configuration on the floating static route and adjust the AD.
- [ ] Create a floating static route to that network.

**Answer:** Verify that the static route to the server is present in the routing table.

---

### Question 139

**What is the effect of entering the switchport port-security configuration command on a switch?**

**Options:**

- [ ] It dynamically learns the L2 address and copies it to the running configuration.
- [x] It enables port security on an interface.
- [ ] It enables port security globally on the switch.
- [ ] It restricts the number of discovery messages, per second, to be received on the interface.

**Answer:** It enables port security on an interface.

---

### Question 140

**A network administrator is configuring a WLAN. Why would the administrator use multiple lightweight APs?**

**Options:**

- [ ] to centralize management of multiple WLANs
- [ ] to monitor the operation of the wireless network
- [ ] to provide prioritized service for time-sensitive applications
- [x] to facilitate group configuration and management of multiple WLANs through a WLC

**Answer:** to facilitate group configuration and management of multiple WLANs through a WLC

---

### Question 141

**Refer to the exhibit. PC-A and PC-B are both in VLAN 60. PC-A is unable to communicate with PC-B. What is the problem?**

**Options:**

- [ ] The native VLAN should be VLAN 60.
- [ ] The native VLAN is being pruned from the link.
- [ ] The trunk has been configured with the switchport nonegotiate command.
- [x] The VLAN that is used by PC-A is not in the list of allowed VLANs on the trunk.

**Answer:** The VLAN that is used by PC-A is not in the list of allowed VLANs on the trunk.

**Explanation:** VLAN 60, the VLAN that is associated with PC-A and PC-B, has not been allowed across the link.

---

### Question 142

**A network administrator is configuring a WLAN. Why would the administrator use RADIUS servers on the network?**

**Options:**

- [ ] to centralize management of multiple WLANs
- [x] to restrict access to the WLAN by authorized, authenticated users only
- [ ] to facilitate group configuration and management of multiple WLANs through a WLC
- [ ] to monitor the operation of the wireless network

**Answer:** to restrict access to the WLAN by authorized, authenticated users only

---

### Question 143

**What is the effect of entering the switchport mode access configuration command on a switch?**

**Options:**

- [ ] It enables BPDU guard on a specific port.
- [ ] It manually enables a trunk link.
- [ ] It disables an unused port.
- [x] It disables DTP on a non-trunking interface.

**Answer:** It disables DTP on a non-trunking interface.

---

### Question 144

**A network administrator has configured a router for stateless DHCPv6 operation. However, users report that workstations are not receiving DNS server information. Which two router configuration lines should be verified to ensure that stateless DHCPv6 service is properly configured? (Choose two.)**

![Exhibit](images/image_040.jpeg)

**Options:**

- [ ] The domain-name line is included in the ipv6 dhcp pool section.
- [x] The dns-server line is included in the ipv6 dhcp pool section.
- [x] The ipv6 nd other-config-flag is entered for the interface that faces the LAN segment.
- [ ] The address prefix line is included in the ipv6 dhcp pool section.
- [ ] The ipv6 nd managed-config-flag is entered for the interface that faces the LAN segment.

**Answer:** The dns-server line is included in the ipv6 dhcp pool section, The ipv6 nd other-config-flag is entered for the interface that faces the LAN segment.

**Explanation:** To use the stateless DHCPv6 method, the router must inform DHCPv6 clients through the command ipv6 nd other-config-flag. The DNS server address is indicated in the ipv6 dhcp pool configuration.

---

### Question 145

**A network administrator is configuring a WLAN. Why would the administrator disable the broadcast feature for the SSID?**

**Options:**

- [x] to eliminate outsiders scanning for available SSIDs in the area
- [ ] to centralize management of multiple WLANs
- [ ] to facilitate group configuration and management of multiple WLANs through a WLC
- [ ] to provide prioritized service for time-sensitive applications

**Answer:** to eliminate outsiders scanning for available SSIDs in the area

---

### Question 146

**Refer to the exhibit. An administrator is attempting to install an IPv6 static route on router R1 to reach the network attached to router R2. After the static route command is entered, connectivity to the network is still failing. What error has been made in the static route configuration?**

**Options:**

- [ ] The next hop address is incorrect.
- [x] The interface is incorrect.
- [ ] The destination network is incorrect.
- [ ] The network prefix is incorrect.

**Answer:** The interface is incorrect.

**Explanation:** In this example the interface in the static route is incorrect. The interface should be the exit interface on R1, which is s0/0/0.

---

### Question 147

**What action takes place when a frame entering a switch has a unicast destination MAC address that is not in the MAC address table?**

**Options:**

- [ ] The switch updates the refresh timer for the entry.
- [ ] The switch resets the refresh timer on all MAC address table entries.
- [ ] The switch replaces the old entry and uses the more current port.
- [x] The switch will forward the frame out all ports except the incoming port.

**Answer:** The switch will forward the frame out all ports except the incoming port.

---

### Question 148

**A junior technician was adding a route to a LAN router. A traceroute to a device on the new network revealed a wrong path and unreachable status. What should be done or checked?**

**Options:**

- [ ] Create a floating static route to that network.
- [ ] Check the configuration on the floating static route and adjust the AD.
- [x] Check the configuration of the exit interface on the new static route.
- [ ] Verify that the static route to the server is present in the routing table.

**Answer:** Check the configuration of the exit interface on the new static route.

---

### Question 149

**What is the effect of entering the ip arp inspection vlan 10 configuration command on a switch?**

![Exhibit](images/image_041.jpeg)

**Options:**

- [ ] It specifies the maximum number of L2 addresses allowed on a port.
- [x] It enables DAI on specific switch interfaces previously configured with DHCP snooping.
- [ ] It enables DHCP snooping globally on a switch.
- [ ] It globally enables BPDU guard on all PortFast-enabled ports.

**Answer:** It enables DAI on specific switch interfaces previously configured with DHCP snooping.

---

### Question 150

**What protocol or technology manages trunk negotiations between switches?**

**Options:**

- [ ] VTP
- [ ] EtherChannel
- [x] DTP
- [ ] STP

**Answer:** DTP

---

### Question 151

**A network administrator is configuring a WLAN. Why would the administrator apply WPA2 with AES to the WLAN?**

**Options:**

- [ ] to reduce the risk of unauthorized APs being added to the network
- [ ] to centralize management of multiple WLANs
- [ ] to provide prioritized service for time-sensitive applications
- [x] to provide privacy and integrity to wireless traffic by using encryption

**Answer:** to provide privacy and integrity to wireless traffic by using encryption

---

### Question 152

**Users on a LAN are unable to get to a company web server but are able to get elsewhere. What should be done or checked?**

**Options:**

- [ ] Ensure that the old default route has been removed from the company edge routers.
- [x] Verify that the static route to the server is present in the routing table.
- [ ] Check the configuration on the floating static route and adjust the AD.
- [ ] Create a floating static route to that network.

**Answer:** Verify that the static route to the server is present in the routing table.

---

### Question 153

**What IPv6 prefix is designed for link-local communication?**

**Options:**

- [ ] 2001::/3
- [ ] ff00::/8
- [ ] fc::/07
- [x] fe80::/10

**Answer:** fe80::/10

---

### Question 154

**What is the effect of entering the ip dhcp snooping limit rate 6 configuration command on a switch?**

**Options:**

- [ ] It displays the IP-to-MAC address associations for switch interfaces.
- [ ] It enables port security globally on the switch.
- [x] It restricts the number of discovery messages, per second, to be received on the interface.
- [ ] It dynamically learns the L2 address and copies it to the running configuration.

**Answer:** It restricts the number of discovery messages, per second, to be received on the interface.

---

### Question 155

**A network administrator is configuring a WLAN. Why would the administrator change the default DHCP IPv4 addresses on an AP?**

**Options:**

- [ ] to eliminate outsiders scanning for available SSIDs in the area
- [ ] to reduce the risk of unauthorized APs being added to the network
- [x] to reduce outsiders intercepting data or accessing the wireless network by using a well-known address range
- [ ] to reduce the risk of interference by external devices such as microwave ovens

**Answer:** to reduce outsiders intercepting data or accessing the wireless network by using a well-known address range

---

### Question 156

**What is the effect of entering the ip arp inspection validate src-mac configuration command on a switch?**

**Options:**

- [x] It checks the source L2 address in the Ethernet header against the sender L2 address in the ARP body.
- [ ] It disables all trunk ports.
- [ ] It displays the IP-to-MAC address associations for switch interfaces.
- [ ] It enables portfast on a specific switch interface.

**Answer:** It checks the source L2 address in the Ethernet header against the sender L2 address in the ARP body.

---

### Question 157

**What protocol or technology is a Cisco proprietary protocol that is automatically enabled on 2960 switches?**

**Options:**

- [x] DTP
- [ ] STP
- [ ] VTP
- [ ] EtherChannel

**Answer:** DTP

---

### Question 158

**What address and prefix length is used when configuring an IPv6 default static route?**

**Options:**

- [x] ::/0
- [ ] FF02::1/8
- [ ] 0.0.0.0/0
- [ ] ::1/128

**Answer:** ::/0

---

### Question 159

**What are two characteristics of Cisco Express Forwarding (CEF)? (Choose two.)**

**Options:**

- [ ] When a packet arrives on a router interface, it is forwarded to the control plane where the CPU matches the destination address with a matching routing table entry.
- [x] This is the fastest forwarding mechanism on Cisco routers and multilayer switches.
- [ ] With this switching method, flow information for a packet is stored in the fast-switching cache to forward future packets to the same destination without CPU intervention.
- [x] Packets are forwarded based on information in the FIB and an adjacency table.
- [ ] When a packet arrives on a router interface, it is forwarded to the control plane where the CPU searches for a match in the fast-switching cache.

**Answer:** This is the fastest forwarding mechanism on Cisco routers and multilayer switches, Packets are forwarded based on information in the FIB and an adjacency table.

---

### Question 160

**Which term describes the role of a Cisco switch in the 802.1X port-based access control?**

**Options:**

- [ ] agent
- [ ] supplicant
- [x] authenticator
- [ ] authentication server

**Answer:** authenticator

---

### Question 161

**Which Cisco solution helps prevent ARP spoofing and ARP poisoning attacks?**

**Options:**

- [x] Dynamic ARP Inspection
- [ ] IP Source Guard
- [ ] DHCP Snooping
- [ ] Port Security

**Answer:** Dynamic ARP Inspection

---

### Question 162

**What is an advantage of PVST+?**

**Options:**

- [ ] PVST+ optimizes performance on the network through autoselection of the root bridge.
- [ ] PVST+ reduces bandwidth consumption compared to traditional implementations of STP that use CST.
- [ ] PVST+ requires fewer CPU cycles for all the switches in the network.
- [x] PVST+ optimizes performance on the network through load sharing.

**Answer:** PVST+ optimizes performance on the network through load sharing.

**Explanation:** PVST+ results in optimum load balancing. However, this is accomplished by manually configuring switches to be elected as root bridges for different VLANs on the network.

---

### Question 163

**What protocol or technology uses a standby router to assume packet-forwarding responsibility if the active router fails?**

**Options:**

- [ ] EtherChannel
- [ ] DTP
- [x] HSRP
- [ ] VTP

**Answer:** HSRP

---

### Question 164

**What is the effect of entering the show ip dhcp snooping binding configuration command on a switch?**

**Options:**

- [ ] It switches a trunk port to access mode.
- [ ] It checks the source L2 address in the Ethernet header against the sender L2 address in the ARP body.
- [ ] It restricts the number of discovery messages, per second, to be received on the interface.
- [x] It displays the IP-to-MAC address associations for switch interfaces.

**Answer:** It displays the IP-to-MAC address associations for switch interfaces.

---

### Question 165

**What action takes place when the source MAC address of a frame entering a switch is in the MAC address table?**

**Options:**

- [ ] The switch forwards the frame out of the specified port.
- [x] The switch updates the refresh timer for the entry.
- [ ] The switch replaces the old entry and uses the more current port.
- [ ] The switch adds a MAC address table entry for the destination MAC address and the egress port.

**Answer:** The switch updates the refresh timer for the entry.

---

### Question 166

**A small publishing company has a network design such that when a broadcast is sent on the LAN, 200 devices receive the transmitted broadcast. How can the network administrator reduce the number of devices that receive broadcast traffic?**

**Options:**

- [ ] Add more switches so that fewer devices are on a particular switch.
- [ ] Replace the switches with switches that have more ports per switch.
- [x] Segment the LAN into smaller LANs and route between them.
- [ ] Replace at least half of the switches with hubs to reduce the size of the broadcast domain.

**Answer:** Segment the LAN into smaller LANs and route between them.

**Explanation:** By dividing the one big network into two smaller network, the network administrator has created two smaller broadcast domains.

---

### Question 167

**What defines a host route on a Cisco router?**

**Options:**

- [ ] The link-local address is added automatically to the routing table as an IPv6 host route.
- [x] An IPv4 static host route configuration uses a destination IP address of a specific device and a /32 subnet mask.
- [ ] A host route is designated with a C in the routing table.
- [ ] A static IPv6 host route must include the interface type and the interface number of the next hop router.

**Answer:** An IPv4 static host route configuration uses a destination IP address of a specific device and a /32 subnet mask.

**Explanation:** A host route is an IPv4 address with a 32-bit mask, or an IPv6 address with a 128-bit mask. A host route is marked with L in the output of the routing table.

---

### Question 168

**What else is required when configuring an IPv6 static route using a next-hop link-local address?**

**Options:**

- [ ] administrative distance
- [ ] ip address of the neighbor router
- [ ] network number and subnet mask on the interface of the neighbor router
- [x] interface number and type

**Answer:** interface number and type

---

### Question 169

**A technician is configuring a wireless network for a small business using a SOHO wireless router. Which two authentication methods are used, if the router is configured with WPA2? (Choose two.)**

**Options:**

- [x] personal
- [ ] AES
- [ ] TKIP
- [ ] WEP
- [x] enterprise

**Answer:** personal, enterprise

---

### Question 170

**Which mitigation technique would prevent rogue servers from providing false IPv6 configuration parameters to clients?**

**Options:**

- [ ] enabling DHCPv6 Guard
- [x] enabling RA Guard
- [ ] implementing port security on edge ports
- [ ] disabling CDP on edge ports

**Answer:** enabling RA Guard

**Explanation:** DHCPv6 Guard is a feature designed to ensure that rogue DHCPv6 servers are not able to hand out addresses to clients. RA Guard blocks or rejects unwanted or rogue RA guard messages that arrive at the network switch platform.

---

### Question 171

**A PC has sent an RS message to an IPv6 router attached to the same network. Which two pieces of information will the router send to the client? (Choose two.)**

**Options:**

- [x] prefix length
- [ ] subnet mask in dotted decimal notation
- [ ] domain name
- [ ] administrative distance
- [x] prefix
- [ ] DNS server IP address

**Answer:** prefix length, prefix

**Explanation:** Router is part of the IPv6 all-routers group and received the RS message. It generates an RA containing the local network prefix and prefix length (e.g., 2001:db8:acad:1::/64)

---

### Question 172

**While attending a conference, participants are using laptops for network connectivity. When a guest speaker attempts to connect to the network, the laptop fails to display any available wireless networks. The access point must be operating in which mode?**

**Options:**

- [ ] mixed
- [x] passive
- [ ] active
- [ ] open

**Answer:** passive

**Explanation:** When an access point is configured in passive mode, the SSID is not broadcast so that the name of wireless network will not appear in the listing of available networks for clients.

---

### Question 173

**Which three components are combined to form a bridge ID?**

**Options:**

- [x] extended system ID
- [ ] cost
- [ ] IP address
- [x] bridge priority
- [x] MAC address
- [ ] port ID

**Answer:** extended system ID, bridge priority, MAC address

**Explanation:** The three components that are combined to form a bridge ID are bridge priority, extended system ID, and MAC address.

---

### Question 174

**On a Cisco 3504 WLC Summary page (Advanced > Summary), which tab allows a network administrator to configure a particular WLAN with a WPA2 policy?**

**Options:**

- [ ] SECURITY
- [ ] WIRELESS
- [x] WLANs
- [ ] MANAGEMENT

**Answer:** WLANs

---

## End of Exam

**Total Questions: 174**

---

### Notes:

- Some questions include exhibits (images) referenced as `![Exhibit](images/image_XXX.png)`
- Questions marked with "Match" or "Not all options are used" indicate drag-and-drop or matching questions
- All answers are marked with [x] for easy identification
- Detailed explanations are provided where available

---

**Good luck with your CCNA exam preparation!**