# Cryptographic keys

1. Algorithms, process is all open source
1. Keys determine encrypted data, hash value and digital signature

## Key strength

1. Larger keys are more secure, prevent bruteforce
1. Symmetric encryption = 128 bits
1. Asymmetric keys = 3072 or larger

## Key exchange

Logistical challenge:
1. How do you transfer key across insecure medium
1. Out-of-band key exchange:
 1. Don't send symmetric over the network
 1. Telephone, courier, in-person, etc
1. In-band:
 1. On network
 1. Encrypted key
 1. Asymmetric encryption to deliver symmetric key

## Real-time encryption/decryption

1. Client encrypted key with servers public key
1. Server decrypts with its private key
1. Session key established
1. Needs to be changed often (ephemeral)

## Symmetric key with asymmetric key

Diffie-hellman:
1. Local private uses others public to produce same symmetric keys

## Traditional web server encryption

1. TLS uses encryption keys to protect web server communication:
 1. Based on RSA key pair
 1. One key that encrypts all symmetric keys
1. 1 Point of failure for all web encryption

## PFS perfect forward secrecy

1. Elliptic curve or diffie-hellman ephemeral
 1. Used for session only, afterwards they are not used
1. Every session has different keys
1. More computing power is required
1. Browser needs to support pfs
