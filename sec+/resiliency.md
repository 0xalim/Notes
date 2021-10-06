# Resiliency

Non-persistence:
1. Cloud applications change all the time
1. Snapshots capture configuration + data needed
1. Multiple snapshots allows different fallback interims
1. Can launch os from bootable as well

High availability:
1. Redundancy doesn't always mean 100% uptime
1. Need to manually be turned on
1. Always available though
1. Scalable
1. Higher costs due to many more systems + machines

Order of restoration:
1. Some things should be restored before others
1. Database before applications e.g
1. Backups depend on backup type:
 1. Full, incremental, differential

Diversity:
1. 0day on windows, not on linux
1. Diverse os, services, apps, software is good
1. Diverse security devices:
 1. Firewalls, ips, ids, spam filter
1. Different vendors:
 1. Supply chain security i think
1. Cryptography:
 1. Encryption now, wont work later so use more and better
 1. Different Certificate authority
1. Controls:
 1. Admin control
 1. Physical: locks, rooms, access
 1. Technical, authentication
 1. DEFENSE IN DEPTH!!
