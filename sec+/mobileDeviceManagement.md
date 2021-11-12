# Mobile Device Management

## MDM

1. Manage company and user owned devices
1. Centralized management
1. Set policies for apps, data, and more
1. Access control (PIN, screenlock)

## Application Management

1. Not all applications are secure
1. Manage applications through allow list managed through mdm
1. Hard to keep everything up to date
1. Management challenge

## Content Management

1. MCM = mobile content management
1. Secure access to data
1. Set policies for file sharing and viewing
1. ^ Even for cloud stuff
1. Data loss protection prevents stealing sensitive data
1. Ensuring everything is encrypted

## Remote Wipe

1. Remove all data from mobile data
1. Managed through mdm
1. Nuke everything from web
1. Need to plan for this
1. Better to have backup

## Geolocation

1. Precise tracking details
1. Can be good if lost phone, bad since says location 24/7
1. Most phones allow disabling the above
1. MDM managed

## Geofencing

1. Allow/disable stuff in certain areas
1. Camera disable inside building and more
1. Authentication required when outside certain area

## Screen Lock

1. Locking phone
1. Simple password or even strong ones
1. When it fails too many, erase data
1. Lockout policy for above instead possible

## Push Notifications

1. Information appears on phone to tell you data
1. No user intervention
1. Control displayed notification from mdm

## Password and Pin

1. Difference authentication methods (pin, pattern, etc)
1. Recovery can be done through mdm
1. mdm can remove all security if needed

## Biometric

1. You are the authentication factor
1. Not very secure
1. Enabled and disabled through mdm

## Context Aware Authentication

1. Builds profile of who is authenticating
1. Combine contexts:
 1. Determine ip vs normal ip
 1. Determine gps vs normal gps
 1. Determine other paired devices
1. Emerging technology

## Containerization

1. Seperate personal from business data
1. Create container for company data
1. Limited data sharing
1. Mobile storage segmentation (not kubernetes)
1. Easy to manage offboarding, easily deleted

## Full Device Encryption

1. Scramble all data on mobile device
1. Different strength
1. Bad on batter and cpu
1. Complex integration between software&hardware
1. If lost key then cannot restore the data
