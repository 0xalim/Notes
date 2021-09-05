# Secure Deployments

Dev to production:
1. Deploy safe+reliably
1. Test+deploy new patches on specific dates

Sandboxing:
1. Isolated test environment, to test application
 1. No connection
 1. Safe space
1. Try to break, nothing happens if it does
1. Incremental development helps constant dev of application

Building the app:
1. Dev writes code in secure environment and test in sandbox
1. Test, in development stage, all pieces work together
1. Function test everything

Verify the application:
1. QA - Quality assurance
1. Puts application through multiple tests + new functionality
1. Verify old errors are patched
1. Staging almost in production:
 1. Take production data and use on app to see if it works
 1. Works and feels like production environment
 1. Last chance to test perforamance + usability

Using application:
1. Production, app is now rolled and live
1. Users need time to learn new things
1. Logistic challenges:
 1. New servers, software or restarting services

Secure baseline:
1. Well-defined policy baseline:
 1. All apps follow this baseline
 1. Firewalls, patches, os version
 1. Baselining needs constant updates
1. Integrity check:
 1. Check things are working in production environment
