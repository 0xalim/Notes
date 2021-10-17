# Database Security

1. Protection of data + transportation of data
1. Need to meet compliance - HIPAA, GDPR, etc
1. Breaches are expensive + bad rep

## Tokenization

1. Data is replaced with tokens
1. Used to store credit card numbers:
 1. Temp number during purchase
 1. Thrown away after purchase
1. Not encryption or hashing, no overhead

## Hashing passwords

1. Represent fixed length string of text
1. No collisions if possible
1. One way trip, common to store passwords

## Adding salt

1. Salting hash so same password has different hashes
1. Rainbow table don't work with salted hashes
 1. Slows brute forcing
