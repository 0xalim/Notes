# Wireless Authentication

1. Authentication for:
 1. Mobile users
 1. Temporary users (Coffee)
1. Credentials:
 1. Shared password
 1. 802.1X for employee usage
1. Configuration:
 1. Part of network connection
 1. Prompted during connection

## Wireless security modes

1. Open system: No password
1. WPA3:
 1. Preshared key
 1. Everyone has different session key
 1. SAE (Simultaneous authentication of equal)
 1. WPA3-enterprise for enterprise level
 1. Authenticates users with authentication server
1. Captive portal:
 1. Authentication via web browser
 1. User enters username+password
 1. One authenticated, then session continues
 1. Captive portal probably has time limit

## WPS

1. Wifi protected setup
1. Easy setup of mobile devices
1. Ways to connect:
 1. Pin configured
 1. Button on access point
 1. Bring device close, so communication can happen

## WPS Hack

1. Pin of every device is a drawback to WPS
1. 8 digits = 7 digits + 1 checksum
1. But process validation checks first 4, and last 3 resulting in low number
   of bruteforce attempts needed
