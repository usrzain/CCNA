<!-- markdownlint-disable -->


- Fault Tolerance
- Scalability
- QoS
- Security


# What Network protocols do:

- Encoding : converting the message into some other form for transmission.Means into binary or wave form

- Formatting and Encapsulating : converting the message into correct format/structure depending upon type of message and channel of transmission.
  
- Message size : Among sender and receiver, the size of message must match.

- Message timing :
    - Flow rate 
    - response timeout : If no response comes int a specific time period then what action to take.
    - Access method : When two devices want to talk to each other than, so they should know that when one is talking either the other one is listening/sending. Because If both will only talk and no one will listen then the data will be lost.
    - Message Delivery Options : 
      - Unicast
      - Multicast
      - Broadcast





# Internet Layer Protocols:

- Internet Protocol:
  - IPv4
  - NAT
  
- Messaging
  - ICMPv4 : This protocol is used by network devices to report errors to end user devices to tell them exactly why the packet fails to reach it's destination
  - ICMPv6 : just includes the IPv6
  - ICMPv6 ND : for address resolution and finding of duplicate address

- Routing Protocols:
  - BGP
  - OSPF
  - IS-IS
  - EIGRP


# Network Access Layer Protocols:

- Address Resolution
  - ARP

- Data Link Protocols:
  - Ethernet
  - W-LAN

# Ethernet Frames
- Data Link Layer has 2 sub layers:
  - LLC : 
    - It allows multiple upper layer protocols ( IPv4 / IPv6 ) to use single network interface
    - Flow control
    - Error Checking : identifies corrupted / missing frames at link level

  - MAC:
    - How and when devices gain access to shared medium (CSMA/CD, CSMA/CA)
    - Adding MAC Headers / trailers 

- Minimum Ethernet frame size : 64 Bytes
- Maximum Ethernet frame size : 1518 Bytes
- Ethernet frame less tha 64 Bytes is called as   "collision fragment" or "runt frame" and get automatically discarded by receivers.
- Frames more than 1500 Bytes are called "jumbo" or "baby Giant frames".
- Frames less than minimum or greater than max are  automatically discarded by receivers.


# Ethernet frame Fields

- Preamble & Start Frame Delimiter ( SFD ) ---> 8 bytes. Tell the receiver end to be ready fro frame.
- Dest MAC Address ----> 8 bytes
- SRC MAC Address ----> 8 bytes
- Type / Ether-type / length ----->  2 byte  , holds info of upper layer protocol.
- Data ----> 46 - 1500 bytes
- Frame Check Sequence ( FCS ) -----> 4 Bytes , for error detection. it uses CRC that it includes CRC or Data , receiving end also calculates CRC If CRC matches than OK otherwise frame is dropped because data has been corrupted.







# Physical Layer characteristics:

- Bandwidth : no of bits a medium can transfer

- Throughput : actual no of bits send by the medium which is always less than B/W

- Good-put : The bo of bits transferred of actual Data removing over-head like acknowledgements, session management packets etc. it is always less than Throughput



# Copper Cabling:

- UTP : Unshielded Twisted pair, the eight wires are twisted in 4 pairs. They are connected with RJ-45 connected.

- STP : Shielded Twisted pair, same as UTP but has a special outer layer or wrapping which is used to protect from EMI and RFI. It uses a special type of connector called as STP data connecters.


# Categories of Copper Cabling :

- It means that according to categories, how much B/W a type can support:

  - CAT-3 : supports voice communication only
  - CAT-5 / CAT-5e : used for Data transmission, support up-to 1000 Mbps
  - CAT-6 : used for Data transmission, support up-to 10 Gbps
  - CAT-7 : used for Data transmission, support 10 Gbps
  - CAT-8 : used for Data transmission, support 40 Gbps



# Characteristics of IP:

- Connection-less : no connection already established with destination
- Best Effort : IP packet delivery is un-reliable
- Media Independent


# IPv4 Packet Header Fields:

- Size : 4 Bytes
- Version : IPv4 or IPv6
- TTL : 
- Protocol : it indicates the type of upper layer protocol from which the data belongs to, which will help the receiving point to handle the packet to correct protocol.
- Header checksum : used to detect corruption
- Source IP
- Destination IP


# IPv4 in Detail:

- 255.255.255.252  is called subnet mask
- /30 is called prefix length
- If we will AND each bit of IPv4 address with it's subnet mask, then we can know the network address.
- 






# IPv4 In Detail:

