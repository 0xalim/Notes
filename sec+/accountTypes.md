# Account Types

## User account

1. User account per person
1. ID number available for userid
1. Storage and files can be private to user
1. Does not have privileged access

## Shared and Generic Accounts

1. Shared accounts
1. Used by multiple people, guest login, anonymous login
1. Hard to create audit trail
1. Hard to give proper privileges
1. Password management becomes difficult
1. Best practice: dont use them

## Guest account

1. Allows access to computer without username password
1. Restricted access to operating system
1. Still hard security challenge due to privesc possible
1. Must be controlled, even disabled

## Service Accounts

1. Used by background services:
 1. Web server
 1. Database server
1. Can be defined for specific service:
 1. Can give granular privileges as needed
1. Commonly use username and password
1. Privileged accounts:
 1. Root or admin account
 1. Complete access to system
 1. Manage hardware, driver, software, etc
 1. Should not be used for normal administration
 1. Needs to be secure: 2fa, recent password changes
