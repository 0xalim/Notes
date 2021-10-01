# Authentication methods

## Directory services

1. Keeps all orgs user;pass in single database
1. Constantly updated+replicated
1. All authentication request made go to this db
1. Example: Kerberos / LDAP

## Federation

1. Authentication to network via creds from 3rd party
1. Example: Login with linkedin, logged in your network

## Attestation

1. Make sure people use hardware and software you give them
1. Because: Software + hardware you trust + easy to troubleshoot
1. Should be automated
1. Remote attestation:
 1. Machine provides report to verify its software/hardway
 1. Encrypted + signed
 1. IMEI can be included in the report

## Short Message Service (SMS)

1. Text message
1. Login sent to phone via SMS
 1. Authentication code for example
1. Less secure
 1. Numbers can be assigned to differnet phones
 1. SMS can be intercepted

## Push Notification

1. Authenticate via application on mobile device
1. Authentication code shown in this app
1. Security security:
 1. No encryption with this application
 1. Application is vulnerable

## Authentication app

1. Pseudo random number gen
1. Can be physical hardware token generator
1. Software based

## TOTP

1. Time based one Time password algorithm
 1. Time limit for authentication code
 1. Login to service, they require code open your app and code is there
 1. Algorithm is based on time of day + secret key

## HOTP

1. HMAC-based one Time password
1. One time password, use one and never again
1. Token-based authentication (numbers)

## Phone Call

1. Automated phone call says number
1. Less secure:
 1. Modify phone configuration
 1. Number added to multiple devices, MITM

## Static Code

1. Authentication that does not change
1. Think PIN
1. Numbers + characters

## Smart Card

1. Integrated circuit:
 1. Contact: Slide into reader
 1. Contactless: Near reader
1. Credit cards / access control cards
1. Certificate on this card
1. Multiple authentication included ontop
