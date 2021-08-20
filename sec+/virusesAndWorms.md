# Viruses And Worms

* Virus requires execution of program to replicate
1. Needs human to launch application
1. Worm jumps from machines without humans launching program
1. Virus replicates using file system
1. Virus can encrypt, delete data some and just annoying and useless
1. Anti-virus can stop virus before it executes
 1. Keep virus signatures up to date

Virus types:
1. Program virus: Part of application, launching will exec virus
1. Boot sector virus: when os is starting up it execs the virus
1. Script virus: within the os or browser
1. Macro virus: inside of microsoft office applications

Fileless virus:
1. Stealth virus that never intalls/saves itself as file on fs
1. Avoids anti-virus detection via never saving itself
1. Virus exists in memory but never saved in disk
1. Executed by clicking link in email/website
 1. Download software that runs (flash/java file)
 1. Launches powershell, downloads virus in third party location
 1. Execute virus in ram
 1. Can exploit machine now
 1. Can write to registry so that everytime machine restarts its in memory

Worms:
1. No human intervention is needed
1. Exploits vulner in apps/os to move system to system
1. Takes advantage of intranet
1. Finding worm, creating a signature will have firewall filter worm

Wannacry:
1. worm that propogates automatically
1. Installed crypto-malware
1. First tries to find other sources
1. Exploit is eternalblue
1. One sources are found, download backdoor (doublepulsar)
1. Double pulsar installs wannacry and machine is now encrypted
