# SSL Stripping

SSL stripping/HTTP downgrade:
1. Combines on-path+downgrade attack
1. Attacker sits in middle via:
 1. Proxy server
 1. ARP spoofing
 1. Rogue wi-fi hotspot
1. Victim doesn't notice besides that the website is http not secure
1. Client and sever problem

SSL:
1. SSL 2.0 deprecated 2011
1. SSL 3.0 deprecated 2015
1. TLS 1.0 can downgrade to SSL 2.0
1. TLS 1.1 deprecated 2020
1. TLS 1.2+1.3 current

How:
1. User sends request to server (http)
1. Attacker intercepts request and sends it to server (http)
1. Server sends 301 redirect back to user (https)
1. Attacker sends back request (https)
1. Attacker decrypts and sends request to original user (http)
