# Request Forgeries

Cross-site request:
1. Website requests content from other servers
 1. Youtube videos on websites
 1. Social media comments/content on websites
1. HTML requests on websites make these requests from browser
 1. Normal use
 1. Unauthenticated requests

Client and server:
1. Website contains client+server side code
1. Client side:
 1. Renders the page
 1. HTML/js
1. Server side:
 1. Requests from client
 1. HTML/php
 1. Uploading content to server

Cross-site request forgery (CSRF):
1. XSRF, CSRF, sea surf
1. Takes advantege of users logged into other sites
 1. Attacker makes request from your browser to, say paypal
 1. Requests happens without users knowledge
1. Web dev oversight
 1. Anti-forgery techniques
 1. Cryptographic token to prevent forgery

Server-side request forgery (SSRF):
1. Web server must be vulnerable to this attack
 1. Attacker sends packet/requests
 1. Web server performs request even though it's malicious
 1. Attacker can gain access to internal networks through SSRF
1. Caused by bad programming
 1. Never trust user input
 1. Server needs to validate input/ouput from server
