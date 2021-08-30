# MAC Flooding and Cloning

Mac address:
1. Ethernet media access control card:
 1. Physical address of network adapter
 1. Unique to each device
1. 48 bits, or 6 bytes long
1. All in hex
1. First 3 bytes = Manufacturer ID (OUI)
1. Last 3 bytes = Serial number, unique

Lan switching:
1. Forward or drop frames it's receiving depending on destination mac address
1. Constantly updates MAC address list
 1. Ages often in 5 minutes
1. Maintains loop-free environment
 1. By using STP, spanning tree protocol

Mac:
1. Switch first receives traffic
1. Log mac address if not in table
1. Logs each mac address depending on source/destination
 1. First checks destination, logged if not already as destination exit A
 1. Checks source machine, logs ethernet port used as pair 

Mac flooding:
1. Mac table has finite amount of entries
1. Attacker sends traffic to switch with random mac address
1. Switch logs address
1. Attacker fills up table
1. Now switch dumps incoming data to all machines, because doesn't know which
   machine specifically to send it to
1. Switch is now a hub
 1. All traffic is transmitted to all machines
 1. Attacker can capture network traffic now easily
1. Some switches prevent this via FloodGuard

Mac cloning/spoofing:
1. Attacker changes mac address to an existing one, cloning essentially
1. Can gain access to system because of same mac address
1. DoS possible, traffic not being sent to actual user
1. Easily manipulated through software
 1. Macchanger for example
 1. But mac address never truly changes, burnt on hardware
