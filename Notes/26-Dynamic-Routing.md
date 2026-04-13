# 26-Dynamic Routing – Notes

## Overview

Dynamic routing allows routers to automatically learn and update routes.

Unlike static routing:

- no need to manually configure every route
- routers adapt automatically to network changes
- better scalability for real networks

Key idea:

- routers exchange routing information
- routers build routing tables dynamically
- routers choose the best path automatically

---

## Static vs Dynamic Routing

### Static Routing

- manually configured with `ip route`
- does not adapt to failures automatically
- good for small/simple networks

Problem:

- if a link fails → route stays in table → traffic is lost

---

### Dynamic Routing

- routers exchange routes automatically
- routers detect failures and update routes
- routers can switch to backup paths

Example behavior:

- router learns a network from neighbor
- adds it to routing table
- if path fails → route is removed or replaced

---

## Key Concepts

### Advertisement

Routers share routes with neighbors.

Example:

- R4 advertises network to R2
- R2 advertises to R1
- R1 adds route to routing table

---

### Adjacency (Neighbor relationship)

Routers must form a relationship before exchanging routes.

- only directly connected routers form adjacency
- routing info is exchanged between neighbors

---

### Routing Table

Contains:

- best routes only (not all possible routes)
- one or multiple routes per destination (if equal)

---

## Network Route vs Host Route

### Network Route

- route to a subnet
- prefix < /32

Example:

- 192.168.1.0/24

---

### Host Route

- route to a single IP
- prefix = /32

Example:

- 192.168.1.10/32

---

## Types of Routing Protocols

### 1. IGP (Interior Gateway Protocol)

- used inside a company/network
- within one Autonomous System (AS)

Examples:

- RIP
- EIGRP
- OSPF
- IS-IS

---

### 2. EGP (Exterior Gateway Protocol)

- used between different organizations
- between Autonomous Systems

Example:

- BGP (only modern EGP)

---

## Autonomous System (AS)

- a network under one administrative control
- example: a company network or ISP

Inside AS → IGP  
Between AS → EGP  

---

## Routing Algorithm Types

### Distance Vector

Protocols:

- RIP
- EIGRP

Concept:

- "routing by rumor"
- router only knows what neighbors tell it

Router learns:

- destination network
- distance (metric)
- next hop (direction)

No full network view.

---

### Link State

Protocols:

- OSPF
- IS-IS

Concept:

- routers build full network map
- each router calculates best path independently

Characteristics:

- more accurate
- faster convergence
- higher CPU and memory usage

---

### Path Vector

Protocol:

- BGP

Used:

- between Autonomous Systems

Not required in-depth for CCNA.

---

## Metric (Route Cost)

Metric = value used to choose best route  
Lower metric = better route  

Important:

- metric is only compared inside the same protocol

---

## Metrics by Protocol

### RIP

- metric = hop count
- each router = 1 hop

Problem:

- ignores bandwidth
- slow link = same as fast link

---

### EIGRP

- metric = bandwidth + delay
- only slowest link bandwidth is considered
- delay is cumulative

More accurate but complex.

---

### OSPF

- metric = cost
- based on bandwidth

Better than RIP because it considers speed.

---

### IS-IS

- metric = cost
- default = fixed value (often 10)

Without tuning → behaves like hop count.

---

## Equal Cost MultiPath (ECMP)

If:

- same protocol
- same destination
- same metric

Then:

- multiple routes are added
- traffic is load-balanced

Example:

- 2 equal paths → both used

---

## Dynamic Routing Behavior Example

If 2 routes exist:

- via R2 → fast link
- via R3 → slow link

### With RIP:

- both = same hop count
- both used (ECMP)

### With OSPF:

- fast link = lower cost
- only best route used

---

## Administrative Distance (AD)

Used when routes come from different protocols.

Key idea:

- metric compares routes inside same protocol
- AD compares between different protocols

Lower AD = more trusted

---

## Default Administrative Distance Values

Must memorize for CCNA:

- Connected → 0
- Static → 1
- eBGP → 20
- EIGRP → 90
- OSPF → 110
- IS-IS → 115
- RIP → 120
- iBGP → 200
- Unusable → 255

---

## Important Rule

### Route Selection Order

1. Longest prefix match
2. Lowest AD
3. Lowest metric

---

## Example (Important)

If router learns:

- RIP route (metric 3)
- OSPF route (metric 10)

Result:

- OSPF is chosen (lower AD)

Metric ignored because protocols differ.

---

## Longest Prefix Match (CRITICAL)

Router always chooses:

- most specific route

Example:

Routes:

- 10.0.0.0/22
- 10.0.0.0/24
- 10.0.0.0/28

Destination:

- 10.0.0.14

Chosen route:

- /28 (most specific)

AD and metric are ignored here.

---

## Static Routes and AD

Default:

- static route AD = 1

---

## Floating Static Route

A static route with higher AD.

Purpose:

- backup route

Example:

```bash
ip route 192.168.1.0 255.255.255.0 10.0.0.1 100
```

Behavior:

- not used normally
- activated only if dynamic route disappears

---

## Why Dynamic Routing is Better

- automatic route learning
- automatic failover
- scalable
- less manual configuration

---

## Summary

Dynamic routing allows routers to:

- exchange routing information
- learn routes automatically
- adapt to network changes

Key concepts:

- IGP vs EGP
- Distance vector vs link state
- Metric = best path inside protocol
- AD = best protocol selection
- ECMP = load balancing
- Longest prefix match = highest priority rule

---

## Mental Model (Important)

When a router chooses a route:

1. Check longest prefix match
2. If multiple → compare AD
3. If same protocol → compare metric
4. If same metric → ECMP

---

## What to Master for CCNA

- difference static vs dynamic routing
- IGP vs EGP
- distance vector vs link state
- metrics (basic idea)
- administrative distance values
- ECMP concept
- longest prefix match (VERY IMPORTANT)
- floating static routes

---

## Final Insight

Think of routing like decision layers:

- specificity first (prefix)
- trust second (AD)
- performance third (metric)

If you understand this hierarchy → you understand routing.
