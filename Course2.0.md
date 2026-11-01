<!-- markdownlint-disable -->

# Subnet trick

58.27.254.146/255.255.255.240

- subtract the last octet ( not 255, nor 0 ) with 256
  - 256-240 = 16

- 16 will be the block size
- Divide the IP address with block size:
  - 146/16 = 9.125
- Exempt the decimals = 9
- multiply 9 with 16 => 9 x 16 = 144

- Network ID : 58.27.254.144/28






# Switch Light

- Amber : system is receiving power but not working properly
- Green : system is operating normally     

# Switch Boot-up Sequence:

- POST Execution
- It will make a search into 'Flash' memory to find  OS
- Check either the OS is valid or not
  - Load the OS
  - Uncompress the OS
  - copy to RAM
  - 


# Configuring SSH 

- show ip ssh               -->        Verify which ssh version device supports
- ip domain cisco.com
- crypto key generate rsa   -->        generate RSA key pairs
- username <username> secret <password>
- line vty 0 15             -->        on line from 0 - 15 
- transport input ssh       -->        only accept the connection for SSH
- login local               -->        use local database for authentication
- exit
- ip ssh version 2




- enable secret <password > -->        set password for enable
- line console 0            -->        on line for console connection 
- password <password >      -->        use this configured password


- service password encryption  -->     encrypt all password which will be shown in configuration



-----------------------------------------



# By default system captures the last 10 commands lines in it's buffer

# show history displays contents of buffer

# 


------------------------------------------

# Switch forwarding Methods:

- Cut through switch:
--------------------

  - As soon as it receives the first "6" Bytes of frame, it decide the egress interface using MAC address table
  - Then on the basis of decision, it forwards the remaining bytes out of egress interface
  - No error checking is done 

- Store and Forward:
--------------------

  - When a frame is received on a port, it puts the frame byte-by-byte into memory buffer
  - It waits until complete frame has been completed
  - Then FCS value in trailer is separated from entire frame
  - Rest of frame ( excluding FCS ) is used to create a new FCS value using CRC algorithm
  - Then old and new FCS values are compared
  - If both are different, then error is detected and frame is discarded.
  - Other wise it will simply make a lookup into MAC address table / CAM table & will forward the frame out of decided egress port.


- Fragment-Free:
----------------

  - It waits to receive exactly 64 Bytes . Why 64 Bytes because network collision happens in first 64 bytes of transmission.
  - Checks through 64 bytes that either the frame has been fragmented or not due to collision.
  - Once 64th byte has been checked safely without collision, then switch immediately selects the egress port and begins streaming the frame out while remaining bytes are still arriving.



# Collision Domain:

- It simply means the segments in which collisions of frames can happen which will cause frame fragmentation / errors


  
- Why collision can happen ?
  - Because If both end starts sending data at the same time, their frames can collide and get fragmented
  - In legacy Hubs, this used to happen that all devices were using same segment. Then If talking at same time collision could happen.So , devices on same collision domain must be aware that when it should talk when no other one is talking and using the segment.


- How collisions happens in switches.
  - If on a port switch is operating on full duplex ( can send and receive data at same time ), will keep sending data all time. & does not listen for incoming data
  - While if PC is set at half duplex, then it will keep waiting for switch to stop sending data so it will send data because it senses that collision might happen if it will send the data, even switch can handle both incoming and sending data.


# On switch, on every port a collision domain can happen
# But as when a broadcast message is sent, then the message is sent out of all ports, so it is a single broadcast domain.


# Broadcast domain : is the boundary in which a packet can be sent to all of members as a broadcast message.

# Router have different broadcast domains per network.


# What LAN switches do to alleviate network congestion:

- Fast port speeds : to reduce congestion
- Fast internal switching : to provide high performance
- Large frame buffers : to store large frames for temporary basis before dropping them
- High port density

---------------------------------------------------------------




# VLANS:

- When VLAN is used, a TAG Field is added to ethernet frame in between SRC MAC Add & Type Field
- This TAG field contains multiple sub fields:

  - Type : 2 - Byte , Tag Protocol ID
  - User Priority : 3 bit value 
  - Canonical Format Identifier ( CFI ): 1 - bit identifier that enables Token Ring frames to be carried across Ethernet links
  - VLAN ID : 12-bit , that supports up to 4096 VLAN IDs.

# How VLAN forwarding # Tagging works :

