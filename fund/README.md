# Fundamentals

This file contains fundamental information that is commonly used while doing
anything related to cybersecurity; motivation for resource is just a quick
refresher on a specific tool or idea that might need a little refresher.
Forgetting information is natural, the brain is a processing machine not a
storage device.

## Network

### whois

Find registrar, contact information, creation, update, expiration dates and
other useful information of a particular domain name.

Use: whois [DOMAIN]

### nslookup & dig

Find ip address and other information about domain names.

Use: nslookup \[TYPE] \[DOMAIN]
Use: dig \[DOMAIN] \[OPTIONS]

A -> IPv4
AAAA -> IPv6
CNAME -> Canonical name
MX -> Mail Servers
SOA -> Start of Authority
TXT -> TXT Records

### DNSdumpster.com

Lists some subdomains and also information found via nslookup & dig. Has
graphic feature showing vertices and edges + dynamic graph mode. Good for
presenting information.

Use: dnsdumpster.com

### shodan.io

Lists networks found via crawler throughout the entire internet. One of best
passive reconnaisance tools.

Use: shodain.io
