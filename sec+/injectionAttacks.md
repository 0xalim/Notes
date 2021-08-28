# Code injection

Adding information into data stream:
1. Enabled due to bad programming
1. No input validation, executes any input
1. HTML, SQL, LDAP, XML

SQL injection:
1. Structured query langauge
1. Common dbms language
1. No input validation:
 1. Attacker can modify data in db
 1. Attacker can run any query they want
1. sql injection example
 1. ' OR 1-1' --

XML injection and LDAP injection:
1. XML - extensible markup language
 1. Set of rules for data transfer + storage
1. XML injection:
 1. Modify the XML requests
 1. No validation is bad
1. LDAP - Lightweight directory access protocol
1. LDAP injection:
 1. Modify LDAP requests to manipulate application results

DLL injection:
1. Dynamic-link library:
 1. Windows library contains code+data
 1. Many applications use dll
1. Injecting DLL and have apps run malicious library code
 1. ProcessB attachs to ProcessA
 1. Allocate memory in ProcessA
 1. Copy DLL to ProcessA
 1. Process A runs the code as a new thread; now ProcessB can have more rights 
