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
nodejs:
1. js programming framework for scalable internet applications
1. can handle 10,000's concurrent connections
1. 1 thread, 1 process
1. handles concurrency using:
 1. nonblocking i/o
 1. asynh. event-driven model
 1. callbacks

callback:
asynch variation for a function
called at the completion of a task or an event
basically callback is rather than process waiting for a return, callback just
says ill call you back when i have the infromation for you instead of the
process having to wait for a return

ways to define a callback function:

```
function-call(param,callback);
function callback (param){body}
```

```
function(param, function(param) {body});
```

```
function(param, (param)=>{body})
```
nodejs built in modules and apis:
1. net: support socket programming
1. fs: file system
1. http: web server
1. https: secure web server
1. url: parse and handle url
1. querystring: parse and farmat url query strings
1. process: global object controlling the current nodejs process

# Lesson 4
traditional application development:
1. presentation layer (ui)
1. business layer (domain layer, bll?)
1. data access layer (persistence layer, logging ,networking)

monolitic:
1. build as a single deployable unit
1. normally run as single process
1. resource intensive
1. e.g: php, nodejs
1. con: 
 1. no physical separation between different areas of the system
 1. if 1 thing needs update, it may break everything and they all need
	to be updated as a result
 1. hard to scale properly; requires lots of resources
 1. deployments are cumbersome and time consuming
 1. limited to technology stack;

ci/cd:
1. git push and it auto changes microservice

now working on sprint1:
microservice 1 requirements:
1. take 1 input, generate 2 output
1. think of theme or w/e like chinese zodiac year
1. express.js lab2 part 2.c
1. microservice in 1 repository
1. test locally with google cloud shell that it works
1. deploy on azure

microservice 2 requirements:
1. 2 input, 2 output
1. theme like enter startdate enddate, compute days&&weeks between dates
1. same as microservice1 express.js
1. test locally
1. deploy on heroku

frontend requirements:
1. ajax in lab 2; plus below:
1. one input+button to call micro1 -> output display
1. 2 input+button to call micro2 -> output display
1. ui on github

