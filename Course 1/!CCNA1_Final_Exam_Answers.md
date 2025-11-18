# CCNA 1 - Introduction to Networks (Version 7.00)

## ITNv7 Course Final Exam - Practice Questions with Answers

---

---

### Question 1

**Which two traffic types use the Real-Time Transport Protocol (RTP)? (Choose two.)**

- [ ] video
- [ ] web
- [ ] file transfer
- [ ] voice
- [ ] peer to peer

---

### Question 2

**Which wireless technology has low-power and data rate requirements making it popular in home automation applications?**

- [ ] ZigBee
- [ ] LoRaWAN
- [ ] 5G
- [ ] Wi-Fi

<details> <summary>💡 Show Answer</summary>

**Explanation: ZigBee is an IEEE 802.15.4 wireless standard designed for creating personal-area networks. Low energy, power, and data rate requirements make Zigbee a popular protocol for connecting home automation devices.**

</details>

---

### Question 3

**Which layer of the TCP/IP model provides a route to forward messages through an internetwork?**

- [ ] application
- [ ] network access
- [ ] internet
- [ ] transport

<details> <summary>💡 Show Answer</summary>

**Explain: The OSI model network layer corresponds directly to the internet layer of the TCP/IP model and is used to describe protocols that address and route messages through an internetwork.**

</details>

---

### Question 4

**Which type of server relies on record types such as A, NS, AAAA, and MX in order to provide services?**

- [ ] DNS
- [ ] email
- [ ] file
- [ ] web

<details> <summary>💡 Show Answer</summary>

**Explain: A DNS server stores records that are used to resolve IP addresses to host names. Some DNS record types include the following:**

A – an end device IPv4 address NS – an authoritative name server AAAA – an end device IPv6 address MX – a mail exchange record

</details>

---

### Question 5

**What are proprietary protocols?**

- [ ] protocols developed by private organizations to operate on any vendor hardware
- [ ] protocols that can be freely used by any organization or vendor
- [ ] protocols developed by organizations who have control over their definition and operation
- [ ] a collection of protocols known as the TCP/IP protocol suite

<details> <summary>💡 Show Answer</summary>

**Explain: Proprietary protocols have their definition and operation controlled by one company or vendor. Some of them can be used by different organizations with permission from the owner. The TCP/IP protocol suite is an open standard, not a proprietary protocol.**

</details>

---

### Question 6

**What service is provided by DNS?**

- [ ] Resolves domain names, such as cisco.com, into IP addresses.
- [ ] A basic set of rules for exchanging text, graphic images, sound, video, and other multimedia files on the web.
- [ ] Allows for data transfers between a client and a file server.
- [ ] Uses encryption to secure the exchange of text, graphic images, sound, and video on the web.

---

### Question 7

**A client packet is received by a server. The packet has a destination port number of 110. What service is the client requesting?**

- [ ] DNS
- [ ] DHCP
- [ ] SMTP
- [ ] POP3

---

### Question 8

**What command can be used on a Windows PC to see the IP configuration of that computer?**

- [ ] show ip interface brief
- [ ] ping
- [ ] show interfaces
- [ ] ipconfig

---

### Question 9

**A wired laser printer is attached to a home computer. That printer has been shared so that other computers on the home network can also use the printer. What networking model is in use?**

- [ ] client-based
- [ ] master-slave
- [ ] point-to-point
- [ ] peer-to-peer (P2P)

<details> <summary>💡 Show Answer</summary>

**Explanation: Peer-to-peer (P2P) networks have two or more network devices that can share resources such as printers or files without having a dedicated server.**

</details>

---

### Question 10

**What characteristic describes a virus?**

- [ ] a network device that filters access and traffic coming into a network
- [ ] the use of stolen credentials to access private data
- [ ] an attack that slows or crashes a device or network service
- [ ] malicious software or code running on an end device

---

### Question 11

**Three bank employees are using the corporate network. The first employee uses a web browser to view a company web page in order to read some announcements. The second employee accesses the corporate database to perform some financial transactions. The third employee participates in an important live audio conference with other corporate managers in branch offices. If QoS is implemented on this network, what will be the priorities from highest to lowest of the different data types?**

- [ ] financial transactions, web page, audio conference
- [ ] audio conference, financial transactions, web page
- [ ] financial transactions, audio conference, web page
- [ ] audio conference, web page, financial transactions

<details> <summary>💡 Show Answer</summary>

**Explanation: QoS mechanisms enable the establishment of queue management strategies that enforce priorities for different categories of application data. Thus, this queuing enables voice data to have priority over transaction data, which has priority over web data.**

</details>

---

### Question 12

**Match the description to the IPv6 addressing component. (Not all options are used.)**

- [ ] Place the options in the following order:

---

### Question 13

**Refer to the exhibit. If Host1 were to transfer a file to the server, what layers of the TCP/IP model would be used?**

![Question 13](exam_images/question_img_1.jpeg)

- [ ] only application and Internet layers
- [ ] only Internet and network access layers
- [ ] only application, Internet, and network access layers
- [ ] application, transport, Internet, and network access layers
- [ ] only application, transport, network, data link, and physical layers
- [ ] application, session, transport, network, data link, and physical layers

<details> <summary>💡 Show Answer</summary>

**Explanation: The TCP/IP model contains the application, transport, internet, and network access layers. A file transfer uses the FTP application layer protocol. The data would move from the application layer through all of the layers of the model and across the network to the file server.**

</details>

---

### Question 14

**Match the characteristic to the forwarding method. (Not all options are used.)**

<details> <summary>💡 Show Answer</summary>

**Explanation: A store-and-forward switch always stores the entire frame before forwarding, and checks its CRC and frame length. A cut-through switch can forward frames before receiving the destination address field, thus presenting less latency than a store-and-forward switch. Because the frame can begin to be forwarded before it is completely received, the switch may transmit a corrupt or runt frame. All forwarding methods require a Layer 2 switch to forward broadcast frames.**

</details>

---

### Question 15

**Refer to the exhibit. The IP address of which device interface should be used as the default gateway setting of host H1?**

![Question 15](exam_images/question_img_2.png)

- [ ] R1: S0/0/0
- [ ] R2: S0/0/1
- [ ] R1: G0/0
- [ ] R2: S0/0/0

<details> <summary>💡 Show Answer</summary>

**Explanation: The default gateway for host H1 is the router interface that is attached to the LAN that H1 is a member of. In this case, that is the G0/0 interface of R1. H1 should be configured with the IP address of that interface in its addressing settings. R1 will provide routing services to packets from H1 that need to be forwarded to remote networks.**

</details>

---

### Question 16

**What service is provided by Internet Messenger?**

- [ ] An application that allows real-time chatting among remote users.
- [ ] Allows remote access to network devices and servers.
- [ ] Resolves domain names, such as cisco.com, into IP addresses.
- [ ] Uses encryption to provide secure remote access to network devices and servers.

---

### Question 17

**Refer to the exhibit. Match the network with the correct IP address and prefix that will satisfy the usable host addressing requirements for each network.**

<details> <summary>💡 Show Answer</summary>

**Explanation: Network A needs to use 192.168.0.128 /25, which yields 128 host addresses. Network B needs to use 192.168.0.0 /26, which yields 64 host addresses. Network C needs to use 192.168.0.96 /27, which yields 32 host addresses. Network D needs to use 192.168.0.80/30, which yields 4 host addresses.**

</details>

---

### Question 18

**Refer to the exhibit. Which protocol was responsible for building the table that is shown?**

![Question 18](question_img_3.jpeg)

- [ ] DHCP
- [ ] ARP
- [ ] DNS
- [ ] ICMP

<details> <summary>💡 Show Answer</summary>

**Explanation: The table that is shown corresponds to the output of the arp -a command, a command that is used on a Windows PC to display the ARP table.**

</details>

---

### Question 19

**A network administrator notices that some newly installed Ethernet cabling is carrying corrupt and distorted data signals. The new cabling was installed in the ceiling close to fluorescent lights and electrical equipment. Which two factors may interfere with the copper cabling and result in signal distortion and data corruption? (Choose two.)**

- [ ] crosstalk
- [ ] extended length of cabling
- [ ] RFI​
- [ ] EMI
- [ ] signal attenuation
- [ ] It will send the frame and use its own MAC address as the destination.
- [ ] It will send an ARP request for the MAC address of the destination device.
- [ ] It will send the frame with a broadcast MAC address.
- [ ] It will send a request to the DNS server for the destination MAC address.
- [ ] It will send an ARP request for the MAC address of the default gateway.

---

### Question 22

**A client packet is received by a server. The packet has a destination port number of 53. What service is the client requesting?**

- [ ] DNS
- [ ] NetBIOS (NetBT)
- [ ] POP3
- [ ] IMAP

---

### Question 23

**A network administrator is adding a new LAN to a branch office. The new LAN must support 25 connected devices. What is the smallest network mask that the network administrator can use for the new network?**

- [ ] 255.255.255.128
- [ ] 255.255.255.192
- [ ] 255.255.255.224
- [ ] 255.255.255.240

---

### Question 24

**What characteristic describes a Trojan horse?**

- [ ] malicious software or code running on an end device
- [ ] an attack that slows or crashes a device or network service
- [ ] the use of stolen credentials to access private data
- [ ] a network device that filters access and traffic coming into a network

---

### Question 25

**What service is provided by HTTPS?**

- [ ] Uses encryption to provide secure remote access to network devices and servers.
- [ ] Resolves domain names, such as cisco.com, into IP addresses.
- [ ] Uses encryption to secure the exchange of text, graphic images, sound, and video on the web.
- [ ] Allows remote access to network devices and servers.

---

### Question 26

**A technician with a PC is using multiple applications while connected to the Internet. How is the PC able to keep track of the data flow between multiple application sessions and have each application receive the correct packet flows?**

- [ ] The data flow is being tracked based on the destination MAC address of the technician PC.
- [ ] The data flow is being tracked based on the source port number that is used by each application.
- [ ] The data flow is being tracked based on the source IP address that is used by the PC of the technician.
- [ ] The data flow is being tracked based on the destination IP address that is used by the PC of the technician.

