# CCNA 2 Quiz

> คำถามสำหรับการสอบ CCNA 2

---

## Question 1

**Refer to the exhibit. What will router R1 do with a packet that has a destination IPv6 address of 2001:db8:cafe:5::1?**

![Question 1 Exhibit](images/image_1.png)


**Options:**

- forward the packet out GigabitEthernet0/0
- drop the packet
- forward the packet out GigabitEthernet0/1
- forward the packet out Serial0/0/0

---

## Question 2

**Refer to the exhibit. Currently router R1 uses an EIGRP route learned from Branch2 to reach the 10.10.0.0/16 network. Which floating static route would create a backup route to the 10.10.0.0/16 network in the event that the link between R1 and Branch2 goes down?**

![Question 2 Exhibit](images/image_2.png)


**Options:**

- ip route 10.10.0.0 255.255.0.0 Serial 0/0/0 100
- ip route 10.10.0.0 255.255.0.0 209.165.200.226 100
- ip route 10.10.0.0 255.255.0.0 209.165.200.225 100
- ip route 10.10.0.0 255.255.0.0 209.165.200.225 50

---

## Question 3

**Refer to the exhibit. R1 was configured with the static route command ip route 209.165.200.224 255.255.255.224 S0/0/0 and consequently users on network 172.16.0.0/16 are unable to reach resources on the Internet. How should this static route be changed to allow user traffic from the LAN to reach the Internet?**

![Question 3 Exhibit](images/image_3.png)


**Options:**

- Add an administrative distance of 254.
- Change the destination network and mask to 0.0.0.0 0.0.0.0
- Change the exit interface to S0/0/1.
- Add the next-hop neighbor address of 209.165.200.226.

---

## Question 4

Which option shows a correctly configured IPv4 default static route?


**Options:**

- ip route 0.0.0.0 255.255.255.0 S0/0/0
- ip route 0.0.0.0 0.0.0.0 S0/0/0
- ip route 0.0.0.0 255.255.255.255 S0/0/0
- ip route 0.0.0.0 255.0.0.0 S0/0/0

---

## Question 5

**Refer to the exhibit. Which static route command can be entered on R1 to forward traffic to the LAN connected to R2?**

![Question 5 Exhibit](images/image_4.png)


**Options:**

- ipv6 route 2001:db8:12:10::/64 S0/0/0
- ipv6 route 2001:db8:12:10::/64 S0/0/1 fe80::2
- ipv6 route 2001:db8:12:10::/64 S0/0/0 fe80::2
- ipv6 route 2001:db8:12:10::/64 S0/0/1 2001:db8:12:10::1

---

## Question 6

What is a method to launch a VLAN hopping attack?


**Options:**

- introducing a rogue switch and enabling trunking
- sending spoofed native VLAN information
- sending spoofed IP addresses from the attacking host
- flooding the switch with MAC addresses

---

## Question 7

A cybersecurity analyst is using the macof tool to evaluate configurations of switches deployed in the backbone network of an organization. Which type of LAN attack is the analyst targeting during this evaluation?


**Options:**

- VLAN hopping
- DHCP spoofing
- MAC address table overflow
- VLAN double-tagging

---

## Question 8

**Refer to the exhibit. A network administrator is configuring a router as a DHCPv6 server. The administrator issues a show ipv6 dhcp pool command to verify the configuration. Which statement explains the reason that the number of active clients is 0?**

![Question 8 Exhibit](images/image_5.png)


**Options:**

- The default gateway address is not provided in the pool.
- No clients have communicated with the DHCPv6 server yet.
- The IPv6 DHCP pool configuration has no IPv6 address range specified.
- The state is not maintained by the DHCPv6 server under stateless DHCPv6 operation.

---

## Question 9

**Refer to the exhibit. A network administrator configured routers R1 and R2 as part of HSRP group 1. After the routers have been reloaded, a user on Host1 complained of lack of connectivity to the Internet The network administrator issued the show standby brief command on both routers to verify the HSRP operations. In addition, the administrator observed the ARP table on Host1. Which entry should be seen in the ARP table on Host1 in order to gain connectivity to the Internet?**

![Question 9 Exhibit](images/image_6.png)


**Options:**

- the virtual IP address and the virtual MAC address for the HSRP group 1
- the virtual IP address of the HSRP group 1 and the MAC address of R1
- the virtual IP address of the HSRP group 1 and the MAC address of R2

---

## Question 10

Match the forwarding characteristic to its type. (Not all options are used.)

## Question 11

Which statement is correct about how a Layer 2 switch determines how to forward frames?


**Options:**

- Frame forwarding decisions are based on MAC address and port mappings in the CAM table.
- Only frames with a broadcast destination address are forwarded out all active switch ports.
- Cut-through frame forwarding ensures that invalid frames are always dropped.
- Unicast frames are always forwarded regardless of the destination MAC address.

---

## Question 12

Which statement describes a result after multiple Cisco LAN switches are interconnected?


**Options:**

- The broadcast domain expands to all switches.
- One collision domain exists per switch.
- There is one broadcast domain and one collision domain per switch.
- Frame collisions increase on the segments connecting the switches.
- Unicast frames are always forwarded regardless of the destination MAC address.

---

## Question 13

Match the link state to the interface and protocol status. (Not all options are used.)

## Question 14

**Refer to the exhibit. How is a frame sent from PCA forwarded to PCC if the MAC address table on switch SW1 is empty?**

![Question 14 Exhibit](images/image_7.png)

SW1 forwards the frame directly to SW2. SW2 floods the frame to all ports connected to SW2, excluding the port through which the frame entered the switch.

SW1 floods the frame on all ports on the switch, excluding the interconnected port to switch SW2 and the port through which the frame entered the switch.


**Options:**

- SW1 floods the frame on all ports on SW1, excluding the port through which the frame entered the switch.
- SW1 drops the frame because it does not know the destination MAC address.

---

## Question 15

An administrator is trying to remove configurations from a switch. After using the command erase startup-config and reloading the switch, the administrator finds that VLANs 10 and 100 still exist on the switch. Why were these VLANs not removed?


**Options:**

- Because these VLANs are stored in a file that is called vlan.dat that is located in flash memory, this file must be manually deleted.
- These VLANs cannot be deleted unless the switch is in VTP client mode.
- These VLANs are default VLANs that cannot be removed.
- These VLANs can only be removed from the switch by using the no vlan 10 and no vlan 100 commands.

---

## Question 16

Match the description to the correct VLAN type. (Not all options are used.)

## Question 17

**Refer to the exhibit. A network administrator has connected two switches together using EtherChannel technology. If STP is running, what will be the end result?**

![Question 17 Exhibit](images/image_8.png)


**Options:**

- STP will block one of the redundant links.
- The switches will load balance and utilize both EtherChannels to forward packets.
- The resulting loop will create a broadcast storm.
- Both port channels will shutdown.

---

## Question 18

What is a secure configuration option for remote access to a network device?


**Options:**

- Configure an ACL and apply it to the VTY lines.
- Configure 802.1x.
- Configure SSH.
- Configure Telnet.

---

## Question 19

Which wireless encryption method is the most secure?


**Options:**

- WPA2 with AES
- WPA2 with TKIP
- WEP
- WPA

---

## Question 20

After attaching four PCs to the switch ports, configuring the SSID and setting authentication properties for a small office network, a technician successfully tests the connectivity of all PCs that are connected to the switch and WLAN. A firewall is then configured on the device prior to connecting it to the Internet. What type of network device includes all of the described features?


