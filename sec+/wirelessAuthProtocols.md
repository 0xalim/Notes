# Wireless Authentication Protocol

## Wireless Authentication

1. Username;password is common
1. Commonly used on wireless networks

## EAP

1. Extensible authentication protocol
1. EAP has many different RFCs and variations
1. EAP integrates 802.1X:
 1. No comms until authentication

## IEE 802.1X

1. Port based network access control (NAC)
1. No access till authentication
1. Access database used in conjuction (LDAP, RADIUS)

## IEEE and EAP

1. Supplicant: Client
1. Authenticator: Device that gives access
1. Authentication server: Validates the credentials

## EAP Fast

1. EAP Flexible authentication via secure tunneling:
 1. Auth server and supplicant share PAC (secret)
1. Supplicant receives PAC
1. Supplicant and Auth server authenticate to TLS
1. Common to have Auth db and eap services in 1 (RADIUS server)

## PEAP

1. Protected extensible flexible authentication protocol
1. Same as EAP but instead of PAC, use digital certificate
1. Client does not need certificate only the server
1. Microsoft + PEAP -> MSCHAPv2 databases
1. Supplicants can authenticate also with:
 1. Generic token card
 1. Hardware token generator

## EAP-TLS

1. EAP TLS:
 1. Strong + wide adoption
1. Requires digital certificate on server + all devices:
 1. Exchanging of ceritificate for mutual authentication
 1. TLS tunnel then active
1. Complex implementation:
 1. Public key infrastructure (PKI) to manage certificates in environment
 1. Deply + manage all certs on all clients
 1. Not all devices can support this

## EAP-TTLS

1. EAP tunneled TLS:
 1. Only single certificate on single server
 1. Build tls tunnel using this digital
1. Use other authentication method inside this tls tunnel

## RADIUS Federation

1. Members in one org can authenticate to network of another org
1. 802.1X as authentication method:
 1. Radius in backend
 1. EAP to authenticate
1. Driven by eduroam:
 1. People in one university can use their login in another university
