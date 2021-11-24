# Cloud Security Controls

## HA Across Zones

1. Available zones:
 1. Self contained isolated locations within cloud region
 1. Multiple regions in the world
 1. Each has independant networking, etc
1. Build applications to be readily available
 1. Run as active/standby
 1. Apps can detect outage and move to another az
1. Can use load balancer for better availability

## Resource Policies

1. Identity and Access Management (IAM)
1. Who gets what access, and access to what
1. Map job functions to roles
1. Granular controls due to cloud :
 1. IP, time, group
1. Centralized user accounts over all platforms

## Secrement Management

1. API keys, passwords, certs
1. Can be easily overwhelming
1. Manage access control policy
1. Need audit trail and logging

## Integration and Auditing

1. Security accross multiple platforms:
 1. Different os, and applications
1. Log storage and reporting:
 1. SIEM good here
1. Auditing:
 1. Validate security controls
 1. Verify compliance with financial and user data