**Options:**

- firewall appliance
- wireless router
- switch
- standalone wireless access point

---

## Question 21

**Refer to the exhibit. Which static route would an IT technician enter to create a backup route to the 172.16.1.0 network that is only used if the primary RIP learned route fails?**

![Question 21 Exhibit](images/image_9.png)


**Options:**

- ip route 172.16.1.0 255.255.255.0 s0/0/0
- ip route 172.16.1.0 255.255.255.0 s0/0/0 91
- ip route 172.16.1.0 255.255.255.0 s0/0/0 121
- ip route 172.16.1.0 255.255.255.0 s0/0/0 111

---

## Question 22

**Refer to the exhibit. Host A has sent a packet to host B. What will be the source MAC and IP addresses on the packet when it arrives at host B?**

![Question 22 Exhibit](images/image_10.png)


**Options:**

- Source MAC: 00E0.FE91.7799 Source IP: 10.1.1.10
- Source MAC: 00E0.FE10.17A3 Source IP: 10.1.1.10
- Source MAC: 00E0.FE10.17A3 Source IP: 192.168.1.1
- Source MAC: 00E0.FE91.7799 Source IP: 10.1.1.1
- Source MAC: 00E0.FE91.7799 Source IP: 192.168.1.1

---

## Question 23

**Refer to the exhibit. In addition to static routes directing traffic to networks 10.10.0.0/16 and 10.20.0.0/16, Router HQ is also configured with the following command:**

What is the purpose of this command?

![Question 23 Image](images/image_11.png)

Packets with a destination network that is not 10.10.0.0/16 or is not 10.20.0.0/16 or is not a directly connected network will be forwarded to the Internet.

Packets from the 10.10.0.0/16 network will be forwarded to network 10.20.0.0/16, and packets from the 10.20.0.0/16 network will be forwarded to network 10.10.0.0/16.


**Options:**

- ip route 0.0.0.0 0.0.0.0 serial 0/1/1
- Packets that are received from the Internet will be forwarded to one of the LANs connected to R1 or R2.
- Packets that are destined for networks that are not in the routing table of HQ will be dropped.

---

## Question 24

What protocol or technology disables redundant paths to eliminate Layer 2 loops?


**Options:**

- VTP
- STP
- EtherChannel
- DTP

---

## Question 25

Which two VTP modes allow for the creation, modification, and deletion of VLANs on the local switch? (Choose two.)


**Options:**

- client
- master
- distribution
- slave
- server
- transparent

---

## Question 26

**Refer to the exhibit. Based on the exhibited configuration and output, why is VLAN 99 missing?**

![Question 26 Exhibit](images/image_12.png)


**Options:**

- because VLAN 99 is not a valid management VLAN
- because there is a cabling problem on VLAN 99
- because VLAN 1 is up and there can only be one management VLAN on the switch
- because VLAN 99 has not yet been created

---

## Question 27

Which three steps should be taken before moving a Cisco switch to a new VTP management domain? (Choose three.)


**Options:**

- Configure the switch with the name of the new management domain.
- Reset the VTP counters to allow the switch to synchronize with the other switches in the domain.
- Configure the VTP server in the domain to recognize the BID of the new switch.
- Download the VTP database from the VTP server in the new domain.
- Select the correct VTP mode and version.
- Reboot the switch.

---

## Question 28

A network administrator is preparing the implementation of Rapid PVST+ on a production network. How are the Rapid PVST+ link types determined on the switch interfaces?


**Options:**

- Link types can only be configured on access ports configured with a single VLAN.
- Link types can only be determined if PortFast has been configured.
- Link types are determined automatically.
- Link types must be configured with specific port configuration commands.

---

## Question 29

**Refer to the exhibit. All the displayed switches are Cisco 2960 switches with the same default priority and operating at the same bandwidth. Which three ports will be STP designated ports? (Choose three.)**

![Question 29 Exhibit](images/image_13.png)


**Options:**

- fa0/9
- fa0/13
- fa0/10
- fa0/20
- fa0/21
- fa0/11

---

## Question 30

How will a router handle static routing differently if Cisco Express Forwarding is disabled?


**Options:**

- It will not perform recursive lookups.
- Ethernet multiaccess interfaces will require fully specified static routes to avoid routing inconsistencies.
- Static routes that use an exit interface will be unnecessary.
- Serial point-to-point interfaces will require fully specified static routes to avoid routing inconsistencies.

---

## Question 31

**Refer to the exhibit. Which route was configured as a static route to a specific network using the next-hop address?**

![Question 31 Exhibit](images/image_14.png)


**Options:**

- S 10.17.2.0/24 [1/0] via 10.16.2.2
- S 0.0.0.0/0 [1/0] via 10.16.2.2
- S 10.17.2.0/24 is directly connected, Serial 0/0/0
- C 10.16.2.0/24 is directly connected, Serial0/0/0

---

## Question 32

Compared with dynamic routes, what are two advantages of using static routes on a router? (Choose two.)


**Options:**

- They improve netw​ork security.
- They take less time to converge when the network topology changes.
- They improve the efficiency of discovering neighboring networks.
- They use fewer router resources.

---

## Question 33

What is the IPv6 prefix that is used for link-local addresses?


**Options:**

- FF01::/8
- 2001::/3
- FC00::/7
- FE80::/10

---

## Question 34

What is the effect of entering the spanning-tree portfast configuration command on a switch?


**Options:**

- It disables an unused port.
- It disables all trunk ports.
- It enables portfast on a specific switch interface.
- It checks the source L2 address in the Ethernet header against the sender L2 address in the ARP body.

---

## Question 35

Which two statements are characteristics of routed ports on a multilayer switch? (Choose two.)​


**Options:**

- In a switched network, they are mostly configured between switches at the core and distribution layers.
- The interface vlan command has to be entered to create a VLAN on routed ports.
- They support subinterfaces, like interfaces on the Cisco IOS routers.
- They are used for point-to-multipoint links.
- They are not associated with a particular VLAN.

---

## Question 36

Successful inter-VLAN routing has been operating on a network with multiple VLANs across multiple switches for some time. When an inter-switch trunk link fails and Spanning Tree Protocol brings up a backup trunk link, it is reported that hosts on two VLANs can access some, but not all the network resources that could be accessed previously. Hosts on all other VLANS do not have this problem. What is the most likely cause of this problem?


**Options:**

- The protected edge port function on the backup trunk interfaces has been disabled.
- The allowed VLANs on the backup link were not configured correctly.
- Dynamic Trunking Protocol on the link has failed.
- Inter-VLAN routing also failed when the trunk link failed.

---

## Question 37

Which command will start the process to bundle two physical interfaces to create an EtherChannel group via LACP?


**Options:**

- interface port-channel 2
- channel-group 1 mode desirable
- interface range GigabitEthernet 0/4 – 5
- channel-group 2 mode auto

---

## Question 38

What action takes place when a frame entering a switch has a multicast destination MAC address?


**Options:**

- The switch will forward the frame out all ports except the incoming port.
- The switch forwards the frame out of the specified port.
- The switch adds a MAC address table entry mapping for the destination MAC address and the ingress port.
- The switch replaces the old entry and uses the more current port.

---

## Question 39

A junior technician was adding a route to a LAN router. A traceroute to a device on the new network revealed a wrong path and unreachable status. What should be done or checked?


**Options:**

- Verify that there is not a default route in any of the edge router routing tables.
- Check the configuration on the floating static route and adjust the AD.
- Create a floating static route to that network.
- Check the configuration of the exit interface on the new static route.

