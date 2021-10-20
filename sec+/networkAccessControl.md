# Network Access Control

## Edge vs access control

Edge (Firewall edge of internal network)
1. Internet link
1. Managed through firewall rules
1. Rarely changed

Access control
1. Control from in/out of network
1. Based on rules:
 1. User
 1. Group
 1. Location...
1. Easy to change security posture

## Posture assessment

1. Can't trust all machines that enter network
1. Bring your own device idea, more mainstream nowadays
1. Malware infected, missing antimalware
1. Unatuthorized applications
1. Posture assessment checks security of device:
 1. Trusted device?
 1. Antivirus? Updated?
 1. Corporate applications installed?
 1. Encrypted?

## Health checks

1. Persistent agents installed on system:
 1. Require periodic updates
1. Dissolvable agents no installation required:
 1. Runs during posture assessment

## Posture assessment

1. Can't trust all machines that enter network
1. Bring your own device idea, more mainstream nowadays
1. Malware infected, missing antimalware
1. Unatuthorized applications
1. Posture assesment checks security of device:
 1. Trusted device?
 1. Antivirus? Updated?
 1. Corporate applications installed?
 1. Encrypted?

## Health checks

1. Persistent agents installed on system:
 1. Require periodic updates
1. Dissolvable agents no installation required:
 1. Runs during posture assessment
 1. Terminate when not required anymore
1. Agentless NAC:
 1. Integrated in active directory
 1. Checks made during login/off

## Failing assessment

1. If minimum requirements not met then quarantine network and dont use
1. Install necessary software updates that then meet the requirements