- No Broadcast packets
- Limited Broadcast uses a IPv4 address : 255.255.255.255 and Router drops it by default
- Directed broadcast uses last address of subnet and a packet is only sent to all of the hosts in network and can be routed to other networks but dropped by router
- Broadcast domain means hosts all on same network segment.

- Multicast packet can use destination IP as : 224.0.0.0 - 239.255.255.255
- Public IPv4 ranges:
  - 10.0.0.0/8    : 10.0.0.0 -10.255.255.255
  - 172.16.0.0/12 : 172.16.0.0 - 172.31.255.255
  - 192.168.0.0/16 : 192.168.0.0 - 192.168.255.255

- Loopback addresses in host can be used to check either TCP/IP Config is operational or not.
- Link Local Address is known as APIPA which is given by host to itself after failure DHCP attempt. It has range : 169.254.0.0/16 > 169.254.0.1 to 169.254.255.254

- Class A : 0.0.0.0/8 ------ 127.0.0.0/8
- Class B : 128.0.0.0/16  ------ 191.255.0.0/16
- Class C : 192.0.0.0/24  ------ 223.255.255.0/24

- IANA manages and allocates blocks of IPv4 and IPv6 address across   Reginal Internet Registries (RIR)

----------------

# Issue with IPv4:

- IP address space depletion.





# IPv6 Packet Header Fields

- Version : IPv4 or IPv6
- payload : length of data encapsulated
- Next header : indicates which upper layer protocol's this payload is
- Hop Limit : same as TTL but no checksum is calculated in IPv6
- Source IP
- Destination IP 



# IPv6 in Details

- Length : 128 bits
- 32 Hex values
- 1 Hex value comprises of 4 binary values

# RULES in IPv6 representation:

  - Omit leading zeros, but don't omit trailing zeros of a 
  - If consecutive HEX 0 values are in between the Address, it can be replaced with ::

# IPv6 Address Types:

- Unicast
- multicast : single IPv6 packet is sent to multiple destinations
- Anycast : a single IPv6 is assigned to multiple devices


# IPv6 prefix Length:

- No subnet mask as IPv4
- Just prefix length which ranges from 0 - 128
- denoted by / 
- normally /64 is used


# Types of IPv6 address:

- A devices can have 2 types of IPv6 Address
  - Global Unicast Address ( GUA ) : It is similar to public IPv4 , globally unique and internet routable address. It can be configured / assigned dynamically
  - Link Local Address ( LLA ) : Like a private IPv4 which is not internet routable, rather for a single link local / network, it is address via which it talks to hosts on same network


# More About GUA:

- Parts of GUA:
  - Global Routing Prefix : which is assigned by ISP and via which it keeps track normally it is /48 part of whole 128 digits
  - Subnet ID : It is part of address which can be used by organization to create different subnets, usually it is / 32 
  - Interface ID : Host part , it is part which is assigned to hosts: /64 is common.unlike IPv4 and device can have host ID > All 0s and All 1s because there is not network ID / Broadcast IP
- Configuration:
  -  ipv6 address 2001:db8:acad:1::1/64
-  

# Dynamic Addressing Of GUA:

- It uses 2 ICMPv2 messages for dynamic addressing:
  
  - ICMPv6 Router Solicitation (RS) messages  ------->  It is sent by host enabled with IPv6 addressing, to find out IPv6 enabled router on link local ( network )

  - ICMPv6 Router Advertisement ( RA ) messages  -------->  It is sent by Router enabled with IPv6 in response with RS to tell host:
      - Network prefix & Prefix Length
      - Default Gateway address
      - DNS Address and domain name 



# Methods of RA Messages:

# SLAAC :

- It tells that use all info from RA message and generate your own address
- In this method host does not have to rely on DHCPv6 server
- It is stateless because no central server to keep record.
- It sends Prefix length and Interface ID ( Using EUI-64 process ) is created by client itself.


# SLAAC & Stateless DHCPv6 : 
- It tells that use all info from RA message and generate your own address
- But for DNS and domain name you will need to ask DHCPv6 server
- The RA LLA is used as default gateway ( as a next hop for host )


# Stateful DHCPv6 :
- It tells to use router's LLA as a next hop
- But for all other things like GUA, DNS, domain name, prefix length , you will need to contact DHCPv6 server.
- Then PC sends a DHCPv6 Solicit message to all DHCPv6 server for necessary information.




# How client generates it's own interface ID:

- EUI-64 Process:
  - divide the MAC address into two portions : OUI ( vendor code assigned by IEEE ) and Device identifier
  - insert hexadecimal value fffe in between the center of both.
  - Convert the first 2 Hex digits to binary and flip the 7th bit

