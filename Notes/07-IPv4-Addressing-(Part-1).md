# 07-IPv4 Addressing Basics – Notes

## Overview

This lesson introduces Layer 3 and focuses on IPv4 addressing.

Main ideas:

- Layer 3 connects different networks
- routers operate at Layer 3
- IPv4 addresses are logical addresses
- IPv4 uses network and host portions
- IPv4 addresses can be written in binary or dotted decimal

This lesson is the foundation for routing and subnetting later.

---

## Layer 3 review

Layer 3 is the Network layer of the OSI model.

Its main functions are:

- connectivity between different networks
- logical addressing with IP addresses
- path selection between source and destination

Layer 2 uses MAC addresses.  
Layer 3 uses IP addresses.

Routers operate at Layer 3.

---

## Switches vs routers

A switch expands a LAN.  
It does not separate networks.

If multiple switches are connected together, they still form one LAN unless a router is inserted.

A router separates networks.

Example:

- PC1 and PC2 on 192.168.1.0/24
- PC3 and PC4 on 192.168.2.0/24

The router must have one IP address in each connected network.

Example:

- R1 G0/0 = 192.168.1.254
- R1 G0/1 = 192.168.2.254

---

## Broadcast behavior

Switches forward broadcast frames inside the local LAN.

Routers do not forward Layer 2 broadcasts between networks.

So:

- broadcasts stay in the local network
- routers separate broadcast domains

This is one reason routers are used to divide networks.

---

## IPv4 basics

IPv4 is the main Layer 3 protocol used today.

An IPv4 header contains many fields, but the most important for now are:

- source IP address
- destination IP address

Each IPv4 address is:

- 32 bits long
- divided into 4 octets
- each octet = 8 bits

Example:

192.168.1.254

Each decimal number represents one octet.

---

## Binary and dotted decimal

Computers use binary.  
Humans usually use dotted decimal.

Example:

192.168.1.254

Binary form:

- 192 = 11000000
- 168 = 10101000
- 1 = 00000001
- 254 = 11111110

So the full IPv4 address is really a 32-bit binary value.

---

## Binary review

Binary is base 2.

Each bit position has a value:

- 128
- 64
- 32
- 16
- 8
- 4
- 2
- 1

To convert binary to decimal:

- add the values of all bits set to 1

Example:

10001111

= 128 + 8 + 4 + 2 + 1  
= 143

---

## Decimal to binary

To convert decimal to binary:

1. write the bit values
2. subtract the largest possible value
3. write 1 where used, 0 where not used

Example:

221

- 221 - 128 = 93
- 93 - 64 = 29
- 29 cannot subtract 32 → 0
- 29 - 16 = 13
- 13 - 8 = 5
- 5 - 4 = 1
- 1 cannot subtract 2 → 0
- 1 - 1 = 0

Result:

11011101

Important note: the transcript had one binary example marked as wrong in the video itself, so rely on the method, not only on that line.

---

## Octets

Each group of 8 bits in an IPv4 address is called an octet.

Example:

192.168.1.254

has four octets.

---

## Prefix length

An IPv4 address is divided into:

- network portion
- host portion

The prefix length tells us where that split happens.

Example:

192.168.1.254/24

`/24` means:

- first 24 bits = network portion
- last 8 bits = host portion

So:

- network = 192.168.1
- host = 254

---

## Examples of prefix lengths

### /24
First 24 bits are the network portion.

Example:

192.168.1.254/24

- network portion = first 3 octets
- host portion = last octet

### /16
First 16 bits are the network portion.

Example:

154.78.111.32/16

- network portion = 154.78
- host portion = 111.32

### /8
First 8 bits are the network portion.

Example:

12.128.251.23/8

- network portion = 12
- host portion = 128.251.23

---

## IP classes

IPv4 addresses were traditionally divided into classes.

The class is determined by the first octet.

### Class A
- first bit = 0
- first octet range = 0 to 127
- prefix = /8

### Class B
- first bits = 10
- first octet range = 128 to 191
- prefix = /16

### Class C
- first bits = 110
- first octet range = 192 to 223
- prefix = /24

### Class D
- first bits = 1110
- first octet range = 224 to 239
- used for multicast

### Class E
- first bits = 1111
- first octet range = 240 to 255
- reserved / experimental

For CCNA basics, focus mainly on:

- Class A
- Class B
- Class C

---

## Loopback range

The 127.x.x.x range is reserved for loopback.

Examples:

- 127.0.0.1
- 127.23.68.241

Packets sent to this range are processed locally by the same device.

This is used to test the local TCP/IP stack.

Because of this, the usable Class A range is often considered to end at 126, not 127.

---

## Class characteristics

### Class A
- few networks
- many hosts per network

### Class B
- medium number of networks
- medium number of hosts

### Class C
- many networks
- fewer hosts per network

This is because the size of the network portion and host portion changes by class.

---

## Network size by class

### Class A
- prefix = /8
- host bits = 24
- about 16.7 million addresses per network

### Class B
- prefix = /16
- host bits = 16
- about 65,536 addresses per network

### Class C
- prefix = /24
- host bits = 8
- 256 addresses per network

But not all addresses are usable by hosts.

---

## Usable host addresses

In each network:

- first address = network address
- last address = broadcast address

These two addresses cannot be assigned to hosts.

So:

- Class C /24 has 256 total addresses
- usable host addresses = 254

---

## Netmask

Cisco often uses dotted decimal netmasks instead of slash notation.

A netmask is:

- all 1s for the network portion
- all 0s for the host portion

Examples:

### /8
255.0.0.0

### /16
255.255.0.0

### /24
255.255.255.0

These are just another way to represent the prefix length.

---

## Network address

If the host portion is all 0s, the address is the network address.

Example:

192.168.1.0/24

This identifies the network itself.

It cannot be assigned to a host.

The first usable address is one above it:

192.168.1.1

---

## Broadcast address

If the host portion is all 1s, the address is the broadcast address.

Example:

192.168.1.255/24

This is used to send traffic to all hosts in the local network.

It cannot be assigned to a host.

The last usable address is one below it:

192.168.1.254

---

## Broadcast at Layer 3 and Layer 2

If a packet is sent to the broadcast IP address of a local network, the Ethernet frame carrying it uses the broadcast MAC address:

FFFF.FFFF.FFFF

So:

- broadcast IP = all hosts in the subnet
- broadcast MAC = all devices in the local LAN

---

## Key concepts to remember

- Layer 3 uses IP addresses
- routers connect different networks
- IPv4 addresses are 32 bits long
- IPv4 addresses are written in dotted decimal for readability
- prefix length tells you where the network portion ends
- Class A = /8, Class B = /16, Class C = /24
- first address = network address
- last address = broadcast address
- network and broadcast addresses are not usable by hosts
- netmask and prefix length represent the same thing in different formats

---

## Summary

This lesson introduced the fundamentals of IPv4 addressing.

Main points:

- switches connect devices in one LAN
- routers separate networks
- IPv4 addresses identify networks and hosts
- addresses are 32 bits and split into 4 octets
- binary and dotted decimal are two ways of representing the same address
- prefix length defines the network portion
- classes A, B, and C have different default prefix lengths
- every subnet has a network address and a broadcast address

These concepts are essential before learning routing, subnetting, and more advanced IP topics.
