<!-- markdownlint-disable -->


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
<Standby>: Router acts as a designated backup on active router. Periodically sending hello messages and waiting to take over If active router fails
<Active>:  Router currently forwarding Data packets sent to Virtual MAC and Virtual IP , while sending HELLO Messages to tell standby router that I am alive

# HSRP Timers:

- Hello Timer : 3 seconds
- wait time : 10 seconds  for which the standby router wait before announcing the gratuitous ARP that I have become the active router now.



# HSRP for IPv6:

- HSRP IPv6 group will have Virtual MAC Address derived from the HSRP Group Number
- HSRP IPv6 group will have Virtual IPv6 Link Local Address derived from HSRP Virtual MAC Address derived above.
-  