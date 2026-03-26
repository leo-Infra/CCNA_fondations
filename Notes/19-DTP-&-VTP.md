# 19-DTP and VTP – Notes

## Overview

This lesson introduces two Cisco proprietary protocols:

- DTP = Dynamic Trunking Protocol
- VTP = VLAN Trunking Protocol

These topics were removed from the CCNA exam topic list, but they are still useful to understand because they may still appear in questions and are important in real Cisco environments.

---

## DTP

DTP stands for Dynamic Trunking Protocol.

It is a Cisco proprietary protocol used by Cisco switches to negotiate whether a switchport should operate as:

- an access port
- a trunk port

This means two Cisco switches can automatically form a trunk without manual trunk configuration.

---

## Important facts about DTP

- DTP is Cisco proprietary
- It runs only on Cisco devices
- It is enabled by default on Cisco switch interfaces
- Manual configuration is recommended instead of relying on DTP
- For security reasons, DTP should usually be disabled

---

## Why DTP is not recommended

DTP can be exploited by attackers.

Because of that, best practice is:

- manually configure access ports or trunk ports
- disable DTP negotiation when possible

---

## DTP modes

The main DTP-related administrative modes are:

- access
- trunk
- dynamic auto
- dynamic desirable

---

## Dynamic desirable

A port in **dynamic desirable** mode actively tries to form a trunk.

It will form a trunk if the other side is:

- trunk
- dynamic desirable
- dynamic auto

If the other side is access, it will not form a trunk.

---

## Dynamic auto

A port in **dynamic auto** mode does not actively try to form a trunk.

It is passive.

It will form a trunk only if the other side is:

- trunk
- dynamic desirable

If both sides are dynamic auto, no trunk forms.

---

## Access mode

A port in **access** mode is forced to be an access port.

It does not negotiate a trunk.

If connected to:

- dynamic auto → access
- dynamic desirable → access
- trunk → misconfiguration

---

## Trunk mode

A port in **trunk** mode is forced to be a trunk.

If connected to:

- dynamic auto → trunk
- dynamic desirable → trunk
- trunk → trunk
- access → misconfiguration

---

## Operational behavior summary

### access + access
Result: access

### access + dynamic auto
Result: access

### access + dynamic desirable
Result: access

### access + trunk
Result: misconfiguration

### dynamic auto + dynamic auto
Result: access

### dynamic auto + dynamic desirable
Result: trunk

### dynamic auto + trunk
Result: trunk

### dynamic desirable + dynamic desirable
Result: trunk

### dynamic desirable + trunk
Result: trunk

### trunk + trunk
Result: trunk

---

## Important DTP point

DTP will not form a trunk with:

- routers
- PCs
- non-switch devices

So if you want to use router on a stick, you must manually configure the switch interface as a trunk.

DTP will not automatically create a trunk toward a router.

---

## Older vs newer switches

On older Cisco switches, the default mode is often:

- dynamic desirable

On newer Cisco switches, the default mode is often:

- dynamic auto

This explains why an older replacement switch can unexpectedly form a trunk.

---

## Useful command: show interfaces switchport

Command:

```bash
show interfaces g0/0 switchport
```

Useful fields:

- Administrative Mode = configured mode
- Operational Mode = actual resulting mode

Example:

- administrative mode = dynamic desirable
- operational mode = trunk

This means the interface was configured to negotiate and ended up forming a trunk.

---

## Disabling DTP

Command:

```bash
switchport nonegotiate
```

This stops the interface from sending DTP frames.

Important notes:

- `switchport mode access` also disables DTP negotiation
- `switchport mode trunk` does **not** stop DTP by itself
- if you want a manually configured trunk without DTP negotiation, use both:
  - `switchport mode trunk`
  - `switchport nonegotiate`

---

## Trunk encapsulation negotiation

On switches that support both:

- 802.1Q
- ISL

DTP can also negotiate the trunk encapsulation type.

Default encapsulation mode:

```bash
switchport trunk encapsulation negotiate
```

If both switches support ISL, ISL is preferred.

If you want to manually force trunk mode on such a switch, you must first manually set encapsulation:

```bash
switchport trunk encapsulation dot1q
switchport mode trunk
```

---

## DTP frames

DTP frames are sent in:

- VLAN 1 when using ISL
- the native VLAN when using 802.1Q

By default, the native VLAN is VLAN 1.

---

## VTP

VTP stands for VLAN Trunking Protocol.

It is another Cisco proprietary protocol.

Purpose:

- configure VLANs centrally on one switch
- let other switches synchronize their VLAN database

This is useful in large networks with many VLANs, because you do not need to manually create every VLAN on every switch.

---

## Important facts about VTP

- Cisco proprietary
- Used to synchronize VLAN databases
- Works over trunk links
- Usually not recommended in modern networks
- Can be dangerous if used carelessly

