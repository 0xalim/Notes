# Network Appliances

## Jump Server

1. Provide admin control in a secure manner to protected network
1. Highly secured device ssh'd, tunnel or vpn to gain access to network
1. So configuration only allowed through this hardened device
1. Jump server is a significant security concern

## Hardware security module

1. Manages keys, certs
1. Used in large environments
1. Redundant power
1. High-end cryptographic hardware
1. Provides secure storage
1. Cryptographic accelerators: offload crypto work to this device
 1. Helps with crypto work in other machines

## Sensors and collectors

1. Collect all statistics in environments to 1 central point:
 1. Switch
 1. Router
 1. Srever
 1. Firewalls
1. Sensors:
 1. IPS
 1. Firewall logs
 1. Auth logs
 1. Web access / database logs
1. Collectors:
 1. Collects all data, parses through all data and presents to admin
 1. Can be proprietary, so only works with 1 device (like firewalls, SIEM)
1. SIEM: Security information and event management tool, generic collector
