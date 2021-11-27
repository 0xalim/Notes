# Identity Controls

## Identity Provider

1. Authentication as  service
1. Service that vouches for you
1. Used by cloud based applications
1. Standards:
 1. SAML, 0Auth, OpenID Connect

## Attributes

1. Identifier property
1. Personal attributes:
 1. Name
 1. Email
 1. Phone
1. Other attributes:
 1. Depaterment
 1. Job title

## Certificates

1. Assigned to person or device
1. Binds identity to owner:
 1. Can encrypt data
 1. Create digital signatures
1. Requires public key infrastructure:
 1. CA needed, thats trusted
 1. CA signs all certificates

## Tokens and Cards

1. Smart card = ID cards, may require pin
1. USB token or key

## SSH Keys

1. Use keys instead of username and password
1. Key management is critical

## SSH key based authentication

1. Create public/private keys with ssh-keygen
1. Create public key to ssh server with ss-copy-id user@host
1. Can now ssh to server with only keys
