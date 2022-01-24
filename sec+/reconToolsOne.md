# Reconnaissance Tools Part 1

## traceroute

1. Determines packet path between endpoints
1. Uses ICMP TTL exceeded error messages:
 1. Router 1 = 1 ttl, router 2 = 2 ttl...
 1. Some firewalls will filter and prevent this
1. Not all os have same implementation of traceroute
1. Windows uses ICMP echo request (same as ping):
 1. Not very good
 1. ICMP outgoing is filtered
 1. ICMP echo reply from final endpoint, ICMP time exceeded for others
1. Other OS can modify these packets + protocols
1. IOS send udp ports over 33434

## Mechanics of traceroute

1. When ttl error message received, can gain info about IP and time taken
1. Error message received because ttl goes to 0; we increment from 0

## Dig & nslookup

1. Lookup info on dns servers
1. nslookup:
 1. On all os's
 1. Lookups names and ip addres
 1. Slowly being deprecated
1. Dig:
 1. Replacement for nslookup
 1. More advanced information

## ipconfig / ifconfig

1. Determine tcp/ip and network adapter information
1. ipconfig for windows, ifconfig for linux
1. Shows dns, subnetmask, and more information

## ping

1. Test reachability
1. Uses ICMP
1. Determines if machine is responding

## pathping

1. Windows only command
1. Combines ping and traceroute
1. Phase 1: traceroute
1. Phase 2: measure round trip time and packet loss

## netstat

1. Network statistics
1. Shows who your machine is communicating to
1. Shows who your machine is accepting communication from
1. -a for all active connections
1. -b (windows) show binaries
1. -n only ip address

## Address Resolution Protocol

1. Determine mac address based on an ip address
1. arp -a is the local arp table
1. arp table is refreshed when pinging another machine

## route

1. Determine devic routing able
1. windows: route print
1. linux: netstat -r