---

## Question 40

Select the three PAgP channel establishment modes. (Choose three.)


**Options:**

- auto
- default
- passive
- desirable
- extended
- on

---

## Question 41

A static route has been configured on a router. However, the destination network no longer exists. What should an administrator do to remove the static route from the routing table?


**Options:**

- Remove the route using the no ip route command.
- Change the administrative distance for that route.
- Change the routing metric for that route.
- Nothing. The static route will go away on its own.

---

## Question 42

**Refer to the exhibit. What can be concluded about the configuration shown on R1?**

![Question 42 Exhibit](images/image_15.png)


**Options:**

- R1 is configured as a DHCPv4 relay agent.
- R1 is operating as a DHCPv4 server.
- R1 will broadcast DHCPv4 requests on behalf of local DHCPv4 clients.
- R1 will send a message to a local DHCPv4 client to contact a DHCPv4 server at 10.10.10.8.

---

## Question 43

Match the step to each switch boot sequence description. (Not all options are used.)

## Question 44

**Refer to the exhibit. R1 has been configured as shown. However, PC1 is not able to receive an IPv4 address. What is the problem?​**

![Question 44 Exhibit](images/image_16.png)


**Options:**

- The ip helper-address command was applied on the wrong interface.
- R1 is not configured as a DHCPv4 server.​
- A DHCP server must be installed on the same LAN as the host that is receiving the IP address.
- The ip address dhcp command was not issued on the interface Gi0/1.

---

## Question 45

What two default wireless router settings can affect network security? (Choose two.)

![Question 45 Image](images/image_17.png)


**Options:**

- The SSID is broadcast.
- MAC address filtering is enabled.
- WEP encryption is enabled.
- The wireless channel is automatically selected.
- A well-known administrator password is set.

---

## Question 46

What is the common term given to SNMP log messages that are generated by network devices and sent to the SNMP server?


**Options:**

- traps
- acknowledgments
- auditing
- warnings

---

## Question 47

A network administrator is adding a new WLAN on a Cisco 3500 series WLC. Which tab should the administrator use to create a new VLAN interface to be used for the new WLAN?


**Options:**

- WIRELESS
- MANAGEMENT
- CONTROLLER
- WLANs

---

## Question 48

A network administrator is configuring a WLAN. Why would the administrator change the default DHCP IPv4 addresses on an AP?


**Options:**

- to restrict access to the WLAN by authorized, authenticated users only
- to monitor the operation of the wireless network
- to reduce outsiders intercepting data or accessing the wireless network by using a well-known address range
- to reduce the risk of interference by external devices such as microwave ovens

---

## Question 49

Which two functions are performed by a WLC when using split media access control (MAC)? (Choose two.)


**Options:**

- packet acknowledgments and retransmissions
- frame queuing and packet prioritization
- beacons and probe responses
- frame translation to other protocols
- association and re-association of roaming clients

---

## Question 50

On what switch ports should BPDU guard be enabled to enhance STP stability?


**Options:**

- all PortFast-enabled ports
- only ports that are elected as designated ports
- only ports that attach to a neighboring switch
- all trunk ports that are not root ports

---

## Question 51

Which network attack is mitigated by enabling BPDU guard?


**Options:**

- rogue switches on a network
- CAM table overflow attacks
- MAC address spoofing
- rogue DHCP servers on a network

---

## Question 52

Why is DHCP snooping required when using the Dynamic ARP Inspection feature?


**Options:**

- It relies on the settings of trusted and untrusted ports set by DHCP snooping.
- It uses the MAC address table to verify the default gateway IP address.
- It redirects ARP requests to the DHCP server for verification.
- It uses the MAC-address-to-IP-address binding database to validate an ARP packet.

---

## Question 53

**Refer to the exhibit. Router R1 has an OSPF neighbor relationship with the ISP router over the 192.168.0.32 network. The 192.168.0.36 network link should serve as a backup when the OSPF link goes down. The floating static route command ip route 0.0.0.0 0.0.0.0 S0/0/1 100 was issued on R1 and now traffic is using the backup link even when the OSPF link is up and functioning. Which change should be made to the static route command so that traffic will only use the OSPF link when it is up?​**

![Question 53 Exhibit](images/image_18.png)


**Options:**

- Change the administrative distance to 120.
- Add the next hop neighbor address of 192.168.0.36.
- Change the destination network to 192.168.0.34.
- Change the administrative distance to 1.

---

## Question 54

**Refer to the exhibit. What is the metric to forward a data packet with the IPv6 destination address 2001:DB8:ACAD:E:240:BFF:FED4:9DD2?**

![Question 54 Exhibit](images/image_19.png)


**Options:**

- 90
- 128
- 2170112
- 2681856
- 2682112
- 3193856

---

## Question 55

A network administrator is configuring a new Cisco switch for remote management access. Which three items must be configured on the switch for the task? (Choose three.)


**Options:**

- IP address
- VTP domain
- vty lines
- default VLAN
- default gateway
- loopback address

---

## Question 56

**Refer to the exhibit. Which statement shown in the output allows router R1 to respond to stateless DHCPv6 requests?**

![Question 56 Exhibit](images/image_20.png)


**Options:**

- ipv6 nd other-config-flag​
- prefix-delegation 2001:DB8:8::/48 00030001000E84244E70​
- ipv6 dhcp server LAN1​
- ipv6 unicast-routing
- dns-server 2001:DB8:8::8​

---

## Question 57

**Refer to the exhibit. A Layer 3 switch routes for three VLANs and connects to a router for Internet connectivity. Which two configurations would be applied to the switch? (Choose two.)**

![Question 57 Exhibit](images/image_21.png)


**Options:**

- (config)# interface gigabitethernet1/1
(config-if)# switchport mode trunk
- (config)# interface gigabitethernet 1/1
(config-if)# no switchport
(config-if)# ip address 192.168.1.2 255.255.255.252
- (config)# interface vlan 1
(config-if)# ip address 192.168.1.2 255.255.255.0
(config-if)# no shutdown
- (config)# interface fastethernet0/4
(config-if)# switchport mode trunk
- (config)# ip routing

---

## Question 58

A technician is troubleshooting a slow WLAN and decides to use the split-the-traffic approach. Which two parameters would have to be configured to do this? (Choose two.)


**Options:**

- Configure the 5 GHz band for streaming multimedia and time sensitive traffic.
- Configure the security mode to WPA Personal TKIP/AES for one network and WPA2 Personal AES for the other network
- Configure the 2.4 GHz band for basic internet traffic that is not time sensitive.
- Configure the security mode to WPA Personal TKIP/AES for both networks.
- Configure a common SSID for both split networks.

---

## Question 59

A company has just switched to a new ISP. The ISP has completed and checked the connection from its site to the company. However, employees at the company are not able to access the internet. What should be done or checked?


**Options:**

- Verify that the static route to the server is present in the routing table.
- Check the configuration on the floating static route and adjust the AD.
- Ensure that the old default route has been removed from the company edge routers.
- Create a floating static route to that network.

---

## Question 60

Which information does a switch use to populate the MAC address table?


**Options:**

- the destination MAC address and the incoming port
- the destination MAC address and the outgoing port
- the source and destination MAC addresses and the incoming port
- the source and destination MAC addresses and the outgoing port
- the source MAC address and the incoming port
- the source MAC address and the outgoing port

---

## Question 61

**Refer to the exhibit. A network administrator is reviewing the configuration of switch S1. Which protocol has been implemented to group multiple physical ports into one logical link?**

