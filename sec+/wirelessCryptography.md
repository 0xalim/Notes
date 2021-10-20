# Wireless Cryptography

## Securing wireless network

1. Need to properly secure wireless communication due to confidential info
1. Authenticate users:
 1. Who gets access to network?
 1. user;pass + 2fa + other authentication
1. Ensure all traffic is encrypted
1. Verify integrity of communication via message integrity checks

## Wireless encryption

1. Wireless computers are radio transmitters + receivers
1. Encryption necessary to prevent others from dropping in
1. Only people with encryption/decryption key can listen and transmit

## WPA2 and CCMP

1. Wifi protected access 2
1. CCMP block cipher mode:
 1. Counter mode + cipher blockc chaining message authentication protocol
1. CCMP data confidentiality with AES, integrity check using CBC-MAC

## WPA3 and GCMP

1. Wifi protected access 3
1. GCMP:
 1. Galois counter mode protocol
 1. Stronger than wpa2
1. GCMP confidentiality with AES, integrity check using GMAC (Galois)

## WPA2 PSK problem

1. Pre shared key brute-force problem:
 1. Gain hash to party by listening in on four-way handshake initially
 1. Some other methods exist to gain hash without listening to above
1. With has, attacker can bruteforce the pre-shared key:
 1. Weak psk is easy to bruteforce
 1. GPU faster at bruteforcing now
 1. Cloud-based systems can crack single password

## SAE

1. WPA3 changes pre-shared key authentication process
1. Mutual authentication: both parties authenticated
1. Create session key without sending key across network
1. No four way handshake
1. Session key thrown away when terminated communication
1. Simultaneous authentication of equals (SAE):
 1. Diffie hellman derived key creation
 1. Added authentication component
 1. Everyone uses different session key, even with same pre-shared key
 1. Dragonfly handshake (IEEE)
