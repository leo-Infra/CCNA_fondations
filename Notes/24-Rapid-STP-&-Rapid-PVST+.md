# 24-Rapid STP / Rapid PVST+ – Notes

## Overview

This lesson introduces Rapid Spanning Tree Protocol (RSTP), specifically Cisco Rapid PVST+.

Main goals:

- compare STP versions
- understand why RSTP is faster than classic STP
- learn new RSTP states and roles
- understand RSTP BPDUs
- understand RSTP link types
- compare Rapid PVST+ with classic STP and PVST+

Rapid STP is the modern improvement over classic STP and is the version most relevant for CCNA.

---

## STP Version Comparison

### IEEE standard versions

- 802.1D = classic STP
- 802.1w = RSTP
- 802.1s = MSTP

### Cisco versions

- PVST+ = Cisco per-VLAN version of classic STP
- Rapid PVST+ = Cisco per-VLAN version of RSTP

---

## 1. Classic STP (802.1D)

Characteristics:

- original STP standard
- one STP instance shared by all VLANs
- cannot load balance per VLAN
- slow convergence
- can take up to 50 seconds after topology changes

---

## 2. PVST+

Characteristics:

- Cisco enhancement of classic STP
- one STP instance per VLAN
- allows load balancing by VLAN
- still uses classic STP behavior and timers
- still slow compared to RSTP

Main advantage:

- different VLANs can block different links
- better bandwidth usage

---

## 3. RSTP (802.1w)

Characteristics:

- faster version of STP
- same basic purpose: prevent Layer 2 loops
- still only one instance shared by all VLANs
- cannot load balance by VLAN
- much faster convergence than classic STP

---

## 4. Rapid PVST+

Characteristics:

- Cisco enhancement of RSTP
- one RSTP instance per VLAN
- fast convergence
- load balancing by VLAN
- this is the most important Cisco version for CCNA

---

## 5. MSTP (802.1s)

Characteristics:

- multiple spanning tree instances
- allows grouping many VLANs into a smaller number of STP instances
- allows load balancing
- easier to manage in very large VLAN environments

Example:

- VLANs 1–100 in instance 1
- VLANs 101–200 in instance 2

Advantage:

- easier than configuring root bridges separately for hundreds of VLANs

---

## Why Rapid STP is needed

Classic STP is too slow for modern networks.

Classic STP may take up to:

- 20s Max Age
- 15s Listening
- 15s Learning

Total = 50s convergence

RSTP improves this dramatically by using a handshake / negotiation process instead of relying mainly on timers.

Result:

- convergence usually takes only a few seconds

---

## Key similarity with classic STP

RSTP keeps the same core STP logic:

- same purpose
- same root bridge election
- same root port election
- same designated port election

So if you already understand STP, RSTP is easier to learn.

---

## Root bridge election in RSTP

Same rule as STP:

- lowest Bridge ID wins

No change here.

---

## Root port selection in RSTP

Same rule as STP:

- lowest root cost wins
- tie-breaker: lowest neighbor Bridge ID
- final tie-breaker: lowest neighbor Port ID

No change here.

---

## Designated port selection in RSTP

Same rule as STP:

- one designated port per segment
- best BPDU on the segment wins

No change here either.

---

## RSTP path costs

RSTP updated the port costs to support higher speeds.

Typical RSTP costs:

- 10 Mbps = 2,000,000
- 100 Mbps = 200,000
- 1 Gbps = 20,000
- 10 Gbps = 2,000
- 100 Gbps = 200
- 1 Tbps = 20

These are different from classic STP costs:

- 10 Mbps = 100
- 100 Mbps = 19
- 1 Gbps = 4
- 10 Gbps = 2

For exams, know that RSTP uses a newer extended cost scale.

---

## RSTP Port States

Classic STP had:

- Blocking
- Listening
- Learning
- Forwarding
- Disabled

RSTP simplifies this to 3 states:

- Discarding
- Learning
- Forwarding

---

## Discarding state

Discarding combines or replaces:

- Blocking
- Disabled

And the Listening state is effectively removed.

A port in Discarding:

- does not forward traffic
- may process STP information
- is not forwarding user frames

---

## Learning state

A port in Learning:

- does not forward traffic yet
- learns MAC addresses

---

## Forwarding state

A port in Forwarding:

- forwards traffic
- learns MAC addresses
- fully operational

---

## Why RSTP is faster

RSTP does not rely on classic STP transitional delays the same way.

Instead, it uses a handshake process between switches to decide quickly whether a port can move to forwarding.

This allows many ports to move directly to forwarding when appropriate.

---

## RSTP Port Roles

RSTP keeps:

- Root
- Designated

But replaces the old "non-designated" concept with two separate roles:

- Alternate
- Backup

So the RSTP port roles are:

- Root
- Designated
- Alternate
- Backup

---

## 1. Root port

Same as classic STP:

- one per non-root switch
- best path to root bridge

---

## 2. Designated port

Same as classic STP:

- one per segment
- sends the best BPDU on that segment

---

## 3. Alternate port

An alternate port is a discarding port that receives a superior BPDU from another switch.

This is basically the RSTP replacement for the usual non-designated blocking port in STP.

Important role:

