# Log Files

## Network log files

1. Switch, routers, access points, etc
1. Network changes:
 1. Routing updates
 1. Auth issues
 1. Netsec issues

## System log files

1. Os info
1. Also security events:
 1. Monitoring apps
 1. Brute force, file changes
1. Requires filtering due to too much info
1. Eventviewer for windows
1. Syslog for linux

## App log files

1. Specific to applications
1. Windows: event viewer
1. Linux: /var/log
1. View logs to SIEM for good filtering

## Security log files

1. Look at blocked & allowed traffic
1. Exploits blocked
1. DNS sinkhole traffic
1. Logged on IPS, firewall, proxy
1. Good for documenting traffic, summary of attack

## Web log files

1. See url & files they are trying to access
1. Access errors: unauthorized access
1. Exploits attempted also logged
1. Service related logs also created

## DNS log files

1. View lookup request
1. View ip of request
1. Identify query to known bad url to recognize infected machines
1. Block or modify bad requests

## Auth log files

1. Knows who logged and who didn't
1. Logs attempts:
 1. Source ip, name, auth method, success or failure
1. Correlate with file access and such

## Dump files

1. Store contents of memory into diagnostic file
1. Good for devs since they can view what happened
1. Windows: task manager, right clikc and create dump file
1. Apps: some apps have own dump file process

## VoIP

1. View inbound and outbound call info
1. Can see when device has been used, accessed and such
1. SIP traffic log:
 1. Session initiation protocol
 1. Setup, management and teardown
 1. Inbound & outbound calls
 1. Alert depending on country code
