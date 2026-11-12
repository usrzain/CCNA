<!-- markdownlint-disable -->


# DHCPv6:

<PREFACE>

# LLA will always will be of subnet : fe80::/10

# fe80::<followed_By-Device-Identifier(size:64-bits)>

# Device Identifier : 

- Random Generation: generate a random 64-bit number for the interface ID
- PC takes it's 48 bit unique MAC Address, splits in half, insert ff:fe in the middle, flips the 7th bit. And then use this string as a 64 bit identifier.

<PREFACE>



# STEPS:
--------

- Client Request  : client sends a request to obtain an IP address
- Server Response : server sends an IP address from pre defined pool and sends it to client
- Config Parameter: Additional params such as lease time, DNS, domain name is provided
- Lease Renewal   : Client requests for renew their IP Address lease before it expires.


# Four Messages:
-----------------

- Solicit Message : Client sends this message to multicast address, to find out DHCPv6 servers.
  - SRC IP : Link-Local Address
  - SRC port : UDP 546
  - DST IP : multicast address ( ff02::1:2 )( which represents the group of routers present in network acting as DHCPv6 server that can provide IPv6 address )
  - DST port : UDP 547



- Advertise Message : DHCPv6 server sends this unicast message to client
  - SRC IP : link local IP of DHCPv6 server
  - SRC port : UDP 547
  - DST IP : link local IP or Client
  - DST port : UDP 546

  This message will contain:
  - Server Identifier Option : DHCP Unique Identifier (DUID) of it-self
  - Offered IPv6 address
  - prefix ( / subnet mask in CIDR notation )
  - DNS Server
  - Preference of it-self . As If any case more than one DHCPv6 server responses.


- Request Message : When client gets the Advertise message, it chooses the DHCPv6 server which will have highest preference. Once selected , then this message is sent to a multicast address ( ff02::1:2 ), requesting to assign the offered IPv6 address

  This message contains:
  - DHCP Unique Identifier (DUID) of selected DHCPv6 server ( to tell every DHCPv6 server in multicast group that I have selected this server )



- Acknowledgement Message : This message is sent by DHCPv6 server in response, acknowledging the client that please go ahead, you can use this IPv6 address.



------------

# Types of IPv6 GUA Assignment:

1. Stateless:

    - SLAAC Only:
      - <FLAG-in-RA-Message:__A=1__>
      - PC uses the RA sent by Router to all info needed for address assignment (i.e., network prefix, prefix-length, and default gateway )
      - Default gateway is always the LLA of Router sending RA.
      - Then PC creates it's own 64 bit of host ID from Random generation of strings / EUI (from MAC Address)



    - SLAAC + Stateless DHCPv6:
      - <FLAG-in-RA-Message:__O=1__>
      - RA contains info related to IPv6 config and inform PC to contact DHCPv6 stateless server for additional info
      - For network prefix & prefix length and Default gateway, PC uses the RA while for  DNS or NTP server PC contact DHCPv6 server.
      - Then PC creates it's own 64 bit of host ID from Random generation of strings / EUI (from MAC Address)



2. Stateful:

    - DHCPv6 Server:
      - <FLAG-in-RA-Message:__M=1__>
      - RA inform the PC to contact DHCPv6 server for all info except default gateway.
      - Host obtain info related to default gateway from Router RA.
      - Host contacts DHCPv6 for complete GUA IPv6 address and other info from DHCPv6 server. 
      - Here the DHCPv6 server manages a pool of addresses and assigns entire, exact 128 bit IPv6 address to PC similar to
  
------------


# SLAAC Configuration:

- enable IPV6 / SLAAC ( default ): <ipv6 unicast routing>

- enable SLAAC and Stateless DHCPv6 : <ipv6 nd other-config flag>

- enable Stateful DHCPv6 : <ipv6 nd managed-config flag>
                           <ipv6 nd prefix default no-autoconfig>

- IPv6 all nodes group : <FE80::1>

- IPv6 group for all routers : <FF02::1>


# DAD Application:

