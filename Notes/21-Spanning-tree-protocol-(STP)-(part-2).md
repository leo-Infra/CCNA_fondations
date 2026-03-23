# 21-Spanning Tree Protocol (STP) – Part 2 (Operations, Timers, Features, Config)

## Overview

This lesson deepens STP understanding:

- port states and transitions
- STP timers
- BPDU structure
- optional features (PortFast, BPDU Guard, etc.)
- basic configuration (root bridge, load balancing)

---

## STP Port States

There are 5 states:

- Blocking
- Listening
- Learning
- Forwarding
- Disabled

Stable states:
- Forwarding (root + designated ports)
- Blocking (non-designated ports)

Transitional states:
- Listening
- Learning

---

## 1. Blocking State

Role:
- non-designated ports

Behavior:
- does NOT forward traffic
- drops all regular frames
- receives BPDUs
- does NOT forward BPDUs
- does NOT learn MAC addresses

Purpose:
- prevent Layer 2 loops

---

## 2. Listening State

Role:
- transitional (toward forwarding)

Duration:
- 15 seconds (Forward Delay timer)

Behavior:
- processes BPDUs only
- does NOT forward traffic
- does NOT learn MAC addresses

Purpose:
- ensure no loops before enabling forwarding

---

## 3. Learning State

Role:
- transitional

Duration:
- 15 seconds (Forward Delay timer)

Behavior:
- processes BPDUs
- does NOT forward traffic
- DOES learn MAC addresses

Purpose:
- prepare MAC table before forwarding

---

## 4. Forwarding State

Role:
- root ports and designated ports

Behavior:
- forwards normal traffic
- sends and receives BPDUs
- learns MAC addresses

This is normal switch operation.

---

## 5. Disabled State

- interface is administratively shut down
- not part of STP logic

---

## State Transition Summary

Blocking → Listening → Learning → Forwarding

Total default delay:
- 15s (Listening)
- 15s (Learning)

Total = 30 seconds before forwarding

---

## STP Timers

There are 3 main timers:

- Hello timer
- Forward Delay timer
- Max Age timer

---

## 1. Hello Timer

Default:
- 2 seconds

Function:
- interval at which root bridge sends BPDUs

Important:
- only root bridge generates BPDUs after convergence
- other switches forward them on designated ports only

---

## 2. Forward Delay Timer

Default:
- 15 seconds

Function:
- duration of Listening state
- duration of Learning state

Total transition time:
- 30 seconds

---

## 3. Max Age Timer

Default:
- 20 seconds

Function:
- how long a switch waits without receiving BPDUs before recalculating topology

---

## Max Age Example

Normal operation:
- BPDU received → timer resets to 20s

Failure:
- no BPDU received → timer counts down to 0

When it reaches 0:
- switch recalculates STP topology
- may activate previously blocked port

---

## Convergence Time

Worst case transition:

- 20s (Max Age)
- 15s (Listening)
- 15s (Learning)

Total = 50 seconds

---

## Important Behavior

- Forwarding → Blocking = immediate
- Blocking → Forwarding = slow (must pass through states)

Reason:
- avoid creating loops

---

## BPDU (Bridge Protocol Data Unit)

STP communication packet.

---

## BPDU MAC Addresses

- PVST+ (Cisco): 0100.0ccc.cccd
- Standard STP: 0180.c200.0000

Must be memorized for exam.

---

## BPDU Fields (simplified)

- Protocol ID (always 0000)
- Version (0 for STP)
- Type (configuration BPDU)
- Flags (topology changes)
- Root ID (root bridge info)
- Root path cost
- Bridge ID (sender switch)
- Port ID
- Timers (hello, max age, forward delay)

---

## Message Age

- increases as BPDU travels through switches
- reduces effective max age

Advanced concept (less important for CCNA)

---

## Important Rule

Root bridge timers apply to entire network.

---

## STP Optional Features (Toolkit)

Main ones to know:

- PortFast
- BPDU Guard

Also introduced:

- Root Guard
- Loop Guard

---

## PortFast

Purpose:
- eliminate 30-second delay on access ports

Behavior:
- immediately enters Forwarding state
- skips Listening and Learning

Use case:
- ports connected to end hosts only

---

## PortFast Warning

DO NOT use on switch-to-switch links.

Reason:
- can create Layer 2 loops instantly

---

## PortFast Configuration

Interface mode:

spanning-tree portfast

Global mode:

spanning-tree portfast default

Effect:
- enables PortFast on all access ports

---

## BPDU Guard

Purpose:
- protect against accidental loops

Behavior:
- if BPDU received → interface shuts down

---

## BPDU Guard Configuration

Interface:

spanning-tree bpduguard enable

Global (with PortFast):

spanning-tree portfast bpduguard default

---

## Recovery from BPDU Guard

To re-enable port:

shutdown  
no shutdown  

But only after fixing the issue.

---

## Root Guard

Purpose:
- prevent another switch from becoming root

Behavior:
- blocks port if superior BPDU received

---

## Loop Guard

Purpose:
- prevent loops due to unidirectional failures

Behavior:
- keeps port in blocking if BPDUs stop arriving

---

## STP Modes

Command:

spanning-tree mode ?

Options:

- pvst (classic)
- rapid-pvst (default)
- mst (not required for CCNA)

---

## Root Bridge Configuration

Primary root:

spanning-tree vlan X root primary

Effect:
- sets priority to 24576 (or lower if needed)

---

## Secondary root:

spanning-tree vlan X root secondary

Effect:
- sets priority to 28672

---

## Manual Priority

Alternative:

spanning-tree vlan X priority VALUE

Must follow:
- increments of 4096

---

## Load Balancing with VLANs

Concept:

- different VLANs use different root bridges
- traffic uses different paths

Example:

- VLAN10 → SW1 root
- VLAN20 → SW2 root

Result:
- better bandwidth usage
- no idle links

---

## Interface Configuration

Two parameters:

### Cost
- influences root port selection

Command:

spanning-tree vlan X cost VALUE

---

### Port Priority

- used in final tie-breaker

Command:

spanning-tree vlan X port-priority VALUE

Range:
- 0 to 224 (increments of 32)

---

## Key Concepts to Remember

- STP prevents loops using blocking ports
- transitions are slow to ensure safety
- timers control convergence speed
- PortFast improves access port performance
- BPDU Guard protects against misconfigurations
- root bridge selection controls traffic flow
- VLAN-based STP allows load balancing

---

## Practical Insight

Default STP is safe but slow.

Real networks:

- use PortFast for hosts
- use BPDU Guard for protection
- tune root bridge for optimal paths

---

## Summary

This lesson covered STP operation in depth:

- port states and transitions
- STP timers and convergence
- BPDU structure
- PortFast and BPDU Guard
- root bridge configuration
- VLAN-based load balancing

Next step:

- Rapid STP (faster convergence)
