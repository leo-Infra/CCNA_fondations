# 15-Subnetting (Part 3) – Notes

## End of the subnetting series

This part ends the series on subnetting.

Main content:

- correction of the previous quiz
- subnetting on Class A networks
- introduction to VLSM
- resources to continue training

The principle remains the same:

- determine how many subnets are needed
- determine how many hosts it takes
- choose the right prefix

---

## Correction question 1

Given network:

172.30.0.0/16

Objective:

100 subnets
at least 500 hosts per subnet

### Number of Subnets
You have to borrow enough bits:

- 6 bits → 64 subnets
- 7 bits → 128 subnets

So you have to borrow:

7-bit

The new prefix is:

/23

### Number of hosts
There remains:

9 host bits

Calculation:

2^9 - 2 = 510

So:

/23 allows 100 subnets with at least 500 hosts

Mask:

255,255,254.0

---

## Correction question 2

Host:

172.21.111.201/20

Objective:

find the subnet to which it belongs

Method:

- write the address
- set all host bits to 0
- convert back to decimal

Result:

172.21.96.0/20

---

## Correction question 3

Address given:

192.168.91.78/26

Objective:

find the broadcast address

Method:

- write the address
- set all host bits to 1
- convert back to decimal

Result:

192,168,91,127

---

## Correction question 4

Given network:

172.16.0.0/16

Objective:

divide it into 4 equal-sized subnets
find the network address and broadcast address of the **second subnet**

### Number of Subnets
4 subnets = 2 bits borrowed

New prefix:

/18

### Second Subnet
Network address:

172.16.64.0

Broadcast address:

172.16.127.255

---

## Correction question 5

Given network:

172.30.0.0/16

Objective:

make grants of 1000 hosts each
how many subnets can be created?

### Host requirement
For 1000 hosts:

2^10 - 2 = 1022

So it is necessary:

10 host bits

### Bits borrowed
On a /16, if you keep 10 host bits, you can borrow:

6-bit

### Number of Subnets
2^6 = 64

Result:

64 subnets

---

## Subnetting on Class A

The process is exactly the same as for:

- Class C
- Class B

The difference:

a Class A network typically begins with:

/8

So there are a lot more bits available.

---

## Class A Example

Given network:

10.0.0.0/8

Objective:

create 2000 subnets

### Number of Subnets
We must find:

2^x ≥ 2000

- 10 bits → 1024
- 11 bits → 2048

So you have to borrow:

11-bit

New prefix:

/19

### Number of hosts
Remaining Host Bits:

13

Calculation:

2^13 - 2 = 8190

Result:

- prefix: /19
- hosts usable by subnet: 8190

---

## Second Class A Example

Host:

10.217.182.223/11

Objective:

identify:

1. network address
2. broadcast address
3. first usable address
4. last usable address
5. usable hosts

### Results

Network address:

10,192.0.0

Broadcast address:

10,223,255,255

First usable address:

10.192.0.1

Last usable address:

10,223,255,254

Number of usable hosts:

2^21 - 2 = 2097150

---

## VLSM

VLSM means:

**Variable-Length Subnet Mask**

Unlike FLSM:

**Fixed-Length Subnet Mask**

With FLSM:
all the subnets are the same size

With VLSM:
the subnets can have **different sizes**

The goal is to use the address space more efficiently.

---

## VLSM Example

Given network:

192.168.1.0/24

Needs:

- Tokyo LAN A: 110 hosts
- Toronto LAN B: 45 hosts
- Toronto LAN A: 29 hosts
- Tokyo LAN B: 8 hosts
- point-to-point link: 2 hosts

If we used FLSM, there wouldn't be enough space.

With VLSM, we assign the subnets **from the largest to the smallest**.

---

## VLSM Basic Rule

Always start with:

1. the largest subnet
2. then the second largest
3. then continue to the smallest

Why?

Because if we put the small subnets first, we risk breaking the space necessary for the big ones.

---

