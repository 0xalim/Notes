# Cryptographic Attacks

How do you know received encrypted data did not get modified/read?
1. Attacker doesn't have key, so they need to break cryptography
1. Implementation is the problem, not the cryptography itself

Birthday Attack:
1. Chances people share same birthday in small class is higher than expected
 1. Compare every student to every other, rather than 2 student comparison
1. Known as hash collision
 1. 2 plaintext, equal to same hash
 1. Find collision via bruteforce
1. Attacker tries to generate hash's of same value for collision
 1. Protection: increase hash output size

Collisions:
1. Hash digests are supposed to be unique
1. MD5 hash collision during '96 is an example

Downgrade attack:
1. Mitm where you influence both parties to downgrade to easier hash