<details> <summary>💡 Show Answer</summary>

**Explanation: The source port number of an application is randomly generated and used to individually keep track of each session connecting out to the Internet. Each application will use a unique source port number to provide simultaneous communication from multiple applications through the Internet.**

</details>

---

### Question 27

**A network administrator is adding a new LAN to a branch office. The new LAN must support 61 connected devices. What is the smallest network mask that the network administrator can use for the new network?**

- [ ] 255.255.255.240
- [ ] 255.255.255.224
- [ ] 255.255.255.192
- [ ] 255.255.255.128

---

### Question 28

**Refer to the exhibit. Match the network with the correct IP address and prefix that will satisfy the usable host addressing requirements for each network. (Not all options are used.)**

![Question 28](question_img_4.jpeg)

<details> <summary>💡 Show Answer</summary>

**Explanation: Network A needs to use 192.168.0.0 /25 which yields 128 host addresses. Network B needs to use 192.168.0.128 /26 which yields 64 host addresses. Network C needs to use 192.168.0.192 /27 which yields 32 host addresses. Network D needs to use 192.168.0.224 /30 which yields 4 host addresses.**

</details>

---

### Question 29

**What characteristic describes a DoS attack?**

- [ ] the use of stolen credentials to access private data
- [ ] a network device that filters access and traffic coming into a network
- [ ] software that is installed on a user device and collects information about the user
- [ ] an attack that slows or crashes a device or network service

---

### Question 30

**Match the application protocols to the correct transport protocols.**

![Question 30](question_img_5.jpeg)

---

### Question 31

**What service is provided by SMTP?**

- [ ] Allows clients to send email to a mail server and the servers to send email to other servers.
- [ ] Allows remote access to network devices and servers.
- [ ] Uses encryption to provide secure remote access to network devices and servers.
- [ ] An application that allows real-time chatting among remote users.

---

### Question 32

**Which scenario describes a function provided by the transport layer?**

- [ ] A student is using a classroom VoIP phone to call home. The unique identifier burned into the phone is a transport layer address used to contact another network device on the same network.
- [ ] A student is playing a short web-based movie with sound. The movie and sound are encoded within the transport layer header.
- [ ] A student has two web browser windows open in order to access two web sites. The transport layer ensures the correct web page is delivered to the correct browser window.

<details> <summary>💡 Show Answer</summary>

**Explain: The source and destination port numbers are used to identify the correct application and window within that application.**

33.Refer to the exhibit. Host B on subnet Teachers transmits a packet to host D on subnet Students. Which Layer 2 and Layer 3 addresses are contained in the PDUs that are transmitted from host B to the router?

Layer 2 destination address = 00-00-0c-94-36-ab Layer 2 source address = 00-00-0c-94-36-bb Layer 3 destination address = 172.16.20.200 Layer 3 source address = 172.16.10.200

Layer 2 destination address = 00-00-0c-94-36-dd Layer 2 source address = 00-00-0c-94-36-bb Layer 3 destination address = 172.16.20.200 Layer 3 source address = 172.16.10.200

Layer 2 destination address = 00-00-0c-94-36-cd Layer 2 source address = 00-00-0c-94-36-bb Layer 3 destination address = 172.16.20.99 Layer 3 source address = 172.16.10.200

Layer 2 destination address = 00-00-0c-94-36-ab Layer 2 source address = 00-00-0c-94-36-bb Layer 3 destination address = 172.16.20.200 Layer 3 source address = 172.16.100.200

</details>

---

### Question 34

**What does the term “attenuation” mean in data communication?**

- [ ] strengthening of a signal by a networking device
- [ ] leakage of signals from one cable pair to another
- [ ] time for a signal to reach its destination
- [ ] loss of signal strength as distance increases

<details> <summary>💡 Show Answer</summary>

**Explanation: Data is transmitted on copper cables as electrical pulses. A detector in the network interface of a destination device must receive a signal that can be successfully decoded to match the signal sent. However, the farther the signal travels, the more it deteriorates. This is referred to as signal attenuation.**

</details>

---

### Question 35

**Refer to the exhibit. An administrator is trying to configure the switch but receives the error message that is displayed in the exhibit. What is the problem?**

![Question 35](question_img_6.png)

- [ ] The entire command, configure terminal, must be used.
- [ ] The administrator is already in global configuration mode.
- [ ] The administrator must first enter privileged EXEC mode before issuing the command.
- [ ] The administrator must connect via the console port to access global configuration mode.

---

### Question 36

**Which two protocols operate at the top layer of the TCP/IP protocol suite? (Choose two.)**

- [ ] TCP
- [ ] IP
- [ ] UDP
- [ ] POP
- [ ] DNS
- [ ] Ethernet

---

### Question 37

**A company has a file server that shares a folder named Public. The network security policy specifies that the Public folder is assigned Read-Only rights to anyone who can log into the server while the Edit rights are assigned only to the network admin group. Which component is addressed in the AAA network service framework?**

- [ ] automation
- [ ] accounting
- [ ] authentication
- [ ] authorization

---

### Question 38

**What three requirements are defined by the protocols used in network communcations to allow message transmission across a network? (Choose three.)**

- [ ] message size
- [ ] message encoding
- [ ] connector specifications
- [ ] media selection
- [ ] delivery options
- [ ] end-device installation

---

### Question 39

**What are two characteristics of IP? (Choose two.)**

- [ ] does not require a dedicated end-to-end connection
- [ ] operates independently of the network media
- [ ] retransmits packets if errors occur
- [ ] re-assembles out of order packets into the correct order at the receiver end
- [ ] guarantees delivery of packets

<details> <summary>💡 Show Answer</summary>

**Explain: The Internet Protocol (IP) is a connectionless, best effort protocol. This means that IP requires no end-to-end connection nor does it guarantee delivery of packets. IP is also media independent, which means it operates independently of the network media carrying the packets.**
</details>

---
### Question 40
An employee of a large corporation remotely logs into the company using the appropriate username and password. The employee is attending an important video conference with a customer concerning a large sale. It is important for the video quality to be excellent during the meeting. The employee is unaware that after a successful login, the connection to the company ISP failed. The secondary connection, however, activated within seconds. The disruption was not noticed by the employee or other employees. What three network characteristics are described in this scenario? (Choose three.)

security

quality of service

scalability

powerline networking

integrity

fault tolerance

<details> <summary>💡 Show Answer</summary>

**Explanation: Usernames and passwords relate to network security. Good quality video, to support video conferencing, relates to prioritizing the video traffic with quality of service (QoS). The fact that a connection to an ISP failed and was then restored but went unnoticed by employees relates to the fault tolerant design of the network.**

</details>

---

### Question 41

**What are two common causes of signal degradation when using UTP cabling? (Choose two.)**

- [ ] improper termination
- [ ] low-quality shielding in cable
- [ ] installing cables in conduit
- [ ] low-quality cable or connectors
- [ ] loss of light over long distances

<details> <summary>💡 Show Answer</summary>

**Explanation: When terminated improperly, each cable is a potential source of physical layer performance degradation.**

</details>

---

### Question 42

**Which subnet would include the address 192.168.1.96 as a usable host address?**

- [ ] 192.168.1.64/26
- [ ] 192.168.1.32/27
- [ ] 192.168.1.32/28
- [ ] 192.168.1.64/29

<details> <summary>💡 Show Answer</summary>

**Explanation: For the subnet of 192.168.1.64/26, there are 6 bits for host addresses, yielding 64 possible addresses. However, the first and last subnets are the network and broadcast addresses for this subnet. Therefore, the range of host addresses for this subnet is 192.168.1.65 to 192.168.1.126. The other subnets do not contain the address 192.168.1.96 as a valid host address.**

</details>

---

### Question 43

**Refer to the exhibit. On the basis of the output, which two statements about network connectivity are correct? (Choose two.)**

![Question 43](question_img_7.jpeg)

- [ ] This host does not have a default gateway configured.
- [ ] There are 4 hops between this device and the device at 192.168.100.1.
- [ ] There is connectivity between this device and the device at 192.168.100.1.
- [ ] The connectivity between these two hosts allows for videoconferencing calls.
- [ ] The average transmission time between the two hosts is 2 milliseconds.

<details> <summary>💡 Show Answer</summary>

**Explain: The output displays a successful Layer 3 connection between a host computer and a host at 19.168.100.1. It can be determined that 4 hops exist between them and the average transmission time is 1 milliseconds. Layer 3 connectivity does not necessarily mean that an application can run between the hosts.**

</details>

---

### Question 44

**Which two statements describe how to assess traffic flow patterns and network traffic types using a protocol analyzer? (Choose two.)**

- [ ] Capture traffic on the weekends when most employees are off work.
- [ ] Capture traffic during peak utilization times to get a good representation of the different traffic types.
- [ ] Only capture traffic in the areas of the network that receive most of the traffic such as the data center.
- [ ] Perform the capture on different network segments.
- [ ] Only capture WAN traffic because traffic to the web is responsible for the largest amount of traffic on a network.

<details> <summary>💡 Show Answer</summary>

**Explanation: Traffic flow patterns should be gathered during peak utilization times to get a good representation of the different traffic types. The capture should also be performed on different network segments because some traffic will be local to a particular segment.**

</details>

---

### Question 45

**What is the consequence of configuring a router with the ipv6 unicast-routing global configuration command?​**

- [ ] All router interfaces will be automatically activated.
- [ ] The IPv6 enabled router interfaces begin sending ICMPv6 Router Advertisement messages.
- [ ] Each router interface will generate an IPv6 link-local address.​
- [ ] It statically creates a global unicast address on this router.​

---

### Question 46

**Which three layers of the OSI model map to the application layer of the TCP/IP model? (Choose three.)**

- [ ] application
- [ ] network
- [ ] data link
- [ ] session
- [ ] presentation
- [ ] transport

<details> <summary>💡 Show Answer</summary>

