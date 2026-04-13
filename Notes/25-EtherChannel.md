# 25-EtherChannel – Notes

## Overview

EtherChannel allows multiple physical interfaces to be grouped into one logical interface.

This logical interface behaves like a single link, while still using multiple member links underneath.

Main benefits:

- increased bandwidth
- link redundancy
- better use of available switch ports
- avoids STP blocking multiple parallel links

This topic is important for CCNA, especially Layer 2 and Layer 3 EtherChannel with LACP.

---

## Why EtherChannel is needed

If you connect two switches with multiple separate Layer 2 links, STP sees those links as redundant.

Result:

- only one link forwards traffic
- other links are blocked
- bandwidth is wasted

Example:

If four links exist between two switches, STP may allow only one link to forward and block the other three.

That means:

- redundancy exists
- but extra bandwidth is not actually used

EtherChannel solves this by grouping those links into one logical interface.

STP then sees one logical link instead of several separate links.

---

## EtherChannel concept

An EtherChannel is a bundle of physical interfaces that acts as one logical interface.

Other names:

- Port-Channel
- LAG (Link Aggregation Group)

Important behavior:

- STP treats the EtherChannel as one interface
- all member links can forward traffic
- no Layer 2 loop is created just because multiple member links exist

So EtherChannel provides:

- redundancy
- increased usable bandwidth
- loop-free operation

---

## Broadcast behavior with EtherChannel

Even if four physical links are bundled, the EtherChannel behaves like one logical interface.

So if a switch needs to send one broadcast frame through the EtherChannel:

- it sends one logical frame through the Port-Channel
- the other switch receives one logical copy
- it does not receive four separate broadcast copies

This is why Layer 2 loops are not created inside the EtherChannel bundle.

---

## Traffic distribution

EtherChannel does not send one single flow across all links randomly.

It load-balances traffic across member interfaces based on flows.

A flow is a conversation between two endpoints.

Examples:

- PC1 to Server1
- PC1 to Printer1
- PC2 to Printer1

Each flow is mapped to one physical link inside the EtherChannel.

Important:

- all frames in the same flow use the same physical interface
- this avoids out-of-order frame delivery

Different flows can use different member links.

That is how EtherChannel increases bandwidth usage while maintaining traffic consistency.

---

## Load-balancing inputs

The switch uses a hashing algorithm to decide which member link is used.

Possible inputs include:

- source MAC address
- destination MAC address
- source + destination MAC
- source IP address
- destination IP address
- source + destination IP

Some switches can also use Layer 4 information, such as TCP/UDP port numbers.

The exact options depend on the switch model.

---

## Default load balancing

A switch can use different load-balancing methods depending on platform.

Example default:

- source + destination IP for IP traffic
- source + destination MAC for non-IP traffic

This means:

- IP packets are usually balanced based on IP addresses
- non-IP frames are balanced based on MAC addresses

---

## Load-balancing verification

Command:

```bash
show etherchannel load-balance
```

This displays the current hashing method used by the switch.

---

## Load-balancing configuration

Command:

```bash
port-channel load-balance METHOD
```

Examples of methods:

- src-mac
- dst-mac
- src-dst-mac
- src-ip
- dst-ip
- src-dst-ip

Important detail:

- the show command uses the keyword `etherchannel`
- the config command uses the keyword `port-channel`

Same topic, different Cisco terms.

---

## EtherChannel negotiation methods

There are three ways to build an EtherChannel:

1. PAgP
2. LACP
3. Static EtherChannel

---

## 1. PAgP

PAgP = Port Aggregation Protocol

Characteristics:

- Cisco proprietary
- dynamically negotiates EtherChannel formation
- works only with Cisco devices

Modes:

- desirable
- auto

Behavior:

- desirable actively tries to form the EtherChannel
- auto is passive

Valid combinations:

- desirable + desirable = works
- desirable + auto = works
- auto + auto = does not work

---

## 2. LACP

LACP = Link Aggregation Control Protocol

Characteristics:

- IEEE standard (802.3ad)
- dynamically negotiates EtherChannel formation
- works with non-Cisco devices too
- preferred method for CCNA and real multi-vendor environments

Modes:

- active
- passive

Behavior:

- active actively tries to form the EtherChannel
- passive waits

Valid combinations:

- active + active = works
- active + passive = works
- passive + passive = does not work

---

## 3. Static EtherChannel

Static EtherChannel uses no negotiation protocol.

Mode:

- on

Behavior:

- forces interfaces to form an EtherChannel without negotiation

Valid combination:

- on + on = works

Invalid combinations:

- on + active = does not work
- on + passive = does not work
- on + desirable = does not work
- on + auto = does not work

Static mode is generally less preferred because dynamic protocols help detect problems.

---

## EtherChannel member limits

Up to 8 interfaces can actively participate in one EtherChannel.

LACP can support up to 16 total interfaces:

- 8 active
- 8 standby

Standby links wait for an active member to fail.

---

## Creating an EtherChannel

Basic command on member interfaces:

```bash
channel-group NUMBER mode MODE
```

Examples:

```bash
channel-group 1 mode desirable
channel-group 1 mode active
channel-group 1 mode on
```

This command creates or joins a logical Port-Channel interface.

---

## Channel-group number

The channel-group number identifies the Port-Channel interface locally.

Important:

- member interfaces on the same switch must use the same channel-group number
- the number does not have to match on the neighbor switch

Example:

- Switch A can use channel-group 1
- Switch B can use channel-group 2

That is still fine, as long as the negotiation/protocol settings match.

---

## Channel protocol command

Optional command:

