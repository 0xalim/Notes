# Securing Compute Clouds 

## Compute Cloud Instances

1. Perform computation:
 1. Amazon elastic compute cloud (ec2)
 1. Google comput engine
 1. Microsoft azure vm
1. Manage computing resources

## Security Groups

1. Firewall for compute instances
1. OSI layer 4/3 port number blocking:
 1. 4 is for tcp/udp
 1. 3 is for ipaddress

## Dynamic Resource Allocation

1. Give more resources when needed
1. Scale up and down: 
 1. Give more resources where needed
 1. Pay only for what is used
 1. Rapid elasticity
1. Ongoing monitoring for above ^

## Instance Awareness

1. Granular security controls
1. Define and set policies:
 1. Allow file uploads to specific shares
 1. Deny uploads to personal shares
 1. Deny spreadsheets, graphic files, credit card numbers

## Virtual Private Cloud Endpoints

1. VPC gateway endpoints:
 1. Allow cloud subnets to communicate with other cloud services
1. Keep resources private
1. Add VPC subnet between storage network and private network

## Container Security

1. Still has security concerns:
 1. Bugs
 1. Bad security controls
 1. Misconfiguration
1. Use container specific os
1. Group container types on same host
