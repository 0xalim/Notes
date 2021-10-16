# Endpoint Protection

Stop attackers:
1. Inbound attacks
1. Sending data outband
1. On many different platforms, mobile, pc, etc
1. Defense in depth for layer protection

## Antivirus and antimalware

Antivirus:
1. Term does not only refer to virus but also:
 1. Trojan
 1. Worm
 1. Macro virus

Antimalware:
1. Stops ransomware, fileless malware (RAM), spyware

## Endpoint detection and response

Different method of threat protection:
1. Not just signature based
1. Need to scale for increasing threats

Detect a threat:
1. Behavioral analysis
1. Machine learning
1. Process monitoring
1. Essentially this is an agent on endpoint
1. Find root cause, analysis phase

Repsonding to threat:
1. Isolates the system, quarantine the threat + rollback
1. API driven, all automated

## Data loss prevention

Prevents data leakage
1. Many sources and sinks
1. Cloud based systems
1. Endpoint clients

# Next generation firewall

1. Uses OSI application layer
1. Can allow or disallow certain features
1. Identify attacks and malware
1. Examine encrypted data

## Host based firewall

1. Software based firewall, runs on machine
1. Allow or disallow incoming application traffic
1. Can stop malware before it starts
1. Can be managed by central hub

## Finding intrusions

Host based intrusion detection system:
1. Uses log file to identify intrusions
1. Can reconfigure firewall as needed

Host based intrusion prevention system:
1. Recognize and block known attacks
1. Built onto protection software

HIPS identification:
1. Signatures
1. Heuristics: random large changes
1. Behavioral: identify weird things
