# Secure Coding Techniques

Balance between time+quality:
1. Testing: QA - quality assurance
1. Vulnerabilities WILL be found.

Stored procedures:
1. Store codes so that you only call, user has no modification capabilities
 1. Example: SQL commands stored, not taken from user
 1. No direct access to server

Obfuscation/camo:
1. Readable code -> difficult to understand
1. Computer understands the code thought
1. Prevents debugging from users and looking at source code

Code reuse:
1. Security vulnerabilities may be copied if devs copy paste code
1. Dead code:
 1. Code that does processes, but result doesn't get used

Input validation:
1. Document all input methods
1. Check + correct all input (normalization):
 1. Input requires only x amount of letters/numbers
 1. Improper input "--: stuff like that
1. Fuzzers will find vulnerabilities

Validation points:
1. Server side:
 1. Checks occur server side
 1. Protect against malicious users
 1. Safest
1. Client side:
 1. Users machine will validation input
 1. Additional performance
1. Use both!

Memory management:
1. Never trust data input
1. Buffer overflows are risk
1. In-built functions may be insecure

Third-party libraries:
1. Library written by some other party
1. Don't have to write much, already given
1. Security risk:
 1. May be insecure, who knows
 1. Testing is required

Data exposure:
1. How does application handle data?
 1. Encryption when stored, across network
1. Input/output logged

Version control:
1. Store different versions of stuff, backup if needed
1. Commonly used in software development:
 1. And os, wiki software, cloud-based file storage
1. Checksums to make sure file has not been tampered
