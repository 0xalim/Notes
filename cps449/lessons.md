# All lectures + labs

```
every single lesson in this file
eveyr single lab in this file
anything noteworthy for this course, in this file
```

## Lesson 0
saas: service as a service (googleDocs)
paas: platform as a service a service (heroku)
iaas: infrastructure as a service (azure vm) 

examples of cloud services:
 * google cloud shell: our linux tty
 * microsoft azure cloud: our server
 * bitbucket/github: workspace management

# Lesson 1
virtual machine: software based computer; multiple vm's can run on 1 hardware
machien

hypervisor: software layer (application) that controls all vm's. coordinates 
between physical pc and vm's. 2 main types:
1. runs directly on physical computer (e.g vmware)
1. runs on the os of a physical computer (vbox)

iaas vs paas:
can download, install and have runtime on iaas not paas. in paas you can only
play around the data and applications, nothing else.

vm network:
1. can be assigned a public ip address

explain ip's, network, ports, etc

webpages:
1. plaintxt files written as html

2 http message:
1. response
1. request

http header: 
GET /index.html HTTP/1.1
host: www.kjhdsfkgh.edu/w.e
user-agent: firefox...
accept: text/html, applications/xhtml+xml
accept-language: en-us
accept-encoding: gzip, deflate
accept-charset: iso-8859
keep-alive: 115
connection: keepalive

requests: GET, POST, HEAD

# Lesson 2
ajax: web2.0 nearly real-time web (half-duplex)
websocket: real-time full-duplex communication over 1 tcp connection

each browser window:
1. load content
1. renders:
 1. process html + script to display page
1. respond to events
 1. user actions

js exec'd in web browser sandbox mode: no direct file access
js enforced by same-origin policy: can only rw- webpage from the
same domain, protocol, and port

http review:
1. client sends request, web sends response
1. *sychronous* mechanism: has to idle for response
1. page must be reloaded to get updated information

*asychronous* motivation:
1. live results/updated without having to refresh page
1. when input is given, send ajax request to form
 1. page is not reloaded
 1. similar to search hints and such

ajax - asynch. js and xml:
1. browser sends data to server
1. server process and sends to browser
1. browser receives data and displays to user

# lesson 3

