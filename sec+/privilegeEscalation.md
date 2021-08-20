# Privilege Escalation

Gain higher privilege access using normal user (lower priv user):
1. Via:
 1. Exploit a vulnerability
 1. Bug or design flaw
1. Higher level menas more access
1. High priority patches
1. Vertical privilege escalation normaluser-\>root/kernel
1. Horizontal privilege escalation normalA-\>normalB
 1. For files and resources

Mitigating privilege escalation:
1. Patch quickly
1. Update antivirus/antimalware software
1. Data execution prevention:
 1. Only data in executable areas can run
1. ASLR: Address space layout randomization
 1. Prevents user from knowing where process is running on physical hardware
 1. Prevents buffer overflow

