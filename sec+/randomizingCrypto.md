# Randomizing Cryptography

Non-randomized crypto:
1. Easy to tell what the original information is if no randomization
1. Security concern
1. Can reverse by having lots of similar encrypted data

Cryptographic nonce:
1. Nonce = arbitrary number used once, hence nonce
1. (pseudo)Random number
 1. Should not be easy to guess
1. Using nonce to login
 1. Server sends nonce
 1. Hash pass with nonce
 1. Send pass to server
1. No replay attack possible
1. Type of nonce:
 1. IV - Initialization vector
 1. IV used for WEP, and some SSL
1. Salt:
 1. Nonce for passwords
 1. Makes pass hash unpredictable
