# PKI

## PKI

1. Manage digital certs:
 1. Policy, procedure, hardware
1. Creating certs and giving them to users and devices
1. CA does above

## Key Management

1. Key generation:
 1. Required strength
 1. Good algorithm
1. Cert generation: Allocate to user
1. Distribution: Give to user safely
1. Storage: Securely store
1. Revoce: Manage keys that have been compromised
1. Expiration: Only limited amount of usage

## Digital Certs

1. Public key cert bound with digital signature from CA
1. Digital signature adds trust
1. Web of trust is when users sign and trust others themselves
1. Cert creation can be built on os:

## Commercial Certificate Auths

1. Stored in operating system and browser
1. Can purchase certificate
1. Create key pair, send public key to CA and it will be signed
1. ^ Certificate signing request
1. Now server has been signed + trusted

## Private Certificate Auths

1. You are the CA
1. You sing them
1. Medium-large orgs: Require due to many servers to be trusted

## PKI Trust Relationships

1. Single CA: Everyone receives their cert from one authority
1. Hierarchical:
 1. Single CA distributes to multiple CA
 1. Distribute load to others
 1. Easier to revoke group from one distributed CA

## Registration Authority

1. Entity requesting the certificate needs to be verified
1. Important, CA signs depending on this entity
1. Manages renewel

## What is in Digital Certificate

1. Issued by
1. Expiry
1. Details:
 1. Country, state, locality, org name, common name
 1. Who the Issuer is

## Important Cert Attributes

1. Common name: FQDN, fully qualified domain name:
 1. Just a normal URL essentially
1. Subject alternative name:
 1. Additional host names for the cert
 1. Same website, but many names
1. Expiration

## Key revoke

1. Certificate revocation list:
 1. Large file stored in CA
 1. File containing all revoked certs
1. Reasons:
 1. Changing attributes
 1. Newly acquired company
 1. Vulnerability

## Getting revocation details to browser

1. Using online tool like OCSP
1. Browser can check single certificate instead of downloading list
1. Not all browsers suppor this
