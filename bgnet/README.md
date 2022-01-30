# Beej's Guide to Network Programming

## Chapter 2 What is a socket?

Everything in unix is a file. Communication even happens through files, or
file descriptors. Some different sockets:

1. DAPRA Internet Address
1. Unix Sockets
1. CCITT X.25, and more.

### 2.1 Two Types of Internet Sockets

Types: Raw sockets, datagram sockets. Raw -> Stream sockets; datagram -> 
connectionless. This is TCP vs. UDP. TCP Establishes connection and sends in
order. UDP doesn't necessarily need to connect, dump packets and pray.

### 2.2 Low Level Nonsense and Network Theory

OSI Model for the 100th time:
1. Application
1. Presentation
1. Session
1. Transport
1. Network
1. Data Link
1. Physical

More consistent model with Unix:
1. Application: telnet, etc.
1. Host-host transport layer: TCP, UDP.
1. Internet Layer: IP, routing.
1. Network Access Layer: Ethernet, WiFi, etc.

Encapsulate data, send it; receiver breaks it down. 

## Chapter 3 Ip Addresses, Structs, and Data Munging