- backup for the root port
- if the root port fails, the alternate port can move to forwarding quickly

---

## 4. Backup port

A backup port is a discarding port that receives a superior BPDU from another interface on the same switch.

This happens only when two ports on the same switch connect to the same shared segment, usually through a hub.

Important:

- backup ports are rare in modern networks
- hubs are old technology and almost never used now

Still, the role exists in RSTP.

---

## Alternate vs Backup

### Alternate port
- receives better BPDU from another switch
- backup to root port

### Backup port
- receives better BPDU from another port on the same switch
- backup to designated port

This distinction is one of the major RSTP differences from classic STP.

---

## Built-in classic STP features

RSTP includes functionality that used to require optional classic STP features.

The important ones are:

- PortFast
- UplinkFast
- BackboneFast

These are effectively built into RSTP behavior.

---

## UplinkFast in RSTP

In classic STP, UplinkFast helped a blocked port move quickly to forwarding when the main uplink failed.

In RSTP, this fast recovery behavior is built in.

That is why alternate ports can quickly take over.

---

## BackboneFast in RSTP

In classic STP, BackboneFast helped switches react more quickly to indirect failures.

In RSTP, this is also built in.

So RSTP reacts faster to topology changes without needing separate BackboneFast configuration.

---

## PortFast in RSTP

PortFast is also built into RSTP through the edge port concept.

An edge port:

- connects to an end host
- can move immediately to forwarding
- does not need the normal switch-to-switch handshake

So in RSTP:

- PortFast and edge behavior are effectively the same concept

---

## RSTP BPDU differences

The BPDU format is mostly similar to classic STP, but there are a few important differences.

### Protocol Version
- classic STP = 0
- RSTP = 2

This is important to remember.

### BPDU Type
RSTP uses a different BPDU type than classic STP.

### Flags
Classic STP used only a small part of the BPDU flags field.

RSTP uses all 8 bits of the flags field.

These flags support the rapid negotiation process.

---

## Who sends BPDUs in RSTP

This is a major difference.

### Classic STP
- only the root bridge originates BPDUs
- other switches forward them

### RSTP
- all switches generate and send their own BPDUs

This helps RSTP react more quickly to changes.

---

## Neighbor loss detection

Classic STP typically waits a long time before deciding a neighbor is lost.

RSTP is much faster.

Typical rule:

- if 3 consecutive BPDUs are missed, the neighbor is considered lost
- with 2-second hello time, this is about 6 seconds

When this happens:

- the switch flushes MAC addresses learned on that interface
- quickly adjusts the topology

---

## Compatibility with classic STP

RSTP is backward compatible with classic STP.

If an RSTP switch connects to a classic STP switch:

- that specific interface operates in classic STP mode
- it uses the slower classic behavior on that link

Other interfaces can still use RSTP normally.

So a mixed environment is possible, but slower on those legacy links.

---

## RSTP Link Types

RSTP defines 3 link types:

- Edge
- Point-to-point
- Shared

Do not confuse link types with port roles or port states.

---

## 1. Edge link type

Edge ports connect to end hosts.

Behavior:

- immediately move to forwarding
- no risk of Layer 2 loop from another switch

Configuration:

```bash
spanning-tree portfast
```

This is how you configure an edge port.

So in practice:

- PortFast = edge port behavior

---

## 2. Point-to-point link type

This is a direct switch-to-switch connection.

Usually:

- full-duplex
- normal modern inter-switch links

Switches can usually detect this automatically.

Optional manual configuration:

```bash
spanning-tree link-type point-to-point
```

---

## 3. Shared link type

A shared link connects to a hub.

Characteristics:

- half-duplex
- shared collision domain

Optional manual configuration:

```bash
spanning-tree link-type shared
```

In practice, this is very uncommon today because hubs are obsolete.

---

## Edge / Point-to-point / Shared summary

### Edge
- end host connection
- immediate forwarding
- configured with PortFast

### Point-to-point
- direct switch-to-switch full-duplex link

### Shared
- hub-based half-duplex segment

---

## Practical impact of RSTP

Compared to classic STP, RSTP gives:

- faster convergence
- quicker reaction to failures
- fewer delays before forwarding
- better usability in modern switched networks

Rapid PVST+ adds one more major benefit:

- one instance per VLAN
- VLAN-based load balancing

---

## Key concepts to remember

- RSTP is the faster evolution of classic STP
- Rapid PVST+ is Cisco’s per-VLAN version of RSTP
- root bridge, root port, and designated port elections stay the same
- RSTP uses Discarding, Learning, Forwarding
- alternate and backup ports replace the old non-designated concept
- all switches generate BPDUs in RSTP
- classic STP optional features like PortFast, UplinkFast, and BackboneFast are built in
- edge ports are configured with PortFast
- point-to-point links are direct switch links
- shared links involve hubs

---

## Summary

Rapid STP keeps the same general STP logic but improves convergence dramatically.

Main improvements:

- faster recovery after failures
- fewer states
- built-in fast transition features
- all switches originate BPDUs
- alternate and backup port roles
- support for immediate forwarding on edge ports

Cisco Rapid PVST+ combines this faster behavior with per-VLAN instances, which makes it the most important practical STP version to know for CCNA-level Cisco switching.
