# Protecting Data

Data:
1. Important asset
1. Many locations:
 1. drive
 1. Network
 1. CPU
1. Encryption, security policies to protect
1. Differnet permissions for different people

Data sovereignty:
1. Different laws everywhere
1. GDPR - general data protection regulations:
 1. collecting data on eu ppl, store in eu only

Data masking:
1. Data obfuscation, hiding data revealing only a bit
1. ^ to protect PII, personal identification information
1. Hidden from view, but still stored somewhere
1. Many techniques:
 1. Substituting, shuffling
 1. Masking and encryption

Encryption:
1. Encode to unreadable format
 1. plaintext -> ciphertext
1. 2-way, can go back and forth if you have key

Diffusion:
1. 1 change in plaintext, massive change in ciphertext

Data at-rest:
1. Data in ssd, hdd, etc
1. Need to encrypt data:
 1. Encrypt everything
 1. Only db
 1. File/folder-level
1. Apply permissions:
 1. Access control lists
 1. Only authorized users

Data in-transit:
1. Data moving accross networks
1. Not much protection, due to so many switches, routers and devices
1. Network based protection, firewall and ips
1. encryption:
 1. TLS, transport layer security
 1. IPsec, internet protocol security

Data in-use:
1. Data in cpu, ram, register and cahce
1. Almost always decrypted

Tokenization:
1. Replace sensitive data with non-sensitiev placeholder
1. Credit card processing, temp token during payment
1. Middle man cannot use them later
1. No encryption or hashing:
 1. Not mathematically related
 1. No encryption overhead
1. How:
 1. device registers personal data with remote token service server
 1. Server sends token back
 1. Use device to do task
 1. Token transmitted to payment processing server, communicat with token server
 1. Once valid, approve transaction

Information Rights Management (IRM):
1. Control documents, pdf, emails and more
1. Restrict access to unauthorized person:
 1. Prevent copy/paste
 1. Control screenshot
 1. Manage printing
