# Load Balancing

1. Distribute load by having multiple servers
1. All automated
1. Large scale implementations, web server farms + database farms
1. Fault tolerance: Server outage has no effect

## Load balancer

1. Configure load to manage accross servers
1. TCP offload:
 1. TCP overheads taken care of by balancer instead of individual servers
 1. Better efficiency + speed
1. SSL offload:
 1. Encryption/decryption done by load balancer
 1. Cleartext sent to servers in same data center
1. Caching:
 1. Faster response
1. Prioritization:
 1. QoS
1. Content switching:
 1. Servers that take care of only certain applications
 1. Application-centric balancing

## Scheduling

1. Round robin:
 1. User1 -> Server 1, User 2 -> Server 2 ...
1. Weighted round robin:
 1. Specific servers prioritized, others are side servers
1. Dynamic round-robin:
 1. Send to lightest load
1. Active/active load balancing:
 1. If server fails, others take it's position

## Affinity

1. Some applications require communication with same instance:
 1. So balancer will offload to same server always if needed
 1. Tracked through IP or session ID
 1. Session persistence

## Active/passive

1. Others are passive, if active fails then take it's position
1. Fault tolerance
