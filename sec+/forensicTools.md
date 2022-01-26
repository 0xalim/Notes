# Forensic Tools

## dd

1. Create copy of a drive
1. Good for copying for forensic reasons
1. Create disk image: dd of=/dev/sda of=/tmp/sda-image.img
1. Restore from image: dd if=/tmp/sda-image.img of=/dev/sda

## memdump

1. Copy information in system memory to file
1. 3rd party tools can read the above file
1. System memory should be logged

## Winhex

1. View information in hex
1. Edit disks, file, ram
1. Can clone disks
1. Can securely wipe entire disk
1. Much more forensic tools, all in 1 windows forensic tool

## FTK imager

1. For windows
1. Copy image of drives and store in format that 3rd party can read from
1. Can fully encrypt a drive
1. Can import other formats

## Autopsy

1. Perform digital forensic in hard drive and smartphones
1. Can view:
 1. Downloaded files
 1. Browser history + cache
 1. Email
 1. DB

## Exploitation framework

1. Prebuilt toolkit for exploitation
1. Build custom attacks and create modules
1. Metasploit is exploitation framework
1. Social engineer toolkit is another

## Password crackers

1. Online cracking is possible
1. Offline cracking:
 1. High speed brute force, rainbow table attack and more
1. Bruteforce is really limited though due to hardware, time, etc

## Data sanitization

1. Completely remove data off a hard drive
1. One way trip
