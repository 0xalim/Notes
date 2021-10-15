# Symmetric and Asymmetric Crytography

## Symmetric

1. Single shared key
 1. Encrypt and decrypt with it
 1. If it's leaked you need another key
1. Secret key algorithm
1. Difficult to distribute with others
1. Faster than asymmetric

## Asymmetric

1. Public key cryptography
 1. Two or more keys
 1. 1 Public 1 private
1. Private noone can see
1. Public everyone can see
1. Only private can decrypt public encrypted

## Key pair

1. Asymmetric encryption
1. Key generation:
 1. Lots of randomization + prime numbers
 1. Everyone can have public, only 1 with private

## Symmetric from asymmetric

1. Using asymmetric to generate public on both sides
1. Xpriv+Ypub = SymKey; Ypriv+Xpub = SymKey

## Elliptic curve cryptography

1. Asymmetric encryption:
 1. Large overhead from math computation
1. Elliptic crypto:
 1. Smaller keys
 1. Smaller storage required
 1. Good for mobile + iot devices