![Question 61 Exhibit](images/image_22.png)


**Options:**

- PAgP
- DTP
- LACP
- STP

---

## Question 62

Which type of static route is configured with a greater administrative distance to provide a backup route to a route learned from a dynamic routing protocol?


**Options:**

- floating static route
- default static route
- summary static route
- standard static route

---

## Question 63

What action takes place when a frame entering a switch has a unicast destination MAC address appearing in the MAC address table?


**Options:**

- The switch updates the refresh timer for the entry.
- The switch forwards the frame out of the specified port.
- The switch purges the entire MAC address table.
- The switch replaces the old entry and uses the more current port.

---

## Question 64

The exhibit shows two PCs called PC A and PC B, two routes called R1 and R2, and two switches. PC A has the address 172.16.1.1/24 and is connected to a switch and into an interface on R1 that has the IP address 172.16.1.254. PC B has the address 172.16.2.1/24 and is connected to a switch that is connected to another interface on R1 with the IP address 172.16.2.254. The serial interface on R1 has the address 172.16.3.1 and is connected to the serial interface on R2 that has the address 172.16.3.2/24. R2 is connected to the internet cloud. Which command will create a static route on R2 in order to reach PC B?

![Question 64 Image](images/image_23.png)


**Options:**

- R2(config)# ip route 172.16.2.1 255.255.255.0 172.16.3.1
- R2(config)# ip route 172.16.2.0 255.255.255.0 172.16.2.254
- R2(config)# ip route 172.16.2.0 255.255.255.0 172.16.3.1
- R2(config)# ip route 172.16.3.0 255.255.255.0 172.16.2.254

---

## Question 65

What protocol or technology allows data to transmit over redundant switch links?


**Options:**

- EtherChannel
- DTP
- STP
- VTP

---

## Question 66

**Refer to the exhibit. Which three hosts will receive ARP requests from host A, assuming that port Fa0/4 on both switches is configured to carry traffic for multiple VLANs? (Choose three.)**

![Question 66 Exhibit](images/image_24.png)


**Options:**

- host B
- host C
- host D
- host E
- host F
- host G

---

## Question 67

**Refer to the exhibit. The network administrator configures both switches as displayed. However, host C is unable to ping host D and host E is unable to ping host F. What action should the administrator take to enable this communication?**

![Question 67 Exhibit](images/image_25.png)


**Options:**

- Associate hosts A and B with VLAN 10 instead of VLAN 1.
- Configure either trunk port in the dynamic desirable mode.
- Include a router in the topology.
- Remove the native VLAN from the trunk.
- Add the switchport nonegotiate command to the configuration of SW2.

---

## Question 68

What is the effect of entering the shutdown configuration command on a switch?


**Options:**

- It enables BPDU guard on a specific port.
- It disables an unused port.
- It enables portfast on a specific switch interface.
- It disables DTP on a non-trunking interface.

---

## Question 69

What would be the primary reason an attacker would launch a MAC address overflow attack?


**Options:**

- so that the switch stops forwarding traffic
- so that legitimate hosts cannot obtain a MAC address
- so that the attacker can see frames that are destined for other hosts
- so that the attacker can execute arbitrary code on the switch

---

## Question 70

During the AAA process, when will authorization be implemented?


**Options:**

- Immediately after successful authentication against an AAA data source
- Immediately after AAA accounting and auditing receives detailed reports
- Immediately after an AAA client sends authentication information to a centralized server
- Immediately after the determination of which resources a user can access

---

## Question 71

A company security policy requires that all MAC addressing be dynamically learned and added to both the MAC address table and the running configuration on each switch. Which port security configuration will accomplish this?


**Options:**

- auto secure MAC addresses
- dynamic secure MAC addresses
- static secure MAC addresses
- sticky secure MAC addresses

---

## Question 72

Which three Wi-Fi standards operate in the 2.4GHz range of frequencies? (Choose three.)


**Options:**

- 802.11a
- 802.11b
- 802.11g
- 802.11n
- 802.11ac

---

## Question 73

To obtain an overview of the spanning tree status of a switched network, a network engineer issues the show spanning-tree command on a switch. Which two items of information will this command display? (Choose two.)


**Options:**

- The root bridge BID.
- The role of the ports in all VLANs.
- The status of native VLAN ports.
- The number of broadcasts received on each root port.
- The IP address of the management VLAN interface.

---

## Question 74

**Refer to the exhibit. Which trunk link will not forward any traffic after the root bridge election process is complete?**

![Question 74 Exhibit](images/image_26.png)


**Options:**

- Trunk1
- Trunk2
- Trunk3
- Trunk4

---

## Question 75

Which method of IPv6 prefix assignment relies on the prefix contained in RA messages?


**Options:**

- EUI-64
- SLAAC
- static
- stateful DHCPv6

---

## Question 76

Which two protocols are used to provide server-based AAA authentication? (Choose two.)


**Options:**

- 802.1x
- SSH
- SNMP
- TACACS+
- RADIUS

---

## Question 77

A network administrator is configuring a WLAN. Why would the administrator disable the broadcast feature for the SSID?


**Options:**

- to eliminate outsiders scanning for available SSIDs in the area
- to reduce the risk of interference by external devices such as microwave ovens
- to reduce the risk of unauthorized APs being added to the network
- to provide privacy and integrity to wireless traffic by using encryption

---

## Question 78

Which mitigation technique would prevent rogue servers from providing false IP configuration parameters to clients?


**Options:**

- implementing port security
- turning on DHCP snooping
- disabling CDP on edge ports
- implementing port-security on edge ports

---

## Question 79

A network administrator configures the port security feature on a switch. The security policy specifies that each access port should allow up to two MAC addresses. When the maximum number of MAC addresses is reached, a frame with the unknown source MAC address is dropped and a notification is sent to the syslog server. Which security violation mode should be configured for each access port?


**Options:**

- shutdown
- restrict
- warning
- protect

---

## Question 80

What protocol or technology defines a group of routers, one of them defined as active and another one as standby?


**Options:**

- EtherChannel
- VTP
- HSRP
- DTP

---

## Question 81

**Refer to the exhibit. After attempting to enter the configuration that is shown in router RTA, an administrator receives an error and users on VLAN 20 report that they are unable to reach users on VLAN 30. What is causing the problem?**

![Question 81 Exhibit](images/image_27.png)


**Options:**

- There is no address on Fa0/0 to use as a default gateway.
- RTA is using the same subnet for VLAN 20 and VLAN 30.
- Dot1q does not support subinterfaces.
- The no shutdown command should have been issued on Fa0/0.20 and Fa0/0.30.

---

## Question 82

Which three pairs of trunking modes will establish a functional trunk link between two Cisco switches? (Choose three.)


**Options:**

- dynamic desirable – dynamic desirable
- dynamic desirable – trunk
- dynamic auto – dynamic auto
- access – dynamic auto
- dynamic desirable – dynamic auto
- access – trunk

---

## Question 83

A technician is configuring a router for a small company with multiple WLANs and doesn’t need the complexity of a dynamic routing protocol. What should be done or checked?


**Options:**

- Verify that there is not a default route in any of the edge router routing tables.
- Create static routes to all internal networks and a default route to the internet.
- Create extra static routes to the same location with an AD of 1.
- Check the statistics on the default route for oversaturation.

---

## Question 84

