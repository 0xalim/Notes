# Network Segmentation

1. Physical, logical (same host), virtual
1. For performance
1. Security:
 1. Users should not access db
 1. Only some ports through tunnels
1. Compliance:
 1. PCI compliance for credit card information security

## Physical segmentation

1. Separate devices
1. Air gap
1. Must be connected to communicate together
1. No mixing on services: users and db
1. Drawback:
 1. Lot more machines
 1. Lot more wasted resources (interfaces)

## Logical segmentation

1. Virtual local area network (vlan)
1. Separated logically on the same machine
1. No communication still possible

## Screened subnet

1. DMZ zone, literally
1. Network for users from the internet through firewall, no access to internal
1. Only access to services off dmz, email, web, etc

## Extranet

1. Separate from the internal network, this is extranet
1. Full access not allowed from users off internet
1. It's like a secondary internal network, go through firewall, switch, etc

## Intranet

1. Only accessible inside the network
1. Still accessible from remote site if designed this way
1. Internal servers for employees only
1. Private information, so VPN connection most likely

## East-west traffic

1. Need to know how traffic travels inside a data center
1. East-west: Data transporting inside data center, same level
1. North-south: Data transporting in or out of data center
1. Different security policies for both above ^

## Zero trust

1. Holistic approach where no machines trust others even inside of same network
1. All for better security, need authentication and such
1. Encryption, permissions, firewalls, monitoring all still in place
