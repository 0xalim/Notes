# Other Application Attacks

Memory vulnerabilities:
1. Difficult
1. Memory leak:
 1. Memory never deallocated
 1. Grows in size
 1. System crash
1. Null pointer dereference:
 1. Attacker gets program to point and reference null rather than actual value
 1. Application can crash
 1. DoS
1. Integer overflow:
 1. Buffer overflow
 1. Too much integer in small size, overflows to other areas of memory
 1. Too hard to repeat

Dir traversal:
1. Can read files normally should not be able to
1. Vulnerable to web server software vulnerability
1. Web application code vulnerability, bad coding

Improper error handling:
1. Errors normally happen, think 404 on websites
1. Erro message shows too much information:
 1. Network information
 1. Service version
 1. Memory dump
 1. Stack trace
 1. DB dump

Improper input handling:
1. All input should be considered malicious
1. Invalid input:
 1. SQL injection
 1. Buffer overflow
 1. DoS
1. Takes lots of time to find this

API Attack:
1. Looks for vulners in this communication path
1. Expose data, DoS, intercept traffic, get priv access
1. Similar to get request, api requests and responses
1. Application can interpret these api calls

Resource exhaustion:
1. Use up resources of machine causing DoS
1. ZIP bomb:
 1. 4.5kb file uncompresses to 4.5 petabyte
 1. Anti-virus will identify this
1. DHCP starvation:
 1. Attacker floods network with fake new devices asking for ip address
 1. Fake device due to MAC address change each time
 1. DHCP pool exhausted, no more ip addresses to give
 1. No users can connect to network due to no more ip address to give
 1. Configuration needed to limit dhcp request