Important: VTP only synchronizes the VLAN database.

It does **not** assign interfaces to VLANs automatically.

You still need commands like:

```bash
switchport access vlan 10
```

on each switchport.

---

## VTP versions

There are three versions:

- VTP version 1
- VTP version 2
- VTP version 3

Modern switches often support all three.

Version 1 is the default.

---

## VTP modes

There are three VTP modes:

- server
- client
- transparent

---

## VTP server

A VTP server:

- can add VLANs
- can modify VLANs
- can delete VLANs
- stores the VLAN database in NVRAM
- increases the revision number when VLAN changes occur
- advertises the latest VLAN database over trunk links

Cisco switches are in VTP server mode by default.

Important:

A VTP server also behaves like a VTP client, meaning it can synchronize to another VTP server with a higher revision number.

---

## VTP client

A VTP client:

- cannot add VLANs
- cannot modify VLANs
- cannot delete VLANs
- synchronizes its VLAN database to the server
- forwards VTP advertisements over trunk ports

Traditionally, VTP clients do not store the VLAN database in NVRAM, except in VTPv3.

---

## VTP transparent

A VTP transparent switch:

- does not synchronize its VLAN database
- keeps its own VLAN database
- can add, modify, and delete its own VLANs
- stores its VLAN database in NVRAM
- forwards VTP advertisements if they belong to the same domain
- does not advertise its own VLAN database for synchronization

Transparent mode is often safer than server/client behavior because it avoids database overwrites.

---

## VTP domain

VTP works within a VTP domain.

Switches must be in the same domain to synchronize VLAN information.

Default domain:

- null / no domain name

If a switch with a null domain receives a VTP advertisement with a domain name, it can automatically join that domain.

---

## Revision number

The revision number is one of the most important VTP concepts.

The revision number increases whenever VLANs are:

- added
- modified
- deleted

Switches assume that the VLAN database with the highest revision number is the newest and most correct one.

This is convenient, but it is also dangerous.

---

## VTP danger

A switch with an old or incorrect VLAN database but a **higher revision number** can overwrite the VLAN database of the whole VTP domain.

Example:

- production network revision = 5
- old switch revision = 50
- same VTP domain name

If the old switch is connected, other switches may synchronize to it and lose the correct VLAN database.

This can cause major outages.

That is one of the main reasons why VTP is often avoided in real networks.

---

## Resetting the VTP revision number

Two useful methods:

### 1. Change the VTP domain to an unused name
This resets the revision number to 0.

### 2. Change the switch to transparent mode
This also resets the revision number to 0.

These methods are useful before connecting an old switch to a VTP domain.

---

## VTP status command

Command:

```bash
show vtp status
```

This displays useful information such as:

- VTP version
- domain name
- operating mode
- revision number
- number of existing VLANs

---

## Default VLAN count in VTP

By default, switches already contain these VLANs:

- VLAN 1
- VLAN 1002
- VLAN 1003
- VLAN 1004
- VLAN 1005

So the default number of VLANs shown is usually 5.

---

## VTP and VLAN ranges

VTP version 1 and version 2 support only the normal VLAN range:

- 1 to 1005

They do not support extended VLANs:

- 1006 to 4094

VTP version 3 supports the extended VLAN range.

---

## VTP version 2 and version 3

### Version 2
Very similar to version 1.

Main difference:
- support for Token Ring VLANs

This is not very relevant today.

### Version 3
Adds more improvements and supports extended VLANs.

However, detailed VTPv3 behavior is beyond basic CCNA scope.

---

## Transparent mode and forwarding

A VTP transparent switch does not synchronize its own database, but it can still forward advertisements.

Important condition:

- it forwards advertisements only if the VTP domain matches

If the transparent switch is in a different domain, it will not forward those advertisements to other switches.

---

## Key DTP concepts to remember

- DTP negotiates access vs trunk automatically
- dynamic desirable actively tries to form a trunk
- dynamic auto is passive
- access mode prevents trunk formation
- trunk mode forces trunking
- `switchport nonegotiate` disables DTP negotiation
- DTP should usually be disabled for security reasons

---

## Key VTP concepts to remember

- VTP synchronizes VLAN databases
- server mode can modify VLANs
- client mode cannot modify VLANs
- transparent mode keeps its own VLAN database
- switches are in server mode by default
- revision number determines which database is considered newest
- a bad switch with a higher revision number can overwrite the domain
- VTP only works correctly within the same domain

---

## Summary

DTP and VTP are both Cisco proprietary protocols.

DTP is used to negotiate whether switchports operate as access ports or trunk ports.

VTP is used to synchronize VLAN databases across switches.

Both can reduce manual configuration, but both can also create problems.

For real networks, manual configuration is generally preferred:

- manually set access or trunk mode
- disable DTP
- be careful with VTP or avoid it unless truly necessary