**Explanation: The TCP/IP model consists of four layers: application, transport, internet, and network access. The OSI model consists of seven layers: application, presentation, session, transport, network, data link, and physical. The top three layers of the OSI model: application, presentation, and session map to the application layer of the TCP/IP model.**

</details>

---

### Question 47

**Refer to the exhibit. If PC1 is sending a packet to PC2 and routing has been configured between the two routers, what will R1 do with the Ethernet frame header attached by PC1?**

- [ ] nothing, because the router has a route to the destination network
- [ ] open the header and use it to determine whether the data is to be sent out S0/0/0
- [ ] open the header and replace the destination MAC address with a new one
- [ ] remove the Ethernet header and configure a new Layer 2 header before sending it out S0/0/0

<details> <summary>💡 Show Answer</summary>

**Explanation: When PC1 forms the various headers attached to the data one of those headers is the Layer 2 header. Because PC1 connects to an Ethernet network, an Ethernet header is used. The source MAC address will be the MAC address of PC1 and the destination MAC address will be that of G0/0 on R1. When R1 gets that information, the router removes the Layer 2 header and creates a new one for the type of network the data will be placed onto (the serial link).**

</details>

---

### Question 48

**What will happen if the default gateway address is incorrectly configured on a host?**

- [ ] The host cannot communicate with other hosts in the local network.
- [ ] The host cannot communicate with hosts in other networks.
- [ ] A ping from the host to 127.0.0.1 would not be successful.
- [ ] The host will have to use ARP to determine the correct address of the default gateway.
- [ ] The switch will not forward packets initiated by the host.

---

### Question 49

**What are two features of ARP? (Choose two.)**

- [ ] When a host is encapsulating a packet into a frame, it refers to the MAC address table to determine the mapping of IP addresses to MAC addresses.
- [ ] An ARP request is sent to all devices on the Ethernet LAN and contains the IP address of the destination host and its multicast MAC address.
- [ ] If a host is ready to send a packet to a local destination device and it has the IP address but not the MAC address of the destination, it generates an ARP broadcast.
- [ ] If no device responds to the ARP request, then the originating node will broadcast the data packet to all devices on the network segment.
- [ ] If a device receiving an ARP request has the destination IPv4 address, it responds with an ARP reply.

---

### Question 50

**A network administrator is adding a new LAN to a branch office. The new LAN must support 90 connected devices. What is the smallest network mask that the network administrator can use for the new network?**

- [ ] 255.255.255.128
- [ ] 255.255.255.240
- [ ] 255.255.255.248
- [ ] 255.255.255.224

---

### Question 51

**What are two ICMPv6 messages that are not present in ICMP for IPv4? (Choose two.)**

- [ ] Neighbor Solicitation
- [ ] Destination Unreachable
- [ ] Host Confirmation
- [ ] Time Exceeded
- [ ] Router Advertisement
- [ ] Route Redirection

---

### Question 52

**A client packet is received by a server. The packet has a destination port number of 80. What service is the client requesting?**

- [ ] DHCP
- [ ] SMTP
- [ ] DNS
- [ ] HTTP

---

### Question 53

**What is an advantage for small organizations of adopting IMAP instead of POP?**

- [ ] POP only allows the client to store messages in a centralized way, while IMAP allows distributed storage.
- [ ] Messages are kept in the mail servers until they are manually deleted from the email client.
- [ ] When the user connects to a POP server, copies of the messages are kept in the mail server for a short time, but IMAP keeps them for a long time.
- [ ] IMAP sends and retrieves email, but POP only retrieves email.

<details> <summary>💡 Show Answer</summary>

**Explanation: IMAP and POP are protocols that are used to retrieve email messages. The advantage of using IMAP instead of POP is that when the user connects to an IMAP-capable server, copies of the messages are downloaded to the client application. IMAP then stores the email messages on the server until the user manually deletes those messages.**

</details>

---

### Question 54

**A technician can ping the IP address of the web server of a remote company but cannot successfully ping the URL address of the same web server. Which software utility can the technician use to diagnose the problem?**

- [ ] tracert
- [ ] ipconfig
- [ ] netstat
- [ ] nslookup

<details> <summary>💡 Show Answer</summary>

**Explain: Traceroute (tracert) is a utility that generates a list of hops that were successfully reached along the path from source to destination.This list can provide important verification and troubleshooting information. The ipconfig utility is used to display the IP configuration settings on a Windows PC. The Netstat utility is used to identify which active TCP connections are open and running on a networked host. Nslookup is a utility that allows the user to manually query the name servers to resolve a given host name. This utility can also be used to troubleshoot name resolution issues and to verify the current status of the name servers.**

</details>

---

### Question 55

**Which two functions are performed at the LLC sublayer of the OSI Data Link Layer to facilitate Ethernet communication? (Choose two.)**

- [ ] implements CSMA/CD over legacy shared half-duplex media
- [ ] enables IPv4 and IPv6 to utilize the same physical medium
- [ ] integrates Layer 2 flows between 10 Gigabit Ethernet over fiber and 1 Gigabit Ethernet over copper
- [ ] implements a process to delimit fields within an Ethernet 2 frame
- [ ] places information in the Ethernet frame that identifies which network layer protocol is being encapsulated by the frame
- [ ] Other case:
- [ ] responsible for internal structure of Ethernet frame
- [ ] applies source and destination MAC addresses to Ethernet frame
- [ ] handles communication between upper layer networking software and Ethernet NIC hardware
- [ ] adds Ethernet control information to network protocol data
- [ ] implements trailer with frame check sequence for error detection
- [ ] Other case:
- [ ] integrates Layer 2 flows between 10 Gigabit Ethernet over fiber and 1 Gigabit Ethernet over copper
- [ ] places information in the Ethernet frame that identifies which network layer protocol is being encapsulated by the frame
- [ ] implements CSMA/CD over legacy shared half-duplex media
- [ ] adds Ethernet control information to network protocol data
- [ ] applies source and destination MAC addresses to Ethernet frame
- [ ] Other case:
- [ ] enables IPv4 and IPv6 to utilize the same physical medium
- [ ] adds Ethernet control information to network protocol data
- [ ] applies source and destination MAC addresses to Ethernet frame
- [ ] responsible for the internal structure of Ethernet frame
- [ ] implements trailer with frame check sequence for error detection
- [ ] Other case:
- [ ] enables IPv4 and IPv6 to utilize the same physical medium
- [ ] applies source and destination MAC addresses to Ethernet frame
- [ ] integrates Layer 2 flows between 10 Gigabit Ethernet over fiber and 1 Gigabit Ethernet over copper
- [ ] handles communication between upper layer networking software and Ethernet NIC hardware
- [ ] responsible for internal structure of Ethernet frame

<details> <summary>💡 Show Answer</summary>

**Explanation: The data link layer is actually divided into two sublayers:**

- Logical Link Control (LLC): This upper sublayer defines the software processes that provide services to the network layer protocols. It places information in the frame that identifies which network layer protocol is being used for the frame. This information allows multiple Layer 3 protocols, such as IPv4 and IPv6, to utilize the same network interface and media.
- Media Access Control (MAC): This lower sublayer defines the media access processes performed by the hardware. It provides data link layer addressing and delimiting of data according to the physical signaling requirements of the medium and the type of data link layer protocol in use.

</details>

---

### Question 56

**The global configuration command ip default-gateway 172.16.100.1 is applied to a switch. What is the effect of this command?**

- [ ] The switch can communicate with other hosts on the 172.16.100.0 network.
- [ ] The switch can be remotely managed from a host on another network.
- [ ] The switch is limited to sending and receiving frames to and from the gateway 172.16.100.1.
- [ ] The switch will have a management interface with the address 172.16.100.1.

<details> <summary>💡 Show Answer</summary>

**Explanation: A default gateway address is typically configured on all devices to allow them to communicate beyond just their local network.In a switch this is achieved using the command ip default-gateway [ip address]

</details>

---

### Question 57

**What happens when the transport input ssh command is entered on the switch vty lines?**

- [ ] The SSH client on the switch is enabled.
- [ ] The switch requires a username/password combination for remote access.
- [ ] Communication between the switch and remote users is encrypted.
- [ ] The switch requires remote connections via a proprietary client software.

<details> <summary>💡 Show Answer</summary>

**Explanation: The transport input ssh command when entered on the switch vty (virtual terminal lines) will encrypt all inbound controlled telnet connections.**

</details>

---

### Question 58

**Match the type of threat with the cause. (Not all options are used.)**

---

### Question 59

**A disgruntled employee is using some free wireless networking tools to determine information about the enterprise wireless networks. This person is planning on using this information to hack the wireless network. What type of attack is this?**

- [ ] DoS
- [ ] access
- [ ] reconnaissance
- [ ] Trojan horse

<details> <summary>💡 Show Answer</summary>

**Explanation: A reconnaissance attack is the unauthorized discovery and documentation of various computing networks, network systems, resources, applications, services, or vulnerabilities.**

</details>

---

### Question 60

**What service is provided by HTTP?**

- [ ] Uses encryption to secure the exchange of text, graphic images, sound, and video on the web.
- [ ] Allows for data transfers between a client and a file server.
- [ ] An application that allows real-time chatting among remote users.
- [ ] A basic set of rules for exchanging text, graphic images, sound, video, and other multimedia files on the web.

---

### Question 61

**A client packet is received by a server. The packet has a destination port number of 67. What service is the client requesting?**

- [ ] FTP
- [ ] DHCP
- [ ] Telnet
- [ ] SSH

---

### Question 62

**What are two problems that can be caused by a large number of ARP request and reply messages? (Choose two.)**

- [ ] Switches become overloaded because they concentrate all the traffic from the attached subnets.
- [ ] The ARP request is sent as a broadcast, and will flood the entire subnet.
- [ ] The network may become overloaded because ARP reply messages have a very large payload due to the 48-bit MAC address and 32-bit IP address that they contain.
- [ ] A large number of ARP request and reply messages may slow down the switching process, leading the switch to make many changes in its MAC table.
- [ ] All ARP request messages must be processed by all nodes on the local network.

