# Enhancing Privacy

## Tokenization

1. Data and replace with another data (token)
1. Can tie token to original data
1. Common with credit card processing:
 1. Mobile especially
1. Not encryption or hashing:
 1. Cannot find original data unless have db
 1. No overhead
 1. No mathematical relationship

## Data minimization

1. Minimal data collection
1. Regulations:
 1. HIPAA has this rule
 1. GDPR has similar, relevant but not excessive information
1. Internal data use should be limited
1. Cannot look through medical records of specific people for example

## Data masking

1. Obfuscate data
1. Protects PII
1. May only be hidden from view:
 1. Stored data is still intact
 1. Control view based on perms
1. Techniques:
 1. Masking, substituting, shiffling, etc

## Anonymization

1. Completely make it impossible to identify individual data from dataset
1. Hashing, masking
1. Leave out personal info but keep public info open, can still analyze but keep things private
1. Cannot be reversed

## Psuedo-anonymization

1. May be reversible
1. Random replacement:
 1. Record displays different content everytime it is viewed by tied to actual content
1. Consistent replacement:
 1. Constantly shows same replacement
