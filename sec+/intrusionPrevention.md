# Intrusion Prevention

## Network IDS/IPS

1. Looks at network traffic
1. Focuses on os and application exploits, and:
 1. BOF
 1. XSS...
1. Detection vs Prevention:
 1. Detect - only an alarm
 1. Prevent - stop it

## Passive monitoring

1. Examines copy of the network traffic via port mirror (SPAN) or network tap
1. All traffic that goes through switch or so gets copied and ips examines it
1. Not inline, so cannot prevent in real time

## Out-of-band response

1. When malicious traffic identified, stop it using RST frames
1. Limited udp response available

## Inline monitoring

1. Monitors all traffic through it
1. Sits physically in line of connection

## In-band response

1. Can block traffic in real time, so can prevent 
1. IPS drops connection preventing anymore traffic

## Identification technologies

1. Signature based: Traffic that matches signature is dropped
1. Anomaly based: Build baseline of whats normal:
 1. From 0 file transfer to large amount in short time, probably malicious
1. Behavior based: Knows what malicious formats (xss, sql) looks like even if
   there is no signature for it specifically
1. Heuristics: Big data + AI + Machine learning to identify malicious traffic