# But as EUI-64 contains MAC address details of actual device, so users a concerned that they can be traced back which is why random generated interface ID can be used.


- Randomly generated 64-Bit Interface ID:
  - Once generated then it can use DAD to verify either this has been used already or not

------------------------------------------------------

# More About LLA:

- If a host even does not have GUA, it can still have LLA and talk to Default Gateway
- Address structure : 1111 1110 10XX XXXX
- LLA can have a range of :     >   1111 1110 1000 0000 (fe80) - 1111 1110 1011 1111
- It is always /10 network which is same

- Devices randomly generates values or using Extended Unique Identifier ( EUI ), which uses the MAC address 
- Configuration:
  -   ipv6 address fe80::1:1 link-local


# Dynamic Addressing For LLA in windows:

- Use EUI-64:
  - generate a EUI-64 Interface ID
  - Then append fe80:: as a prefix

- Use Random generation of whole Interface ID and then append the network prefix

# Dynamic Addressing For LLA in Routers:

- Serial interfaces does not have MAC address, so for them the MAC address of 1st physical G0/0/0 is used , that why LLA of Serial interface and g0/0/0 is same.it is possible because link local address must be unique on that link.
-------------------------------------------------------



# Multicast IPv6 Addresses:

- Multicast IPv6 can only be destination address not sources address.

- Types of Multicast Address:
  - Well known multicast address
  - Solicited node multicast address


---------------------------------------------------------


# Types Of Memory in Network devices:

- RAM:
    - Volatile ( means lost when powered OFF)
    - Read/Write (Fast)
    - Stores active running config, routing tables, packet buffers, ARP table


- ROM:
    - Non-Volatile
    - Read ( Only )
    - Stores POST codes, bootstrap instruction


- Flash:
    - Non-Volatile
    - Read / write
    - stores Firmware


- NVRAM ( Non-Volatile RAM ):
    - Non-Volatile
    - Read / Write
    - Stores the startup config loaded during boot.

# ARP ( Address Resolution Protocol )

- ARP Table is stored temporarily in RAM 
- Windows PC will remove ARP entry within 15 and 45 seconds.
- Switch uses a technique called as Dynamic ARP inspector (DAI) to tackle ARP Spoofing attacks.
- ARP message is encapsulated in an Ethernet Frame.








# Neighbor Discovery Protocol ( Used by IPv6 )

- It uses Neighbor Solicitation Message for discovery of neighbor's MAC Address corresponding to ARP which uses ARP Request and reply.

- It creates a ICMPv6 message
  - Type : 135
  - TARGET : DST IPv6 Address


# To resolve IPv6 --> MAC Address, two ICMPv6 messages are used:

- Neighbor Solicitation ( Like ARP Request )
- Neighbor Advertisement ( Like ARP Response )

---------------


# For DST IPv6 address in ICMPv6 messages, a special type of multicast address is used called as "Solicited Node" multicast

# It is like this : FF02::1:FFXX:XXXX

# XX:XXXX  is the last 6 digits of IPv6 Destination Address.

---------------

  - Link Layer Address : SRC MAC Address




- The ICMPv6 message is encapsulated in IPv6 Header
- Ethernet Header
  - SRC MAC
    - DST MAC : 3333:XXXX:XXXX
    - XXXX:XXXX are last 8 digits of multicast IPv6 address derived above.
- Ethernet Frame is sent on the wire.


# ------> Response to ND Message:

- Destination device configured with IPv6 will generate a Solicited Multicast Node address:
  - FF02::1:FFXX:XXXX (1)     -->    XX:XXXX will be from it's IPv6 address
  - 3333:XXXX:XXXX    (2)      -->    XXXX:XXXX will be from it's IPv6 address

- When ND Message's frame come to it's NIC, it compares the DST MAC Address with it's (2), If matches then it will process the frame and reply otherwise no.




# Types of ND Protocol Messages:

- Neighbor Solicitation : Used of IPv6 to MAC Address resolution 





# IPv6 Stateless Address Auto-Configuration ( SLAAC ):

- TYPES OF ADDRESS ASSIGNMENT:
  - State-full :
        A server or device keeps track of
        - Each assigned Address ( means expiration etc)
        - availability of pool address
        - resolves duplicate address
  
  - State-Less :
        No server keeps track of anything like state-full assignment
        - Rather a device is generate an IPv6 Address.
        - Then check for duplication, run a Duplicate Address Detection ( DAD )
        - If IPv6 address comes already in use, then generate a new one.