```bash
channel-protocol pagp
channel-protocol lacp
```

This manually sets the negotiation protocol.

Usually not necessary, because the `channel-group ... mode ...` command already implies the protocol.

Examples:

- desirable / auto imply PAgP
- active / passive imply LACP

Still, it is useful to know for exam questions.

---

## Layer 2 EtherChannel

A Layer 2 EtherChannel is made of switchports.

Typical process:

1. select member interfaces
2. configure them in the same way
3. assign them to the channel-group
4. configure the Port-Channel interface as access or trunk

Example:

```bash
interface range g0/0 - 3
channel-group 1 mode active
```

Then configure the logical interface:

```bash
interface port-channel 1
switchport mode trunk
```

STP sees only the Port-Channel, not the individual physical links.

---

## Trunking with EtherChannel

If the EtherChannel is Layer 2 and used as a trunk:

- trunking is configured on the Port-Channel interface
- the settings are reflected on the member interfaces

Example settings that must match:

- trunk mode
- allowed VLANs
- native VLAN

Verification command:

```bash
show interfaces trunk
```

Only the Port-Channel appears as the trunk, not the individual member links.

---

## Layer 3 EtherChannel

A Layer 3 EtherChannel is made of routed ports instead of switchports.

Typical process:

1. make physical interfaces routed ports with `no switchport`
2. assign them to a channel-group
3. configure the logical Port-Channel interface with an IP address

Example:

```bash
interface range g0/0 - 3
no switchport
channel-group 1 mode active
```

Then configure:

```bash
interface port-channel 1
no switchport
ip address 192.168.1.1 255.255.255.252
```

---

## Why Layer 3 EtherChannel matters

Layer 3 EtherChannel is useful because:

- no STP runs on routed links
- no Layer 2 loop problem on those links
- bandwidth can still be increased with multiple physical links

This is common in modern network design where switch-to-switch links often use Layer 3 rather than Layer 2.

---

## EtherChannel and STP

For Layer 2 EtherChannel:

- STP sees the Port-Channel as one logical link
- STP no longer blocks individual member links inside the bundle

However:

- STP still matters between different Port-Channels in a Layer 2 topology
- EtherChannel does not eliminate Layer 2 loops everywhere
- it only prevents STP from blocking parallel links inside one bundle

For Layer 3 EtherChannel:

- STP is not involved on that routed Port-Channel

---

## Configuration consistency requirements

Member interfaces must match in key settings, or the interface can be excluded from the EtherChannel.

Important parameters that must match include:

- speed
- duplex
- switchport mode (access or trunk)
- trunk allowed VLANs
- trunk native VLAN

For Layer 3 EtherChannel, member interfaces should all be routed ports.

If one member does not match, it may be suspended or excluded.

---

## Verification commands

### Main verification command

```bash
show etherchannel summary
```

This is the most useful EtherChannel verification command.

It shows:

- Port-Channel number
- Layer 2 or Layer 3 status
- whether the Port-Channel is in use
- member interface states

---

## Important flags in `show etherchannel summary`

### Port-Channel flags

- `S` = Layer 2 EtherChannel (switchport)
- `R` = Layer 3 EtherChannel (routed)
- `U` = in use
- `D` = down

### Member port flags

- `P` = bundled in port-channel
- `s` = suspended

The ideal member interface flag is usually:

- `P`

That means the interface is successfully bundled.

---

## Example meanings

### `SU`
- Layer 2 EtherChannel
- in use

### `RU`
- Layer 3 EtherChannel
- in use

### `P`
- physical member is properly bundled

### `D`
- down

### `s`
- suspended, often due to mismatched configuration

---

## Additional verification command

```bash
show etherchannel port-channel
```

This gives more detail about Port-Channel interfaces, including protocol and mode information.

Still, the most commonly used command is:

```bash
show etherchannel summary
```

---

## EtherChannel and running configuration

When you configure the Port-Channel interface:

- some configuration is reflected on the member interfaces automatically

For example, if you configure the Port-Channel as a trunk, the member interfaces show matching trunk behavior.

This is why configuration is usually done on the logical interface after the bundle is formed.

---

## Key commands to remember

### Check load balancing

```bash
show etherchannel load-balance
```

### Change load balancing

```bash
port-channel load-balance METHOD
```

### Create/join EtherChannel

```bash
channel-group NUMBER mode MODE
```

### Manually set negotiation protocol

```bash
channel-protocol lacp
channel-protocol pagp
```

### Main verification

```bash
show etherchannel summary
```

### More detailed verification

```bash
show etherchannel port-channel
```

---

## Key concepts to remember

- EtherChannel groups multiple physical links into one logical interface
- STP treats a Layer 2 EtherChannel as one link
- EtherChannel improves bandwidth and redundancy
- traffic is load-balanced by flow, not per frame randomly
- all frames in the same flow use the same physical member link
- PAgP is Cisco proprietary
- LACP is the preferred industry standard
- static mode uses `on`
- member links must have matching configurations
- Layer 2 EtherChannel uses switchports
- Layer 3 EtherChannel uses routed ports and IP addressing on the Port-Channel

---

## Summary

EtherChannel solves a major problem in switched networks:

- parallel links normally get blocked by STP
- EtherChannel allows those links to be used together as one logical connection

Main benefits:

- higher bandwidth
- redundancy
- efficient use of links
- support for both Layer 2 and Layer 3 designs

For CCNA, the most important practical focus is:

- understanding EtherChannel purpose
- configuring Layer 2 and Layer 3 EtherChannels
- especially using LACP
- verifying them with `show etherchannel summary`
