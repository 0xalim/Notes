# Log Management

## Syslog

1. Central logging receiver
1. Integrated with SIEM
1. Each log is labeled
1. Daemon options:
 1. Rsyslog: rocket fast system for log processing
 1. Syslog-ng: additioanl filtering and storing options
 1. NXLog: collection from many diverse log types

## Journalctl

1. Linux logging
1. System logs are stored in binary format for linux
1. Journalctl allows query of binary format logs

## Bandwidth monitor

1. Percentage of network use over time
1. SMNP, netflow, sflow, ipfix can view this
1. Can identify fundamental issues

## Metadata

1. Data that describes data
1. In emails: header details
1. Mobile: type of phone, gps location
1. Web: os, useragent, ip address
1. Files: names, address, title

## Netflow

1. Standard for gathering netstats of network devices
1. Established standard
1. Probe and collecter:
 1. Probe gather info by sitting inline, integrated or copy of data
 1. Collector is central place to view these logs
1. Good for reports

## IPFIX

1. IP flow information export
1. Newer version of netflow
1. Flexible data support: customize data received from collectors

## Sflow

1. Sampled flow: Only some of the network traffic, not all
1. Embedded in infrastructure, doesn't need full device

## Protocol analyzer output

1. Solve complex application issues
1. Gather packets on network
1. View unindentified traffic, filter, and view ascii output
