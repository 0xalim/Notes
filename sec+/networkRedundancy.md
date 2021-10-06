# Network Redundancy

## Load balancing

1. Balances network on multiple servers
1. Some are active, some are on standby
1. If active fails then passive one takes its spot

## NIC teaming

Load balancing/LBFO:
1. No active/standby instead all are on active and share bandwidth
1. Multiple network adapters in 1 machine
1. NICs communicate via multicast connection to each other
1. If one NIC does not respond to hello then we assume its down and use others

## Port aggregation

Different ways to link up switches so that if one does fail then we are
still able to stay online and connected.