A company is deploying a wireless network in the distribution facility in a Boston suburb. The warehouse is quite large and it requires multiple access points to be used. Because some of the company devices still operate at 2.4GHz, the network administrator decides to deploy the 802.11g standard. Which channel assignments on the multiple access points will make sure that the wireless channels are not overlapping?


**Options:**

- channels 1, 5, and 9
- channels 1, 6, and 11
- channels 1, 7, and 13
- channels 2, 6, and 10

---

## Question 85

A network administrator of a small advertising company is configuring WLAN security by using the WPA2 PSK method. Which credential do office users need in order to connect their laptops to the WLAN?


**Options:**

- the company username and password through Active Directory service
- a key that matches the key on the AP
- a user passphrase
- a username and password configured on the AP

---

## Question 86

**Refer to the exhibit. What are the possible port roles for ports A, B, C, and D in this RSTP-enabled network?**

![Question 86 Exhibit](images/image_28.png)


**Options:**

- alternate, designated, root, root
- designated, alternate, root, root
- alternate, root, designated, root
- designated, root, alternate, root

---

## Question 87

**Refer to the exhibit. Which static route would an IT technician enter to create a backup route to the 172.16.1.0 network that is only used if the primary RIP learned route fails?**

![Question 87 Exhibit](images/image_29.png)


**Options:**

- ip route 172.16.1.0 255.255.255.0 s0/0/0
- ip route 172.16.1.0 255.255.255.0 s0/0/0 121
- ip route 172.16.1.0 255.255.255.0 s0/0/0 111
- ip route 172.16.1.0 255.255.255.0 s0/0/0 91

---

## Question 88

What mitigation plan is best for thwarting a DoS attack that is creating a MAC address table overflow?


**Options:**

- Disable DTP.
- Disable STP.
- Enable port security.
- Place unused ports in an unused VLAN.

---

## Question 89

A network engineer is troubleshooting a newly deployed wireless network that is using the latest 802.11 standards. When users access high bandwidth services such as streaming video, the wireless network performance is poor. To improve performance the network engineer decides to configure a 5 Ghz frequency band SSID and train users to use that SSID for streaming media services. Why might this solution improve the wireless network performance for that type of service?


**Options:**

- Requiring the users to switch to the 5 GHz band for streaming media is inconvenient and will result in fewer users accessing these services.
- The 5 GHz band has more channels and is less crowded than the 2.4 GHz band, which makes it more suited to streaming multimedia.
- The 5 GHz band has a greater range and is therefore likely to be interference-free.
- The only users that can switch to the 5 GHz band will be those with the latest wireless NICs, which will reduce usage.

---

## Question 90

Which DHCPv4 message will a client send to accept an IPv4 address that is offered by a DHCP server?


**Options:**

- broadcast DHCPACK
- broadcast DHCPREQUEST
- unicast DHCPACK
- unicast DHCPREQUEST

---

## Question 91

**Refer to the exhibit. Which destination MAC address is used when frames are sent from the workstation to the default gateway?**

![Question 91 Exhibit](images/image_30.png)


**Options:**

- MAC address of the virtual router
- MAC address of the standby router
- MAC addresses of both the forwarding and standby routers
- MAC address of the forwarding router

---

## Question 92

After a host has generated an IPv6 address by using the DHCPv6 or SLAAC process, how does the host verify that the address is unique and therefore usable?

The host sends an ICMPv6 neighbor solicitation message to the DHCP or SLAAC-learned address and if no neighbor advertisement is returned, the address is considered unique.


**Options:**

- The host sends an ICMPv6 echo request message to the DHCPv6 or SLAAC-learned address and if no reply is returned, the address is considered unique.
- The host checks the local neighbor cache for the learned address and if the address is not cached, it it considered unique.
- The host sends an ARP broadcast to the local link and if no hosts send a reply, the address is considered unique.

---

## Question 93

Match the purpose with its DHCP message type. (Not all options are used.)

## Question 94

Which protocol adds security to remote connections?


**Options:**

- FTP
- HTTP
- NetBEUI
- POP
- SSH

---

## Question 95

**Refer to the exhibit. A network administrator is verifying the configuration of inter-VLAN routing. Users complain that PC2 cannot communicate with PC1. Based on the output, what is the possible cause of the problem?**

![Question 95 Exhibit](images/image_31.png)


**Options:**

- Gi0/0 is not configured as a trunk port.
- The command interface GigabitEthernet0/0.5 was entered incorrectly.
- There is no IP address configured on the interface Gi0/0.
- The no shutdown command is not entered on subinterfaces.
- The encapsulation dot1Q 5 command contains the wrong VLAN.

---

## Question 96

**Refer to the exhibit. A network administrator is configuring inter-VLAN routing on a network. For now, only one VLAN is being used, but more will be added soon. What is the missing parameter that is shown as the highlighted question mark in the graphic?**

![Question 96 Exhibit](images/image_32.png)


**Options:**

- It identifies the subinterface.
- It identifies the VLAN number.
- It identifies the native VLAN number.
- It identifies the type of encapsulation that is used.
- It identifies the number of hosts that are allowed on the interface.

---

## Question 97

Match each DHCP message type with its description. (Not all options are used.)

## Question 98

What network attack seeks to create a DoS for clients by preventing them from being able to obtain a DHCP lease?


**Options:**

- IP address spoofing
- DHCP starvation
- CAM table attack
- DHCP spoofing

---

## Question 99

**Refer to the exhibit. If the IP addresses of the default gateway router and the DNS server are correct, what is the configuration problem?**

![Question 99 Exhibit](images/image_33.png)


**Options:**

- The DNS server and the default gateway router should be in the same subnet.
- The IP address of the default gateway router is not contained in the excluded address list.
- The default-router and dns-server commands need to be configured with subnet masks.
- The IP address of the DNS server is not contained in the excluded address list.

---

## Question 100

**Refer to the exhibit. A network administrator has added a new subnet to the network and needs hosts on that subnet to receive IPv4 addresses from the DHCPv4 server. What two commands will allow hosts on the new subnet to receive addresses from the DHCP4 server? (Choose two.)**

![Question 100 Exhibit](images/image_34.png)


**Options:**

- R1(config-if)# ip helper-address 10.2.0.250
- R1(config)# interface G0/1
- R1(config)# interface G0/0
- R2(config-if)# ip helper-address 10.2.0.250
- R2(config)# interface G0/0
- R1(config-if)# ip helper-address 10.1.0.254

---

## Question 101

What protocol or technology uses source IP to destination IP as a load-balancing mechanism?


**Options:**

- VTP
- EtherChannel
- DTP
- STP

---

## Question 102

What protocol should be disabled to help mitigate VLAN attacks?


**Options:**

- CDP
- ARP
- STP
- DTP

---

## Question 103

What protocol or technology requires switches to be in server mode or client mode?


**Options:**

- EtherChannel
- STP
- VTP
- DTP

---

## Question 104

What are two reasons a network administrator would segment a network with a Layer 2 switch? (Choose two.)


**Options:**

- to create fewer collision domains
- to enhance user bandwidth
- to create more broadcast domains
- to eliminate virtual circuits
- to isolate traffic between segments
- to isolate ARP request messages from the rest of the network

---

## Question 105

What command will enable a router to begin sending messages that allow it to configure a link-local address without using an IPv6 DHCP server?


**Options:**

- a static route
- the ipv6 route ::/0 command
- the ipv6 unicast-routing command
- the ip routing command

---

## Question 106

A network administrator is using the router-on-a-stick model to configure a switch and a router for inter-VLAN routing. What configuration should be made on the switch port that connects to the router?


**Options:**

