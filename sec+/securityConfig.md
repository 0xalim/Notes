# Security Config

## Config changes

1. Firewall rules can block apps and manage app flows
1. Mobiel device manager (MDM):
 1. Enable mobiel functionality
 1. Regardless of physical location
1. Data Loss Prevention (DLP):
 1. Block transfer of sensitive info
1. Content filter:
 1. Limit access to known bad webs
 1. Large blocklists are used to share suspicious urls
1. Updating and revoking certs:
 1. Manage certs and remove if needed

## Isolation

1. Isolate compromised device
1. Prevent malware from spreading
1. Prevent C2 remote access
1. Network isolation: Seperate VLAN, no comms with outside devices
1. Process isolation: Limit app execution, prevent activity but allow management

## Containment

1. Application containment: every app runs in sandbox
1. Cannot access other processes
1. Limited access to os
1. Ransomware cannot jump between apps:
 1. Can then disable remote access, shares, local accounts, etc

## Segmentation

1. Separate network: internal vs external
1. Segmented: Segment even internal networks, cannot communicate out of their segments

## SOAR

1. Security orchestration, automation and response
1. Integrate 3rd party tools and automate their tasks
1. Runbooks:
 1. Cookbook on how to run a task
 1. Step by step instructions
1. Playbook:
 1. More broad process, how to recover from ransomware for example
