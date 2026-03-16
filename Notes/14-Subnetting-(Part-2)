# 14-Subnetting (Part 2)

## Continuation of subnetting

This part continues the learning of subnetting with:

- exercises on class C networks
- the introduction of subnetting on Class B networks
- a clearer method to solve problems

The principle remains the same:

- determine the number of subnets needed
- determine the number of hosts needed
- choose the right prefix

---

## Correction from previous fiscal year

Given network:

192.168.1.0/24

Objective:

Create 4 subnets with 45 hosts each.

In the previous section, we saw that **/26** is the right choice.

Why?

- /27 gives 30 usable hosts
- /26 yields 62 usable hosts

So you have to use:

192.168.1.0/26

---

## Subnets in /26

### Subnet 1
Network address:

192.168.1.0/26

Broadcast address:

192.168.1.63

Range:

192,168.1.0 → 192,168.1.63

---

### Subnet 2
Network address:

192.168.1.64/26

Broadcast address:

192,168,1,127

Range:

192,168.1.64 → 192,168.1,127

---

### Subnet 3
Network address:

192.168.1.128/26

Broadcast address:

192,168,1,191

Range:

192,168.1,128 → 192,168.1,191

---

### Subnet 4
Network address:

192.168.1.192/26

Broadcast address:

192,168,1,255

Range:

192,168.1,192 → 192,168.1,255

---

## Important Tip: Increment

To go faster, you can use **the value of the last bit borrowed**.

In a /26:

- the mask is 255.255.255.192
- the last bit of the network in the last byte is **64**

So the subnets increase by:

64

Example:

- 192 168 1.0
- 192 168 1 64
- 192 168 1 128
- 192 168 1 192

This value is often called the **block size**.

---

## Example: create 5 subnets of equal size

Given network:

192.168.255.0/24

Objective:

Create 5 subnets of equal size.

---

## Number of subnets

Formula:

2^(borrowed bits)

If you borrow:

- 1 bit → 2 subnets
- 2 bits → 4 subnets
- 3 bits → 8 subnets

Since we need 5 subnets, 4 is not enough.

It is therefore necessary to borrow 3 bits.

The new prefix is:

/27

---

## Subnets in /27

A /27 gives a block size of:

32

The subnets are therefore:

- 192.168.255.0/27
- 192.168.255.32/27
- 192.168.255.64/27
- 192.168.255.96/27
- 192.168.255.128/27
- 192.168.255.160/27
- 192.168.255.192/27
- 192.168.255.224/27

5 are used, but the /27 makes it possible to create 8.

---

## Identify the subnet of a host

Example:

What subnet contains the host:

192.168.5.57/27

Method:

- identify the prefix
- set all host bits to 0
- find the network address

Result:

192.168.5.32/27

So:

192.168.5.57 belongs to the subnet:

192.168.5.32/27

---

## Second example

Host:

192.168.29.219/29

Result:

192.168.29.216/29

So:

192.168.29.219 belongs to the subnet:

192.168.29.216/29

---

## Mental Table of Class C

For class C networks, it is necessary to know the patterns:

### /25
- 2 subnets
- 126 usable hosts
- block size: 128

### /26
- 4 subnets
- 62 usable hosts
- block size: 64

### /27
- 8 subnets
- 30 usable hosts
- block size: 32

### /28
- 16 subnets
- 14 usable hosts
- block size: 16

### /29
- 32 subnets
- 6 usable hosts
- block size: 8

### /30
- 64 subnets
- 2 usable hosts
- block size: 4

### /31
- special case point-to-point

### /32
- a single IP address

---

## Subnetting Class B

The process is exactly the same.

The difference is that a class B network usually starts with:

/16

So there are more bits available to make subnets.

---

## Example 1: 80 subnets

Given network:

172.16.0.0/16

Objective:

Create 80 subnets.

The following are used:

2^(borrowed bits)

- 6 bits → 64 subnets
- 7 bits → 128 subnets

It is therefore necessary to borrow 7 bits.

Final Prefix:

/23

Mask:

255,255,254.0

---

## Example 2: 500 subnets

Given network:

172.22.0.0/16

Objective:

Create 500 subnets.

- 8 bits → 256 subnets
- 9 bits → 512 subnets

So you have to borrow 9 bits.

Final Prefix:

/25

---

Example 3: 250 subnets and 250 hosts per subnet

Given network:

172.18.0.0/16

Objective:

- 250 subnets
- 250 hosts per subnet

For grants:

- 8 bits borrowed → 256 subnets

For hosts:

- 8 bits remaining hosts → 254 usable hosts

So the right choice is:

/24

---

## Identify the subnet in a Class B

Example:

Host:

172.25.217.192/21

Method:

- write the address
- identify network/host bits
- set host bits to 0

Result:

172.25.216.0/21

---

## Patterns to remember

### Number of Subnets
Each bit borrowed doubles the number of subnets:

- 2
- 4
- 8
- 16
- 32
- 64
- 128
- etc.

### Number of hosts
Each host bit doubles the number of available addresses.

Formula:

2^(host bits) - 2

The '-2' corresponds to:

- network address
- broadcast address

---

## Simple resolution method

When you're given a subnetting exercise:

### 1. Read the request
Example:
- how many subnets?
- how many guests?

### 2. Calculate Subnets
Use:

2^(borrowed bits)

### 3. Calculate hosts
Use:

2^(host bits) - 2

### 4. Choose the smallest prefix that works
We always take the smallest subnet that meets the need.

### 5. Find network addresses
With block size / incrementation.

---

## Important takeaways

- The subnetting on class B follows exactly the same logic as on class C.
- Each bit borrowed doubles the number of subnets.
- Each host bit doubles the number of addresses.
- Block size allows you to quickly find network addresses.
- To identify the subnet of a host, you must set the host bits to 0.
- The most important thing is not to memorize all tables, but to understand patterns.