- Configure it as a trunk port and allow only untagged traffic.
- Configure the port as an access port and a member of VLAN1.
- Configure the port as an 802.1q trunk port.
- Configure the port as a trunk port and assign it to VLAN1.

---

## Question 107

What are three techniques for mitigating VLAN attacks? (Choose three.)


**Options:**

- Use private VLANs.
- Enable BPDU guard.
- Enable trunking manually
- Enable Source Guard.
- Disable DTP.
- Set the native VLAN to an unused VLAN.

---

## Question 108

Match the DHCP message types to the order of the DHCPv4 process. (Not all options are used.)

## Question 109

In which situation would a technician use the show interfaces switch command?


**Options:**

- to determine if remote access is enabled
- when packets are being dropped from a particular directly attached host
- when an end device can reach local devices, but not remote devices
- to determine the MAC address of a directly attached network device on a particular interface

---

## Question 110

What is a drawback of the local database method of securing device access that can be solved by using AAA with centralized servers?


**Options:**

- There is no ability to provide accountability.
- User accounts must be configured locally on each device, which is an unscalable authentication solution.
- It is very susceptible to brute-force attacks because there is no username.
- The passwords can only be stored in plain text in the running configuration.

---

## Question 111

What action does a DHCPv4 client take if it receives more than one DHCPOFFER from multiple DHCP servers?


**Options:**

- It sends a DHCPREQUEST that identifies which lease offer the client is accepting.
- It sends a DHCPNAK and begins the DHCP process over again.
- It discards both offers and sends a new DHCPDISCOVER.
- It accepts both DHCPOFFER messages and sends a DHCPACK.

---

## Question 112

**Refer to the exhibit. The network administrator is configuring the port security feature on switch SWC. The administrator issued the command show port-security interface fa 0/2 to verify the configuration. What can be concluded from the output that is shown? (Choose three.)**

![Question 112 Exhibit](images/image_35.png)


**Options:**

- Three security violations have been detected on this interface.
- This port is currently up.
- The port is configured as a trunk link.
- Security violations will cause this port to shut down immediately.
- There is no device currently connected to this port.
- The switch port mode for this interface is access mode.

---

## Question 113

What method of wireless authentication is dependent on a RADIUS authentication server?


**Options:**

- WEP
- WPA Personal
- WPA2 Personal
- WPA2 Enterprise

---

## Question 114

A network administrator has found a user sending a double-tagged 802.1Q frame to a switch. What is the best solution to prevent this type of attack?


**Options:**

- The native VLAN number used on any trunk should be one of the active data VLANs.
- The VLANs for user access ports should be different VLANs than any native VLANs used on trunk ports.
- Trunk ports should be configured with port security.
- Trunk ports should use the default VLAN as the native VLAN number.

---

## Question 115

**Refer to the exhibit. Which two conclusions can be drawn from the output? (Choose two.)**

![Question 115 Exhibit](images/image_36.png)


**Options:**

- The EtherChannel is down.
- The port channel ID is 2.
- The port channel is a Layer 3 channel.
- The bundle is fully operational.
- The load-balancing method used is source port to destination port.

---

## Question 116

Match the step number to the sequence of stages that occur during the HSRP failover process. (Not all options are used.)

## Question 117

On a Cisco 3504 WLC Summary page ( Advanced > Summary ), which tab allows a network administrator to configure a particular WLAN with a WPA2 policy?


**Options:**

- WLANs
- SECURITY
- WIRELESS
- MANAGEMENT

---

## Question 118

**Refer to the exhibit. A network engineer is configuring IPv6 routing on the network. Which command issued on router HQ will configure a default route to the Internet to forward packets to an IPv6 destination network that is not listed in the routing table?​**

![Question 118 Exhibit](images/image_37.png)


**Options:**

- ipv6 route ::/0 serial 0/0/0
- ip route 0.0.0.0 0.0.0.0 serial 0/1/1
- ipv6 route ::1/0 serial 0/1/1
- ipv6 route ::/0 serial 0/1/1

---

## Question 119

Users are complaining of sporadic access to the internet every afternoon. What should be done or checked?


**Options:**

- Create static routes to all internal networks and a default route to the internet.
- Verify that there is not a default route in any of the edge router routing tables.
- Create a floating static route to that network.
- Check the statistics on the default route for oversaturation.

---

## Question 120

What action takes place when the source MAC address of a frame entering a switch appears in the MAC address table associated with a different port?


**Options:**

- The switch purges the entire MAC address table.
- The switch replaces the old entry and uses the more current port.
- The switch updates the refresh timer for the entry.
- The switch forwards the frame out of the specified port.

---

## Question 121

A network administrator is configuring a WLAN. Why would the administrator use a WLAN controller?


**Options:**

- to centralize management of multiple WLANs
- to provide privacy and integrity to wireless traffic by using encryption
- to facilitate group configuration and management of multiple WLANs through a WLC
- to provide prioritized service for time-sensitive applications

---

## Question 122

A new Layer 3 switch is connected to a router and is being configured for interVLAN routing. What are three of the five steps required for the configuration? (Choose three.)


**Options:**

- enabling IP routing
- modifying the default VLAN
- entering “no switchport” on the port connected to the router
- establishing adjacencies
- assigning ports to VLANs
- adjusting the route metric
- assigning the ports to the native VLAN

---

## Question 123

Which three statements accurately describe duplex and speed settings on Cisco 2960 switches? (Choose three.)


**Options:**

- An autonegotiation failure can result in connectivity issues.
- When the speed is set to 1000 Mb/s, the switch ports will operate in full-duplex mode.
- The duplex and speed settings of each switch port can be manually configured.
- Enabling autonegotiation on a hub will prevent mismatched port speeds when connecting the hub to the switch.
- By default, the speed is set to 100 Mb/s and the duplex mode is set to autonegotiation.
- By default, the autonegotiation feature is disabled.

---

## Question 124

**Refer to the exhibit. A network administrator configures R1 for inter-VLAN routing between VLAN 10 and VLAN 20. However, the devices in VLAN 10 and VLAN 20 cannot communicate. Based on the configuration in the exhibit, what is a possible cause for the problem?**

![Question 124 Exhibit](images/image_38.png)


**Options:**

- The port Gi0/0 should be configured as trunk port.
- The encapsulation is misconfigured on a subinterface.
- A no shutdown command should be added in each subinterface configuration.
- The command interface gigabitEthernet 0/0.1 is wrong.

---

## Question 125

A network administrator uses the spanning-tree portfast bpduguard default global configuration command to enable BPDU guard on a switch. However, BPDU guard is not activated on all access ports. What is the cause of the issue?


**Options:**

- BPDU guard needs to be activated in the interface configuration command mode.
- Access ports configured with root guard cannot be configured with BPDU guard.
- Access ports belong to different VLANs.
- PortFast is not configured on all access ports.

---

## Question 126

Which two types of spanning tree protocols can cause suboptimal traffic flows because they assume only one spanning-tree instance for the entire bridged network? (Choose two.)


**Options:**

- MSTP
- RSTP
- Rapid PVST+
- PVST+
- STP

---

## Question 127

**Refer to the exhibit. A network administrator is configuring the router R1 for IPv6 address assignment. Based on the partial configuration, which IPv6 global unicast address assignment scheme does the administrator intend to implement?**

![Question 127 Exhibit](images/image_39.png)


**Options:**

- stateful
- stateless
- manual configuration
- SLAAC

---

## Question 128