## VLSM – Tokyo LAN A

Need:

110 hosts

You need one:

/25

Why?

- /25 → 126 usable hosts
- /26 → 62 usable hosts, insufficient

### Results

Network address:

192.168.1.0/25

Broadcast address:

192,168,1,127

First usable:

192.168.1.1

Last worn:

192,168,1,126

Usable Hosts:

126

---

## VLSM – Toronto LAN B

Need:

45 hosts

You need one:

/26

Why?

- /26 → 62 usable hosts
- /27 → 30 usable hosts, insufficient

The next block starts just after the previous broadcast.

### Results

Network address:

192.168.1.128/26

Broadcast address:

192,168,1,191

First usable:

192,168,1,129

Last worn:

192,168,1,190

Usable Hosts:

62

---

## VLSM – Toronto LAN A

Need:

29 hosts

You need one:

/27

Why?

- /27 → 30 usable hosts

### Results

Network address:

192.168.1.192/27

Broadcast address:

192,168,1,223

First usable:

192,168,1,193

Last worn:

192,168,1,222

Usable Hosts:

30

---

## VLSM – Toronto LAN B

Need:

45 hosts

You need one:

/26

Why?

- /26 → 62 usable hosts
- /27 → 30 usable hosts, insufficient

The next block starts just after the previous broadcast.

### Results

Network address:

192.168.1.128/26

Broadcast address:

192,168,1,191

First usable:

192,168,1,129

Last worn:

192,168,1,190

Usable Hosts:

62

---

## VLSM – Toronto LAN A

Need:

29 hosts

You need one:

/27

Why?

- /27 → 30 usable hosts

### Results

Network address:

192.168.1.192/27

Broadcast address:

192,168,1,223

First usable:

192,168,1,193

Last worn:

192,168,1,222

Usable Hosts:

30

---

## VLSM – Tokyo LAN B

Need:

8 hosts

You need one:

/28

Why?

- /28 → 14 usable hosts
- /29 → 6 usable hosts, insufficient

### Results

Network address:

192.168.1.224/28

Broadcast address:

192,168,1,239

First usable:

192,168,1,225

Last worn:

192,168,1,238

Usable Hosts:

14

---

## VLSM – Point-to-point link

Need:

2 hosts

For the NACC, the following are the main points:

/30

Why?

- /30 → 2 usable hosts

Even if /31 can work on point-to-point, for CCNA exercises it is better to answer /30 if you ask for a subnet for 2 hosts.

### Results

Network address:

192.168.1.240/30

Broadcast address:

192,168,1,243

First usable:

192,168,1,241

Last worn:

192,168,1,242

Usable Hosts:

2

---

## VLSM Final Outcome

Allocated grants:

- 192.168.1.0/25
- 192.168.1.128/26
- 192.168.1.192/27
- 192.168.1.224/28
- 192.168.1.240/30

Each subnet has a size adapted to the real need.

It's more effective than FLSM.

---

## VLSM steps to remember

### 1. Sort the needs from the largest to the smallest
Always start with the subnet that needs the largest number of hosts.

### 2. Choose the smallest prefix sufficient
Do not take a subnet too large if a smaller one is enough.

### 3. Place the first subnet at the beginning of the address space
Example:

192.168.1.0/25

### 4. Take the following address after the broadcast
It becomes the network address of the next subnet.

### 5. Repeat until all grants are awarded

---

## Resources to train

Recommended sites:

- subnettingquestions.com
- subnetting.org
- subnettingpractice.com

Objective:

exercise at least once a day for a week.

---

## Important takeaways

- Class A subnetting follows exactly the same logic as classes B and C.
- The more bits you borrow, the more subnets you create.
- The more host bits we keep, the more usable hosts we have.
- VLSM allows to use subnets of different sizes.
- In VLSM, it is always necessary to assign the subnets from the largest to the smallest.
- VLSM saves address space.
- For the CCNA, a point-to-point link to 2 hosts is usually processed in /30.
