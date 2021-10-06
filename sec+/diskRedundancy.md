# Disk Redundancy

Duplicate local machine:
1. Backup still available
1. Maintain uptime
1. Failures:
 1. Hardware
 1. Software: bugs, crash
1. No system failure

## Geographic dispersal

1. Hurricane, flooding
1. Use multiple data centers
1. East coast/west coast
1. Disaster recovery center

## Disk redundancy

1. Multipath I/O:
 1. If one path fails in network devices, others are available
 1. Redirection is possible
1. RAID: Redundant array of independent disks
 1. Multiple drives in array
 1. 1 drive fails, others are still possible to use

## RAID

1. Raid 0: High performance, but in failure all is list
1. Raid 1: Mirrored data, but double space required
1. Raid 5: Data in multiple drives, 1 drive has parity
1. Can combine above if needed
