# 20-Spanning Tree Protocol (STP) – Notes (Part 1)

## Overview

This lesson introduces Spanning Tree Protocol (STP), a fundamental Layer 2 protocol.

Main objectives:

- understand network redundancy
- understand Layer 2 loops and their dangers
- understand how STP prevents loops
- learn STP election process (root bridge, root ports, designated ports)

This is the foundation before learning Rapid STP.

---

## Network redundancy

Redundancy is essential in modern networks.

Key points:

- networks must run continuously (24/7/365)
- downtime can cause major business impact
- multiple paths must exist in case of failure
- redundancy should be implemented at all possible points

Without redundancy:

- a single failure can break connectivity

With redundancy:

- alternate paths allow traffic to continue flowing

---

## Problem with redundancy at Layer 2

Redundancy introduces a major issue at Layer 2:

Layer 2 loops

Example:

- multiple switches connected in a loop
- broadcast traffic circulates endlessly

This leads to serious problems.

---

## Broadcast storm

When a broadcast frame is sent:

- switches flood it out all ports (except incoming port)
- in a looped topology, frames circulate infinitely

There is no TTL field in Ethernet frames, so:

- frames never expire
- they continue looping forever

Result:

- network congestion
- legitimate traffic cannot pass

This is called a broadcast storm.

---

## MAC address flapping

Another issue caused by loops:

MAC address flapping

Explanation:

- switches learn MAC addresses from incoming frames
- if the same MAC appears on multiple interfaces repeatedly
- the switch constantly updates its MAC table

Result:

- unstable forwarding
- degraded network performance

---

## Solution: Spanning Tree Protocol (STP)

STP is a Layer 2 protocol designed to prevent loops.

Standard:

- IEEE 802.1D (classic STP)

Key concept:

- STP blocks redundant paths to eliminate loops
- keeps backup paths available in case of failure

---

## STP behavior

STP places ports into different states:

### Forwarding state
- normal operation
- sends and receives traffic

### Blocking state
- does not forward normal traffic
- only processes STP messages (BPDUs)

Blocked ports act as backup links.

If an active link fails:

- blocked ports can transition to forwarding

---

## BPDU (Bridge Protocol Data Unit)

STP uses BPDUs to exchange information.

Key facts:

- sent every 2 seconds (Hello timer)
- used to build STP topology
- allow switches to discover each other

Devices that send BPDUs:

- switches only

Devices that do NOT send BPDUs:

- PCs
- routers

---

## Terminology: bridge

In STP, the term "bridge" is used.

Important:

- bridge = switch (in modern networks)
- historical term from older devices

---

## STP goal

STP creates a loop-free topology by:

- selecting one active path
- blocking redundant paths

Result:

- no Layer 2 loops
- redundancy still available

---

## STP process overview

STP works in multiple steps:

1. elect root bridge
2. select root ports
3. select designated ports
4. block remaining ports

---

## Step 1: Root bridge election

The root bridge is the central reference point.

Election rule:

- switch with lowest Bridge ID becomes root

---

## Bridge ID

Bridge ID consists of:

- bridge priority (default 32768)
- extended system ID (VLAN ID)
- MAC address

Lowest Bridge ID wins.

If priorities are equal:

- lowest MAC address wins

---

## Extended system ID

Modern STP includes VLAN ID in Bridge ID.

Reason:

- PVST (Per VLAN Spanning Tree)
- each VLAN has its own STP instance

Effect:

- Bridge ID differs per VLAN

Example:

- VLAN 1 → priority = 32768 + 1 = 32769
- VLAN 2 → priority = 32770

---

## Bridge priority rules

Bridge priority can only change in increments of 4096.

Valid values:

- 0
- 4096
- 8192
- 12288
- ...
- 61440

Lower priority = more likely to become root bridge

---

## Root bridge properties

- all ports are designated ports
- all ports are forwarding
- root bridge generates BPDUs

Other switches:

- forward BPDUs but do not generate new ones

---

## Step 2: Root port selection

Each non-root switch selects ONE root port.

Root port:

- best path to root bridge

Root bridge has NO root ports.

---

## STP path cost

Each interface has a cost:

- 10 Mbps → 100
- 100 Mbps → 19
- 1 Gbps → 4
- 10 Gbps → 2

Root cost:

- total cost to reach root bridge

---

## Root port selection rule

Choose port with:

- lowest root cost

---

## Root port tiebreakers

If multiple paths have same cost:

1. lowest neighbor Bridge ID
2. lowest neighbor port ID

Important:

- neighbor’s values are used for tie-breaking

---

## Port ID

Port ID consists of:

- port priority (default 128)
- port number

Lower port ID wins in tie situations.

---

## Step 3: Designated port selection

Each link (collision domain) must have:

- exactly one designated port

Designated port:

- forwards traffic
- represents the segment toward root

---

## Designated port selection rules

Choose port with:

1. lowest root cost
2. lowest bridge ID (tie-breaker)

---

## Non-designated ports

Ports that are neither:

- root port
- designated port

become:

- non-designated ports

State:

- blocking

Purpose:

- prevent loops

---

## STP topology result

Final topology:

- single active path between devices
- redundant paths blocked
- no loops

---

## Key rules to remember

- one root bridge per VLAN
- one root port per non-root switch
- one designated port per link
- remaining ports are blocking

---

## STP convergence behavior

At startup:

- all switches assume they are root

After receiving superior BPDUs:

- switches update their view
- topology stabilizes

After convergence:

- only root bridge generates BPDUs

---

## Example logic summary

To determine topology:

1. find root bridge (lowest Bridge ID)
2. assign root ports (lowest path cost)
3. assign designated ports per link
4. block remaining ports

---

## Important concepts for CCNA

- redundancy is required but dangerous at Layer 2
- loops cause broadcast storms
- STP prevents loops by blocking ports
- BPDU exchange is fundamental
- root bridge is central to STP logic
- path cost determines best path
- tie-breakers are critical for exams

---

## Summary

This lesson introduced STP fundamentals.

Key takeaways:

- Layer 2 loops are dangerous
- broadcast storms can destroy networks
- STP prevents loops by disabling redundant paths
- root bridge is elected using Bridge ID
- switches choose root ports based on cost
- designated ports are selected per link
- remaining ports are blocked

Next step:

- deeper STP behavior (states, timers, convergence)
- Rapid STP improvements
