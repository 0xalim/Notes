# Cross Site Scripting

Originally named xss cause info shared from 1 site to another.

Now xss is vulner from target to attacker
1. Common in web based applications
1. Malware that uses js

Reflected (non-persistent) xss attack:
1. Web that allows scripts to run in user input fields
 1. Search box
 1. Input field
1. Attacker sends malicious link in email to target targetting a vulnerability
 1. Runs script that sends creds/cookies to attacker
 1. Output of script sent back to attacker

Stored (Persistent) xss attack
1. Script stored in webpage including malicious payload
 1. Forums
 1. Message post
1. Every1 who vists gets that payload
1. No specific target, everyone
1. Spreads quickly in social media/networking

Protecting against xss:
1. Never click links in email
1. Disable javascript
 1. Limited protection
1. Keep browser+applications updated
1. Validate input fields if developer