- When frame comes on access port, it provides an internal VLAN label inside switch to frame and buffer it.
- The switch will check the destination MAC Address
  - If the dest MAC address has been learnt on any other access port, it will no add the tag rather simply send the frame out of then port.
  - If the dest MAC address has been learnt on trunk port for same VLAN ID, it will apply the TAG and send it across the trunk port
  - If the dest MAC address has not been learnt any where, it will flood the frame out of all ports.
    - on Access port without applying TAG
    - on trunk port with applying TAG

# Native VLANs & 802.1Q Tagging:

- If switch receives a frame which is not received on access 


# VLAN Configurations:

- Normal Range VLANs:
  - 1 - 1005
  - 1002 - 1005 are reserved for legacy network tech
  - VLANs 1 , 1002 - 1005 are automatically created and cannot be deleted
  - VLAN database is stored in vlan.dat file in flash memory
  - VTP is used among switch to help sync the VLAN Database




- Extended Range VLANs:
  - 1006 - 4094
  - Configurations are saved by default in running config
  - support fewer VLAN features than normal VLAN range.


<!-- 

  // To configure Voice VLAN:

  switchport voice vlan <vlan-id>
  mls qos trust cos                   ----------->    For QoS 

 -->



 # Delete VLANS:

 - delete flash:vlan.dat
 - When a vlan is deleted, the port assigned to that port does not re-assigned to default VLAN. Because after vlan is deleted, the port becomes in-active 


# Dynamic Trunking Protocol:

- An interface can be configured as 
  - trunking
  - non-trunking
  - negotiating trunking with neighbor interface

- trunk negotiation is managed by DTP
- It is Cisco Proprietary

# Switch Negotiated Interface Modes:

- access
- dynamic desirable : 
            - interface actively sends DTP packets and tries to convert the link to trunk
            - interface is able to be a trunk
            - Forms trunk with trunk / dynamic desirable / dynamic auto
            - It will only be trunk , if neighbor interface is also trunk / desirable mode
- dynamic auto  :
            - interface actively listens for  DTP packets and tries to convert the link to trunk when receive a DTP message. But does not initiate negotiation
            - form trunk with trunk / dynamic desirable



-----------------------------------------------------------------

# Inter VLAN Routing :

- Legacy Inter-VLAN Routing : 
    - uses separate physical interfaces for individual VLAN
- Router On a Stick : 
    - Create Sub interfaces to act gateway for different subnets
- Create SVI:
    - Create VLAN interface and give IP address of gateway for that VLAN
    - Much faster than ROAS
    - No need of external link from switch to router
    - latency is much lower
    - BUT these L3 switches are more expensive



# Router On a Stick:

- encapsulation dot1q <vlan_id>
  - This sub-interface will only respond to 802.1Q encapsulated traffic from the specified <vlan_id>



# SVI:

- Important , enable IP routing:
  - (config)# ip routing


# Use 'no switchport' to create a L2 port to L3 port


-------------------------------------------------------------------------------------------

# Spanning Tree Protocol ( STP )

- In IEEE, it is known as 802.1Q / IEEE MAC Binding Standard


# Need For STP:
- Layer 2 Loops
- Broadcast storm


# Results Of Layer 2 Loop:

- MAC Address instability : 1 MAC address can be learnt on different ports
- Link saturation
- High CPU utilization on switches and end devices
- Network instability

# Spanning Tree Algorithm ( STA ):

- Created by Radia Perlman in 1985.
- This algorithm 
  - Creates a loop free topology by selecting a single root bridge while all other switches determine a single least-cost path to that single root bridge.
  - Block redundant paths.
  - If a failure occurs, those blocked ports can be unblocked and used to permit traffic to use as an alternate path to root bridge.



# STP Operations:
----------------------

# STP builds a loop free topology in four step process:

- Elect Root Bridge
- Elect Root ports on non-root switches
- Elect Designated port
- Elect Alternate ports on non-root switches


# BPDUs are used during this election, BPDUs contains:

- Sender Bridge ID




# Bridge ID is consists Of:

- Bridge Priority:
    - 4 bits value
    - default for all cisco switches : 32768
    - Range : 0 - 61440 with increment of 4096
    - Lowest Bridge Priority is preferred.

- Extended System ID:
    - 12 bits value
    - Identify which this BPDU belongs to
    - In original 802.1D it was not present because only VLAN was used, but when VLANs were introduces then this value was added.

- MAC Address:
    - 48 bits
    - Lowest MAC expressed in HEX is preferred.



# BPDUs contains:

- Sender Bridge ID
- Root Bridge
- Root path cost


