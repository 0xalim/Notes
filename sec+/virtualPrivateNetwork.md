# Virtual Private Network

1. Sending data encrypted between 2 points
1. Concentrator:
 1. Device that encrypts/decrypts all the information
 1. Can be standalone, or integrated in the firewall
1. Deployment options:
 1. Software based
 1. Hardware based cryptographic devices
1. Built into os or through 3rd party applications

## Remote access vpn

1. Encrypted tunnel created between you and concentrator while remote accessing
1. All information from concentrator to internal network sent through cleartext
1. Some software only allow communication that is encrypted through vpn usage

## SSL VPN

1. Port 443
1. Not client in the middle, remote access between 2 machines
1. Need to authenticate users, no digital certs or passwords needed
1. Can be run from a browser
1. HTML5 VPN:
 1. Includes web cryptography API
 1. Can create vpn tunnel without vpn applications
 1. Need browser that is compliant with html5

## Full tunnel

1. All data travels through concentrator first
1. Concentrator then decides where data goes

## Split tunnel

1. Admin can configure so some data goes through concentrator, but not all
1. Data that is destined to be for another location can go through split tunnel

## Site-to-site vpn

1. Remote site needs communication with corporate network:
 1. 2 Concentrator or firewalls that has concentrator integration communicate
    through a vpn tunnel, all data then travels to destination in cleartext
1. Always almost on in this situation

## L2TP

1. Layer 2 tunneling protocol
1. Commonly implemented with IPsec
1. Connecting 2 networks as if same layer 2 network, but reality its layer 3

## IPsec

1. Internet protocol security
1. Authentication + encryption for all packets
1. OSI layer 3
1. Packet signing possible providing confidentiality and integrity
1. Standardized
1. AH - authentication header
1. ESP - Encapsulation security payload

## Transport mode and tunnel mode

1. Transport port:
 1. Keep ip header in place, but encapsulate data with IPsec header and trailer
 1. Issue is ip header still in cleartext
1. Tunnel mode:
 1. New ip header in place, original data and ip header encapsulated in IPsec
    header and trailer

## Authentication header

1. No encryption just integrity checking for all communication
1. Add hash of packet to the packet
1. No encryption, but can guarentee data origin

## Encapsulation security payload

1. Encryption + authentication for data
1. SHA2 for hash, AES for encrypti on
1. Format:
 1. New header + esp header + real ip header + data + esp trailer + integrity
    check value

## Combine both AH + ESP

1. Transport mode format: ip header + ah header + esp header + data + esp
   trailer + integrity value
1. Tunnel mode format: new header + ah header + esp header + real header + data
   \+ esp trailer + integrity value
1. Tunnel mode most common for better security
