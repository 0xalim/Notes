# Account Policies

## Account policies

1. Control access to account:
 1. Not just username;password
 1. Determine best policies
1. Authentication process:
 1. Password policy
 1. Authentication factor policy
1. What permissions does account have after login?
1. Defense in depth needed

##  Perform routine audits

1. Make sure everything follows policy
1. Need to make sure routine is scheduled
1. Actions can be logged automaticaly via tools

## Auditing

1. Permission auditing
1. Make sure everyone have correct permissions
1. Usage auditing:
 1. How are resources being used
 1. Are system+apps secure

## Password Complexiy and Length

1. Making sure password is strong:
 1. Resists bruteforce + guessing
1. Increase password entropy:
 1. No single words
 1. No obvious words
 1. Better to mix upper+lower case and special chars
1. Atleast 8 chars long
1. Prevent password reuse

## Account Lockout and Disablement

1. Too many incorrect passwords will cause lockout:
 1. Prevent bruteforce
 1. Should be normal for user
 1. Big issue for service account due to not allowing service anymore
1. Disable account:
 1. Do not delete, may contain important keys

## Location Based Policise

1. Done with IP subnet
1. Difficult with mobile devices
1. Geolocation:
 1. Uses coords
 1. 802.11 wireless which is less accurate
 1. Ip address
1. Geofencing:
 1. Automatic policies based on geolocation
1. Geotagging:
 1. Add location to metadata
 1. Latitude, longitude, distance and time stamps
1. Time based policies: only allow during work hours
