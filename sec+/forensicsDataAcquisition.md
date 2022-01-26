# Forensics Data Acquisition

## Order of volatility

1. Different data stored for different amount of time
1. Order:
 1. Cpu register, cpu cache
 1. Router table, arp cache, kernel, memory
 1. temporary file systems
 1. disk
 1. remote logging and monitored data
 1. physical configs, network topology
 1. archival media

## Disk

1. SSD, flash, harddrive copy
1. Power down machine to prevent changes
1. Connect imaging device with write protection
1. Forensic clone of disk

## RAM

1. Difficult to capture
1. Changes constantly
1. Memory dump via some apps, 3rd party tools or os
1. Important data:
 1. Borwsing data, clipboard, encryption keys, command history

## Swap

1. Place to store ram data to free up current memory
1. Contains portion of application
1. Similar data to ram dump

## Os

1. Files and data
1. Core operating system files, libs, to compare for infected files
1. Other os data:
 1. Users, ports, processes running and attached devices

## Devices

1. Mobile devices, tables
1. Connect over usb and create image
1. Data:
 1. Calls, contact info, text, email data, images, video

## Firmware

1. Extract device firmware to check for rootkits
1. Implementation widely varies
1. Rootkits allow remote control even if os gets updated
1. Data discovery:
 1. Exploit data, firmware functionality, real-time data

## Snapshot

1. VM imaging
1. First snapshot is full backup
1. Each snapshot is incremental update
1. Restoring requires original and all snapshots
1. Data:
 1. All info about vm
 1. System imagine essentially: os, apps, user data, etc

## Cache

1. Store user data that is used very much
1. CPU, disk, internet cache
1. Contains specialized data
1. Some cache lasts long time (browser cache)
1. Data:
 1. URL, browser content, cookies, etc

## Network

1. Capture network info, packets, connections
1. Inbound and outbound sessions, os and app traffic
1. Packet data:
 1. Raw data
 1. Can rewind and rerun the traffic
1. Third party packet captures: IPS, firewall

## Artifacts

1. Digital items left behind
1. Locations:
 1. Log info
 1. Flash memory
 1. Prefetch cache files
 1. Recyle bin
 1. Browser bookmarks and logins