- Used for finding duplication of chosen IPv6 GUA
- sends a ICMPv6 Neighbor Solicitation (NS) message
- Destination : Solicited-node multicast address (last 24 bits of IPv6 address )
- If reply comes, duplication is here
- No reply , no duplication.
- Mostly it is unique because of use of unique MAC 
- But still performed by OS



# DHCPv6 Router Roles:

A DHCPv6 router can be

- DHCPv6 Client
- DHCPv6 Server
- DHCP Relay Agent


# Configure Stateless DHCPv6 Server:

- Enable IPv6 Routing                 : <ipv6 unicast-routing>
- Define DHCP Pool Name               : <ipv6 dhcp pool POOL-NAME>
- Configure DNS and Domain Name       : <dns-server IP>
                                        <domain-name example.com>
- Bind the DHCPv6 pool with Interface : <ipv6 nd other-config-flag>,
                                        <ipv6 dhcp server POOL-NAME>



# Configure Stateless DHCPv6 Client:

- Enable IPv6 Routing                   : <ipv6 unicast-routing>
- Configure client router to create LLA : <ipv6 enable>
- Configure client to use SLAAC         : <ipv6 address autoconfig>
- Verify IP on client



# Configure Stateful DHCPv6 Server:

- Enable IPv6 Routing                   : <ipv6 unicast-routing>
- Define DHCP Pool Name                 : <ipv6 dhcp pool POOL-NAME>
- Define Address prefix                 : <address prefix ADDRESS-PREFIX>
- Configure DNS                         : <dns-server DNS-IP>
- Configure Domain Name                 : <domain-name DOMAIN-NAME>
- Bind the DHCPv6 pool with Interface   : <ipv6 nd managed-config-flag>,
                                          <ipv6 nd prefix default no-autoconfig>
                                          <ipv6 dhcp server POOL-NAME>



# Configure Stateful DHCPv6 Client:

- Enable IPv6 Routing                   : <ipv6 unicast-routing>
- Configure client router to create LLA : <ipv6 enable>
- Configure client to use SLAAC         : <ipv6 address dhcp>
- Verify IP & other info on client



# DHCPv6 Server Verification commands:

- <show ipv6 dhcp pool>
- <show ipv6 dhcp binding>





# Configure DHCPv6 relay Agent:

- <ipv6 dhcp relay destination IPV6-ADDRESS INTERFACE-NAME>


# Verify DHCPv6 Relay Agent:

- <show ipv6 dhcp interface>
- <show ipv6 dhcp binding>
- <ipconfig /all>




-------------------------------------------------------------------
=======


# First Hop Redundancy Protocol ( FHRP )

- It's Cisco Proprietary is Hot Stand By Router Protocol (HSRP)

- In LAN Networks If default gateway gets down, then no other hope is left.

- For  this reason a <Virtual_Router> is created by combining two or more routers. That If  router fails then the standby can take over.
- A protocol  like FHRP is used to identify that which router will remain active & which will standby and active router's MAC address will be used for layer 2 forwarding.


# Flow or working :

- Active router will keep sending Hello multicast Virtual Router group .
- if a standby router stop seeing Hello from active router , it will hold for 10 sec
- After the hold time of 10 sec, the standby router becomes the active one. and becomes the owner of 
  - VIP = which is assigned by the admin
  - Virtual MAC : It is created depends upon type of redundancy protocol is in use:
      - For HSRPv1 , virtual MAC : <0000.0C07.ACxx>, where xx is the group of which the HSRP is part of .
- So, when the standby router takes over, it will be connected to switch on different port
- So, it will need to tell the switch that the VIP to Virtual MAC address binding should be done via the port on which I am connected.
- In this scenario, Standby router which has become the active one, send a <Gratuitous_ARP>, which is a broadcast frame, telling switch to learn the Virtual MAC on this new port.



# List of Options available for FHRP:


=======

# HSRP :

- It is Cisco proprietary
- Active router is elected based on factors:
  - The Router having highest priority. If priority matches then the router with highest IPv4 address
  - Default priority is 100
  - <standby> <priority> 0-255

- By default If a router comes UP and find that the router which is already a active router have lower priority than it self. It will have no chance to speak for itself and become stand by router in HSRP Group
- But If we want that If the router finds that It have more priority than the current one. And it should speak for it's right to become the Active router, so we configure on the router below command telling to speak up for your right.
  - <standby> [group] <preempt>
