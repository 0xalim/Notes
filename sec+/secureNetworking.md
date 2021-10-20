# Secure Networking

## Domain name resolution

1. DNS has no security, so we use DNSsec
1. Validates dns responses providing:
 1. Origin authentication
 1. Data integrity
1. Via public key crypto to sign dns record

## Using dns for security

1. Sinkhole address: if user visits known malicious address don't give ip add
   instead redirect
1. Log this, might be infected machine reaching to c2 or other malicious web

## Out of band management

1. Loss of remote connection, backup plan is available
1. Switch/routers have different management interface (usb/eth)
1. Connect a moddem and dial in 
1. Some orgs have console router / comm server:
 1. Out of band access for multiple device
 1. Centralized place to gain remote access if usual is not available

## QoS

1. Quality of service
1. Different apps have different network requirements:
 1. Voice is real-time
 1. Streaming requires a buffer, some lag
 1. Database applications are interactive
1. Give priority for applications requiring:
 1. Maximum bandwidth
 1. Traffic rate
 1. Vlan
 
## IPv6 security

1. More address space
1. Harder to scan ports
1. No need for NAT
1. No arp, so no arp spoofing
1. New attacks will appear older ones will phase out

## Taps and port mirrors

1. Capturing network traffic, device
1. Physical taps: Place tap in middle of wire, active or passive
1. Port mirror: Port redirection (cisco SPAN), software based, limited

## Monitoring Service

1. Cybersecurity monitoring organization
1. Constant checks for versions, patchs
1. Constant security posture check
1. Identifying threats
1. Can react quick
1. Maintain compliance

## File integrity monitoring (FIM)

1. Monitor files that should never change, or change infrequent
1. Windows: System file checker (SFC)
1. Linux: Tripwire