# How does SLAAC works:


# STEP # 1:
-----------


- Before assignment of any IPv6, just the device can communicate with local network over L3 network it auto-configures a link local address.

- auto-configuring a link local address is by combining:

    - Link Local Prefix   :  FE80::/64

          &

    - EUI-64 interface identifier, generated from the interface's MAC address

      - Take a interface's MAC Address : 7007.1234.5678
      - Insert 0xFFFE in the middle of Address   ---------->   7007.12FFFE23.5678
      - Flip the 7th Bit of MAC address          ---------->   7207.12FFFE23.5678

          Now the EUI-64 Interface Identifier is :   7207.12FFFE23.5678



    After combination with FE80::/64
  
# Link Local Address is FE80::72:07:12:FF:FE:23:56:78 / 64




# STEP # 2 : Node performs DAD on Local Segment
-----------------------------------------------

- It needs to find that, the Link Local Address which has been generated above is not duplicate.
- Then DAD is needed to be run 
- DAD uses a special Type of Address : "Solicited Node Address"
  - FF02::1:FFxx:xxxx
  - xx:xxxx are last 6 digits of Link Local IPv6 Address
  - Now host joins auto-generated solicited-node multicast group

- Now the last 6 digits of Link Local address : 23:56:78
- After combining with Soliciteddress : FF02:0000:1:FF23:5678



- Now If any other Device on Local segment having same Link Local address, then it must be be listening for messages on the solicited-node multicast group auto-generated from this address - FF02::1:FF34:5678


- Create a ICMPv6 Header:
  - Type
  - Target : Link Local Address :FE80::72:07:12:FF:FE:23:56:78 / 64
- IPv6 Header :
  - Dst : Solicitied Address : FF02:0000:1:FF23:5678
  - SRC : Empty


- If any one listening on same multicast group FF02:0000:1111:FF34:5678, will response the I have this MAC Address , the host will know that it should not use this link local address.Otherwise keep going.



# STEP # 3: Node sends Router Solicitation Message:
-----------------------------------------------

- Now using link local address, host can start the process of auto-configuring a global unique unicast address using SLAAC.






------------------------------------------------------


# How ICMP works:

- It is used to check
  - host reachability
  - Destination / Service unreachable
  - to send a TTL message to sender that message cannot be delivered further. it is used by trace-route tool
  - 


# Out Of Band Management Access : Console Access
# In Band Management Access : Telnet , SSH, HTTP



--------------------------------------------------------


# Transport Layer

- Tracking converstaion
- segmentation and reassembling of data
- add header info
- identifying applications
- conversion multiplexing


# TCP Features:

- Session establishment
- Reliable delivery
- Provides same order delivery
- support flow control : If it know that a data flow is more than resources, it can ask to slow down the flow rate


# TCP Deep Dive

- Adds a 20 Bytes Header
  - Src Port
  - DST port
  - Seq Number
  - Ack Number
  - Header Length
  - Reserved
  - Control bits
  - window size
  - checksum
  - urgent

-------> HTTP, FTP, SMTP SSH


# UDP Deep Dive

- 8 Bytes Header
  - Src Port
  - DST port
  - Length
  - checksum

-------> DNS, DHCP, VoIP, SNMP, TFTP


# Ports:

- Well Known ports : 0 to 1023
- Registered : 1024 - 49151
- Private / Dynamic : 49152 - 65535

# TCP Session Termination:


# Control bits is 6 bit field which can be used to raise flags
- When a client has no more data , it sends a FIN Flag 






-----------------------------------

# Presentation Layer:

- Formatting
- Compressing
- Encrypting of Data


# GET : get the Data
# POST : send text Data
# PUT : send images


# Email Protocols:

- SMTP:
  - port : 25
  - used to send Email

- POP:
  - port : 110
  - used to retrieve Email
  - whole Emails are downloaded


- IMAP:
  - port : 
  - used to retrieve Email
  - copies of Email are downloaded


----------------------------

# DNS

# Types Of records stored in DNS:

- A  -----> end device IPv4 Address
- NS -----> Authoritative Name Server
- AAAA ------> end device IPv6 Address
- MX  -------> Mail exchange record

# DNS Heirarchy:

-  TLD represents the type or org/country/origin
  
  - .com  --> business / industry
  - .org  --> non-profit org
  - .au   --> Australia
  - 




# Virus is itself is a file which replicates it self
# Worm is itself a program which does not need file 
# Trojan horses is a piece of software which looks OK


-------------------------------

# PING Response:

! : OK
. : waiting for reply
U : destination unreachable