<details> <summary>💡 Show Answer</summary>

**Explanation: ARP requests are sent as broadcasts: (1) All nodes will receive them, and they will be processed by software, interrupting the CPU. (2) The switch forwards (floods) Layer 2 broadcasts to all ports.**

A switch does not change its MAC table based on ARP request or reply messages. The switch populates the MAC table using the source MAC address of all frames. The ARP payload is very small and does not overload the switch.

</details>

---

### Question 63

**A group of Windows PCs in a new subnet has been added to an Ethernet network. When testing the connectivity, a technician finds that these PCs can access local network resources but not the Internet resources. To troubleshoot the problem, the technician wants to initially confirm the IP address and DNS configurations on the PCs, and also verify connectivity to the local router. Which three Windows CLI commands and utilities will provide the necessary information? (Choose three.)**

- [ ] netsh interface ipv6 show neighbor
- [ ] arp -a
- [ ] tracert
- [ ] ping
- [ ] ipconfig
- [ ] nslookup
- [ ] telnet

---

### Question 64

**During the process of forwarding traffic, what will the router do immediately after matching the destination IP address to a network on a directly connected routing table entry?**

- [ ] analyze the destination IP address
- [ ] switch the packet to the directly connected interface
- [ ] look up the next-hop address for the packet
- [ ] discard the traffic after consulting the route table

<details> <summary>💡 Show Answer</summary>

**Explanation: A router receives a packet on an interface and looks at the destination IP address. It consults its routing table and matches the destination IP address to a routing table entry. The router then discovers that it has to send the packet to the next-hop address or out to a directly connected interface. When the destination address is on a directly connected interface, the packet is switched over to that interface.**

</details>

---

### Question 65

**What characteristic describes antispyware?**

- [ ] applications that protect end devices from becoming infected with malicious software
- [ ] a network device that filters access and traffic coming into a network
- [ ] software on a router that filters traffic based on IP addresses or applications
- [ ] a tunneling protocol that provides remote users with secure access into the network of an organization

---

### Question 66

**A network administrator needs to keep the user ID, password, and session contents private when establishing remote CLI connectivity with a switch to manage it. Which access method should be chosen?**

- [ ] Telnet
- [ ] AUX
- [ ] SSH
- [ ] Console

---

### Question 67

**What are the two most effective ways to defend against malware? (Choose two.)**

- [ ] Implement a VPN.
- [ ] Implement network firewalls.
- [ ] Implement RAID.
- [ ] Implement strong passwords.
- [ ] Update the operating system and other application software.
- [ ] Install and update antivirus software.

<details> <summary>💡 Show Answer</summary>

**Explanation: A cybersecurity specialist must be aware of the technologies and measures that are used as countermeasures to protect the organization from threats and vulnerabilities.**

</details>

---

### Question 68

**Which type of security threat would be responsible if a spreadsheet add-on disables the local software firewall?**

- [ ] brute-force attack
- [ ] Trojan horse
- [ ] DoS
- [ ] buffer overflow

<details> <summary>💡 Show Answer</summary>

**Explanation: A Trojan horse is software that does something harmful, but is hidden in legitimate software code. A denial of service (DoS) attack results in interruption of network services to users, network devices, or applications. A brute-force attack commonly involves trying to access a network device. A buffer overflow occurs when a program attempts to store more data in a memory location than it can hold.**

</details>

---

### Question 69

**Which frame field is created by a source node and used by a destination node to ensure that a transmitted data signal has not been altered by interference, distortion, or signal loss?**

- [ ] User Datagram Protocol field
- [ ] transport layer error check field
- [ ] flow control field
- [ ] frame check sequence field
- [ ] error correction process field

---

### Question 70

**A network administrator is adding a new LAN to a branch office. The new LAN must support 4 connected devices. What is the smallest network mask that the network administrator can use for the new network?**

- [ ] 255.255.255.248
- [ ] 255.255.255.0
- [ ] 255.255.255.128
- [ ] 255.255.255.192

---

### Question 71

**What service is provided by POP3?**

- [ ] Retrieves email from the server by downloading the email to the local mail application of the client.
- [ ] An application that allows real-time chatting among remote users.
- [ ] Allows remote access to network devices and servers.
- [ ] Uses encryption to provide secure remote access to network devices and servers.

---

### Question 72

**What two security solutions are most likely to be used only in a corporate environment? (Choose two.)**

- [ ] antispyware
- [ ] virtual private networks
- [ ] intrusion prevention systems
- [ ] strong passwords
- [ ] antivirus software

---

### Question 73

**What characteristic describes antivirus software?**

- [ ] applications that protect end devices from becoming infected with malicious software
- [ ] a network device that filters access and traffic coming into a network
- [ ] a tunneling protocol that provides remote users with secure access into the network of an organization
- [ ] software on a router that filters traffic based on IP addresses or applications

---

### Question 74

**What mechanism is used by a router to prevent a received IPv4 packet from traveling endlessly on a network?**

- [ ] It checks the value of the TTL field and if it is 0, it discards the packet and sends a Destination Unreachable message to the source host.
- [ ] It checks the value of the TTL field and if it is 100, it discards the packet and sends a Destination Unreachable message to the source host.
- [ ] It decrements the value of the TTL field by 1 and if the result is 0, it discards the packet and sends a Time Exceeded message to the source host.
- [ ] It increments the value of the TTL field by 1 and if the result is 100, it discards the packet and sends a Parameter Problem message to the source host.

---

### Question 75

**A client packet is received by a server. The packet has a destination port number of 69. What service is the client requesting?**

- [ ] DNS
- [ ] DHCP
- [ ] SMTP
- [ ] TFTP

---

### Question 76

**An administrator defined a local user account with a secret password on router R1 for use with SSH. Which three additional steps are required to configure R1 to accept only encrypted SSH connections? (Choose three.)**

- [ ] Configure DNS on the router.
- [ ] Generate two-way pre-shared keys.
- [ ] Configure the IP domain name on the router.
- [ ] Generate the SSH keys.
- [ ] Enable inbound vty SSH sessions.
- [ ] Enable inbound vty Telnet sessions.

---

### Question 77

**Which two functions are performed at the MAC sublayer of the OSI Data Link Layer to facilitate Ethernet communication? (Choose two.)**

- [ ] handles communication between upper layer networking software and Ethernet NIC hardware
- [ ] implements trailer with frame check sequence for error detection
- [ ] places information in the Ethernet frame that identifies which network layer protocol is being encapsulated by the frame
- [ ] implements a process to delimit fields within an Ethernet 2 frame
- [ ] adds Ethernet control information to network protocol data
- [ ] Case 2:
- [ ] places information in the Ethernet frame that identifies which network layer protocol is being encapsulated by the frame
- [ ] adds Ethernet control information to network protocol data
- [ ] responsible for internal structure of Ethernet frame
- [ ] enables IPv4 and IPv6 to utilize the same physical medium
- [ ] implements trailer with frame check sequence for error detection
- [ ] Case 3:
- [ ] integrates Layer 2 flows between 10 Gigabit Ethernet over fiber and 1 Gigabit Ethernet over copper
- [ ] enables IPv4 and IPv6 to utilize the same physical medium
- [ ] handles communication between upper layer networking software and Ethernet NIC hardware
- [ ] adds Ethernet control information to network protocol data
- [ ] implements CSMA/CD over legacy shared half-duplex media
- [ ] Case 4:
- [ ] applies delimiting of Ethernet frame fields to synchronize communication between nodes
- [ ] places information in the Ethernet frame that identifies which network layer protocol is being encapsulated by the frame
- [ ] adds Ethernet control information to network protocol data
- [ ] implements trailer with frame check sequence for error detection
- [ ] handles communication between upper layer networking software and Ethernet NIC hardware
- [ ] Case 5:
- [ ] applies delimiting of Ethernet frame fields to synchronize communication between nodes
- [ ] applies source and destination MAC addresses to Ethernet frame
- [ ] places information in the Ethernet frame that identifies which network layer protocol is being encapsulated by the frame
- [ ] handles communication between upper layer networking software and Ethernet NIC hardware
- [ ] adds Ethernet control information to network protocol data

---

### Question 78

**An IPv6 enabled device sends a data packet with the destination address of FF02::2. What is the target of this packet?​**

- [ ] all IPv6 enabled devices on the local link​
- [ ] all IPv6 DHCP servers​
- [ ] all IPv6 enabled devices across the network​
- [ ] all IPv6 configured routers on the local link​

<details> <summary>💡 Show Answer</summary>

**Explanation: FF02::2 identifies all IPv6 routers that exist on the link or network. FF02::1 is the target for all IPv6 enabled devices on the link or network.​**

</details>

---

### Question 79

**What are the three parts of an IPv6 global unicast address? (Choose three.)**

- [ ] subnet ID
- [ ] subnet mask
- [ ] broadcast address
- [ ] global routing prefix
- [ ] interface ID

<details> <summary>💡 Show Answer</summary>

**Explanation: The general format for IPv6 global unicast addresses includes a global routing prefix, a subnet ID, and an interface ID. The global routing prefix is the network portion of the address. A typical global routing prefix is /48 assigned by the Internet provider. The subnet ID portion can be used by an organization to create multiple subnetwork numbers. The interface ID is similar to the host portion of an IPv4 address.**

</details>

---

### Question 80

**A network administrator is designing the layout of a new wireless network. Which three areas of concern should be accounted for when building a wireless network? (Choose three.)**

- [ ] extensive cabling
- [ ] mobility options
- [ ] packet collision
- [ ] interference
- [ ] security
- [ ] coverage area

<details> <summary>💡 Show Answer</summary>

**Explanation: The three areas of concern for wireless networks focus on the size of the coverage area, any nearby interference, and providing network security. Extensive cabling is not a concern for wireless networks, as a wireless network will require minimal cabling for providing wireless access to hosts. Mobility options are not a component of the areas of concern for wireless networks.**

</details>

---

### Question 81

**A new network administrator has been asked to enter a banner message on a Cisco device. What is the fastest way a network administrator could test whether the banner is properly configured?**

