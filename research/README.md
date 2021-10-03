# WebGuard Research Material

## Javascript Security

* BrowserShield

Rewrites web pages to secure vulnerabilities relied on embedded scripts.

* Google Caja

Rerwite scripts into object-capability language; passive data becomes active
content this way.

* ConScript

Host webpages by fine-grained security policies.

* FlashJaX

Javascript + Adobe ActionScript content security.

* Advertisement Networks:
1. ADSafe: Javascript for ads.
1. Politz: Verify sandbox sources (fixes vulners in ADSafe).
1. Finifter: Third party exploiting prototype objects of hosting page.
1. Privad: Targeted ads, more secure browsing experience
1. RePriv: Tracks user data and interest, with users permission.

* Chudnov, Naumann DA

Inlined information flow monitoring for Js; for sure having a look at this
first.

* JSFlow

Tracking information flow of Js on sites that utilize third-party code, deploys
as a browser extension. 

* GUARDIA

Develop framework for enforcing secuirty policies for Javascript web
applications.

* ScriptProtect

Prevent XSS by using Js to prevent string-to-code conversions; preventing first-
party origin from XSS.

* WebGuard

What I'm looking to do :D ^ All the above will be used for research purposes to
understand the local environment and provide necessary foundational information
as to what will work, why certain things should not be looked into further due
to not providing any use for 'x' reasons. WebGuard will essentially plant
security clearances at gateways of *sinks and sources.* This will be the main
idea, everything coming in and passing out will be monitored one way or another.
First we will look into logging and monitoring all this information, hopefully
we have some time to implement some policies for testing. If that does not come
to be because of timing reasons within project deadline[??] we will continue to
work on related things, and continuing to learn :)
