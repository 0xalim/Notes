# Denial of Service

Dos:
1. Force service to fail
1. Overload the service
1. Taking advantage of design vulnerability
 1. Need to keep system updated
1. DoS rival company
1. Used as smokescreen for other exploit on system

Unintentional DoS:
1. Not always due to attacker
1. You can create loop in network via plugging wrong cable in switch
 1. Turn on spanning tree protocol to prevent loops
 1. This is network dos
1. Bandwidth DoS:
 1. Limited access to internet
 1. Large file download causes dos
1. Physical reasons:
 1. Electrical failure, water failure in buildings

DDoS:
1. Many machines vs a service
 1. Via botnet
 1. Coordinated attack
1. Asymmetric threat:
 1. Attacker has less resources to overload, but uses many machines to help

DDoS Amplification:
1. Reflection attack
 1. Turn services against the target
1. Using protocols with little security:
 1. NTP, DNS, ICMP
 1. This is protocol abuse

Application DoS:
1. Making the application break or work more
 1. Increase downtime + costs
1. Zip Bomb is an example
 1. 42kb zip -> 4.5 petabyte on disk
1. Rapid elasticity:
 1. Regarding cloud based services
 1. Adds more resources as computer starts filling up
 1. Attacker increases workload and uses up cloud services resources

Operational technology (OT) DoS:
1. Hardware+software for industrial equipment:
 1. Electrical grid
 1. Traffic control
1. Can't use firewall here, need different approach in general
