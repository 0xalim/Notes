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

### traceroute

Determine ip address of all machines hopped until destination. Works by setting
ttl to 1 of a udp packet. When TTL goes to 0 it reveals the ip address of the
machine. Keep incrementing by 1 to reveal all ip address of all machines hopped.

Use: traceroute [HOST]

### telnet

Communicate with a remote system via cli. All data is in clear text.

Use: telnet \[HOST] \[PORT]

### netcat

Functions as a client that connects to ports, or a server that listens to ports.

Use: nc \[OPTIONS] \[HOST] \[PORT]

l -> Listen
v -> Verbose (vv -> very verbose)
p -> port
n -> Numeric only

### nmap

Network mapper. Lots of insane features, can even be used as a vulnerability
scanner with NSE script options.

Use: nmap \[OPTIONS] \[PORT] \[IP]

(host discovery)
sn -> No scan
PR -> Arp packet
PE -> ICMP packet
PP -> ICMP timestamp
PM -> ICMP address mask
PS -> TCP ping (3-way if not privileged)
PA -> TCP ACK  (3-way if not privileged)
PU -> UDP ping

(port discovery)
sS -> TCP SYN (priv)
sT -> TCP connect
sU -> UDP
sX,sF,sN -> XMAS, FIN, NULL (good for stateless firewall)
sA -> TCP ACK (good for figuring out firewall rules)
sW -> Window Scan (same as above)
T# -> 0-5(paranoid, insane) {1: irl, 4: ctf}
--max-rate -> max packets per second

(spoofing)
S -> spoof ip address
e -> network interface
pn -> no ping
sI -> provide zombie ip
--spoof-mac -> spoof mac address (only if on same subnet)
D -> add multiple decoy ip address amongst your own
  (ME for my ip, RND for random ip)
f -> fragment packets into 8 (ff for 16)
mtu -> select size of packet (choose multiple of 8)

(info)
--reason -> gives reason as to its conclusion
v -> verbose (vv for very verbose)
d -> debug (dd for detailed debug)
sV -> service version

(nse + format)
sC -> default script
--script -> set specific scripts
  (auth, broadcast, brute, default, discovery, dos, exploit, external, fuzzer,
   intrusive, malware, safe, version, vuln)
oN -> Normal
oG -> Greppable
oX -> XML

### hydra

Used to bruteforce login details for any service.

Use: hydra -l [USER] -p \[PASS] \[OPTIONS]

l -> user (-L userlist)
p -> pass (-P passlist)
s -> port (include service name, e.g -s 10021 ftp)
v -> verbose (very verbose)
o -> out file
