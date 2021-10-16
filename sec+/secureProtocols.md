# Secure Protocols

## Voice and Video

SRTP: Secure real-time transport protocol
1. Secure version of rtp
1. AES communication
1. Authentication, integrity + replay protection
 1. Due to SHA1 hashing

## Time Synchronization

NTP: Network time protocol
1. Attackers use ntp to amplify for DDoS
1. NTPSec: Secure ntp
 1. Cleaned up vulnerability

## Email

S/MIME: Secure multipurpose mail extensions
1. Uses public key encryption + digital signing of mail content
1. Requires public key infrastructure

Pop3 / IMAP:
1. Use starttls to encrypt pop3 with ssl or use imap with ssl

TLS:
1. Encrypt browser communication with tls

## Web

1. TLS: Not ssl (old)
1. HTTP:
 1. Public key encryption
 1. Symmetric key delivered via asymmetric encryption

## IPSec

1. Security for OSI Layer 3
 1. Authentication for every packet
1. Confidentiality + integrity:
 1. Encryption and packet signing
1. Standardized:
 1. Can also be vendor specific
1. IPSec protocols:
 1. Authentication Header
 1. Encapsulation security payload

## File transfer

FTPS: FTP over ssl so secure
1. File transfer protocol

SFTP:
1. Over ssh
1. Gui file system available

## LDAP

LDAP: lightweight directory access protocol
1. Used to access central domain on network
1. Protocol for reading and writing directories over ip network
1. Windows active directory, apple opendirectory + more use ldap

## Directory services

LDAPS: ldap with ssl
SASL: simple authentication and security layer
1. Framework that apps use to communicate safely:
 1. Using kerberos
 1. Client certificates

## Remote access

SSH:
1. Replaces telnet since encrypted
1. More security for terminal remote access

## DNS

DNSSEC: DNS security extensions
1. Validates DNS responses providing authenticaton + integrity
1. Uses public key crypto

## Routing and switching

SSH
1. Can use to remote access

SNMPv3: Simple network management protocol v3:
1. Proviedes:
 1. Confidentiality
 1. Integrity
 1. Authentication

HTTPS:
1. Same as ssh but over web browser
1. Secure verison of http

## Network address allocation

DHCP:
1. Gives ip address to machines on network
1. Attackers can modify a lot since bad security here
1. Rogue DHCP need to be authorized
1. DHCP snooping
1. Starvation of DHCP pool by spoofing mac address on single machine

## Subscription services

Automated subscriptions:
1. Anti virus/malware signature updates
1. IPS updates
1. Firewall updates, ip address malicious db update

1. Each subscription uses different update method
1. Need to figure out what protocols these updates use so more security
