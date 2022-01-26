# Incident Response Process

## Security Incidents

1. Employee phished and installed malware
1. Ddos attack currently
1. Confidential information is stolen and leaked
1. Contacted by ransomware and more

## Roles and responsibilities

1. Specialized group to respond to these things
1. IT security management
1. Compliance officers, legal guys
1. Technical staff, frontliners who fix stuff
1. and more

## NIST SP800-61

1. Standard to follow for incident response
1. Incident response lifecycle:
 1. Preparation
 1. Detection and analysis
 1. Containment, eradication, and recovery
 1. Post-incident, activity

## Preparing for an incident

1. Communication methods: phones, email, etc
1. Hardware + software:
 1. laptops, media, forensic software, cameras and more
1. Incident analysis resources:
 1. Document
 1. Network diagrams
 1. Critical file hash values making sure files dont get changed
1. Incident mitigation software: clean os and application images
1. Policies inplace so everyone knows what to do when it happens

## Challenge of detection

1. Different level of details
1. Large volume of attacks, how do you find the most critical
1. Always complex to fix

## Incident precursors

1. These are heads up for incoming attacks:
 1. Logs
 1. Exploit announcment of a service in use by company
 1. Direct threat

## Incident indicators

1. An attack is underway
1. Buffer overflow detected by ids/ips
1. Antivirus detects malware
1. Host-based monitor detects modified files
1. Network traffic compared to the normal traffic

## Isolation and containment

1. Bad to not fix asap
1. Virus, worms can spread
1. Sandbox needed to analyze results and view if files are infected
1. Malware can monitor connectivity which is bad

## Recovery after incident

1. Remove malware, user accounts, fix vulnerabilities, patch
1. Recover via backups or rebuild from scratch
1. Repair vulnerability from newly backup/scratch system

## Reconstitution

1. Not easy to fix lots of machines
1. Months of work to fix all everything
1. Start with quick changes: patch, firewall settings
1. Later phases involve infrastructure changes

## Lessons learnt

1. Always learn and improve
1. Meeting after to improve problems
1. Dont wait too long
