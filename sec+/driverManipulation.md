# Driver Manipulation

Anti-virus/malware:
1. Prevents known vulnerabilities
 1. Need signature
1. Bad against:
 1. Zero-days
 1. New attack types

Drivers:
1. Between hardware <-> os software
1. Often trusted by system
1. HP audio driver acted as keylogger, attacker could abuse this
1. Hardware interactions contain sensitive information
 1. Mouse inputs
 1. Keyboard inputs

Shimming:
1. Jam something in the middle / mitm kinda
1. Windows includes shimming:
 1. Windows lets programs run as if running on older versions of windows
 1. Application compatibility shim cache:
  1. Stores information to be used by current os and program
 1. Attackers developed malware acts as a shim

Refactoring:
1. Metamorphic malware:
 1. Each malware being downloaded is unique version
 1. None of the signatures will match
1. Appears different each time:
 1. Via NOP instruction
 1. Loops
 1. Useless code
1. Metamorphic malware can reoder functions, modify flow, reorder code
1. Difficult to match with signature based anti-virus/malware
