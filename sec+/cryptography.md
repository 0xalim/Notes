# Cryptography

1. Confidentiality: secret
1. Authentication: Knowing its who they say they are
1. Non-repudiation: We can verify they sent that info
1. Integrity: Tamper-proof

## Cryptography terms

1. Plaintext: Unencrypted message
1. Ciphertext: Encrypted messageg
1. Cipher: Algorithm
1. Cryptanalysis: Crack encryption

## Keys

1. Add to cypher to encrypt
1. Larger is better
1. More keys are better

## Overhead

1. Make weak key stronger:
 1. Hash password, hash the hash of password
 1. Key strengthening
 1. Bruteforce required for all hashes

## Keystreching library

1. bcrypt:
 1. Generate hash from password
 1. Blowfish cipher, multiple rounds of hashing
 1. PBKDF2: Rsa standard

## Lightweight

1. Less cpu for IoT devices
1. New standards:
 1. Emerging tech

## HHomomorphic encryption

1. Encrypted data is hard to work with:
 1. Decrypt
 1. Compute
 1. Encrypt back
1. Homomorphic encryption:
 1. Compute while encrypted
 1. Directly on encrypted hash
1. Advantages:
 1. More secure
