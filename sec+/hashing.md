# Hashing and Digital Signature

1. Represents data as a short string of text
 1. Message digest or fingerprint
1. One way trip:
 1. Cannot recover msg from digest
 1. Stores passwords
1. Checksums for download integrity
1. Used for digital signature for authentication + integrtiy
1. Aim for no collision

## Hash example

1. Sha 256
1. 256 bits / 64 hex
1. Small change in msg = large hash difference

## Collisions

1. Any size input, same fixed string size
1. Hash should be unique
1. MD5 hash collision

## Practical hashing

1. Checksum on website and hash on your end should be same for integrtiy
1. Password storage so its not plaintext + salt for better security

## Adding salt

1. Salt is random data added to password when hashing
1. Same password looks different in hash due to salt
1. Rainbow tables dont work with salted hash
1. Slows down brute force

## Digital signature

1. Prove message has not been altered - integrity
1. Prove source of message - authentication
1. Not fake signature - non-repudiation
1. Signed with private key
1. Verified with public key
