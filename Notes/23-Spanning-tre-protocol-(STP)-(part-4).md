# 23-STP Toolkit: Loop Guard – Notes

## Overview

This lesson covers Loop Guard, another STP protection feature.

Main purpose:

- protect the network from Layer 2 loops
- prevent a port from incorrectly moving to Forwarding when it unexpectedly stops receiving BPDUs
- add protection against unidirectional link problems

Loop Guard does not prevent physical link failures.  
It protects STP from making the wrong decision when a failure causes BPDUs to stop arriving.

---

## Why Loop Guard exists

STP already prevents loops by blocking redundant paths.

However, STP assumes that BPDUs are exchanged correctly between switches.

If a switch unexpectedly stops receiving BPDUs on a port that is supposed to receive them, STP may assume that the path is no longer needed for loop prevention and may move that port into Forwarding.

If this happens incorrectly, a Layer 2 loop can form.

Loop Guard prevents that mistake.

---

## Unidirectional link

The main failure scenario explained in this lesson is a unidirectional link.

A unidirectional link is a link where traffic works in only one direction.

Example:

- SW1 can send frames to SW2
- SW2 cannot send frames to SW1

So the link appears partially functional, but not fully operational.

---

## Causes of unidirectional links

Unidirectional links are usually caused by Layer 1 problems, such as:

- damaged fiber cable
- faulty connectors
- faulty transceivers
- physical signal problems

They are more common with fiber than copper.

Why?

Because fiber connections often use two separate fibers:

- one for transmit
- one for receive

If only one of those fibers is damaged, communication may fail in one direction but continue in the other.

---

## Normal fiber behavior

Normally, if one fiber fails, the devices should detect it and bring the interface down.

That is the ideal behavior.

If both switches correctly detect the issue:

- the interfaces go down
- the link is fully removed
- no STP problem occurs

But sometimes the hardware problem is not detected correctly.

In that case:

- interfaces remain up/up
- one direction still works
- the other direction fails

That creates a unidirectional link.

---

## Why unidirectional links are dangerous for STP

STP depends on receiving BPDUs.

Suppose a port is a non-designated blocking port because it receives superior BPDUs from another switch.

If a unidirectional link prevents those BPDUs from arriving, the switch may think:

- "I am no longer receiving BPDUs"
- "maybe there is no longer a loop"
- "I should become designated and start forwarding"

If that happens while the other side still forwards normally, a Layer 2 loop may be created.

So the problem is not the unidirectional link itself.

The problem is:

- STP may misinterpret the loss of BPDUs
- the blocked port may incorrectly transition toward forwarding
- a loop can form

---

## Loop Guard purpose

Loop Guard protects against that scenario.

If a Loop Guard-enabled port unexpectedly stops receiving BPDUs, it does **not** become designated and move toward forwarding.

Instead, it enters a special blocked condition.

This condition is called:

- broken
- loop inconsistent

That prevents the port from forwarding and prevents a loop from forming.

---

## Loop Guard behavior

Without Loop Guard:

1. port stops receiving BPDUs
2. max age timer expires
3. port may become designated
4. port transitions toward forwarding
5. possible Layer 2 loop

With Loop Guard:

1. port stops receiving BPDUs
2. max age timer expires
3. port enters loop inconsistent state
4. port is effectively blocked
5. loop is prevented

---

## Loop inconsistent state

When Loop Guard is triggered, the port enters the loop inconsistent state.

Important points:

- the interface remains physically up/up
- STP blocks it
- it cannot forward normal traffic
- this is a protection state, not a shutdown

In CLI output, it may appear as:

- BKN = broken
- LOOP_Inc = loop inconsistent

---

## Recovery behavior

Loop Guard recovery is automatic.

If the problem is fixed and the port starts receiving BPDUs again:

- the loop inconsistent condition clears
- the port returns to normal STP behavior

No manual shutdown / no shutdown is required.

This is similar to Root Guard recovery.

---

## Where Loop Guard should be used

Loop Guard should typically be enabled on ports that are supposed to receive BPDUs.

That means:

- root ports
- non-designated ports

In other words:

- ports that depend on receiving superior BPDUs from another switch

If they unexpectedly stop receiving those BPDUs, Loop Guard prevents them from becoming forwarding ports incorrectly.

---

## Where Loop Guard should not be used

Loop Guard is not typically used on designated ports.

A designated port is the one sending BPDUs on a link, not primarily receiving superior ones.

Loop Guard is about protecting ports that should continue receiving BPDUs.

---

## Loop Guard configuration

There are two ways to enable Loop Guard.

### Per-interface

```bash
spanning-tree guard loop
```

This is configured in interface configuration mode.

### Globally by default

```bash
spanning-tree loopguard default
```

This enables Loop Guard on all ports by default.

If needed, you can disable it on a specific interface with:

```bash
spanning-tree guard none
```

---

## Verification

A useful verification command is:

```bash
show spanning-tree interface detail
```

This can show whether Loop Guard is enabled on a specific port.

If Loop Guard blocks a port, `show spanning-tree` output may display:

- BKN
- LOOP_Inc

These indicate that the port is blocked because of Loop Guard.

---

## Loop Guard vs Root Guard

Loop Guard and Root Guard are mutually exclusive.

That means:

- they cannot both be active on the same interface at the same time

Why?

Because they protect against opposite kinds of STP problems.

### Root Guard
Used to stop a designated port from becoming a root port.

It reacts to:
- superior BPDUs arriving unexpectedly

### Loop Guard
Used to stop a root port or non-designated port from becoming designated.

It reacts to:
- BPDUs stopping unexpectedly

So:

- Root Guard protects against "too good" BPDUs
- Loop Guard protects against "missing" BPDUs

---

## Which ports should use which feature

Example logic:

- ports expected to receive superior BPDUs from inside your STP topology:
  - use Loop Guard

- boundary ports facing external switches that must not become root:
  - use Root Guard

This is why the two features should not be mixed blindly.

---

## Configuration precedence

If Loop Guard is configured on an interface and then Root Guard is configured on the same interface:

- Root Guard replaces Loop Guard

If Root Guard is configured first and then Loop Guard is configured:

- Loop Guard replaces Root Guard

If Loop Guard is enabled globally and Root Guard is configured on a specific interface:

- Root Guard takes effect on that interface

General IOS rule:

- the more specific command overrides the less specific one

---

## Practical design rule

Loop Guard is useful as an additional line of defense in STP.

It is especially valuable on:

- non-designated ports
- root ports
- fiber-based switch-to-switch links
- environments where undetected one-way failures are possible

It should be considered a safety mechanism, not a replacement for good physical design and good cabling.

---

## Key points to remember

- Loop Guard protects against loops caused by lost BPDUs
- it is especially useful for unidirectional link scenarios
- a unidirectional link means traffic works only one way
- if a Loop Guard-enabled port stops receiving BPDUs, it enters loop inconsistent state
- the port is blocked but not shut down
- recovery is automatic once BPDUs return
- Loop Guard is usually used on root ports and non-designated ports
- Loop Guard and Root Guard cannot both be active on the same port

---

## Summary

Loop Guard is an STP protection feature that prevents a port from incorrectly moving into forwarding state when it unexpectedly stops receiving BPDUs.

Its main job is to stop Layer 2 loops that could result from failures such as unidirectional links.

Main idea:

- if a port that should be receiving BPDUs suddenly stops receiving them, do not trust that condition
- block the port instead of letting it forward

This makes Loop Guard an important extra safety feature in networks where STP stability matters.
