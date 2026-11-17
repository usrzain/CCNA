<!-- markdownlint-disable -->


# LAN Security Concepts:

# AAA Authentication methods:

- Local : username & password is stored in device locally
- Server-Based : 
  - Device will external AAA server
  - Router uses RADIUS or TACACS protocols to communicate with AAA server, and will get authenticated

# AAA Authorization 

# AAA Accounting



-----------------


# 802.1X:

- Client (Supplicant) : Device running 802.1X compliant software
- Switch (Authenticator) : intermediatory device between client and auth server
- Authentication Server 

------------------

# L2 Security Threats:
----------------------

# MAC Address Table Flooding Attack:

- A switch have a limited space to hold the MAC Address table
- threat actor take advantage of this limitation by bombarding the switch with fake source MAC Addresses until the MAC address table space if full.

  

# VLAN Attacks:

- Spoofing the DTP messages to create a trunk link with target switch so a packet can be sent to desired VLAN.
- Introducing a rogue switch and enable trunking.
- Double VLAN Tagging

# Steps to Mitigate VLAN attacks:

- Disable DTP on both trunk and access ports
- Disable unused ports and put them in unused VLAN
- manually enable trunking on trunk ports
- use other VLAN than VLAN 1 as a Native VLAN


# DHCP Starvation Attack & Being a DHCP Server :

- Goal : create a DOS for connecting clients that they cannot get an IP address
- How  : 
    - (1) A Rogue device could be sending multiple DHCP messages to get all IP address the DHCP server has.This is called as DHCP starvation
    - (2) A Rogue device could be connected to network and acting as a DHCP server and sending "DHCP-Offer" & "DHCP-Ack" Messages which allow the user to connect to them and they can act as a Man In The Middle.
- Tool : Gobbler for DHCP Starvation
- Mitigation:
  - Use port security, because Gobbler uses different MAC address for subsequent packet. So this is the solution for DHCP Starvation Attack.  This is DHCP snooping
  - Use DHCP spoofing on trusted ports, to allow the which port can receive or allow DHCP-OFFER / DHCP-ACK messages. So this is the solution for mitigating DHCP Rogue devices to operate.
    - <ip dhcp snooping>  { configure globally}
    - <ip dhcp snooping trust>   { on trusted ports }
    - <ip dhcp snooping limit rate >    { Limit no of dhcp message discovery messages can be sent / sec }
    - <ip dhcp snooping <vlan> >    { Apply dhcp snooping on specific VLAN / range of VLANs }



# ARP Spoofing :

<Definition>

- This pure means that the attacker will act that he has same IP as victim / default gateway.
- The ARP messages will be sent by attacker machine claiming to be the victim / default gateway ( which as identified by their IP address ) but the MAC address will be of switch.

<Effect>

- Cache poisoning : Both victim router or PC will have the MAC address of attacker in their ARP table / ARP cache.
- Man In The Middle Attack



<Objectives>

- Packet sniffing
- Session Hijacking
- DoS



<Remedy>

- Switch must know that only valid ARP Requests are relayed.Denying any invalid / gratuitous ARP Request
- Intercepting all ARP request ports on un-trusted ports.
- Verifying each intercepted packet to know that it has came from a correct MAC Address by checking in IP-to-PC binding.
- Dropping and logging ARP requests coming from invalid sources to prevent ARP poisoning
- Error-Disable the port if configured DAI number of packets is exceeded.


<Enabling-Dynamic-ARP-Inspection (DAI) >

- Enable DHCP snooping globally   <ip dhcp snooping >
  - It creates a table which holds IP-to-MAC binding into "DHCP snooping binding table", that which MAC address has which IP address
  - Stops the rogue servers.

- Enable DHCP snooping and DAI on selected VLANs    <ip dhcp snooping vlan 30,20,40>

- Enable DAI on selected VLANs    <ip arp inspection vlan 30>
  - Check each frame with "DHCP snooping binding table" to check either it is a frame with valid MAC Address or not.

- Configure trusted interfaces for DHCP snooping and ARP Inspection. <int g1/0/24>
                                                                     <ip dhcp snooping trust>
                                                                     <ip arp inspection trust>



# Variations of Dynamic ARP Inspection (DAI):

- DAI is configured to check 
  - Destination MAC address against target MAC address in ARP body to allow only MAC address present in "DHCP snooping binding table" or "ARP ACL"
  - Source MAC Address against target MAC address in ARP body to allow only MAC address present in "DHCP snooping binding table" or "ARP ACL"
  - IP address against target IP address in ARP body to allow only IP address present in "DHCP snooping binding table" or "ARP ACL"

<ip arp inspection validate {[src-mac][dst-mac][ip]}>

# Only 1 arp inspection validate command can be configured otherwise previous will be overwritten






# STP Attacks:

<Definition>

- Attacker spoofs it self as Root bridge.


<Effect>

- All switches assume that the attacker is the new root bridge

<Objectives>

