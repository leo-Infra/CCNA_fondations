# 22-STP Toolkit: Root Guard – Notes

## Overview

This lesson focuses on Root Guard, another STP protection feature.

Main purpose:

- protect the chosen root bridge
- prevent another switch with a lower bridge ID from taking over
- keep the STP topology stable and predictable

Root Guard is especially useful when your switches connect to switches outside of your direct control.

---

## Why root bridge control matters

STP can prevent loops no matter which switch becomes the root bridge.

However, the choice of root bridge still matters because it affects:

- traffic flow
- latency
- congestion
- stability of the topology

A good root bridge should provide efficient paths through the LAN and should be a reliable device.

---

## Root bridge selection considerations

When choosing the root bridge, two important factors are:

### 1. Optimal traffic flow
Traffic should follow the most efficient path possible.

Goal:

- minimize latency
- minimize congestion
- avoid unnecessary detours

Example:

If most hosts need to reach a router or external network, the root bridge should usually be placed near that router.

### 2. Stability and reliability
The root bridge should be a dependable switch.

Example:

- use newer, more reliable switches as root
- avoid using unstable or low-quality devices as root

---

## Why an unexpected root bridge is a problem

If another switch unexpectedly becomes the root bridge, STP recalculates the topology.

Result:

- some links that were previously active may become blocked
- traffic may take longer or less efficient paths
- congestion may appear on links that were not supposed to carry that load

So even if STP still prevents loops, the traffic pattern may become suboptimal.

---

## Root Guard purpose

Root Guard is used to ensure that a port does not accept superior BPDUs.

A superior BPDU is a BPDU claiming a better root bridge than the current one.

If a Root Guard-enabled port receives a superior BPDU:

- it does not become a root port
- it does not accept the new root bridge
- the port is blocked instead

This protects the existing STP design.

---

## Typical use case

Root Guard is commonly used when your switches connect to another organization’s switches.

Example:

- service provider network connected to customer network
- provider wants to keep its own switch as root bridge
- customer switch should not be allowed to influence provider’s STP topology

In this kind of design, Root Guard protects the provider’s side of the connection.

---

## Example scenario

Suppose the provider LAN has its own root bridge, SW1.

The customer LAN also has its own root bridge, SW6.

If the provider and customer networks are connected, both sides exchange BPDUs.

If SW6 has a lower Bridge ID than SW1:

- SW6 advertises superior BPDUs
- provider switches may accept SW6 as the new root bridge
- provider traffic now follows an inefficient path through the customer network

This is exactly what Root Guard is designed to stop.

---

## What Root Guard does

When enabled on a port:

- if normal BPDUs arrive, the port stays usable
- if a superior BPDU arrives, the port is placed into a special blocked condition

This condition is called:

- broken
- root inconsistent

Effectively, the port is disabled for data forwarding.

This prevents the new switch from becoming the root bridge through that port.

---

## Root Guard state

A Root Guard-triggered port enters the Root Inconsistent state.

Important points:

- the port cannot forward normal traffic
- the port is effectively blocked
- this is not the same as a normal forwarding or root port state

In CLI output, the port may appear as:

- BKN = broken
- ROOT_Inc = root inconsistent

---

## Recovery behavior

Root Guard recovery is automatic.

If the port stops receiving superior BPDUs:

- the root inconsistent condition clears
- the port returns to normal STP operation
- no manual shutdown / no shutdown is required

This is different from BPDU Guard, which usually requires manual recovery unless errdisable recovery is configured.

---

## How to fix a Root Guard problem

To restore a Root Guard-blocked link, you must remove the cause of the superior BPDU.

Typical fix:

- increase the priority of the remote switch
- make sure the remote switch no longer advertises itself as a better root

After the superior BPDU condition disappears and the old BPDUs age out, the port will recover automatically.

---

## Root Guard configuration

Root Guard is enabled in interface configuration mode.

Command:

```bash
spanning-tree guard root
```

Important:

- there is no global command to enable Root Guard everywhere
- it must be configured per interface
- it should only be applied where it makes design sense

---

## Why no global Root Guard command exists

Root Guard is not a feature you want on all ports.

Unlike PortFast, which is commonly useful on all access ports, Root Guard is selective.

It should only be used on interfaces where:

- you want to prevent an external switch from influencing STP
- you want to keep the current root bridge inside your own network

---

## Where Root Guard should be configured

Root Guard should be configured on ports facing switches that are outside of your administrative control.

Examples:

- provider-facing customer edge links
- enterprise-facing third-party switches
- boundary ports toward unmanaged or external STP domains

In short:

- configure Root Guard on your side of the boundary

---

## Where Root Guard should NOT be configured

Do not blindly enable Root Guard on every inter-switch connection.

Example:

If the customer also enables Root Guard on ports toward the provider, the customer’s ports may be blocked by the provider’s superior BPDUs.

That would prevent the customer from using the service provider network at all.

So Root Guard must be applied carefully and with a clear design goal.

---

## Relationship with STP roles

Root Guard protects against a port becoming a root port.

It does this by rejecting superior BPDUs.

So its main role is:

- prevent external devices from changing your chosen root bridge

It is not meant to replace normal STP, but to enforce your root bridge policy.

---

## Why Root Guard is useful

Without Root Guard:

- an unexpected switch with a lower Bridge ID can take over as root
- traffic paths may become suboptimal
- congestion may increase
- your STP design can change unexpectedly

With Root Guard:

- your intended root bridge remains in control
- unexpected superior BPDUs are stopped
- topology remains stable

---

## Root Guard vs BPDU Guard

These two features are different.

### Root Guard
- used on ports connected to other switches
- blocks the port only if superior BPDUs are received
- protects root bridge placement
- recovers automatically

### BPDU Guard
- usually used on PortFast access ports
- disables the port if any BPDU is received
- protects against accidental switch connections on host ports
- often requires manual recovery

So:

- Root Guard protects root bridge stability
- BPDU Guard protects access ports from switch connections

---

## Root Guard vs normal STP behavior

Normal STP allows the network to elect the best root bridge based on Bridge ID.

Root Guard changes that behavior on selected ports by saying:

- "do not accept a better root through this interface"

This is why Root Guard is considered a policy enforcement feature.

---

## Operational example summary

1. A switch port has Root Guard enabled.
2. A superior BPDU arrives on that port.
3. The switch does not allow the port to become a root port.
4. The port enters root inconsistent / broken state.
5. Traffic is blocked on that link.
6. Once superior BPDUs stop, the port recovers automatically.

---

## Key points to remember

- Root Guard protects the currently intended root bridge.
- It is used when connecting to switches outside your control.
- It prevents a port from becoming a root port if superior BPDUs are received.
- The blocked condition is called root inconsistent.
- Recovery is automatic once superior BPDUs stop.
- It is configured per interface only.
- It should be used selectively, not everywhere.

---

## Summary

Root Guard is an STP protection feature that helps maintain a stable and intentional root bridge placement.

It is most useful at the boundary of your network, where external switches could otherwise influence your STP topology.

Main idea:

- if a port receives a superior BPDU where it should not, Root Guard blocks that port instead of allowing the new switch to become root

This keeps traffic flow predictable and protects the LAN from unwanted STP changes.