- [ ] Enter CTRL-Z at the privileged mode prompt.
- [ ] Exit global configuration mode.
- [ ] Power cycle the device.
- [ ] Reboot the device.
- [ ] Exit privileged EXEC mode and press Enter .

---

### Question 82

**What method is used to manage contention-based access on a wireless network?**

- [ ] token passing
- [ ] CSMA/CA
- [ ] priority ordering
- [ ] CSMA/CD

---

### Question 83

**What is a function of the data link layer?**

- [ ] provides the formatting of data
- [ ] provides end-to-end delivery of data between hosts
- [ ] provides delivery of data between two applications
- [ ] provides for the exchange of frames over a common local media

---

### Question 84

**What is the purpose of the TCP sliding window?**

- [ ] to ensure that segments arrive in order at the destination
- [ ] to end communication when data transmission is complete
- [ ] to inform a source to retransmit data from a specific point forward
- [ ] to request that a source decrease the rate at which it transmits data

<details> <summary>💡 Show Answer</summary>

**Explanation: The TCP sliding window allows a destination device to inform a source to slow down the rate of transmission. To do this, the destination device reduces the value contained in the window field of the segment. It is acknowledgment numbers that are used to specify retransmission from a specific point forward. It is sequence numbers that are used to ensure segments arrive in order. Finally, it is a FIN control bit that is used to end a communication session.**

</details>

---

### Question 85

**What characteristic describes spyware?**

- [ ] a network device that filters access and traffic coming into a network
- [ ] software that is installed on a user device and collects information about the user
- [ ] an attack that slows or crashes a device or network service
- [ ] the use of stolen credentials to access private data

---

### Question 86

**Which switching method drops frames that fail the FCS check?**

- [ ] store-and-forward switching
- [ ] borderless switching
- [ ] ingress port buffering
- [ ] cut-through switching

---

### Question 87

**Which range of link-local addresses can be assigned to an IPv6-enabled interface?**

- [ ] FEC0::/10
- [ ] FDEE::/7
- [ ] FE80::/10
- [ ] FF00::/8

<details> <summary>💡 Show Answer</summary>

**Explain: Link-local addresses are in the range of FE80::/10 to FEBF::/10. The original IPv6 specification defined site-local addresses and used the prefix range FEC0::/10, but these addresses were deprecated by the IETF in favor of unique local addresses. FDEE::/7 is a unique local address because it is in the range of FC00::/7 to FDFF::/7. IPv6 multicast addresses have the prefix FF00::/8.**

</details>

---

### Question 88

**What service is provided by FTP?**

- [ ] A basic set of rules for exchanging text, graphic images, sound, video, and other multimedia files on the web.
- [ ] An application that allows real-time chatting among remote users.
- [ ] Allows for data transfers between a client and a file server.
- [ ] Uses encryption to secure the exchange of text, graphic images, sound, and video on the web.

---

### Question 89

**A user is attempting to access http://www.cisco.com/ without success. Which two configuration values must be set on the host to allow this access? (Choose two.)**

- [ ] DNS server
- [ ] source port number
- [ ] HTTP server
- [ ] source MAC address
- [ ] default gateway

---

### Question 90

**Which two statements accurately describe an advantage or a disadvantage when deploying NAT for IPv4 in a network? (Choose two.)**

- [ ] NAT adds authentication capability to IPv4.
- [ ] NAT introduces problems for some applications that require end-to-end connectivity.
- [ ] NAT will impact negatively on switch performance.
- [ ] NAT provides a solution to slow down the IPv4 address depletion.
- [ ] NAT improves packet handling.
- [ ] NAT causes routing tables to include more information.

<details> <summary>💡 Show Answer</summary>

**Explanation: Network Address Translation (NAT) is a technology that is implemented within IPv4 networks. One application of NAT is to use private IP addresses inside a network and use NAT to share a few public IP addresses for many internal hosts. In this way it provides a solution to slow down the IPv4 address depletion. However, since NAT hides the actual IP addresses that are used by end devices, it may cause problems for some applications that require end-to-end connectivity.**

</details>

---

### Question 91

**What would be the interface ID of an IPv6 enabled interface with a MAC address of 1C-6F-65-C2-BD-F8 when the interface ID is generated by using the EUI-64 process?**

- [ ] 0C6F:65FF:FEC2:BDF8
- [ ] 1E6F:65FF:FEC2:BDF8
- [ ] C16F:65FF:FEC2:BDF8
- [ ] 106F:65FF:FEC2:BDF8

<details> <summary>💡 Show Answer</summary>

**Explanation: To derive the EUI-64 interface ID by using the MAC address 1C-6F-65-C2-BD-F8, three steps are taken.**

Change the seventh bit of the MAC address from a binary 0 to a binary 1 which changes the hex C, into a hex E.

Insert hex digits FFFE into the middle of the address.

Rewrite the address in IPv6 format.

The three steps, when complete, give the interface ID of 1E6F:65FF:FEC2:BDF8.

</details>

---

### Question 92

**Refer to the exhibit. PC1 issues an ARP request because it needs to send a packet to PC2. In this scenario, what will happen next?**

![Question 92](question_img_8.png)

- [ ] SW1 will send an ARP reply with the SW1 Fa0/1 MAC address.​
- [ ] SW1 will send an ARP reply with the PC2 MAC address.​
- [ ] PC2 will send an ARP reply with the PC2 MAC address.
- [ ] RT1 will send an ARP reply with the RT1 Fa0/0 MAC address.​
- [ ] RT1 will send an ARP reply with the PC2 MAC address.​

<details> <summary>💡 Show Answer</summary>

**Explain: When a network device wants to communicate with another device on the same network, it sends a broadcast ARP request. In this case, the request will contain the IP address of PC2. The destination device (PC2) sends an ARP reply with its MAC address.**

</details>

---

### Question 93

**What service is provided by BOOTP?**

- [ ] Uses encryption to secure the exchange of text, graphic images, sound, and video on the web.
- [ ] Allows for data transfers between a client and a file server.
- [ ] Legacy application that enables a diskless workstation to discover its own IP address and find a BOOTP server on the network.
- [ ] A basic set of rules for exchanging text, graphic images, sound, video, and other multimedia files on the web.

---

### Question 94

**What characteristic describes adware?**

- [ ] a network device that filters access and traffic coming into a network
- [ ] software that is installed on a user device and collects information about the user
- [ ] the use of stolen credentials to access private data
- [ ] an attack that slows or crashes a device or network service

---

### Question 95

**When a switch configuration includes a user-defined error threshold on a per-port basis, to which switching method will the switch revert when the error threshold is reached?**

- [ ] cut-through
- [ ] store-and-forward
- [ ] fast-forward
- [ ] fragment-free

---

### Question 96

**Match a statement to the related network model. (Not all options are used.)**

<details> <summary>💡 Show Answer</summary>

**Explain: Peer-to-peer networks do not require the use of a dedicated server, and devices can assume both client and server roles simultaneously on a per request basis. Because they do not require formalized accounts or permissions, they are best used in limited situations. Peer-to-peer applications require a user interface and background service to be running, and can be used in more diverse situations.**

</details>

---

### Question 97

**What are two primary responsibilities of the Ethernet MAC sublayer? (Choose two.)**

- [ ] error detection
- [ ] frame delimiting
- [ ] accessing the media
- [ ] data encapsulation
- [ ] logical addressing

---

### Question 98

**Refer to the exhibit. What three facts can be determined from the viewable output of the show ip interface brief command? (Choose three.)**

![Question 98](question_img_9.png)

- [ ] Two physical interfaces have been configured.
- [ ] The switch can be remotely managed.
- [ ] One device is attached to a physical interface.
- [ ] Passwords have been configured on the switch.
- [ ] Two devices are attached to the switch.
- [ ] The default SVI has been configured.

<details> <summary>💡 Show Answer</summary>

**Explain: Vlan1 is the default SVI. Because an SVI has been configured, the switch can be configured and managed remotely. FastEthernet0/0 is showing up and up, so a device is connected.**

</details>

---

### Question 99

**Match each type of frame field to its function. (Not all options are used.)**

![Question 99](question_img_10.jpeg)

---

### Question 100

**What is the subnet ID associated with the IPv6 address 2001:DA48:FC5:A4:3D1B::1/64?**

- [ ] 2001:DA48::/64​
- [ ] 2001:DA48:FC5::A4:/64​
- [ ] 2001:DA48:FC5:A4::/64​
- [ ] 2001::/64

---

### Question 101

**Match the firewall function to the type of threat protection it provides to the network. (Not all options are used.)**

![Question 101](question_img_11.png)

<details> <summary>💡 Show Answer</summary>

**Explain:Firewall products come packaged in various forms. These products use different techniques for determining what will be permitted or denied access to a network. They include the following:**

- Packet filtering – Prevents or allows access based on IP or MAC addresses
- Application filtering – Prevents or allows access by specific application types based on port numbers
- URL filtering – Prevents or allows access to websites based on specific URLs or keywords
- Stateful packet inspection (SPI) – Incoming packets must be legitimate responses to requests from internal hosts. Unsolicited packets are blocked unless permitted specifically. SPI can also include the capability to recognize and filter out specific types of attacks, such as denial of service (DoS)

</details>

---

### Question 102

**Users are reporting longer delays in authentication and in accessing network resources during certain time periods of the week. What kind of information should network engineers check to find out if this situation is part of a normal network behavior?**

- [ ] syslog records and messages
- [ ] the network performance baseline
- [ ] debug output and packet captures
- [ ] network configuration files

---

### Question 103

**How does the service password-encryption command enhance password security on Cisco routers and switches?**

- [ ] It requires encrypted passwords to be used when connecting remotely to a router or switch with Telnet.
- [ ] It encrypts passwords that are stored in router or switch configuration files.
- [ ] It requires that a user type encrypted passwords to gain console access to a router or switch.
- [ ] It encrypts passwords as they are sent across the network.

<details> <summary>💡 Show Answer</summary>

