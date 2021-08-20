# Password Attacks

Plaintext/unencrypted passwords:
1. Storing passwords in cleartext
1. No encryption
1. Bad way to store passwords

Hashing passwords:
1. String of text
1. Message Digest / fingerprint
1. Fixed length
1. Unique (not duplicated, meaning no collision hopefully)
1. One-way trip:
 1. Impossible to recover original password from a digest
 1. Common way to store passwords
1. SHA256 is an example

Password file:
1. /etc/shadow on linux for example
1. Different hashing algorithm

Spraying Attack:
1. If you try to login with incorrect password then you get locked out
1. Spraying attacks use most common passwords but small amount of them
 1. Top 10 passwords for example

Brute force:
1. Try every possible password combination until the hash matchs
1. Takes a long time
1. Harder hash, longer time
1. Accounts locked out after x amount of attempts
1. Bruteforce more common in offline attacks
 1. When password file is dumped

Dictionary Attacks:
1. Use dictionary to create passwords
1. Different dictionaries for different industries if needed
1. Applications/crackers can auto change password 
 1. password -> p4ssw0rd
1. Takes long time
 1. Multiple systems / GPU cracking is common

Rainbow table:
1. Optimized pre-built set of hashes
1. Saves time and storage space
1. Doesn't have to contain every hash ever
1. No hash time, so instant lookup almost
1. Apps/os's have different hashes

Salt:
1. Random data added to start of password
1. Same passwords, different salts
1. Salt is stored with password
1. Rainbow tables don't work with salts
1. Slows down bruteforcing

When hashes get out:
1. haveibeenpwned.com, check if your creds have been dumped
