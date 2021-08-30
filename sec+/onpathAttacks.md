# On-path Attack

On-path attacker/MITM:
1. Attacker sits in middle of 2 entities
1. Can:
 1. Intercept
 1. Modify information
1. Possible noone knows attacker is present

What happens:
1. Packets sent to attacker
1. Packets then redirected to destination
 1. May never know it was redirected

ARP poisoning (Address resolution protocol):
1. No security for arp protocol
 1. No authentication
 1. No encryption
1. Implementation:
 1. Workstation sends arp packet asking for destination to return MAC address
 1. Destination replies with mac address
 1. Workstation stores mac address in arp cache locally
1. Attack:
 1. Attacker replies to workstation without any initial prompt with routers
    mac address + ip address, faking it's machine essentially.
 1. Works without prompt because arp has no security

MITM browser attack:
1. The MITM in this case in malware, trojan or virus transmitting data to
   attacker
1. Advantages:
 1. Easy to proxy encrypted traffic
 1. Everything looks normal to victim
 1. Malware waits for you to login specific websites, banks for example