- If preempt is disabled, then the router becoming online first will become the active router if not other routers are online during election process.


# HSRP States

- Router transitions through 6 states from startup to active state:

<Initial>: When an interface has just come UP, no HSRP  has been configured on it yet
<Learn>:   HSRP has just been configure, listens for HELLO Messages but does not know the Virtual IP Address
<Listen>:  From the Hello listened, the router now knows the Virtual IP. But until now the Router is neither active / standby. It just passively monitor HELLOs.
<Speak>:   Router actively sends the HELLO messages and participates in election for active and standby roles.
<Standby>: Router acts as a designated backup on active router. Periodically sending hello messages and waiting to take over If active router fails.
<Active>:  Router currently forwarding Data packets sent to Virtual MAC and Virtual IP , while sending HELLO Messages to tell standby router that I am alive.



Configure
Listen Hello 
Get VIP
Send Hello
If gets that I am Standby then periodically sends HELLO and wait for active to fail
If router is active, then periodically sends HELLO


=======

# HSRP Timers:

- Hello Timer : 3 seconds
- wait time : 10 seconds  for which the standby router wait before announcing the gratuitous ARP that I have become the active router now.



# HSRP for IPv6:

- HSRP IPv6 group will have Virtual MAC Address derived from the HSRP Group Number
- HSRP IPv6 group will have Virtual IPv6 Link Local Address derived from HSRP Virtual MAC Address derived above.



----------------------------

# HSRP for IPv6:

- For this a HSRP Group Number is defined
- HSRP IPV6 group virtual MAC address is derived from HSRP Group number
- HSRP Virtual Link Local Address is derived from Virtual MAC Address


# Virtual Router Redundancy Protocol 2 ( VRRP 2 )

- Non-proprietary Election protocol used for same redundancy within a LAN
- One router will be Virtual Router Master, rest of them will be Backups


# VRRP 3:

- Supports both IPv4 & IPv6
- More scalable than VRRP 2



# Gateway Load Balancing Protocol ( GLBP )

- It is also cisco proprietary for load balancing along redundancy.
- In this all routers in a Group are a single Virtual Router Group
- All routers forward traffic
- Means this is mostly can be used for load balancing, while concurrently providing gateway redundancy.
- Provides one VIP and multiple Virtual MAC.


# GLBP v6 

- Just Cisco Proprietary of GLBP for IPv6 environments


# ICMP Router Discovery protocol (IRDP)

- Legacy FHRP solution
- Allows host to locate 4 routers to provide IPv4 connectivity to non-local IP networks







-----------------------------------------------------------------------------


# Packet Forwarding in Routers:
--------------------------------
- The speed of packet forwarding depends on how fast a packet can be encapsulated into a Layer 2 header and trailer.


# Three (3) Packet Forwarding Mechanisms:
-----------------------------------------


# Process switching:

- Older mechanism
- When a packet arrives, goes to control plane.
- In control plane CPU matches the Destination in Routing table and find appropriate egress interface & forward the traffic.
- Note: This is being done for each & every packet 
- That's why it is slow and rarely implemented nowadays.


# Fast Switching:

- In this, router maintains a <fast-switching-cache>, which hold the info of process switching of packet recently done.
- When a new packet arrives, the first time forwarding is done by using process switching but, then the result is stored in fast switching cache which is being used whenever similar destination arrives without CPU intervention.
- It makes the switching a bit faster



# Cisco Express Forwarding ( CEF ) :

- Default Cisco IOS packet forwarding mechanism
- It builds 2 table 
  - Like Fast switching , it builds a Forwarding Information Base.
  - Adjacency table.

- Table entries are not packet triggered, rather network topology change triggered.

- FIB is an optimized version of Routing Information Board ( RIB ).It holds
  - Target Network
  - Next Hop IP Address
  - Egress interface
  - Pointers referring to entries to use in adjacency table

- Adjacency Tables holds:
  - Destination MAC Address
  - Pre-computed Layer 2 header and trailer
  - Encapsulation Types
  - It is built & maintained using ARP or direct connection info




