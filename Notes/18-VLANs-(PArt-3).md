# 18-VLANs (Part 3) – Notes

## Overview

This lesson continues VLANs and focuses on:

- native VLAN on a router
- observing 802.1Q tags in Wireshark
- Layer 3 switches
- SVIs
- inter-VLAN routing with multilayer switching

It also compares router on a stick with Layer 3 switching.

---

## Native VLAN on a router

In router on a stick, the native VLAN can also be configured on the router side.

Two valid methods exist.

### Method 1: subinterface with native keyword

```bash
interface g0/0.10
encapsulation dot1q 10 native
ip address ...
```

This tells the router:

- VLAN 10 is the native VLAN
- untagged frames belong to this VLAN
- frames sent in this VLAN are untagged

### Method 2: physical interface for native VLAN

Instead of using a subinterface, the physical interface itself can hold the native VLAN IP address.

In that case:

- no `encapsulation dot1q` command is needed
- the physical interface acts as the native VLAN interface

Best practice is still to use an unused native VLAN for security reasons.

---

## Native VLAN and Wireshark

Wireshark can show whether a frame is tagged or untagged.

### Tagged frame

When a frame belongs to a non-native VLAN, it is sent with an 802.1Q tag.

In Wireshark you can see:

- `0x8100` in the Ethernet header
- VLAN ID field
- PCP / DEI fields

### Untagged frame

If a frame belongs to the native VLAN, it is sent untagged.

That means:

- no 802.1Q field is added
- both sides must agree on the native VLAN
- the receiving device assumes the untagged frame belongs to the native VLAN

---

## Layer 3 switch

A multilayer switch, also called a Layer 3 switch, can do both:

- switching
- routing

Unlike a normal Layer 2 switch, it is Layer 3 aware.

It can:

- use IP addresses
- build a routing table
- route traffic between VLANs
- use routed ports
- use SVIs

This makes it more efficient than router on a stick in larger networks.

---

## Why use Layer 3 switching

Router on a stick is efficient in terms of physical interfaces, but all inter-VLAN traffic must:

- go from switch to router
- be routed
- come back to the switch

This can create unnecessary traffic and possible congestion.

With a Layer 3 switch:

- inter-VLAN routing happens directly on the switch
- traffic does not need to leave the switch for internal routing

---

## Routed port

A routed port is a switch interface configured to behave like a router interface.

To configure it:

```bash
no switchport
ip address 192.168.1.193 255.255.255.252
```

Important commands:

### Enable routing on the switch

```bash
ip routing
```

Without this command, inter-VLAN routing will not work.

### Convert a switchport into a routed port

```bash
no switchport
```

After that, the interface can receive an IP address like a router.

---

## Point-to-point link to router

In the example topology:

- SW2 is a multilayer switch
- SW2 connects to R1 with a Layer 3 point-to-point link

Example subnet:

192.168.1.192/30

Addresses:

- SW2 G0/1 = 192.168.1.193
- R1 G0/0 = 192.168.1.194

This link is no longer a trunk carrying VLANs.

It is now a routed Layer 3 link.

---

## Default route on the multilayer switch

To send traffic outside the LAN, the Layer 3 switch needs a default route.

Example:

```bash
ip route 0.0.0.0 0.0.0.0 192.168.1.194
```

This tells the switch to send unknown destinations to R1.

---

## SVI

SVI stands for Switch Virtual Interface.

An SVI is a virtual Layer 3 interface associated with a VLAN.

It is used to:

- assign an IP address to a VLAN
- act as the default gateway for hosts in that VLAN
- route between VLANs

Example:

```bash
interface vlan 10
ip address 192.168.1.62 255.255.255.192
no shutdown
```

The same can be done for VLAN 20, VLAN 30, etc.

---

## Role of the SVI

Hosts in each VLAN use the SVI as their default gateway.

Example:

- VLAN 10 hosts use VLAN 10 SVI IP
- VLAN 20 hosts use VLAN 20 SVI IP
- VLAN 30 hosts use VLAN 30 SVI IP

If a host sends traffic to another subnet:

- it sends the frame to the switch
- the multilayer switch routes it internally using its routing table
- the traffic is forwarded to the correct VLAN

This avoids sending traffic to a separate router for internal VLAN routing.

---

## Conditions for an SVI to be up/up

An SVI will be up/up only if all required conditions are met.

### 1. The VLAN must exist on the switch
If the VLAN does not exist, the SVI stays down/down.

### 2. The VLAN must have an active interface
At least one of the following must be up/up:

- an access port in that VLAN
- a trunk port allowing that VLAN

### 3. The VLAN itself must not be shut down
If the VLAN is administratively shut down, the SVI cannot come up.

### 4. The SVI itself must not be shut down
SVIs are shut down by default, so `no shutdown` is required.

---

## show commands

Useful commands for verification:

### Show IP interfaces

```bash
show ip interface brief
```

Shows:

- physical routed ports
- SVIs
- interface status

### Show routing table

```bash
show ip route
```

Shows:

- connected routes
- local routes
- default route
- networks reachable through SVIs and routed ports

### Show interface status

```bash
show interfaces status
```

A routed port will appear as `routed` instead of showing a VLAN number.

---

## Inter-VLAN routing with a multilayer switch

Example:

A PC in VLAN 20 sends traffic to a PC in VLAN 10.

Process:

1. The PC sends the traffic to its default gateway
2. The default gateway is the SVI on the switch
3. The switch checks its routing table
4. The switch sees VLAN 10 is directly connected through its VLAN 10 SVI
5. The switch forwards the frame into VLAN 10 toward the destination

This is faster and more efficient than sending traffic to an external router.

---

## Multilayer switch vs router on a stick

### Router on a stick
- one trunk link to the router
- router does inter-VLAN routing with subinterfaces
- simple but can become inefficient in busy networks

### Multilayer switch
- switch routes between VLANs internally
- routed port used for upstream connection
- better for larger networks

---

## Key commands to remember

### Enable routing
```bash
ip routing
```

### Configure routed port
```bash
interface g0/1
no switchport
ip address 192.168.1.193 255.255.255.252
```

### Configure SVI
```bash
interface vlan 10
ip address 192.168.1.62 255.255.255.192
no shutdown
```

### Configure default route
```bash
ip route 0.0.0.0 0.0.0.0 192.168.1.194
```

### Native VLAN on router subinterface
```bash
encapsulation dot1q 10 native
```

---

## Key concepts to remember

- Native VLAN traffic is untagged
- Wireshark can show whether a frame is tagged or not
- A multilayer switch can switch and route
- `ip routing` must be enabled for Layer 3 functions
- `no switchport` converts an interface to a routed port
- SVIs are virtual Layer 3 interfaces used for VLAN gateways
- Inter-VLAN routing on a multilayer switch is more efficient than router on a stick
- An SVI will not come up unless the VLAN exists and has at least one active port

---

## Summary

This lesson introduced the last major VLAN routing concept: Layer 3 switching.

Instead of sending inter-VLAN traffic to a router, a multilayer switch can route traffic itself using SVIs.

Main ideas:

- native VLAN can also be configured on a router
- tagged vs untagged traffic can be observed in Wireshark
- Layer 3 switches support routed ports and SVIs
- `ip routing` enables routing on the switch
- SVIs act as gateways for VLANs
- multilayer switching is the preferred inter-VLAN routing method in larger networks