# !!!! (Note: When a BPDU is received on a port, the switch will add the cost of ingress port of segment and then determine the root path cost )


# Root Bridge Election
# Root Port Election on non-root switches
# Designated Port Election:
  - Between two (2) switches we have a single network segment
  - In this network segment only one (1) port is elected as Designated port.
  - The Designated port is the port which has lowest internal root path cost to root bridge
# Rest of ports on the network segment are alternate / blocked ports.


----------------


# Elect a root port / designated port If Multiple Equal root cost paths are present

- Use this sequence to select the switch having 
  - Lowest Sender BID
  - Lowest Sender port priority
    - Default port priority = 128
  - Lowest Sender Port ID



# STP Timers:

- Hello Timer : Interval between BPDUs, default is 2 which can be set to 1 - 10.
- Forward Delay Timer : Time spent in Listening & Learning state. Default is 15 , but can be modified to 4 - 30 seconds.
- Max Age Timer : Max time limit for which a switch waits before attempting to change the STP topology. Default is 20 seconds, can be modified between 6 - 40 seconds.



# Remember, first port roles are elected after that port move through different port states based upon the port role.

# STP States: 5 states
-----------------------

# Operational States (4):
  
  - Blocking
  - Listening
    - Forward Delay : 15 seconds
  - Learning 
    - Forward Delay : 15 seconds
  - Forwarding

# Non-operational States (1):
  - Disabled State
    - Max Age : 20 seconds
    - If on a port no BPDUs are received and no user traffic is received, it is due to it has been administratively shut
    - No user Data / BPDUs are forwarded



---------

# Blocking :

- Port is alternate.
- Does not participate in frame forwarding
- Receives BPDUs to locate Root and find Root bridge ID
- If MAX Age timer of 20 seconds have been passed, & no BPDU has been received on port, then the port is put into blocking state.


# Listening :
- port receives the BPDU
- It uses the BPDU to determine the path to root bridge
- It sends it's own BPDU to inform adjacent switch that it will also take part in active topology.
- No MAC Address table update.
- Users frames are not forwarded


# Learning :
- Receive & process BPDUs
- prepares in frame forwarding
- Begins MAC Address update : populate MAC Address Table 
- Users frames are not forwarded


# Forwarding :
- send and receive user traffic
- send and receive BPDUs
- Update MAC Address table.



------------------------------------------------------


# Different Varieties of STP:

- CST ( Common Spanning Tree ):
  - 802.1D
  - 1 STP Instance for entire bridge network.

- PVST+ :
  - 1 STP Instance per VLAN
  - supports port-fast, uplink-fast, backbone fast, BPDU Guard, BPDU Filter, root guard, loop guard.

- RSTP :
  - Evolution of CST : 802.1w 
  - Provides faster convergence than CST


- Rapid PVST+:
  - mashup of RSTP and PVST+

- MSTP:
  - Maps multiple VLANs into same spanning tree Instance.


- MST:
  - Cisco implementation of MST
  - provides up-to 16 RSTP instances.
  - combines many VLANs with same physical / logical topology into common RSTP instance.
  - Each instance supports port-fast, BPDU guard, BPDU filter, root guard & loop guard.



# Rapid Spanning Tree Protocol ( RSTP ):


# Port States:

- Discarding :
    - It includes STP ports:
      - Disabled
      - blocking
      - Listening
- Learning
- Forwarding


# Port Roles:

- Root port 
- Designated port
- Backup port
- Alternate port


------------------------------

# Port-Fast:

- When a device is connected on a port, the total forward delay is 30 seconds.
- But If for a device using DHCP for address resolution, the DHCP will time out if for 30 seconds the port will not be forwarding user data.

- What port fast do:
  - It transitions the port from blocking port to forwarding state
  - can be used on access ports to minimize the wait 
  - Should not be used on trunk ports as can cause loops


# BPDU Guard:

- If a port has been configured as port-fast, If BPDUs will be received on the port, the port will indicate that another bridge has been connected, which can cause Spanning tree loops.

- For this purpose, BPDU guard is used, that If any BPDU is received on the port, the switch will put the port into Error Disabled ( errdiabled ) state and shut the port
- The port can be recovered when unshut administratively .


---------------------------------------------------------------------------------------------------

# EtherChannel
-------------------

# Why EtherChannel is needed:

- More Bandwidth
- Redundancy

# Advantages:

- Bandwidth increase
- Load Balancing using 
  - SRC MAC
  - DST MAC
  - SRC IP
  - DST IP


