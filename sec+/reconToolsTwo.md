# Reconnaissance Tools Part 2

## curl

1. Client url
1. Retrieve raw data
1. Easy to parse, and automate tasks

## IP scanners

1. Scan a network for ip address
1. Locate active devicse
1. Uses:
 1. arp if local subnet
 1. ICMP for outside local subnet
 1. TCP ACK, ICMP timestamp requets for above too
1. Response means more recon can be done -> nmap, hping, etc

## hping

1. TCP/IP packet assember/analyzer
1. Craft your own packet and send to device
1. Can modify ip, tcp, udp, icmp and more

## nmap

1. Network mapper
1. Port scanner
1. Find operating system + version + kernel version
1. Service scan for more information
1. Additional scripts (NSE) vulnerability scanner

## theharvester

1. Automated OSINT from terminal
1. Scrap information from many websites
1. DNS brutueforce

## sn1per

1. Combine many recon tools
1. metasploit, nmap, theharvester...
1. Can be intrusive and non-intrusive
1. Permission required before use

## scanless

1. Run port scan from differnet hosts
1. Good for stealth scanning

## dnsenum

1. Enumerate dns information from dns server
1. View host information from dns server
1. Find host names in google
1. Subdomain enumeration

## nessus

1. Vulnerability scanner
1. Identify known vulnerabilities
1. Extensive reporting, even how to fix vulnerability

## cuckoo

1. Sandbox for malware
1. Test for malware in safe environment
1. Virtualized environment
1. API calls, network traffic, memory analysis, traffic capture, screenshots
