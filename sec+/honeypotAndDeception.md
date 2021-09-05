# Honeypots and Deception

Honeypot:
1. Attract attackers
1. Not machine with real assets
1. Good to find out methods attackers are using
1. Kippo, google hack honeypot
1. Best to make it look like a real system

Honeynet:
1. Multiple honeypots
1. Honeyfiles:
 1. Bait like passwords.txt
 1. Alert is sent, automated
 1. Trap files

Fake telemetry:
1. Machine learning, big data and finds patterns
1. Train machine learning with malware, and malicious files to know
   what is bad and what is good
1. Attackers send fake telemetry to machine so it learns bad stuff
   once used in production, hackers know holes in machine

DNS sinkhole:
1. DNS handing out incorrect ip address, blackhole dns
1. Attackers redirect malicious websites
1. Can be good:
 1. Redirect malicious domains to benign ip address
1. Integrated with firewall:
 1. Used to identify infected devices trying to communicate outside
 1. Automated alert if needed
