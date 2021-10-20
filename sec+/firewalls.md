# Firewalls

1. Controls ingress/egress traffic
1. Network security
1. Can include content filtering
1. Can include antivirus/malware

## Network based firewalls

1. Filter traffic via ip+port
1. Next generation firewalls determine applications instead
1. Can encrypt traffic
1. Firewalls can be layer 3 devices (routers) so easy to replace:
 1. Advanced routing functionality
 1. NAT functionality

## Stateless firewall

1. Dumber firewall, doesn't know much about context of user-server interaction
1. Each packet is individually examined
1. Need to create rules for inbound/outbound

## Statefull firewall

1. More secure + intelligent
1. Remembers state of the session
1. Everything thats termed valid will be allowed
1. Only has ingress for communication, if it allowed it then doesnt worry about
   continuing communication
1. Session table keeps tabs on active user-server comunication
1. If server starts communicating first, firewall will deny since no session has
   been created

## UTM

1. All in one security appliance
1. Unified threat management (UTM): web security gateway
1. URL filter
1. Content inspection
1. Malware detection
1. Spam filter
1. Routing + switching
1. Firewall, IDS/IPS
1. VPN

## Next generation firewall

1. OSI application layer, tabs on all data in packet
1. Application layer gateway, deep packet inspection (common names)
1. Every packet analyzed + categorized so it can filter correctly
1. Network connected devices
1. Includes IPS that identifies application + apply specific signatures
1. Content filtering and url filtering

## Web application firewall (WAF)

1. Only for web based applications (http/https)
1. Allows / denies depending on user input
1. Stop XSS, SQL injection

## ACL

1. Access control list that allows/denies based on rules
1. Only matched packets will be allowed / denied
1. Top-to-bottom approach:
 1. Keep going down list if it does not match

## Firewall characteristics

1. Opensource vs proprietary:
 1. Open source provides traditional firewall functionality - ...cmon
 1.  Proprietary include application control and high-speed harder -  yikes
1. Hardware vs software:
 1. Hardware provides more speed + flexibility
 1. Software installed on any machine
1. Appliance vs host-based vs virtual:
 1. Appliance has fastest throughput
 1. Host-based are application aware + can view plaintext
 1. Virtual provides east/west network sec
