# Penetration Testing

Pentest:
1. Simulated attack
1. Vulnerability scanning + exploiting the vulnerability
1. Compliance mandate: Test every x days
1. NIST: National instituion of standards+technology
 1. Has guides to testing systems

Rules of engagements:
1. Important document to define purpose and scope
 1. Everyone is aware of test parameters
1. Type of testing:
 1. Physical, internal, external
 1. At what times
1. Rules:
 1. Ip address range
 1. Emergency contacts
 1. How to handle sensitive information
 1. In-scope and out of scope devices/applications

Working knowledge:
1. Unknown: pentester builds from bottom up
1. Known: full disclosure
1. Partially known: Mix of above, focus on certain systems

Exploiting vulnerabilities:
1. Try to break into the system:
 1. Not causing DoS or crashing the systems
 1. Buffer overflows can cause above
 1. Gain privileged access
1. Different vulnerability types:
 1. Brute force
 1. Socia lengineering
 1. DB injection
 1. Buffer overflow

The process:
1. Get into the network, initial exploitation
1. Lateral movement: move from system to system
1. Persistence:
 1. Install backdoors, make accounts, modify passwords
1. Pivot:
 1. Central pivot point to move anywhere on system

Aftermath:
1. Cleanup:
 1. Leave network in original state
 1. Remove binaries and temp files
 1. Remove backdoors
 1. Delete user accounts created during test
1. Bug bounty:
 1. Reward for discovering vulnerabilities
 1. Earn money for hacking system
