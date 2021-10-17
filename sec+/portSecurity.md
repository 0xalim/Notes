# Port security

1. Physical switch interface not tcp/udp
1. Maintain availability, limit traffic, remove unwanted traffic
1. Different devices + ports so different security

## Broadcats

1. Packets that get sent to all machine connected
1. Limited scope: only machines connected get that packet
1. Routing updates + arp requests commonly use this + adds up quickly
1. Can be malicious traffic
1. Not used in ipv6

## Broadcast storm control

1. Switch that control number of broadcasts per second
1. Can be used for multicast + unicast too
1. Different implementations for monitoring broadcast

## Loop protection

1. Network that loops and never ends
1. Easy way to bring down network
1. Difficult to troubleshoot
1. 802.1D standard from IEEE to prevent loops in bridged networks:
 1. Spanning tree protocol
 1. Uses root port, designated port and blocked port
 1. Outages of switches can happen, this protocol can monitor for this
 1. If interfaces available for different routing then it will use it

## BPDU guard

1. Spanning tree takes time to understand layout of machines
1. Sometimes your configuration is only 1 machine and no loop can be created
   so you can bypass the listening and learning states
1. Cisco calls this portfast
1. Bridge protocol data unit: Primary protocol used by spanning tree protocol
1. If bpdu frame recognized then shutdown interface as loop can occur

## DHCP snooping

1. Switch has trusted and untrusted sources
1. Switch monitors dhcp traffic, if on untrusted list then filter this traffic

## MAC filtering

1. Media access control, physical interface
1. Allow / disallow depending on mac address of traffic
1. Allow table of addresses, if another one pops in then immediately recognize
1. However, easy for attackers to mitm all traffic in network, collect allowed
   mac addresses and then spoof your machine as same mac address
1. Bad security, its security through obscurity
