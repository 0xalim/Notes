# Proxy Server

1. Sits between user + external network
1. Receives and sends request on behalf of user
1. Can cache information, has access control, url filtering + content scanning
1. Some applications are required to be configured to work with proxy
1. Some proxies dont need any configuration (invisibile)

## Application proxies

1. Proxy creates application request on behalf of client, need config for this
1. Proxy may only know 1 application
1. Proxy may know multiple applications

## Forward proxy

1. Internal proxy for internal users
1. Proxy sitting between internet and users
1. All requests from user to internet goes through proxy
1. Malicious site requests, url filtering, etc on proxy
1. Responses evaluated from internet, sent to user

## Reverse proxy

1. Inbound traffic from internet to internal network
1. Requests goes through proxy, examines and sent to internal
1. Proxy receives response, examines and sends back to user

## Open proxy

1. Proxy installed on internet, uncontrolled + security concern
1. Any internet user can use
1. Users use this internet proxy to circumvent security mechanisms
1. Proxy essentially is mitm, that can add malicius stuff since its uncontrolled
