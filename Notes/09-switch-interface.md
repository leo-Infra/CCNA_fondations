# 09 — Switch Interfaces

## Concepts studied

Today I studied how to analyze and configure interfaces on Cisco switches.

A common command used to quickly check interface status is:

show ip interface brief

This command displays:
- interface name
- IP address
- interface status
- protocol status

This allows quick verification of whether an interface is **up/up**, **down/down**, or **administratively down**.

---

## Router vs Switch interfaces

Router interfaces are **shutdown by default**.

They appear in the state:

administratively down

They must be enabled manually using:

no shutdown

---

Switch interfaces are **not shutdown by default**.

Their states depend on the cable connection:

- **up/up** → cable connected to another device
- **down/down** → no cable connected

---

## Interface configuration

An interface can be configured with several parameters.

Example:
interface f0/1
speed 100
duplex full
description Link to R1

Common parameters:
- **speed** → sets interface speed
- **duplex** → sets duplex mode
- **description** → documents the interface purpose

---

## Disabling unused ports

Unused ports should be disabled for security reasons.

Example:
interface range f0/5-12
description not in use
shutdown

This prevents unauthorized devices from connecting to the network.

---

## Duplex modes

### Full Duplex
Devices can **send and receive simultaneously**.

Advantages:
- no collisions
- higher performance

---

### Half Duplex
Devices **cannot send and receive at the same time**.

Collisions may occur.

---

## CSMA/CD

CSMA/CD stands for:

Carrier Sense Multiple Access with Collision Detection

Process:

1. the device listens before transmitting
2. if the medium is free → transmission begins
3. if a collision occurs → a jamming signal is sent
4. devices wait a random time before retransmitting

This mechanism was used in **half-duplex Ethernet networks**.

---

## Interface errors

The command used to inspect interface statistics is:

show interfaces

Important counters include:

### Runts
Frames smaller than **64 bytes**.

### Giants
Frames larger than the **maximum Ethernet frame size**.

### CRC errors
Frames that failed the **CRC integrity check**.

Possible causes:
- faulty cable
- interference
- duplex mismatch

### Output errors
Frames the switch attempted to send but failed.

---

## Topics covered

- Interface speed and duplex
- Autonegotiation
- Interface status
- Interface counters and errors