- Change / Disrupt the network topology


<Remedy>

- Port Fast
  - Brings the put directly from blocking to forwarding state bypassing listening and learning states.
  - should be used on ports where PC are intended to be connected.
  - <spanning-tree portfast>

- BPDU Guard
  - Immediately error-disables the port on which any BPDU is received.
  - should be used on port where PC are intended to be connected.
  - <spanning-tree bpduguard enable>





# IPSG : prevents MAC and IP address spoofing attacks
# DAI : prevents ARP spoofing and ARP poisoning
# DHCP Snooping : prevents DHCP starvation and DHCP spoofing attacks


------------------------------------------------------------------------------------------

# Switch Security :


# Port Security:


- Port security aging:

  - Absolute : secure address on port are deleted after specific time
  - Inactivity : secure address on port are deleted after specific time, If they are inactive for specific aging time.


  - switchport port-security aging 


# Port Security Violation Modes:

# Default Port security mode is shutdown
- shutdown : 
  - puts port in err-disabled
  - shuts the port LED
  - sends a syslog message
  - increment the violation counter


- Restrict: 
  - drops the packets 
  - sends the syslog message
  - increment the violation counter


- Protect : 
  - drops the packet
  - No syslog message or violation counter increment occurs



# Port security sticky command will store the learnt MAC address into RAM which will be lost once switch is rebooted
# To prevent this save the running config into startup config which is stored into NVRAM











--------------------------------------------------------------------------------------------------


# Wireless LAN (WLAN)


# Types of Wireless Networks:

- Wireless Personal Area Network (WPAN):

  - 20 - 30 ft
  - 802.15 Standard
  - 2.4 GHz
  - Bluetooth & Zigbee

- Wireless LAN (WLAN): 

  - up-to 300 feet
  - 802.11 Standard
  - 2.4 GHz or 5 GHz



- Wireless MAN :

- Use transmitters to provide wireless services over large geo-graphical area.
- Used to provide Wireless access to Metropolitan cities 
- support licensed frequencies.


- Wireless WAN :

- Used for national and geographical communications




# Wireless Technologies:

- Bluetooth:
  - 802.15 Standard
  - 300 ft

- Wireless Interoperability For Microwave Access 
  - up-to 30 miles
  - Operates like Wi-Fi but more speed and more distance
  - use WiMAX towers

- Cellular Broadband:




# 802.11 Standards:

# 802.11 Standards are rules that define how frequencies are used for wireless links

# WLAN Operates in 2.4 GHz and 5 GHz bands

- 802.11 
  - 2.4 GHz
  - Speed : up-to 2 Mbps

- 802.11a
  - 5 GHz
  - up-to 54 Mbps
  - small area
  - less affective in building penetrating


- 802.11b
  - 2.4 GHz
  - speed up-to 11 Mbps
  - longer range than 802.11a
  - better able to penetrate walls


- 802.11g
  - 2.4 GHz
  - speed up-to 54 Mbps
  - backward compatible with 802.1b with reduces bandwidth capacity


- 802.11n
  - 2.4 GHz / 5 GHZ
  - speed from 150 Mbps to 600 Mbps
  - Distance range : 70 m
  - AP and Clients need MIMO ( Multiple Input Multiple Output ) Antennas


- 802.11ac
  - 5 GHz
  - speed from 450 Mbps to 1 Gbps
  - up-to 8 antennas are supported.


- 802.11ax
  - 2.4 GHz / 5 GHz
  - latest 
  - known as Wi-fi 6   or   High Efficiency Wireless ( HEW )
  - can use 1 GHz and 7 GHz when they will be available


----------------------------------------------------

# WLAN Operation:

# Modes Of Wireless Operation:

- Infrastructure Mode:
  - Wireless endpoints connects to AP
  - This AP is connected to wired network for complete data transfer

- AD-HOC:
  - Wireless P-2-P ( Point-to-Point ) network
  - Example is bluetooth
  - No wired network is involved

- Tethering:
  - A device is connected wirelessly to internet
  - Same device is also working as a Wireless access point 
  - Example is personal Hot-spot


# An AP will keep sending Beacon frames to advertise SSIDs

# An AP will send probing, authentication & association frames only when a client is associating with AP

# Basic Service Set ( BSS ) 

- BSS means a group of wireless connected devices which are connected to a AP and can talk to each other via that AP.

- BSSID : MAC Address of AP

- BSA : Signal Range


# Extended Service Set ( ESS )

- Extend the range of BSS 

- ESA : No of multiple BSS are connected to a wired network to create 
  - This offer's Roaming


# 802.11 Frame:

- From Client:

  - Header :
    - Address 1 : MAC address of receiving AP
    - Address 2 : MAC address of sender
    - Address 3 : MAC address of Destination ( which will be default gateway If off-local LAN) 
    - FCS
    - Sequence Control

  - Payload
  - FCS


