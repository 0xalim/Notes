# Stream and Block Ciphers

## Stream cipher

1. 1 byte at a time encryption:
 1. High speed, low hardware complexity
1. Used with symmetric encryption
1. IV should be different for more security

## Block cipher

1. Encryption of fixed sized length - 64/128 bit blocks
1. Padding if not equal to above values
1. Symmetric encryption
1. Modes of operation is different methods of block cipher

## ECB (electronic codebook)

1. Simplest encryption mode so not very secure
1. Each block encrypted with same key

## CBC (Cipher block chaining)

1. Easy to implement
1. Each block is XORd with previous ciphertext block
 1. Adds randomization
 1. Uses IV for first block since no previous to xor with

## CTR (Counter)

1. Acts like stream cipher
1. Counter encrypted with key then XORd with paintext to get ciphertext
1. Increment counter, and repeat above

## GCM (Galois counter mode)

1. Encryption with authentication:
 1. Combine counter mode with galois authentication
1. Fast and efficient
1. Used in packetized data: wireless, ipsec, ssh, tlc
