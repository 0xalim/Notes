# Secure Programming HOWTO - David A. Wheeler

This directory contains all my notes to the book mentioned in the syllabus of
CPS475 (David A. Wheelers book). Cannot claim that everything will be my work
since essentially all this content is from the book directly, but most things
should be summarized and important notes may or may not be summarized. All in
all - i do not claim any of this to be 100% mine.

Book provides design + implementation guidelines for writing secure programs.
Programming languages include C, Cpp, JS, Java and more. I only named ones that
I care about; oh and Python too :^). Specifically for Linux and Unix based
machines but not limited to.

## Chapter 1: Introduction

`A wise main attacks the city of the mighty and pulls down the stronghold in
which they trust. Proverbs 21:22 (NIV)`

Need for security in applications has become more and more apparent as network
security has proved to be useless to some regard. As soon as patches are
released there might be vulnerabilities regardless and thus will be prone to
exploits. Need for generalized security is clear.

Book does not cover assurance measures, software engineering processes and 
quality assurance approaches. Book does not cover how to properly configure
systems either, or a network for that matter. Bastille-linux is good for Unix
hardening. Unixtools is good for general configurations. Open source PKI book
for public key infrastructure using open source tools. Cheswick 1994 for
firewalls.

Good resources for vulnerabilities now:
1. [CVE](https://cve.mitre.org)
1. SecurityTracker
1. [Internet Storm Center](https://isc.incidents.org)

Book assumptions:
1. Computer security issues in general          (Check)
1. General security model of Unix-like systems  (Uh-oh)
1. Networking (tcp/ip in particular)            (Check)
1. C programming language                       (Kinda?)

Processes:
1. Accept input
1. Process data
1. Call out to other resources
1. Produce output

![Figure 1-1. Abstract view of a program](https://i.imgur.com/B6VheF7.png)

## Chapter 2: Necessary Background For All Interested

### 2.1.1 Unix

In '69 to '70 Ken Thompson, Dennis Ritchie at at&t bell labs began development
on Unix. This was in times of Multics. God bless these 2 guys. Absolute legends
and clear models for anyone who is into this type of stuff :^). Unix was then
written in C, which took Ken multiple tries to do. And Dennis had to rewrite
the language multiple times for this. '79 V7 of Unix was released <- grandfather
of all Unix systems.

Berkeley released BSD. At&t continued dev to develop System III and System V.
After this, major legal battles ensued which is essentially the dark period
of Unix. Many proprietary versions of Unix were being developed and used by
respective hardware vendor. Sun Solaris (Sun Microsystems) was one of them. 
Sun bought out by Oracle later... Unlucky. Berkeley released 3 versions that
were open source: FreeBSD (Multi hardware), OpenBSD (Security), NetBSD (CPU). 

### 2.1.2 Free Software Foundation

Stallman FSF began GNU project, reating free version of Unix. Could be used,
read, modified and redistributed. FSF created gcc, emacs and other tools. But
struggled to create kernel.

### 2.1.3 Linux

Torvalds developed operating system kernel, and named it Linux. Different
communities developed different distributions, but all are based on the same
kernel and GNU libraries.

### 2.1.4 Open source / Free Software

Definitions are confused a lot. FS can be given at no charge but not modified,
read, or redistributed. Open source means source code can be viewed and
modified under 'some' limitations.

### 2.1.5 Comparing Linux and Unix

Unix meant product developed by at&t but nowadays means "Worldwide single Unix
Specification". Linux is not derived from Unix source code but interfaces 
similar to Unix. Both have tremendous similarities, enough for this book.

### 2.2 Security Principles

CIA Triad: `...jesus I am tired of this old relic`
1. Confidentiality: Only read by authorized parties.
1. Integrity: Only modified by authorized parties in authorized ways.
1. Availability: Assets are accessible to authorized parties in a timely manner.

Others exist such as:
1. Authenticity (Or access control).
1. Privacy.
1. Non-repudiation.
1. Auditing.
1. Etc...

### 2.3 Why do Programmers Write Insecure Code?

I'd like to summarize this section simply by a Quote from Aleph One:
`There is no curriculum that addresses computer security in most schools. Even
when there is a computer security curriculim. Thhey often don't discuss how to
write secure programs as a whole. Curriculums only study certain areas such as
cryptography or protocols. These are important, but often fail to discuss
common real-world issues such as buffer overflows....I believe this is one of
the most important problems; even those programmers who go through colleges
and univerisites are very unlikely to learn how to write secure programs, yet
we depend on those very people to write secure programs.`

### 2.4 Is Open Source Good for Security?

Lots of debate, since source code is open for examination by everyone. But
I personally would say, open source programs are better in terms of security
compared to proprietary ones.

### 2.4.1 View of Various Experts

Lots of different opinions. Favorite was Aleph1's and Diffie's for sure :^).

### 2.4.2 Why Closing the Source Doesn't Halt Attacks

Easier to break a car than to build one. Same type of thinking; easier to attack
a program than it is to build one. Attackers only need 1 vulnerability,
defenders have to worry about all the vulnerabilities. Dynamic techniques are
those where you run the program - static are those when you view source code.

Dynamic approach has no difference between open and closed source programs. Just
dump some malicious data and see if you get something. Static approach varies,
open source is just reading the available source code. Closed source requires
the need for decompilers and disassemlbers. Gdb, ghidra and IDA are some
examples.

### 2.4.3 Why Keeping Vulnerabilities Secret Doesn't Make Them Go Away

In general companies did not fix vulnerabilities until they were widely known
to their users. Part of full disclosure problem.

### 2.4.4 How OSS/FS Counters Trojan Horses

Trojans can be inserted in both open and proprietary code. Problem with closed
source is that not many companies perform checks for their code so they are
never always too sure. Software dev's for closed source have not generally been
held liable in courts.

Example is given in book where Borland server had backdoor installed and for 
many years was not changed until it went open source, and discovered by another
project. It is easy to say that generally closed source code has vulnerabilities
but have not been found yet.

### 2.4.5 Other Advantages

Vulnerability assessment scanners are better open source ? Why, I had no idea
but nessus > other scanners. Feels a bit nitpicked honestly. Problems found in
open source is fixed immediately; can't say the same about closed source.

### 2.4.6 Bottom Line

Its generally better to be open than closed source, but it all depends on:
1. Is the code actually being reviewed.
1. Do people have rights to their contribution?
1. People need to know how to write secure code.
1. People need to report back to vendor, not always done.

### 2.5 Types of Secure Programs

1. Programs used as viewers of remote data.
1. Root programs accepting data from non root users.
1. Daemons.
1. Network daemons.
1. Web-based applications.
1. Applets. Mobile code interpreters.
1. Setuid/Setgid programs. Program invoked by normal, granted root priv.

### 2.6 Paranoia is a Virtue

Paranoid mindset needed to write secure programs. Nonsecure programs have many
errors and users tend to prevent avoiding them. Secure programs have users
looking for ways to cause these errors (bugs).

### Why Did I Write This Document

To prevent common pitfalls when writing secure programs. Gives developers a
place for all information especially related to linux.

### 2.8 Sources of Design and Implementation Guidelines

Look in book page 17&18 for sources that are interesting & more niche.

### Other Sources of Security Information

1. Bugtraq mailing list.
1. Secprog mailing list.
1. vuln-dev mailing list.
1. LinuxSecurity.com

### 2.10 Document Conventions

Some fundamental information before reading this book is provided here for
clarity. One part I particularly enjoyed:

` An attacker is called "attacker", "cracker", or "adversary", and not a
"hacker". Some journalists mistakenly use the word "hacker" instead of
"attacker"; this book avoids this misuse, because many Linux and Unix dev's
refer to themselves as "hackers" in the traditional non evil sense of the term.`

## Chapter 3: Summary of Linux and Unix Security Features