- From AP:

  - Header :
    - Address 1 : MAC address of sender
    - Address 2 : MAC address of AP
    - Address 3 : MAC address of Destination ( of Client ) 
    - FCS
    - Sequence Control

  - Payload
  - FCS

# CSMA / CA:

- As the wireless BW is limited, all devices fights for BW.
- How single AP manages to connect with multiple devices at one time.

- It will not allow all traffic to go through AP and get collided
- How that works:
  - Wireless device listen on wireless channel to see that the channel called as carrier is idle.
  - Wireless Client sends a RTS to ask AP , can I send the wireless data ?
  - AP response with Clear-to-Send ( CTS ) message to client in response to RTS
  - Client will send the data 
  - AP will ack that. If no ACK has been received it means an collision has happened. So start the entire process back from listening.


# Wireless Client & AP Association:


# First know about how Clients discover the AP:

- Passive:
  - AP will always be sending a <beacon>, which will hold:
    - SSID
    - Supported standards
    - Security settings 

- Active:
  - When Client will send a <Probe-Request>
    - SSID is already configured on it.
    - supported standards are already configured.


- When Client want to connect to AP via certain SSID and password, both have to agree on some things.
  - Which standard will be used?
  - Which mode will be used ?
  - Which wireless security protocol will be used? WEP, WPA, WPA2, along with encryption standards ?
  - Which Radio Frequency will be used?


----------------------


# Control And Provisioning Of Wireless Access Points (CAPWAP):

- It is a protocol used by WLC to manage wireless networks.

- Encapsulates and forward client traffic between AP & WLC.

- Both AP and WLC creates a secure tunnel on port:
  - UDP 5246, UDP 17, UDP 136 for IPv4
  - UDP 5247, UDP 17, UDP 136 for IPv6

- For additional security with Data-gram Transport Layer Security (DTLS).
- DTLS is disabled by-default for Data plane.
- But DTLS is enabled by-default Control / Management Plane.




# AP MAC functions:

- Beacons & probe responses
- packet acknowledgements & re-transmissions
- frame queue
- MAC layer data encryption


# WLC MAC functions:

- Authentication
- Association and roaming of clients
- termination of 802.11 traffic on wired network.


# Flex Connect AP:

- Allow the AP to be managed from remote location without deploying a local WLC.
- Modes Of operation:
  - Connected : WLC is reachable over wired network and CAPWAP can work.send traffic through CAPWAP tunnel
  - STandalone : WLC is not reachable, it is mostly used for local network usage.


# Channel Management:

# What are channel
Wireless devices talk over a range of frequencies.
Each single range is further divided into sub-ranges called as channels

# How channel is used
- Multiple devices can talk to each other in a channel
- If multiple devices chooses same channel to talk then that channel will be over-crowded & and can cause channel saturation which will lead to interference and data loss


Multiple modulation techniques are used for channel management to mitigate channel saturation, means that all devices can use the same channel and no saturation occurs:

- Direct Sequence Spread Spectrum (DSSS)  :
  - Spread signal over large frequency band
  - used in 802.1b devices to avoid interference from other devices using same 2.4 GHz band.

- Frequency Hoping Spread Sequence (FHSS)  :
  - send signal by switching the carrier frequency among frequency channels.
  - Both transmitter and sender must be sync to receiver must know which channel transmitter is going to hop.
  - This allow more efficient use of channel
  - decreasing channel congestion
  - It was used in original 802.11 Standard, Walkie talkies, cordless phones, bluetooth.


- Orthogonal Frequency Division Multiplexing (OFDM)  :
  - single channel uses multiple sub-channels on adjacent frequencies.
  - No interfering happens in sub-channels at all.
  - Used by 802.11a, 802.11g, 802.11n, 802.11ac
  - 802.11ax uses a variant of OFDM as Orthogonal Frequency Division Multiplexing Multi-access OFDMA.



# Channels in Frequencies:

- 2.4 GHz        ---       11 Channels in USA
                           13 Channels in Europe
                           14 Channels in Japan
- 5 GHz          ---       24 Channels
- 




# WLAN Threats:

- Interception of Data : Wireless data should be encrypted  
- Wireless Intruders : Un-authorized users attempting to access the network
- DOS : 
- Rogue APs: Unauthorized AP

# Secure WLAN:

- SSID Cloaking : 
- MAC Address filtering : 

SSID can easily be discovered. 
MAC Address can be spoofed.

So best method is to use authentication and encryption system.

# Types of authentication system in WLAN security:

- Open :
  - No password is required

- Shared key:
  - Wired Equivalent Privacy (WEP):  It is original 802.11 and uses RC4 for encryption
  - Wi-Fi Protected Access (WPA): Use Temporal Key Integrity Protocol (TKIP)
  - Wi-Fi Protected Access 2 (WPA2): Uses AES as a encryption protocol. AES uses CCMP to recognize if encrypted and non-encrypted bits has been altered
  - Wi-Fi Protected Access 3 (WPA3): 




---------------------------------------------------------------------------

# WLAN Configuration:

