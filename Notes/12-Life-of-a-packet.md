# 12-The Life of a Packet – Notes

## Overview

This part explains **the full path of a packet when it crosses multiple networks**.

Example studied:

PC1 → R1 → R2 → R4 → PC4

PC1 is in the network:

192.168.1.0/24

PC4 is in the network:

192.168.4.0/24

Since these two machines are in **different networks**, the packet must pass through several **routers**.

Each router will make a decision based on its **routing table**.

---

## Verifying the destination network

When PC1 wants to send a package to PC4:

IP Source:

192.168.1.1

IP Destination:

192.168.4.1

PC1 compares the destination with its network:

192.168.1.0/24

Since **192.168.4.1 does not belong to this network**, PC1 knows that it must send the packet to its **default gateway**.

Default gateway:

192.168.1.254 (R1)

---

## ARP to find the gateway MAC

PC1 knows the IP address of the gateway but **does not know its MAC address**.

It therefore uses **ARP (Address Resolution Protocol)**.)

PC1 sends **ARP Request**:

IP Source
192.168.1.1

IP Destination
192,168,1,254

MAC Destination
FFFF.FFFF.FFFF (broadcast)

This request is sent to **all devices on the network**.

---

## router ARP Reply

R 1 receives the request ARP.

Since the requested IP address matches his, he replies with an **ARP Reply**.

IP Source
192,168,1,254

MAC Source
YYYY

IP Destination
192.168.1.1

MAC Destination
1111

PC1 now knows the **MAC address of its default gateway**.

---

## Ethernet Encapsulation

PC1 can now send the package.

The IP packet is encapsulated in an **Ethernet frame**.

MAC Source
1111

MAC Destination
YYYY

Important:

The **IP packet does not change**.

IP Source
192.168.1.1

IP Destination
192.168.4.1

Only the header **Ethernet (Layer 2)** is changed.

---

## Routing Decision on R1

R1 receives the frame.

It:

1. Remove the Ethernet header
2. reads the destination IP address
3. consult its routing table

Command used:

show ip route

R1 finds a route to:

192.168.4.0/24

Next hop:

192.168.12.2 (R2)

R1 must therefore send the packet to **R2**.

---

## ARP between R1 and R2

R1 does not know the MAC address of R2.

It sends an **ARP Request**:

MAC Destination
FFFF.FFFF.FFFF

IP Destination
192.168.12.2

R2 responds with a **ARP Reply**:

MAC address
FCCC

R1 now knows the MAC of R2.

---

## New encapsulation to R2

R1 encapsulates the packet again.

MAC Source
BBBB

MAC Destination
FCCC

Again:

The **IP packet remains the same**.

IP Source
192.168.1.1

IP Destination
192.168.4.1

---

## Routing Decision on R2

R2 receives the frame.

It:

1. Remove the Ethernet header
2. consult its routing table

He finds a road to:

192.168.4.0/24

Next hop:

192.168.24.4 (R4)

R2 must therefore send the packet to **R4**.

---

## ARP between R2 and R4

R2 does not know the MAC address of R4.

It sends an **ARP Request**:

MAC Destination
FFFF.FFFF.FFFF

IP Destination
192.168.24.4

R4 responds with a **ARP Reply**:

MAC address
EEEE

R2 can now send the packet to R4.

---

## Transmission to R4

R2 encapsulates the packet.

MAC Source
DDDD

MAC Destination
EEEE

The IP packet always remains:

IP Source
192.168.1.1

IP Destination
192.168.4.1

---

## R4 and directly connected network

R4 receives the packet.

In its routing table:

192.168.4.0/24

This network is **directly connected**.

But R4 does not yet know the MAC address of PC4.

---

## ARP between R4 and PC4

R4 sends an **ARP Request**.

MAC Destination
FFFF.FFFF.FFFF

IP Destination
192.168.4.1

PC4 replies:

MAC address
4444

R4 now knows PC4 MAC address.

---

## Final Delivery

R4 encapsulates the packet to send it to PC4.

MAC Source
FFFF

MAC Destination
4444

The frame is sent to PC4.

PC4 finally receives the packet.

---

## What happens to the answer

If PC4 responds to PC1:

The process will be **the same in reverse**.

However:

Devices have already learned MAC addresses through ARP.

So **there will be no more ARP requests**.

Packages will be sent directly.

---

## Important takeaways

- The **IP addresses remain the same** throughout the journey.
- The **MAC addresses change with each hop (hop)**.)
- Each router does:
- de-encapsulation
- consultation of the routing table
- re-encapsulation
- **switches do not modify packets**, they only transfer frames.
- The process is called **hop-by-hop forwarding**.