**Explain: The service password-encryption command encrypts plaintext passwords in the configuration file so that they cannot be viewed by unauthorized users.**

</details>

---

### Question 104

**Which two statements are correct in a comparison of IPv4 and IPv6 packet headers? (Choose two.)**

- [ ] The Source Address field name from IPv4 is kept in IPv6.
- [ ] The Version field from IPv4 is not kept in IPv6.
- [ ] The Destination Address field is new in IPv6.
- [ ] The Header Checksum field name from IPv4 is kept in IPv6.
- [ ] The Time-to-Live field from IPv4 has been replaced by the Hop Limit field in IPv6.

<details> <summary>💡 Show Answer</summary>

**Explanation: The IPv6 packet header fields are as follows: Version, Traffic Class, Flow Label, Payload Length, Next Header, Hop Limit, Source Address, and Destination Address. The IPv4 packet header fields include the following: Version, Differentiated Services, Time-to-Live, Protocol, Source IP Address, and Destination IP Address. Both versions have a 4-bit Version field. Both versions have a Source (IP) Address field. IPv4 addresses are 32 bits; IPv6 addresses are 128 bits. The Time-to-Live or TTL field in IPv4 is now called Hop Limit in IPv6, but this field serves the same purpose in both versions. The value in this 8-bit field decrements each time a packet passes through any router. When this value is 0, the packet is discarded and is not forwarded to any other router.**

105. A network administrator wants to have the same network mask for all networks at a particular small site. The site has the following networks and number of devices: IP phones – 22 addresses PCs – 20 addresses needed Printers – 2 addresses needed Scanners – 2 addresses needed

The network administrator has deemed that 192.168.10.0/24 is to be the network used at this site. Which single subnet mask would make the most efficient use of the available addresses to use for the four subnetworks?

255.255.255.192

255.255.255.252

255.255.255.240

255.255.255.248

255.255.255.0

255.255.255.224

</details>

---

### Question 106

**What characteristic describes identity theft?**

- [ ] the use of stolen credentials to access private data
- [ ] software on a router that filters traffic based on IP addresses or applications
- [ ] software that identifies fast-spreading threats
- [ ] a tunneling protocol that provides remote users with secure access into the network of an organization

---

### Question 107

**A network administrator is adding a new LAN to a branch office. The new LAN must support 200 connected devices. What is the smallest network mask that the network administrator can use for the new network?**

- [ ] 255.255.255.240
- [ ] 255.255.255.0
- [ ] 255.255.255.248
- [ ] 255.255.255.224

---

### Question 108

**What are three commonly followed standards for constructing and installing cabling? (Choose three.)**

- [ ] cost per meter (foot)
- [ ] cable lengths
- [ ] connector color
- [ ] pinouts
- [ ] connector types
- [ ] tensile strength of plastic insulator

---

### Question 109

**Refer to the exhibit. What is wrong with the displayed termination?**

- [ ] The woven copper braid should not have been removed.
- [ ] The wrong type of connector is being used.
- [ ] The untwisted length of each wire is too long.
- [ ] The wires are too thick for the connector that is used.

<details> <summary>💡 Show Answer</summary>

**Explanation: When a cable to an RJ-45 connector is terminated, it is important to ensure that the untwisted wires are not too long and that the flexible plastic sheath surrounding the wires is crimped down and not the bare wires. None of the colored wires should be visible from the bottom of the jack.**

</details>

---

### Question 110

**Match the characteristic to the category. (Not all options are used.)**

![Question 110](question_img_12.jpeg)

---

### Question 111

**A client packet is received by a server. The packet has a destination port number of 143. What service is the client requesting?**

- [ ] IMAP
- [ ] FTP
- [ ] SSH
- [ ] Telnet

---

### Question 112

**What are two characteristics shared by TCP and UDP? (Choose two.)**

- [ ] default window size
- [ ] connectionless communication
- [ ] port numbering
- [ ] 3-way handshake
- [ ] ability to to carry digitized voice
- [ ] use of checksum

<details> <summary>💡 Show Answer</summary>

**Explain: Both TCP and UDP use source and destination port numbers to distinguish different data streams and to forward the right data segments to the right applications. Error checking the header and data is done by both protocols by using a checksum calculation to determine the integrity of the data that is received. TCP is connection-oriented and uses a 3-way handshake to establish an initial connection. TCP also uses window to regulate the amount of traffic sent before receiving an acknowledgment. UDP is connectionless and is the best protocol for carry digitized VoIP signals.**

</details>

---

### Question 113

**Refer to the exhibit. Which two network addresses can be assigned to the network containing 10 hosts? Your answers should waste the fewest addresses, not reuse addresses that are already assigned, and stay within the 10.18.10.0/24 range of addresses. (Choose two.)**

- [ ] 10.18.10.200/28
- [ ] 10.18.10.208/28
- [ ] 10.18.10.240/27
- [ ] 10.18.10.200/27
- [ ] 10.18.10.224/27
- [ ] 10.18.10.224/28

<details> <summary>💡 Show Answer</summary>

**Explanation: Addresses 10.18.10.0 through 10.18.10.63 are taken for the leftmost network. Addresses 192 through 199 are used by the center network. Because 4 host bits are needed to accommodate 10 hosts, a /28 mask is needed. 10.18.10.200/28 is not a valid network number. Two subnets that can be used are 10.18.10.208/28 and 10.18.10.224/28.**

</details>

---

### Question 114

**A client packet is received by a server. The packet has a destination port number of 21. What service is the client requesting?**

- [ ] FTP
- [ ] LDAP
- [ ] SLP
- [ ] SNMP

---

### Question 115

**What attribute of a NIC would place it at the data link layer of the OSI model?**

- [ ] attached Ethernet cable
- [ ] IP address
- [ ] MAC address
- [ ] RJ-45 port
- [ ] TCP/IP protocol stack

---

### Question 116

**A network administrator is adding a new LAN to a branch office. The new LAN must support 10 connected devices. What is the smallest network mask that the network administrator can use for the new network?**

- [ ] 255.255.255.192
- [ ] 255.255.255.248
- [ ] 255.255.255.224
- [ ] 255.255.255.240

---

### Question 117

**What technique is used with UTP cable to help protect against signal interference from crosstalk?**

- [ ] wrapping a foil shield around the wire pairs
- [ ] twisting the wires together into pairs
- [ ] terminating the cable with special grounded connectors
- [ ] encasing the cables within a flexible plastic sheath

<details> <summary>💡 Show Answer</summary>

**Explanation: To help prevent the effects of crosstalk, UTP cable wires are twisted together into pairs. Twisting the wires together causes the magnetic fields of each wire to cancel each other out.**

</details>

---

### Question 118

**Refer to the exhibit. The network administrator has assigned the LAN of LBMISS an address range of 192.168.10.0. This address range has been subnetted using a /29 prefix. In order to accommodate a new building, the technician has decided to use the fifth subnet for configuring the new network (subnet zero is the first subnet). By company policies, the router interface is always assigned the first usable host address and the workgroup server is given the last usable host address. Which configuration should be entered into the properties of the workgroup server to allow connectivity to the Internet?**

![Question 118](question_img_13.jpeg)

- [ ] IP address: 192.168.10.65 subnet mask: 255.255.255.240, default gateway: 192.168.10.76
- [ ] IP address: 192.168.10.38 subnet mask: 255.255.255.240, default gateway: 192.168.10.33
- [ ] IP address: 192.168.10.38 subnet mask: 255.255.255.248, default gateway: 192.168.10.33
- [ ] IP address: 192.168.10.41 subnet mask: 255.255.255.248, default gateway: 192.168.10.46
- [ ] IP address: 192.168.10.254 subnet mask: 255.255.255.0, default gateway: 192.168.10.1

<details> <summary>💡 Show Answer</summary>

**Explain: Using a /29 prefix to subnet 192.168.10.0 results in subnets that increment by 8: 192.168.10.0 (1) 192.168.10.8 (2) 192.168.10.16 (3) 192.168.10.24 (4) 192.168.10.32 (5)**

</details>

---

### Question 119

**Refer to the exhibit. The switches are in their default configuration. Host A needs to communicate with host D, but host A does not have the MAC address for its default gateway. Which network hosts will receive the ARP request sent by host A?**

![Question 119](question_img_14.png)

- [ ] only host D
- [ ] only router R1
- [ ] only hosts A, B, and C
- [ ] only hosts A, B, C, and D
- [ ] only hosts B and C
- [ ] only hosts B, C, and router R1

<details> <summary>💡 Show Answer</summary>

**Explain: Since host A does not have the MAC address of the default gateway in its ARP table, host A sends an ARP broadcast. The ARP broadcast would be sent to every device on the local network. Hosts B, C, and router R1 would receive the broadcast. Router R1 would not forward the message.**

</details>

---

### Question 120

**Match a statement to the related network model. (Not all options are used.)**

<details> <summary>💡 Show Answer</summary>

**Explain: Peer-to-peer networks do not require the use of a dedicated server, and devices can assume both client and server roles simultaneously on a per request basis. Because they do not require formalized accounts or permissions, they are best used in limited situations. Peer-to-peer applications require a user interface and background service to be running, and can be used in more diverse situations.**

</details>

---

### Question 121

**Refer to the exhibit. A network engineer has been given the network address of 192.168.99.0 and a subnet mask of 255.255.255.192 to subnet across the four networks shown. How many total host addresses are unused across all four subnets?**

![Question 121](question_img_15.png)

- [ ] 88
- [ ] 200
- [ ] 72
- [ ] 224
- [ ] 158

---

### Question 122

**Which connector is used with twisted-pair cabling in an Ethernet LAN?**

- [ ] LC conector
- [ ] SC conector
- [ ] BNC
- [ ] RJ 11
- [ ] True Answer:
- [ ] RJ 45 (true answer)

---

### Question 123

**A client packet is received by a server. The packet has a destination port number of 22. What service is the client requesting?**

- [ ] SSH
- [ ] SMB/CIFS
- [ ] HTTPS
- [ ] SLP

