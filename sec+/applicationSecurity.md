# Application Security

1. Balancing time + quality:
 1. Security is secondary for developers
1. Quality assurance team (QA):
 1. Testing team
1. Vulnerabilities still can be found

## Input validation

1. Document sinks in code
1. Check and correct all input:
 1. Normalization, so integer inputs should only accept integers
 1. Fuzzers to automate bad input validation

## Fuzzing

1. Sending random input to applications for:
 1. Buffer overflow
 1. SQL injections

## Fuzzing engines

1. Fuzzing options:
 1. Platform dependant
 1. Language specific
1. Resource heavy (CPU/GPU):
 1. Essentially brute forcing against sinks

## Secure cookies

1. Information stored on host by browser
1. Used for tracking, personilization, session management:
 1. Not executable
 1. But contains data
1. Secure cookies have Secure attribute
1. Sensitive info should not be saved in cookies (password)

## HTTP secure headers

1. Additional layer of security for web usage:
 1. Gives more configuration
 1. Not 100% fix
1. Enforce http communication only
1. Only allow scripts, css from same origin (prevent xss)
1. Prevent iframe to prevent xss

## Code signing

1. Confirming modifications to applications came from proper developer
1. Code needs to be signed by developer:
 1. Asymmetric encryption
 1. CA signs dev public key
 1. Dev signs code with private key

## Allow / deny list

1. Sysadmins make policies to allow/deny applications running on machines
1. Allow list:
 1. Nothing runs unless approved
 1. Highly restrictive
1. Deny list:
 1. If in list, prevent from running

## Example of allow / deny list

1. Create hash of application version, everytime it runs check hash if it is
   different then it's a different version being run so prevent it from doing so
1. Certificate: Allow signed apps from certain publishers only, microsoft.
1. Path: Only apps from certain /directory can run
1. Network: Only allow app to run in network zone

## Static code analyzers

1. Static application security testing (SAST)
1. Finds vulnerabilities:
 1. Buffer overflows
 1. DB injections, etc
1. Not everything can be found
