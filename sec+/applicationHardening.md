# Application Hardening

1. Minimize attack surface + entry points
1. Remove all unknown attack points
1. Compliance mandates - HIPAA, etc

## Ports and services

1. Every open port is an entry point, close all non-required
1. Control access with firewall
1. Unknown services opening ports without you knowing
1. Use NMAP to port scan

## Registry

1. Configuration database for windows
1. 3rd party tools compare registry before/after software install

## Disk encryption

1. Full disk encryption: Encrypt everything on disk
 1. Bitlocker, filevault 
1. Self encryption device: Hardware based full disk encryption
 1. No operating system software needed
 1. SED standard is Opal standard

## Operating system hardening

1. All os, even mobile ones
1. Updates:
 1. Service packs
 1. Security patches
1. Users:
 1. Password length
 1. Access control
1. Network access + security
1. Monitors + antivirus/malware

## Patch management

1. Timely updates
1. Third party updates: applications + devices
1. Auto-update is not always best, might break or be vulnerable
1. Emergency updates: 0day attacks make this necessary

## Sandboxing

1. Applications access only what they need
1. Sandbox good in development stage
1. VM, inframes, mobile devices, etc