# Implementation Restrictions:

- Fast Ethernet and Gigabit Ethernet cannot be mixed into single EtherChannel
- Each EtherChannel consists of up-to 8 compatibility-configured ethernet ports.
- EtherChannel provide full duplex BW of 800 Mbps / 8 Gbps between switch and switch / host.
- Catalyst 2960 can support up-to six EtherChannels.
- If one end is trunk, other end must be trunk with same native VLAN.


# EtherChannel can be configured with auto-negotiation using
  - PAgP
  - LACP

# EtherChannel can also be configured a static or un-conditional EtherChannel without LACP / PAgP


# PAgP Operation:
---------------------


- It is for automatic creation of EtherChannel links
- PAgP packets are sent every 30 seconds.
- PAgP Packets are sent between EtherChannel-capable ports to form EtherChannel for negotiating
- PAgP identifies matched Ethernet links and groups them into an EtherCHannel
- EtherChannel is then added into Spanning Tree as single port.


- PAgP packets sent every 30 seconds are used for checking consistency, failures, link additions and make sure all ports in etherChannel have same type of config.


# PAgP Requirements for EtherChannel:

- Same speed
- Same duplex setting
- Same VLANs
- Same VLANs are allowed If Trunk has been configured
- Compatible modes ( If one is auto, other should be desirable & vice versa )
  - Should not be on & auto / desirable
  - should not be auto auto


# PAgP modes:

- On: No negotiation of parameters is done, manually places the port into EtherChannel.
- PAgP desirable : Active State ----->  Port will keep sending PAgP packets to initiate negotiations
- PAgP Auto : Passive States -----> Port will respond to PAgP packets but will not initiate the PAgP negotiation.


-----------------------

# LACP 

- IEEE 802.3ad
- Now defined in 802.1AX
- Supports multi-vendor


# LACP Modes:

- On : Forces port into EtherChannel without negotiations
- LACP active : port initiates the negotiations
- LACP passive : port responds to LACP packets but don't initiate


# Configuration:

- interface range <range_of_interface>
- channel-group <identifier> mode <active|passive|desirable|auto>
- interface port-channel <identifier>
- switchport mode trunk
- switchport trunk allowed vlan 1,2


# Troubleshooting:

- show interfaces port-channel
- show etherchannel summary
- show etherchannel port-channel
- show interfaces etherchannel







------------------------------------------------------------------------------------------

# DHCPv4:

# 4 Step process to obtain a lease:

- DHCP Discover ( Broadcast ): This message will include:
                - it's own MAC Address

- DHCP Offer ( Unicast / Broadcast ): This message will include:
                - Leased IP address
                - Apart from that device will also created an ARP entry for the device's MAC address against the leased IP address.

- DHCP Request ( Broadcast ): This message will include:
                - I accept this offer
                - This message is sent for both 

- DHCP Acknowledgement :
                - First upon receiving the DHCP request, DHCP server will check either this IP has already been leased or not
                - If not, then it will create a new entry for ARP for client lease and reply with DHCPAck message, duplicate of DHCPOffer except the message type is changed to DHCPAck


# Steps to Renew Lease:

- DHCP Request (DHCPREQUEST)

  - Before expiration of leased IP, client will send a message directly to DHCPv4 Server which originally offered DHCP IP
  - If Original DHCP server will not response, client will broadcast another DHCPREQUEST so that any other DHCPv4 servers can extend the lease.


- DHCP Acknowledgement:

  - server verifies the lease info by returning the DHCPaCK


----------------------

# Configuring Router as a DHCP Server:

- exclude some addresses not to assign via DHCP, because those can be assigned statically to printers, routers, servers etc.
- Define IP Pool name
-  Configure the DHCPv4 Pool
-  


<service dhcp>
<ip dhcp excluded-address low-address [high-address]>
<ip dhcp pool pool-name>
<network network-number [mask]>
<default-router address [address]>
<dns-server address [address]>
<domain-name domain>
<lease [time]>
<netbios-name-server address [address]>



# Commands :

- show ip dhcp binding             { to see IP to MAC Binding }
- show ip dhcp server statistics   { display no. of messages of DHCPv4 that have been sent and received}


# Commands for DHCPv4 Relay :

- ip helper-address 192.168.11.6

<Above Command forwards 8 UDP Services: >
  - DHCP/BOOTP Server
  - DHCP/BOOTP Client
  - Time
  - DNS
  - TACACS
  - TFTP
  - 
  - 



------------------------------------------------------------------


