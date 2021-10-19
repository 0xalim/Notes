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

1. 