A WLAN engineer deploys a WLC and five wireless APs using the CAPWAP protocol with the DTLS feature to secure the control plane of the network devices. While testing the wireless network, the WLAN engineer notices that data traffic is being exchanged between the WLC and the APs in plain-text and is not being encrypted. What is the most likely reason for this?

DTLS only provides data security through authentication and does not provide encryption for data moving between a wireless LAN controller (WLC) and an access point (AP).


**Options:**

- Although DTLS is enabled by default to secure the CAPWAP control channel, it is disabled by default for the data channel.
- DTLS is a protocol that only provides security between the access point (AP) and the wireless client.
- Data encryption requires a DTLS license to be installed on each access point (AP) prior to being enabled on the wireless LAN controller (WLC).

---

## Question 129

A new switch is to be added to an existing network in a remote office. The network administrator does not want the technicians in the remote office to be able to add new VLANs to the switch, but the switch should receive VLAN updates from the VTP domain. Which two steps must be performed to configure VTP on the new switch to meet these conditions? (Choose two.)


**Options:**

- Configure the new switch as a VTP client.
- Configure the existing VTP domain name on the new switch.
- Configure an IP address on the new switch.
- Configure all ports of both switches to access mode.
- Enable VTP pruning.

---

## Question 130

**Refer to the exhibit. Consider that the main power has just been restored. PC3 issues a broadcast IPv4 DHCP request. To which port will SW1 forward this request?​**

![Question 130 Exhibit](images/image_40.png)


**Options:**

- to Fa0/1, Fa0/2, and Fa0/3 only
- to Fa0/1, Fa0/2, Fa0/3, and Fa0/4
- to Fa0/1 only​
- to Fa0/1, Fa0/2, and Fa0/4 only​
- to Fa0/1 and Fa0/2 only

---

## Question 131

What action takes place when the source MAC address of a frame entering a switch is not in the MAC address table?


**Options:**

- The switch adds a MAC address table entry for the destination MAC address and the egress port.
- The switch adds the MAC address and incoming port number to the table.
- The switch replaces the old entry and uses the more current port.
- The switch updates the refresh timer for the entry.

---

## Question 132

Employees are unable to connect to servers on one of the internal networks. What should be done or checked?


**Options:**

- Use the “show ip interface brief” command to see if an interface is down.
- Verify that there is not a default route in any of the edge router routing tables.
- Create static routes to all internal networks and a default route to the internet.
- Check the statistics on the default route for oversaturation.

---

## Question 133

What is the effect of entering the ip dhcp snooping configuration command on a switch?


**Options:**

- It enables DHCP snooping globally on a switch.
- It enables PortFast globally on a switch.
- It disables DTP negotiations on trunking ports.
- It manually enables a trunk link.

---

## Question 134

An administrator notices that large numbers of packets are being dropped on one of the branch routers. What should be done or checked?


**Options:**

- Create static routes to all internal networks and a default route to the internet.
- Create extra static routes to the same location with an AD of 1.
- Create extra static routes to the same location with an AD of 1.
- Check the routing table for a missing static route.

---

## Question 135

What are two switch characteristics that could help alleviate network congestion? (Choose two.)


**Options:**

- fast internal switching
- large frame buffers
- store-and-forward switching
- low port density
- frame check sequence (FCS) check

---

## Question 136

What is a result of connecting two or more switches together?


**Options:**

- The number of broadcast domains is increased.
- The size of the broadcast domain is increased.
- The number of collision domains is reduced.
- The size of the collision domain is increased.

---

## Question 137

An administrator is trying to remove configurations from a switch. After using the command erase startup-config and reloading the switch, the administrator finds that VLANs 10 and 100 still exist on the switch. Why were these VLANs not removed?


**Options:**

- These VLANs are default VLANs that cannot be removed.
- These VLANs cannot be deleted unless the switch is in VTP client mode.
- These VLANs can only be removed from the switch by using the no vlan 10 and no vlan 100 commands.
- Because these VLANs are stored in a file that is called vlan.dat that is located in flash memory, this file must be manually deleted.

---

## Question 138

Branch users were able to access a site in the morning but have had no connectivity with the site since lunch time. What should be done or checked?


**Options:**

- Verify that the static route to the server is present in the routing table.
- Use the “show ip interface brief” command to see if an interface is down.
- Check the configuration on the floating static route and adjust the AD.
- Create a floating static route to that network.

---

## Question 139

What is the effect of entering the switchport port-security configuration command on a switch?


**Options:**

- It dynamically learns the L2 address and copies it to the running configuration.
- It enables port security on an interface.
- It enables port security globally on the switch.
- It restricts the number of discovery messages, per second, to be received on the interface.

---

## Question 140

A network administrator is configuring a WLAN. Why would the administrator use multiple lightweight APs?


**Options:**

- to centralize management of multiple WLANs
- to monitor the operation of the wireless network
- to provide prioritized service for time-sensitive applications
- to facilitate group configuration and management of multiple WLANs through a WLC

---

## Question 141

A new Layer 3 switch is connected to a router and is being configured for interVLAN routing. What are three of the five steps required for the configuration? (Choose three.)


**Options:**

- assigning ports to VLANs
- assigning the ports to the native VLAN
- enabling IP routing
- modifying the default VLAN
- installing a static route
- implementing a routing protocol
- creating SVI interfaces

---

## Question 142

A network administrator is configuring a WLAN. Why would the administrator use RADIUS servers on the network?


**Options:**

- to centralize management of multiple WLANs
- to restrict access to the WLAN by authorized, authenticated users only
- to facilitate group configuration and management of multiple WLANs through a WLC
- to monitor the operation of the wireless network

---

## Question 143

What is the effect of entering the switchport mode access configuration command on a switch?


**Options:**

- It enables BPDU guard on a specific port.
- It manually enables a trunk link.
- It disables an unused port.
- It disables DTP on a non-trunking interface.

---

## Question 144

A network administrator has configured a router for stateless DHCPv6 operation. However, users report that workstations are not receiving DNS server information. Which two router configuration lines should be verified to ensure that stateless DHCPv6 service is properly configured? (Choose two.)


**Options:**

- The domain-name line is included in the ipv6 dhcp pool section.
- The dns-server line is included in the ipv6 dhcp pool section.
- The ipv6 nd other-config-flag is entered for the interface that faces the LAN segment.
- The address prefix line is included in the ipv6 dhcp pool section.
- The ipv6 nd managed-config-flag is entered for the interface that faces the LAN segment.

---

## Question 145

A network administrator is configuring a WLAN. Why would the administrator disable the broadcast feature for the SSID?


**Options:**

- to eliminate outsiders scanning for available SSIDs in the area
- to centralize management of multiple WLANs
- to facilitate group configuration and management of multiple WLANs through a WLC
- to provide prioritized service for time-sensitive applications

---

## Question 146

**Refer to the exhibit. An administrator is attempting to install an IPv6 static route on router R1 to reach the network attached to router R2. After the static route command is entered, connectivity to the network is still failing. What error has been made in the static route configuration?**

![Question 146 Exhibit](images/image_41.png)


**Options:**

- The next hop address is incorrect.
- The interface is incorrect.
- The destination network is incorrect.
- The network prefix is incorrect.

---

## Question 147

What action takes place when a frame entering a switch has a unicast destination MAC address that is not in the MAC address table?


**Options:**

- The switch updates the refresh timer for the entry.
- The switch resets the refresh timer on all MAC address table entries.
- The switch replaces the old entry and uses the more current port.
- The switch will forward the frame out all ports except the incoming port.

---

## Question 148