---

### Question 124

**What characteristic describes an IPS?**

- [ ] a tunneling protocol that provides remote users with secure access into the network of an organization
- [ ] a network device that filters access and traffic coming into a network
- [ ] software that identifies fast-spreading threats
- [ ] software on a router that filters traffic based on IP addresses or applications

<details> <summary>💡 Show Answer</summary>

**Explanation: IPS – An intrusion prevention system (IPS) monitors incoming and outgoing traffic looking for malware, network attack signatures, and more. If it recognizes a threat, it can immediately stop it.**

</details>

---

### Question 125

**What service is provided by DHCP?**

- [ ] An application that allows real-time chatting among remote users.
- [ ] Allows remote access to network devices and servers.
- [ ] Dynamically assigns IP addresses to end and intermediary devices.
- [ ] Uses encryption to provide secure remote access to network devices and servers.

---

### Question 126

**Match the header field with the appropriate layer of the OSI model. (Not all options are used.)**

![Question 126](question_img_16.jpeg)

---

### Question 127

**Refer to the exhibit. The switches have a default configuration. Host A needs to communicate with host D, but host A does not have the MAC address for the default gateway. Which network devices will receive the ARP request sent by host A?**

- [ ] i360201v3n1_275353.png
- [ ] only host D
- [ ] only hosts A, B, C, and D
- [ ] only hosts B and C
- [ ] only hosts B, C, and router R1
- [ ] only hosts A, B, and C
- [ ] only router R1

<details> <summary>💡 Show Answer</summary>

**Explanation: Because host A does not have the MAC address of the default gateway in the ARP table, host A sends an ARP broadcast. The ARP broadcast would be sent to every device on the local network. Hosts B, C, and router R1 would receive the broadcast. Router R1 would not forward the message.**

</details>

---

### Question 128

**Which wireless technology has low-power and low-data rate requirements making it popular in IoT environments?**

- [ ] Bluetooth
- [ ] Zigbee
- [ ] WiMAX
- [ ] Wi-Fi

<details> <summary>💡 Show Answer</summary>

**Explanation: Zigbee is a specification used for low-data rate, low-power communications. It is intended for applications that require short-range, low data-rates and long battery life. Zigbee is typically used for industrial and Internet of Things (IoT) environments such as wireless light switches and medical device data collection.**

</details>

---

### Question 129

**What two ICMPv6 message types must be permitted through IPv6 access control lists to allow resolution of Layer 3 addresses to Layer 2 MAC addresses? (Choose two.)**

- [ ] neighbor solicitations
- [ ] echo requests
- [ ] neighbor advertisements
- [ ] echo replies
- [ ] router solicitations
- [ ] router advertisements

---

### Question 130

**A client is using SLAAC to obtain an IPv6 address for its interface. After an address has been generated and applied to the interface, what must the client do before it can begin to use this IPv6 address?**

- [ ] It must send a DHCPv6 INFORMATION-REQUEST message to request the address of the DNS server.
- [ ] It must send a DHCPv6 REQUEST message to the DHCPv6 server to request permission to use this address.
- [ ] It must send an ICMPv6 Router Solicitation message to determine what default gateway it should use.
- [ ] It must send an ICMPv6 Neighbor Solicitation message to ensure that the address is not already in use on the network.

---

### Question 131

**Two pings were issued from a host on a local network. The first ping was issued to the IP address of the default gateway of the host and it failed. The second ping was issued to the IP address of a host outside the local network and it was successful. What is a possible cause for the failed ping?**

- [ ] The default gateway is not operational.
- [ ] The default gateway device is configured with the wrong IP address.
- [ ] Security rules are applied to the default gateway device, preventing it from processing ping requests.
- [ ] The TCP/IP stack on the default gateway is not working properly.

---

### Question 132

**An organization is assigned an IPv6 address block of 2001:db8:0:ca00::/56. How many subnets can be created without using bits in the interface ID space?**

- [ ] 256
- [ ] 512
- [ ] 1024
- [ ] 4096

---

### Question 133

**What subnet mask is needed if an IPv4 network has 40 devices that need IP addresses and address space is not to be wasted?**

- [ ] 255.255.255.0
- [ ] 255.255.255.240
- [ ] 255.255.255.128
- [ ] 255.255.255.192
- [ ] 255.255.255.224

<details> <summary>💡 Show Answer</summary>

**Explanation: In order to accommodate 40 devices, 6 host bits are needed. With 6 bits, 64 addresses are possible, but one address is for the subnet number and one address is for a broadcast. This leaves 62 addresses that can be assigned to network devices. The mask associated with leaving 6 host bits for addressing is 255.255.255.192.**

</details>

---

### Question 134

**Refer to the exhibit. If host A sends an IP packet to host B, what will the destination address be in the frame when it leaves host A?**

- [ ] DD:DD:DD:DD:DD:DD
- [ ] 172.168.10.99
- [ ] CC:CC:CC:CC:CC:CC
- [ ] 172.168.10.65
- [ ] BB:BB:BB:BB:BB:BB
- [ ] AA:AA:AA:AA:AA:AA

<details> <summary>💡 Show Answer</summary>

**Explain: When a host sends information to a distant network, the Layer 2 frame header will contain a source and destination MAC address. The source address will be the originating host device. The destination address will be the router interface that connects to the same network. In the case of host A sending information to host B, the source address is AA:AA:AA:AA:AA:AA and the destination address is the MAC address assigned to the R2 Ethernet interface, BB:BB:BB:BB:BB:BB.**

</details>

---

### Question 135

**What is a benefit of using cloud computing in networking?**

- [ ] Technology is integrated into every-day appliances allowing them to interconnect with other devices, making them more ‘smart’ or automated.
- [ ] Network capabilities are extended without requiring investment in new infrastructure, personnel, or software.
- [ ] End users have the freedom to use personal tools to access information and communicate across a business network.
- [ ] Home networking uses existing electrical wiring to connect devices to the network wherever there is an electrical outlet, saving the cost of installing data cables.

<details> <summary>💡 Show Answer</summary>

**Explanation: Cloud computing extends IT’s capabilities without requiring investment in new infrastructure, training new personnel, or licensing new software. These services are available on-demand and delivered economically to any device anywhere in the world without compromising security or function. BYOD is about end users having the freedom to use personal tools to access information and communicate across a business or campus network. Smart home technology is integrated into every-day appliances allowing them to interconnect with other devices, making them more ‘smart’ or automated. Powerline networking is a trend for home networking that uses existing electrical wiring to connect devices to the network wherever there is an electrical outlet, saving the cost of installing data cables.**

</details>

---

### Question 136

**Which two statements are correct about MAC and IP addresses during data transmission if NAT is not involved? (Choose two.)**

- [ ] Destination IP addresses in a packet header remain constant along the entire path to a target host.
- [ ] Destination MAC addresses will never change in a frame that goes across seven routers.
- [ ] Every time a frame is encapsulated with a new destination MAC address, a new destination IP address is needed.
- [ ] Destination and source MAC addresses have local significance and change every time a frame goes from one LAN to another.
- [ ] A packet that has crossed four routers has changed the destination IP address four times.

---

### Question 137

**What is one main characteristic of the data link layer?**

- [ ] It generates the electrical or optical signals that represent the 1 and 0 on the media.
- [ ] It converts a stream of data bits into a predefined code.
- [ ] It shields the upper layer protocol from being aware of the physical medium to be used in the communication.
- [ ] It accepts Layer 3 packets and decides the path by which to forward the packet to a remote network.

---

### Question 138

**What are three characteristics of the CSMA/CD process? (Choose three.)**

- [ ] The device with the electronic token is the only one that can transmit after a collision.
- [ ] A device listens and waits until the media is not busy before transmitting.
- [ ] After detecting a collision, hosts can attempt to resume transmission after a random time delay has expired.
- [ ] All of the devices on a segment see data that passes on the network medium.
- [ ] A jam signal indicates that the collision has cleared and the media is not busy.
- [ ] Devices can be configured with a higher transmission priority.

<details> <summary>💡 Show Answer</summary>

**Explanation: The Carrier Sense Multiple Access/Collision Detection (CSMA/CD) process is a contention-based media access control mechanism used on shared media access networks, such as Ethernet. When a device needs to transmit data, it listens and waits until the media is available (quiet), then it will send data. If two devices transmit at the same time, a collision will occur. Both devices will detect the collision on the network. When a device detects a collision, it will stop the data transmission process, wait for a random amount of time, then try again.**

</details>

---

### Question 139

**Which information does the show startup-config command display?**

- [ ] the IOS image copied into RAM
- [ ] the bootstrap program in the ROM
- [ ] the contents of the current running configuration file in the RAM
- [ ] the contents of the saved configuration file in the NVRAM

<details> <summary>💡 Show Answer</summary>

**Explain: The show startup-config command displays the saved configuration located in NVRAM. The show running-config command displays the contents of the currently running configuration file located in RAM.​**

</details>

---

### Question 140

**Which two commands can be used on a Windows host to display the routing table? (Choose two.)**

- [ ] netstat -s
- [ ] route print
- [ ] show ip route
- [ ] netstat -r
- [ ] tracert

<details> <summary>💡 Show Answer</summary>

**Explain: On a Windows host, the route print or netstat -r commands can be used to display the host routing table. Both commands generate the same output. On a router, the show ip route command is used to display the routing table. The netstat –s command is used to display per-protocol statistics. The tracert command is used to display the path that a packet travels to its destination.**

</details>

---

### Question 141

**What are two functions that are provided by the network layer? (Choose two.)**

- [ ] directing data packets to destination hosts on other networks
- [ ] placing data on the network medium
- [ ] carrying data between processes that are running on source and destination hosts
- [ ] providing dedicated end-to-end connections
- [ ] providing end devices with a unique network identifier

<details> <summary>💡 Show Answer</summary>

**Explanation: The network layer is primarily concerned with passing data from a source to a destination on another network. IP addresses supply unique identifiers for the source and destination. The network layer provides connectionless, best-effort delivery. Devices rely on higher layers to supply services to processes.**

