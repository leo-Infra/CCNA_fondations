# 11-Routing Fundamentals – Notes

## Routing table

A router stores destination information it knows in a **routing table**.

When a package arrives:
1. The router looks at the destination IP address.
2. It looks for a corresponding route in the table.
3. It sends the package to the best available route.

Each entry in the routing table indicates **where to send the packet** to reach a network.

---

## Important road types

### Connected route (C)

A **connected route** is automatically created when an IP address is configured on an interface.

Example:

IP interface
192.168.1.1/24

Automatically created route:

192.168.1.0/24

This means that the router is directly connected to this network.

If a packet is to reach a machine on this network, the router sends it directly via the corresponding interface.

---

### Local route (L)

A **local route** is the exact IP address of the router**.

Example:

IP interface
192.168.1.1/24

Local road:

192.168.1.1/32

The mask '/32' means **a single IP address**.

If a packet is intended for this address, it is **received by the router itself**.

---

## Correspondence of a route

A route is a packet if the destination IP address belongs to the route network.

Example:

Road:
192.168.1.0/24

Corresponding addresses:
192.168.1.10
192,168,1,60
192,168,1,200

But **192.168.0.10 does not correspond** to this route.

---

## Route Selection

If several routes correspond to a destination, the router chooses **the most specific route**.

The most specific route is the one with **the longest prefix**.

Example:

192.168.1.0/24
192.168.1.1/32

If a package is intended to:

192.168.1.1

The router will use:

192.168.1.1/32

because this route corresponds **exactly to the IP address**.

---

## If no route matches

If the router cannot find a route to the destination:

package is **drop (deleted)**.)

Unlike switches, routers **do not flood the network**.

---

## Important order

To display the routing table on a Cisco router:

show ip route

Use this command to see:

- known networks
- connected roads
- local roads
- the routing protocols used

---

## Important takeaways

- A router decides where to send a packet thanks to the **routing table**.
- Configure an IP on an interface automatically creates **a connected route and a local route**.
- The router always chooses **the most specific route**.
- Without a corresponding route, the package is **deleted**.
