# 13-Subnetting (Part 1)

## CIDR

CIDR stands for **Classless Inter-Domain Routing**.

CIDR replaces the old IPv4 class system:

- Class A
- Class B
- Class C

With CIDR, we are no longer obliged to use only:

- /8 for class A
- /16 for class B
- /24 for class C

This allows you to create networks that are **smaller and more adapted to the real need**.

---

## Reminder on IPv4 classes

### Class A
First byte:

0 to 127

Default prefix:

/8

This gives:

- few networks
- huge number of hosts per network

---

### Class B
First byte:

128 to 191

Default prefix:

/16

This gives:

- more networks than class A
- fewer hosts per network

---

### Class C
First byte:

192 to 223

Default prefix:

/24

This gives:

- many networks
- few hosts per network

---

### Class D
First byte:

224 to 239

Used for special purposes.

---

### Class E
First byte:

240 to 255

Used for special / reserved uses.

---

## Problem with classful addressing

The old system assigned too large blocks.

Example:

A small business that needs about 5000 addresses cannot use a class C network because a /24 does not provide enough addresses.

It must therefore receive a class B network.

A /16 provides approximately:

65536 addresses

Result:

huge waste of addresses.

---

## Example of waste

### Case 1 – Point-to-point link
A network between two routers only needs **2 usable IP addresses**.

If using:

203.0.113.0/24

A /24 provides:

256 total addresses
254 usable addresses

For only 2 routers, this wastes almost the entire block.

---

### Case 2 – Company
A business needs:

5000 hosts

A /24 is too small.
It is therefore often given a /16.

But that leaves tens of thousands of addresses unused.

---

## Why CIDR is useful

CIDR allows you to split a large network into **smaller subnets**.

These subnets are called:

**grants**

The aim is to make better use of the IPv4 address space.

---

## Subnetting

Subnetting = dividing a larger network into several smaller networks.

Example:

instead of using everything:

203.0.113.0/24

you can only use

203.0.113.0/30

or even:

203.0.113.0/31

as required.

---

## Number of usable addresses

General formula:

2^(number of host bits) - 2

The '-2' corresponds to:

- the network address
- the broadcast address

---

## Example of prefixes

### /24
Mask:

255,255,255.0

Host Bits:

8

Calculation:

2^8 - 2 = 254

Usable addresses:

254

---

### /25
Mask:

255 255 255 128

Host Bits:

7

Calculation:

2^7 - 2 = 126

Usable addresses:

126

---

### /26
Mask:

255 255 255 192

Host Bits:

6

Calculation:

2^6 - 2 = 62

Usable addresses:

62

---

### /27
Mask:

255 255 255 224

Host Bits:

5

Calculation:

2^5 - 2 = 30

Usable addresses:

30

---

### /28
Mask:

255 255 255 240

Host Bits:

4

Calculation:

2^4 - 2 = 14

Usable addresses:

14

---

### /29
Mask:

255,255,255,248

Host Bits:

3

Calculation:

2^3 - 2 = 6

Usable addresses:

6

---

### /30
Mask:

255 255 255 252

Host Bits:

2

Calculation:

2^2 - 2 = 2

Usable addresses:

2

For a point-to-point link, **/30 is perfectly suited**.

---

## Special case of /31

Mask:

255,255,255,254

Host Bits:

1

Classic formula:

2^1 - 2 = 0

Normally, this would give 0 usable address.

But for a **point-to-point link**, a /31 can be used.

Why?

Because a link between two routers:

- does not need a network address
- does not need a broadcast address

It is therefore possible to use the two addresses available for the two routers.

Example:

203.0.113.0/31

Addresses

- 203.0.113.0
- 203.0.113.1

Both can be assigned.

---

## Special case of /32

Mask:

255 255 255 255

Host Bits:

0

There is **no host bit**.

A /32 denotes **a single precise IP address**.

/32 is generally not used to configure a conventional network.

But /32 is useful for:

- designate a specific host
- some static roads
- some local roads

---

## Concrete example of subnetting

Given network:

192.168.1.0/24

Need:

4 subnets

Each sub-network shall host:

45 hosts

It is also necessary to count:

- the network address
- the broadcast address

So real need per subnet:

47 addresses

---

## Finding the right prefix

### /30
2 usable addresses
→ insufficient

### /29
6 usable addresses
→ insufficient

### /28
14 usable addresses
→ insufficient

### /27
30 usable addresses
→ insufficient

### /26
62 usable addresses
→ sufficient

So for this need:

**/26 is the right choice**

---

## Important idea

You can't always get **exactly** the number of addresses you want.

Sometimes there is a little unused space.

It's okay.

Having a little margin can be useful for future growth
