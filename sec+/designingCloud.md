# Designing The Cloud

Cloud:
1. On demand power
1. Easy to scale up or down
1. Applications scale and have access everywhere

Thin client:
1. Basic application usage, apps run on remote server
1. Virtual desktop infrastructure (VDI)
1. DaaS desktop as a service if over cloud
1. Cloud is just keyboard, mouse and screen
1. Need network for this

Virtualization:
1. Many different os on same hardware
1. Infrastructure -> hypervisor -> multiple vm's running different os
1. Hypervisor manages all those vm's
1. Expensive, needs lots of cpu and memory
1. Adds overhead + complexity

Application containerization:
1. Containers:
 1. Contain everything you need to run app
 1. Docker
 1. 1 Os for all containers
 1. Isolated, do not interact with each other
 1. Container image adds portability, lightweight

Microservice and API:
1. Monolothic app:
 1. 1 big application that does everything
 1. UI, business logic, data I/O all self contained
 1. Large codebase, bigger complexity
 1. Easy to break
1. Microservice architecture:
 1. API is application programming interface
 1. Break large program into smaller services
 1. API gateway manages connection between machine and microservice
 1. Multiple db if needed
 1. Easy to add microservice
 1. Does not all break if 1 microservice stops working
 1. Better security control

Serverless architecture:
1. FaaS - function as a service
1. No os, specific functions are applications that may be automated
1. Devs create server-side logic:
 1. Stateless compute container, api sends request and this returns response
1. Event triggered, no need to run if not needed
1. Managed by third-party, even security

Transit gateway:
1. Virtual private cloud, VPC:
 1. Contain number of applications running on cloud
1. Transit gateway, router in cloud essentially
 1. Connects VPC's so users can easily use all apps available in all VPCs
 1. May require VPN connection to transit gateway

Resource policies:
1. Assign permissions to cloud resources
1. Azure, allows some resources to be provisioned by some users
 1. Specific service in specific regions
1. Amazon, Specify resources and actions allowed to that resource
1. Amazon, Explicit list of users who have access

Service integration:
1. Service integration and management:
 1. Some apps on azure, others on amazon, etc
 1. Multisourcing, allows management of services no matter what provider
