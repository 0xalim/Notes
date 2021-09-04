# Managing Security

Geo consideration:
1. Level implications:
 1. International business has laws
 1. Personel need passport to clear immigration if travel is needed
1. Backups stored offsite, org-owned or 3rd party
1. Offsite recovery:
 1. Need to host in new location if disaster happens
 1. Traven consideration for staff

Response and recovery control:
1. Incident response and recovery
 1. Need to document everything
 1. Identify the attack
 1. Contain the attack
1. If attack has already happened:
 1. Limit data leaking, and access

SSL/TLS inspection:
1. Secure socket layer, transport layer security
1. Inspect encrypted data without unencrypting everything
1. Relies on trust on both client+server side
1. Browser connecting to website makes sure it has been certified:
1. Certificate authority makes sure website is not malicious

SSL/TLS proxy:
1. Have firewall/ssl decryption between user and internet
1. Create internal CA certificate
1. Add certificate to user browser
1. User machine sends hello msg to web
1. Proxy intercepts, creates its own and sends to website
1. Website responds with public key CA
1. Proxy checks integrity of CA, and encrypts  between proxy and user
 1. Via creating internal key

Hashing:
1. Represent data as short string, message digest
1. 1-way:
 1. Cannot recover original message from digest
1. Used to verify downloaded documents:
 1. Checksums to check download integrity
1. Digital signature:
 1. Authentication, non-repudiation and integrity
1. Create one with no collisions:
 1. Crypto vulnerability

Example:
1. SHA256: 256 bits, 64 hex characters
1. Major difference with 1 change

API considerations:
1. Secure and harden login page, and API
1. On-path attack:
 1. middleman intercepts api message, can replay message
1. API injection:
 1. Inject data into api message
1. DDoS, use api to bring down system

API Security:
1. Authentication:
 1. Only authorized users can communicate, only secure protocols
1. Authorization:
 1. No extended access
 1. Each user has limited role
1. WAF (Web application firewall)