# Routing :

- Lower the <AD> more trust worthy the route is
- EIGRP AD : 90
- OSPF AD : 110
- Directly connected AD : 0
- Static : 1



# Dynamic Routing Evolution:

- RIPv1 was introduced in 1988 , but some of it's basic algorithms were used in ARPANET in 1969
- RIPv2
- OSPF & IS-IS
- Cisco Developed IGRP ( Interior Gatewway Routing Protocol ), which was later replaced by EIGRP
- BGP which was a successor of Exterior Gateway Protocol (EGP)  


# Link State RP:

- OSPF v2 , OSPF v3
- IS-IS , IS-IS for IPv6


# Distance Vector RP:

- RIPv2, RIPng
- EIGRP, EIGRP for IPv6


# Path Vector RP:

- BGP-4
- BGP-MP

-------------------------------

# Routing Protocols & Metric:

- RIP:
  - Hop Count
  - max = 15

- OSPF:
  - Cost : Cumulative BW from SRC to DST
  - Faster links are assigned lower cost 

- EIGRP:
  - Metric based on slowest BW and delay values
  - Includes load and reliability into metric calculation


# Equal Cost Load balancing is implemented by default by Dynamic Routing Protocols


# Important Info:
-----------------

# show ip route :

- S 10.0.0.0[1/0] via 172.16.2.2

  1 = AD
  0 = Metric


# Two common types of Static Routes:

- Default Static Route
- Static route to a specific network


# Which Address and Prefix Length is used in IPv6 for Default Route:

- ::/0

# When a frame comes to a Router and Router does not know the MAC Address of destination, what it will do?

- send an ARP request to find out the MAC Address of the destination IP address.

# Sequence of order how a Router will perform steps when it receives a packet

- Check destination MAC ( to verify that the frame is intended for the port it has received on router )
- Check Ether Type field ( to verify what layer 3 protocol is encapsulated in frame )
- Frame is de-encapsulated
- Destination IP address is examined to determine where should it forward the packet.

# When the outgoing interface of router goes down, what will happen to static route associated with it?

- The static route will be removed as no longer valid.


--------------------------------------

# Static Routing:
-----------------

# IPv4:
- network address
- subnet mask
- ip address of next HOP
- exit Interface
- Exit Interface and next HOP IP
- Distance ( Typically used to configure Floating Static routes by setting Administrative distance )

# IPv6:
- ipv6-prefix
- /prefix-length
- ipv6-address of next HOP
- exit Interface
- Exit Interface and ipv6-address of next HOP
- Distance ( Typically used to configure Floating Static routes by setting Administrative distance )


# Fully Specific Static Route:

- When two routers are connected via serial cable, this will show that they must be point-to-point connected
- But when two routers are said to be connected via Ethernet cable, this will mean that there is a possibility that they are directly point-to-point connected or rather there is a multi-access device such as hub or switch which would be causing multiple routers to share same multi-access network. In this condition, it is use full to specify the next-HOP address along side the exit interface.
  - ip route 172.16.0.0 255.255.0.0 GigabitEthernet 0/0/0 172.16.2.2



# Standard Static Routes:
-------------------------

- The route will specify only next-HOP IP address.


# Default Static Routes:
------------------------

- This route is used as a last resort route.
- More-over this route is used when connecting an Edge Router to service provider network / or a stub router ( which has only one upstream neighbor router )

- /0 for IPv4 network, means none of bits must match between destination IP and and route in Routing table
- ::/0 for IPv6 network, means none of bits must match between destination IP and and route in Routing table





# Floating Static Routes:
-------------------------

- These routes are configure as a backup route, If primary route is not available.
- Floating static routes are configured with higher Administrative distance.
- The router will choose the route with lowest AD. Means first the primary route will be used, If primary route is not present then this floating route will be used.


# Summary Static Routes:
------------------------



# Static Hosts Routes:
----------------------

- Means configuring the static route with /32 and /128 ( for IPv6 ) address.

- They are either learned automatically 
  - L and C shows this

- They are statically configured with /32 subnet mask or /128 prefix mask.

- 



------------------------------------------------------------------------