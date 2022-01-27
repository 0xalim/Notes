# Credential Policies

## Credential Management

1. Password must not be embedded in the application
1. All password stored server side
1. All comm should be encrypted

## Personnel accounts

1. Account being logged in should be single owner acc
1. Files should be private to that user only even if same computer
1. No privileged access to os by these user accounts
1. Everyone should use user accounts unless needed

## 3rd party accs

1. Access to external third party systems
1. 3rd party vendors connecting to our system
1. Add 2fa + audit the security posture of all 3rd parties
1. Do not allow account sharing

## Device accounts

1. Access to mobile +  tablets
1. Local sec:
 1. Device cert in place
 1. Screen locks + unlocking standards managed through mdm
1. More sec:
 1. Geo-based policy
 1. More auth factor (2fa)

## Service accounts

1. Used by daemons
1. Access defined for specific service
1. Determine best password policy for these service accounts

## Admin and root accs

1. Should never be used for normal admin stuff
1. Highly securede acc: 2fa, scheduled password changes, strong passwords
