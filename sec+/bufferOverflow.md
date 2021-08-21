# Buffer Overflow

Overwriting of memory:
1. Spilling over into other memory areas
1. Can spill into specific area of code to run malicious commands
1. RCE
1. Developers need to perform bounds checking, no spills
1. Not a simple exploit:
 1. Takes time to avoid crashing
 1. Takes time to make it do what you want
1. Good buffer overflow:
 1. Easy to attack
 1. Easy to replicate

Consequences:
1. More privilege rights
1. Can crash system
 1. DoS
1. Gain access to specific areas in fs
