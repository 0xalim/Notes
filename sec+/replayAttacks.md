# Replay Attacks

Attacker gains access to raw data:
1. Network tapping (physical)
1. Arp poisoning
1. Installed malware
Then attacker replays attack to appear as someone else

* Not an on-path attack (like mitm), users don't even need to be logged in

Pass the hash:
1. Attacker captures login from client -> server
1. User+password is hashed
1. Attacker now tries to login via those hashed value
1. Server may log the user in
1. Prevention:
 1. SSL encryption between client <-> server
 1. Session ID creates unique authentication hash every time

Browser and Session ID:
1. Cookie contains confidential information (user+pass)
1. Cookie may contain session information and tracking/personalization stuff
1. Considered a privacy risk due to personal data
1. Session ID can be stored in cookie
 1. Attacker tries a session hijack

Session hijacking (or sidejack):
1. Normal user logs in
1. Server tags them with session id
 1. To authenticate user only once, not for every action
1. Session id sniffed by attacker
1. Attacker uses session id to mask as normal logged in user

Header manipulation:
1. Session id gathered via wireshark/kismet
1. Exploits:
 1. XSS exploit to gather session id
1. Modify headers:
 1. In every request made to server to mask as normal user
1. Modify cookies:
 1. Modify cookie in browser to mask as normal logged in user

Prevent session hijacking:
1. Encrypt end-to-end
 1. HTTPS always
 1. Browser extension: HTTPS everywhere, force-tls
1. Encrypt end-to-somewhere (doesn't support https)
 1. Encrypted tunnel
 1. VPN
