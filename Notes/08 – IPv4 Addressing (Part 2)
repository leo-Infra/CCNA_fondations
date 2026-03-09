# 08 – IPv4 Addressing (Part 2)

In this lesson I reviewed how to determine important information about an IPv4 network and how to configure IP addresses on Cisco devices.

Main concepts studied

• IPv4 address classes (review)  
• Determining the maximum number of hosts in a network  
• Finding the network address  
• Finding the broadcast address  
• Identifying the first usable host address  
• Identifying the last usable host address  

Example

Network: 192.168.7.0 /24

Network address  
192.168.7.0

Broadcast address  
192.168.7.255

Number of hosts

2^8 = 256 addresses

Usable hosts

256 - 2 = 254 hosts

First usable address

192.168.7.1

Last usable address

192.168.7.254

Configuration on Cisco devices

Example of assigning an IPv4 address to an interface:

interface g0/2  
ip address 192.168.7.254 255.255.255.0  
no shutdown

Verification command

show ip interface brief

This command allows checking the interface status and assigned IP addresses.
