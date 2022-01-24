# Certificate Concepts

## Online + Offline CA

1. Compromised CA makes everything from that CA untrusted now
1. Some CA can act as online, others offline
1. Intermediate CA can be online, while root can be offline to be safe

## OSCP Stapling

1. Online certificate status protocol: Checks if cert has been revoked
1. CA is responsible for all OSCP requests
1. Instead of above, have cert holder verify their own status:
 1. OSCP is stapled into the TLS handshake
 1. Digitally signed by the CA

## Pinning

1. Compile cert into an application
1. When checking again with server, compare with stored cert
1. This allows client to make sure you are not communicating with middle person

## PKI trust relationship

1. Basic structure is single CA, all certs verified from this
1. Commonly is hierarchical:
 1. This allows easier response if a leaf/intermediate CA is compromsisd
1. Mesh:
 1. Every CA trusts every other CA
 1. Does not scale well
1. Web of trust:
 1. PGP key for example
 1. Sign certs of people you know, and they do the same
1. Mutual auth:
 1. Server + client authenticates each other

## Key escrow

1. Someone else holds your decryption keys
1. 3rd party manages those keys
1. Only when authenticated can a service be granted the key to decrypt
1. Need to trust 3rd party:
 1. Make sure they are keeping up with security
1. Carefully controlled conditions:
 1. Court order, or of that level to access these keys

## Certificate Chaining

1. Not all certs can be signed by root, how do you trust those certs?
1. Chain of trust:
 1. List all certs between root and intermediate CA
1. Chain starts with ssl, ends with root CA
1. Certs between SSL and root CA is a chain certificate/intermediate cert
