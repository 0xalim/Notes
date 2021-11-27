# Access Control

## Access Control

1. Authorization:
 1. What resources does user have access to
 1. Policy enforcement
 1. Determine rights for user

## Mandatory Access Control (MAC)

1. Every object has a label:
 1. Confidential, secret, top secret, etc
1. Users have lowest access:
 1. Users labeled based on predefined rules

## Discretionary Access Control (DAC)

1. Creating objects that users choose what others can/cannot do
1. Create spreadsheet, owner chooses who can do what

## Role based Access Control (RBAC)

1. Role in organization:
 1. Manager, director, project leader , etc
1. Admin provides access based on role of user
1. Windows:
 1. Done in groups

## Attribute based access control (ABAC)

1. Users can have complex relationships to apps and data
1. Consider many parameters
1. System evaluates resources, ip address, time, action, etc
1. Then authorize or not

##  Rule based Access Control

1. Generic term for rules
1. Access determined through rules
1. Rules linked to objects
1. User logging into lab, can only do so from 9-5 or specific browser

## File System Security

1. Storing and accessing files
1. Access control list or:
 1. Group and user permissions
 1. Centrally admined for users
1. Encryption can be built in (ntfs, bfs)

## Conditional Access

1. For mobile workforce, what device they use, changing cloud
1. Conditions:
 1. Employee or partner
 1. Location
 1. Type of apps/device
1. Control:
 1. Allow or block
 1. Require multi factor auth
 1. Limited access from remote
1. Admin can build complex access rules

## Privileged Access Management (PAM)

1. Manage elevated access to system resources
1. Manage admin or root
1. Store privi accounts in vault:
1. Advantage:
 1. Centralized password management
 1. Easy automation
 1. Easy audit + tracking
