# Certificates

## Web SSL

1. Domain validation certificate:
 1. Owner has control over domain
 1. Proves you are connecting to legit website
1. Extended validation certificate:
 1. Additional checks to prove identity
 1. Banks may have this
1. Subject alternative name:
 1. DNS names for website
 1. Wildcard domain to mean everything.website

## Code signing Certificate

1. Developers can sign code
1. OS examines signature to make sure nothing has changed
1. Failed check means it has been modified

## Root Certificate

1. Getting intermediate CA, root cert is center of it all
1. Root CA issues other certificates
1. Very important, needs good security

## Self signed Cert

1. Internal do not need to be signed by CA
1. Only you will use it internally
1. Can build your own CA
1. Need to make exception on all those devices

## Machine Cert

1. Many devices have certs
1. To authenticate a device, you can put a cert that you signed
1. Other business can rely on this cert to authenticate

## Email Certs

1. To enable cryptography you need cert
1. Encryption emails
1. Receive encrypted emails
1. Email can be used as dignature signatures, providing integrity

## User Cert

1. Certificate with user
1. In a card, usb, smart card
1. Additional authentication factor