A junior technician was adding a route to a LAN router. A traceroute to a device on the new network revealed a wrong path and unreachable status. What should be done or checked?


**Options:**

- Create a floating static route to that network.
- Check the configuration on the floating static route and adjust the AD.
- Check the configuration of the exit interface on the new static route.
- Verify that the static route to the server is present in the routing table.

---

## Question 149

What is the effect of entering the ip arp inspection vlan 10 configuration command on a switch?


**Options:**

- It specifies the maximum number of L2 addresses allowed on a port.
- It enables DAI on specific switch interfaces previously configured with DHCP snooping.
- It enables DHCP snooping globally on a switch.
- It globally enables BPDU guard on all PortFast-enabled ports.

---

## Question 150

What protocol or technology manages trunk negotiations between switches?


**Options:**

- VTP
- EtherChannel
- DTP
- STP

---

## Question 151

A network administrator is configuring a WLAN. Why would the administrator apply WPA2 with AES to the WLAN?


**Options:**

- to reduce the risk of unauthorized APs being added to the network
- to centralize management of multiple WLANs
- to provide prioritized service for time-sensitive applications
- to provide privacy and integrity to wireless traffic by using encryption

---

## Question 152

Users on a LAN are unable to get to a company web server but are able to get elsewhere. What should be done or checked?


**Options:**

- Ensure that the old default route has been removed from the company edge routers.
- Verify that the static route to the server is present in the routing table.
- Check the configuration on the floating static route and adjust the AD.
- Create a floating static route to that network.

---

## Question 153

What IPv6 prefix is designed for link-local communication?


**Options:**

- 2001::/3
- ff00::/8
- fc::/07
- fe80::/10

---

## Question 154

What is the effect of entering the ip dhcp snooping limit rate 6 configuration command on a switch?


**Options:**

- It displays the IP-to-MAC address associations for switch interfaces.
- It enables port security globally on the switch.
- It restricts the number of discovery messages, per second, to be received on the interface.
- It dynamically learns the L2 address and copies it to the running configuration.

---

## Question 155

A network administrator is configuring a WLAN. Why would the administrator change the default DHCP IPv4 addresses on an AP?


**Options:**

- to eliminate outsiders scanning for available SSIDs in the area
- to reduce the risk of unauthorized APs being added to the network
- to reduce outsiders intercepting data or accessing the wireless network by using a well-known address range
- to reduce the risk of interference by external devices such as microwave ovens

---

## Question 156

What is the effect of entering the ip arp inspection validate src-mac configuration command on a switch?


**Options:**

- It checks the source L2 address in the Ethernet header against the sender L2 address in the ARP body.
- It disables all trunk ports.
- It displays the IP-to-MAC address associations for switch interfaces.
- It enables portfast on a specific switch interface.

---

## Question 157

What protocol or technology is a Cisco proprietary protocol that is automatically enabled on 2960 switches?


**Options:**

- DTP
- STP
- VTP
- EtherChannel

---

## Question 158

What address and prefix length is used when configuring an IPv6 default static route?


**Options:**

- ::/0
- FF02::1/8
- 0.0.0.0/0
- ::1/128

---

## Question 159

What are two characteristics of Cisco Express Forwarding (CEF)? (Choose two.)

When a packet arrives on a router interface, it is forwarded to the control plane where the CPU matches the destination address with a matching routing table entry.

With this switching method, flow information for a packet is stored in the fast-switching cache to forward future packets to the same destination without CPU intervention.


**Options:**

- This is the fastest forwarding mechanism on Cisco routers and multilayer switches.
- Packets are forwarded based on information in the FIB and an adjacency table.
- When a packet arrives on a router interface, it is forwarded to the control plane where the CPU searches for a match in the fast-switching cache.

---

## Question 160

Which term describes the role of a Cisco switch in the 802.1X port-based access control?


**Options:**

- agent
- supplicant
- authenticator
- authentication server

---

## Question 161

Which Cisco solution helps prevent ARP spoofing and ARP poisoning attacks?


**Options:**

- Dynamic ARP Inspection
- IP Source Guard
- DHCP Snooping
- Port Security

---

## Question 162

What is an advantage of PVST+?


**Options:**

- PVST+ optimizes performance on the network through autoselection of the root bridge.
- PVST+ reduces bandwidth consumption compared to traditional implementations of STP that use CST.
- PVST+ requires fewer CPU cycles for all the switches in the network.
- PVST+ optimizes performance on the network through load sharing.

---

## Question 163

What protocol or technology uses a standby router to assume packet-forwarding responsibility if the active router fails?


**Options:**

- EtherChannel
- DTP
- HSRP
- VTP

---

## Question 164

What is the effect of entering the show ip dhcp snooping binding configuration command on a switch?


**Options:**

- It switches a trunk port to access mode.
- It checks the source L2 address in the Ethernet header against the sender L2 address in the ARP body.
- It restricts the number of discovery messages, per second, to be received on the interface.
- It displays the IP-to-MAC address associations for switch interfaces.

---

## Question 165

What action takes place when the source MAC address of a frame entering a switch is in the MAC address table?


**Options:**

- The switch forwards the frame out of the specified port.
- The switch updates the refresh timer for the entry.
- The switch replaces the old entry and uses the more current port.
- The switch adds a MAC address table entry for the destination MAC address and the egress port.

---

## Question 166

A small publishing company has a network design such that when a broadcast is sent on the LAN, 200 devices receive the transmitted broadcast. How can the network administrator reduce the number of devices that receive broadcast traffic?


**Options:**

- Add more switches so that fewer devices are on a particular switch.
- Replace the switches with switches that have more ports per switch. This will allow more devices on a particular switch.
- Segment the LAN into smaller LANs and route between them.
- Replace at least half of the switches with hubs to reduce the size of the broadcast domain.

---

## Question 167

What defines a host route on a Cisco router?


**Options:**

- The link-local address is added automatically to the routing table as an IPv6 host route.
- An IPv4 static host route configuration uses a destination IP address of a specific device and a /32 subnet mask.
- A host route is designated with a C in the routing table.
- A static IPv6 host route must include the interface type and the interface number of the next hop router.

---

## Question 168

What else is required when configuring an IPv6 static route using a next-hop link-local address?


**Options:**

- administrative distance
- ip address of the neighbor router
- network number and subnet mask on the interface of the neighbor router
- interface number and type

---

## Question 169

A technician is configuring a wireless network for a small business using a SOHO wireless router. Which two authentication methods are used, if the router is configured with WPA2? (Choose two.)


**Options:**

- personal
- AES
- TKIP
- WEP
- enterprise

---

## Question 170

Which mitigation technique would prevent rogue servers from providing false IPv6 configuration parameters to clients?


**Options:**

- enabling DHCPv6 Guard
- enabling RA Guard
- implementing port security on edge ports
- disabling CDP on edge ports

---

## Question 171

A PC has sent an RS message to an IPv6 router attached to the same network. Which two pieces of information will the router send to the client? (Choose two.)


**Options:**

- prefix length
- subnet mask in dotted decimal notation
- domain name
- administrative distance
- prefix
- DNS server IP address

---

## Question 172

While attending a conference, participants are using laptops for network connectivity. When a guest speaker attempts to connect to the network, the laptop fails to display any available wireless networks. The access point must be operating in which mode?


**Options:**

- mixed
- passive
- active
- open

---

## Question 173

Which three components are combined to form a bridge ID?


**Options:**

- extended system ID
- cost
- IP address
- bridge priority
- MAC address
- port ID

---