</details>

---

### Question 142

**Which two statements describe features of an IPv4 routing table on a router? (Choose two.)​**

- [ ] Directly connected interfaces will have two route source codes in the routing table: C and S .
- [ ] If there are two or more possible routes to the same destination, the route associated with the higher metric value is included in the routing table.
- [ ] The netstat -r command can be used to display the routing table of a router.​
- [ ] The routing table lists the MAC addresses of each active interface.
- [ ] It stores information about routes derived from the active router interfaces.
- [ ] If a default static route is configured in the router, an entry will be included in the routing table with source code S .
- [ ] Other case:
- [ ] The netstat -r command can be used to display the routing table of a router.​
- [ ] The routing table stores information about routes derived from the active router interfaces.
- [ ] The routing table lists the MAC addresses of each active interface.
- [ ] Directly connected interfaces will have two route source codes in the routing table: C and S .
- [ ] If a default static route is configured in the router, an entry will be included in the routing table with source code S

<details> <summary>💡 Show Answer</summary>

**Explanation: The show ip route command is used to display the routing table of the router. In IPv4, directly connected interfaces will have one source code:C. The routing table stores information about directly connected routes and remote routes. An entry in the routing table with a source code of S is included if a default static route is configured on the router.**

</details>

---

### Question 143

**What characteristic describes a VPN?**

- [ ] software on a router that filters traffic based on IP addresses or applications
- [ ] software that identifies fast-spreading threats
- [ ] a tunneling protocol that provides remote users with secure access into the network of an organization
- [ ] a network device that filters access and traffic coming into a network

---

### Question 144

**Why would a Layer 2 switch need an IP address?**

- [ ] to enable the switch to send broadcast frames to attached PCs
- [ ] to enable the switch to function as a default gateway
- [ ] to enable the switch to be managed remotely
- [ ] to enable the switch to receive frames from attached PCs

<details> <summary>💡 Show Answer</summary>

**Explanation: A switch, as a Layer 2 device, does not need an IP address to transmit frames to attached devices. However, when a switch is accessed remotely through the network, it must have a Layer 3 address. The IP address must be applied to a virtual interface rather than to a physical interface. Routers, not switches, function as default gateways.**

</details>

---

### Question 145

**Match each description to its corresponding term. (Not all options are used.)**

---

### Question 146

**A user sends an HTTP request to a web server on a remote network. During encapsulation for this request, what information is added to the address field of a frame to indicate the destination?**

- [ ] the network domain of the destination host
- [ ] the IP address of the default gateway
- [ ] the MAC address of the destination host
- [ ] the MAC address of the default gateway

<details> <summary>💡 Show Answer</summary>

**Explanation: A frame is encapsulated with source and destination MAC addresses. The source device will not know the MAC address of the remote host. An ARP request will be sent by the source and will be responded to by the router. The router will respond with the MAC address of its interface, the one which is connected to the same network as the source.**

</details>

---

### Question 147

**What is an advantage to using a protocol that is defined by an open standard?**

- [ ] A company can monopolize the market.
- [ ] The protocol can only be run on equipment from a specific vendor.
- [ ] An open standard protocol is not controlled or regulated by standards organizations.
- [ ] It encourages competition and promotes choices.

<details> <summary>💡 Show Answer</summary>

**Explain: A monopoly by one company is not a good idea from a user point of view. If a protocol can only be run on one brand, it makes it difficult to have mixed equipment in a network. A proprietary protocol is not free to use. An open standard protocol will in general be implemented by a wide range of vendors.**

</details>

---

### Question 148

**Data is being sent from a source PC to a destination server. Which three statements correctly describe the function of TCP or UDP in this situation? (Choose three.)**

- [ ] The source port field identifies the running application or service that will handle data returning to the PC.
- [ ] The TCP process running on the PC randomly selects the destination port when establishing a session with the server.
- [ ] UDP segments are encapsulated within IP packets for transport across the network.
- [ ] The UDP destination port number identifies the application or service on the server which will handle the data.
- [ ] TCP is the preferred protocol when a function requires lower network overhead.
- [ ] The TCP source port number identifies the sending host on the network.

<details> <summary>💡 Show Answer</summary>

**Explanation: Layer 4 port numbers identify the application or service which will handle the data. The source port number is added by the sending device and will be the destination port number when the requested information is returned. Layer 4 segments are encapsulated within IP packets. UDP, not TCP, is used when low overhead is needed. A source IP address, not a TCP source port number, identifies the sending host on the network. Destination port numbers are specific ports that a server application or service monitors for requests.**

</details>

---

### Question 149

**Match each description with the corresponding TCP mechanism. (Not all options are used.)**

![Question 149](question_img_17.png)

---

### Question 150

**Refer to the exhibit. A company uses the address block of 128.107.0.0/16 for its network. What subnet mask would provide the maximum number of equal size subnets while providing enough host addresses for each subnet in the exhibit?**

- [ ] 255.255.255.192
- [ ] 255.255.255.0
- [ ] 255.255.255.128
- [ ] 255.255.255.240
- [ ] 255.255.255.224

<details> <summary>💡 Show Answer</summary>

**Explanation: The largest subnet in the topology has 100 hosts in it so the subnet mask must have at least 7 host bits in it (27-2=126). 255.255.255.0 has 8 hosts bits, but this does not meet the requirement of providing the maximum number of subnets.**

</details>

---

### Question 151

**A network administrator wants to have the same subnet mask for three subnetworks at a small site. The site has the following networks and numbers of devices:**

- [ ] Subnetwork A: IP phones – 10 addresses
- [ ] Subnetwork B: PCs – 8 addresses
- [ ] Subnetwork C: Printers – 2 addresses
- [ ] What single subnet mask would be appropriate to use for the three subnetworks?
- [ ] 255.255.255.0
- [ ] 255.255.255.240
- [ ] 255.255.255.248
- [ ] 255.255.255.252

<details> <summary>💡 Show Answer</summary>

**Explain: If the same mask is to be used, then the network with the most hosts must be examined for number of hosts. Because this is 10 hosts, 4 host bits are needed. The /28 or 255.255.255.240 subnet mask would be appropriate to use for these networks. ​**

</details>

---

### Question 152

**Match each item to the type of topology diagram on which it is typically identified. (Not all options are used.)**

![Question 152](question_img_18.png)

---

### Question 153

**What two pieces of information are displayed in the output of the show ip interface brief command? (Choose two.)**

- [ ] IP addresses
- [ ] interface descriptions
- [ ] MAC addresses
- [ ] next-hop addresses
- [ ] Layer 1 statuses
- [ ] speed and duplex settings

<details> <summary>💡 Show Answer</summary>

**Explanation: The command show ip interface brief shows the IP address of each interface, as well as the operational status of the interfaces at both Layer 1 and Layer 2. In order to see interface descriptions and speed and duplex settings, use the command show running-config interface. Next-hop addresses are displayed in the routing table with the command show ip route, and the MAC address of an interface can be seen with the command show interfaces.**

</details>

---

### Question 154

**A user is complaining that an external web page is taking longer than normal to load.The web page does eventually load on the user machine. Which tool should the technician use with administrator privileges in order to locate where the issue is in the network?**

- [ ] ping
- [ ] nslookup
- [ ] tracert
- [ ] ipconfig /displaydns

<details> <summary>💡 Show Answer</summary>

**Explanation: The Command Prompt command tracert will map the path from the PC to the web server and measure transit delays of packets across the network.**

</details>

---

### Question 155

**Which value, that is contained in an IPv4 header field, is decremented by each router that receives a packet?**

- [ ] Header Length
- [ ] Differentiated Services
- [ ] Time-to-Live
- [ ] Fragment Offset

<details> <summary>💡 Show Answer</summary>

**Explanation: When a router receives a packet, the router will decrement the Time-to-Live (TTL) field by one. When the field reaches zero, the receiving router will discard the packet and will send an ICMP Time Exceeded message to the sender.**

</details>

---

### Question 156

**A network technician is researching the use of fiber optic cabling in a new technology center. Which two issues should be considered before implementing fiber optic media? (Choose two.)**

- [ ] Fiber optic cabling requires different termination and splicing expertise from what copper cabling requires.
- [ ] Fiber optic cabling requires specific grounding to be immune to EMI.
- [ ] Fiber optic cabling is susceptible to loss of signal due to RFI.
- [ ] Fiber optic cable is able to withstand rough handling.
- [ ] Fiber optic provides higher data capacity but is more expensive than copper cabling.

<details> <summary>💡 Show Answer</summary>

**Explanation: Fiber optic media is more expensive than copper cabling used over the same distance. Fiber optic cables use light instead of an electrical signal, so EMI and RFI are not issues. However, fiber optic does require different skills to terminate and splice.**

</details>

---

### Question 157

**Match each description with an appropriate IP address. (Not all options are used.)**

![Question 157](question_img_19.jpeg)

---

### Question 158

**A user is executing a tracert to a remote device. At what point would a router, which is in the path to the destination device, stop forwarding the packet?**

- [ ] when the router receives an ICMP Time Exceeded message
- [ ] when the RTT value reaches zero
- [ ] when the host responds with an ICMP Echo Reply message
- [ ] when the value in the TTL field reaches zero
- [ ] when the values of both the Echo Request and Echo Reply messages reach zero

<details> <summary>💡 Show Answer</summary>

**Explain: When a router receives a traceroute packet, the value in the TTL field is decremented by 1. When the value in the field reaches zero, the receiving router will not forward the packet, and will send an ICMP Time Exceeded message back to the source.**

</details>

---

### Question 159

**Users report that the network access is slow. After questioning the employees, the network administrator learned that one employee downloaded a third-party scanning program for the printer. What type of malware might be introduced that causes slow performance of the network?**

- [ ] virus
- [ ] worm
- [ ] phishing
- [ ] spam

<details> <summary>💡 Show Answer</summary>

**Explanation: A cybersecurity specialist needs to be familiar with the characteristics of the different types of malware and attacks that threaten an organization.**

</details>