# 16-VLANs (Part 1) – Notes

## Overview

VLAN = **Virtual Local Area Network**

VLANs are used to **logically separate a network at Layer 2**.

They are essential for:

- network performance
- network security
- network organization

---

## What is a LAN

A LAN (Local Area Network) is:

-> a **single broadcast domain**

A broadcast domain is:

-> all devices that receive a broadcast frame  
(destination MAC = FFFF.FFFF.FFFF)

---

## Broadcast domains

Key rules:

- Switch → floods broadcast frames
- Router → does NOT forward broadcast frames

So:

-> each router interface = **separate broadcast domain**

Example:

- PCs + switch + router interface = 1 LAN
- another network behind router = another LAN

---

## Problem without VLANs

All devices in the same switch = same broadcast domain.

Problems:

### Performance
- too many broadcast frames
- unnecessary traffic
- network congestion

### Security
- devices communicate directly
- router security rules are bypassed

---

## Subnets are not enough

Even if you split networks into subnets:

- switches work at **Layer 2**
- they only use MAC addresses

So:

-> broadcast traffic is still sent to ALL devices

---

## VLANs solution

VLANs allow you to:

-> split one physical network into multiple logical networks

Example:

- VLAN 10 → Engineering
- VLAN 20 → HR
- VLAN 30 → Sales

Each VLAN becomes:

-> its own broadcast domain

---

## VLAN behavior

Switch rules:

- forwards traffic ONLY inside the same VLAN
- does NOT forward traffic between VLANs

So:

-> VLAN = separate Layer 2 network

---

## Inter-VLAN communication

Devices in different VLANs cannot communicate directly.

They must go through:

-> a router

Process:

1. PC sends frame to default gateway
2. Router processes packet
3. Router sends it back to destination VLAN

This is called:

-> **inter-VLAN routing**

---

## Broadcast with VLANs

Without VLANs:

-> broadcast goes to ALL devices

With VLANs:

-> broadcast stays inside the VLAN only

Example:

PC in VLAN 10 → broadcast  
→ only VLAN 10 devices receive it

---

## VLAN configuration basics

### Default VLAN

- VLAN 1 exists by default
- all ports are in VLAN 1 initially

Also default VLANs:

- 1
- 1002–1005

These cannot be deleted.

---

### Assign a port to a VLAN

Commands:

```
interface range g0/0 - g0/3
switchport mode access
switchport access vlan 10
```

---

### Access port

An access port:

- belongs to ONE VLAN
- connects to end devices (PCs)

---

### VLAN creation

If VLAN does not exist:

-> it is created automatically when assigned

Or manually:

```
vlan 10
name ENGINEERING
```

---

### Show VLANs

Command:

```
show vlan brief
```

Displays:

- VLAN IDs
- VLAN names
- assigned ports

---

## Key concepts

- VLANs separate networks at **Layer 2**
- Each VLAN = **separate broadcast domain**
- Switches do NOT route between VLANs
- Router is required for inter-VLAN communication
- VLANs improve:
  - performance (less broadcast)
  - security (traffic isolation)

---

## Important reminders

- VLANs are configured **per interface**
- Broadcast traffic stays inside VLAN
- Switch only looks at **MAC addresses (Layer 2)**
- Router is needed to connect VLANs

---

## Summary

Without VLANs:

-> one big broadcast domain

With VLANs:

-> multiple isolated networks on the same switch

This allows:

- better performance
- better security
- better network design
