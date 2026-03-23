# 10 — IPv4 Header Structure

## Concepts studied

Today I studied the **structure of an IPv4 packet** and the different fields that compose the **IPv4 header**.  
The IPv4 header contains several fields used by routers and hosts to correctly deliver packets across the network.

A typical IPv4 header has a **minimum size of 20 bytes** but can be longer if options are used.

---

## IPv4 Packet Structure

An IPv4 packet is composed of two main parts:

- **Header**
- **Payload (Data)**

The header contains information required for routing and packet delivery, while the payload contains the transported data (for example ICMP, TCP, or UDP data).

---

## Fields of the IPv4 Header

### Version

Indicates the IP version used.

For IPv4 the value is:

4

---

### Internet Header Length (IHL)

Indicates the length of the IPv4 header.

- Field size: **4 bits**
- Minimum value: **5**
- Minimum header size: **20 bytes**

This value increases if optional fields are added to the header.

---

### DSCP / ECN

Used for **traffic prioritization and congestion notification**.

DSCP (Differentiated Services Code Point) allows network devices to apply **Quality of Service (QoS)** policies.

ECN (Explicit Congestion Notification) allows routers to signal congestion without dropping packets.

---

### Total Length

Specifies the **total size of the IPv4 packet**, including:

- header
- payload

Maximum possible value:

65535 bytes

---

### Identification

Used to identify fragments of the same packet when **fragmentation occurs**.

Each fragment of the same packet shares the same identification value.

---

### Flags

Used to control packet fragmentation.

Three flags exist:

- Reserved bit
- **Don't Fragment (DF)**
- **More Fragments (MF)**

These flags determine whether a packet can be fragmented.

---

### Fragment Offset

Indicates the **position of the fragment in the original packet**.

This allows the destination host to correctly **reassemble fragmented packets**.

---

### Time To Live (TTL)

TTL prevents packets from circulating indefinitely in the network.

Each router that processes the packet **decrements the TTL by 1**.

When TTL reaches **0**, the packet is discarded.

---

### Protocol

Indicates which **Layer 4 protocol** is encapsulated in the packet.

Examples:

- ICMP → 1
- TCP → 6
- UDP → 17

This field allows the receiving host to deliver the packet to the correct protocol.

---

### Header Checksum

Used to verify the **integrity of the IPv4 header**.

Routers recompute the checksum at each hop because the TTL value changes.

---

### Source Address

Contains the **IPv4 address of the sender**.

Example:

192.168.1.1

---

### Destination Address

Contains the **IPv4 address of the receiver**.

Example:

192.168.1.2

---

### Options (optional field)

Additional optional parameters can be included in the header.

Because options increase the header size, they are **rarely used in modern networks**.

---

## Packet analysis with Wireshark

Using **Wireshark**, it is possible to capture packets and inspect the IPv4 header fields.

For example when analyzing an ICMP ping packet we can observe:

- source IP
- destination IP
- header length
- TTL
- protocol
- total packet length

This allows us to understand how packets are structured and transmitted across the network.

---

## Topics covered

- IPv4 packet structure
- IPv4 header fields
- Header size and IHL
- Fragmentation fields
- TTL and protocol field
- Packet inspection with Wireshark
