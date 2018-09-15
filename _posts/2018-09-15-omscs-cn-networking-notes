---
layout: post
date: 2018-09-15
tags: omscs
feelings: neutral
title: omscs cn notes for routing
comments: true
description: class notes on routing
---

xpost to dev.to https://dev.to/swyx/networking-essentials-routing-5gb7
 
 

*This is the third in a series of class notes as I go through the [free Udacity Computer Networking Basics course](https://www.udacity.com/course/computer-networking--ud436).*

[As we established in the first article](https://dev.to/swyx/networking-essentials-architecture-and-principles-2g5e), the Internet is not one thing, it is made up of a decentralized group of networks (from ISPs like Comcast to search servers like Google to  content servers like universities). These are more formally known as Autonomous Systems. When you request content over the Internet, data flows over multiple ASes to get to you. More precisely, there is Intra-domain routing (within your AS) and Inter-domain routing (between ASes). Establishing domains is the function of Switches [as we discussed in the second article](https://dev.to/swyx/networking-essentials-switching-3eba).

## Intra-AS Topology

Nodes are known as **Points of Presence** and are near major population centers. Here's the AS known as the Abilene Network:

![https://cs.stanford.edu/people/eroberts/courses/soco/projects/2003-04/internet-2/images/domesticpeer.gif](https://cs.stanford.edu/people/eroberts/courses/soco/projects/2003-04/internet-2/images/domesticpeer.gif)

There are two types of intra-domain Routing algorithm.

**Distance Vector**

Based on the Bellman-Ford algorithm, routers will send copies of their own routing table ("vectors") to neighbors, and will compute costs to each destination based on shortest path. Since each shortest path of a neighbor is known, the router simply has to add its own routing table's cost to that of the neighbor through which it is trying to reach.

This is a very simple model and updates quickly when distance costs are low, but when failures occur (distance costs increase for whatever reason), bad news travels slowly. A sharp increase in costs cause neighbors to ping back and forth with each other increasing their estimate of lowest cost neighbors until they find a globally stable new equilibrium. This is the [count to infinity problem](http://www.ques10.com/p/3796/what-is-count-to-infinity-problem-in-distance-vect/).

The solution to that is [poison reverse](https://searchnetworking.techtarget.com/definition/poison-reverse). We establish asymmetry in the routing table by introducing an "Infinity" cost in links we definitely don't want to revisit, so that useless pingbacks are avoided.

The **[Routing Information Protocol](https://en.wikipedia.org/wiki/Routing_Information_Protocol)** is an implementation of this. The specific rules are:

- We will set a link to Infinity if it takes 16 or more hops. (intentionally small value) 
- Table refreshes occur every 30 seconds
- Every round has a timeout of 180 seconds
- **Split Horizon rule**: When an update occurs, send to all neighbors except for the source of the update

RIP is ultimately just a short circuiting of the Count to Infinity problem, which causes **slow convergence**; to really solve it, we'll have to explore a different paradigm.

**Link State**

The idea for Link State routing is that each node distributes the **network map** instead of the routing table. Each node computes its own shortest path using [Dijkstra](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm).

Mechanically this looks like:

- Add costs of immediate neighbors
- Flood costs to neighbors

The two implementations of this are:

- **Open Shortest Paths First (OSPF)**
- **Int. System-Int. System (IS-IS)** (more popular)

This is the prevailing routing algorithm today, but one problem with it is scale. The complexity of the protocol is O(n^3).

To cope with this scale problem, we introduce **hierarchy**. OSPF uses "areas" while IS-IS uses "levels".

As an example: OSPF has a backbone area known as **Area 0**, and every other area has a designated "Area 0 Router". The Area 0 routers do computation between themselves, and all other routers do computation between their area and their Area 0 Router. Basically this works on like how domains work in the networking topics we've already explored.

## Inter-AS topology

There are 10's of 1000's of ASes, so they need to tell each other they exist by sending **route advertisements** (or "announcements") using the **Border Gateway Protocol** (BGP).

In **External BGP**, a route advertisement includes a lot of data, but critically:

- **Destination prefix** (eg IP prefix for the university)
- **Next Hop IP** - the address of the next router that the origin router must send traffic to (typically the first router in the neighboring network - Doable because the border router of that network shares the same subnet as the border router of the origin network eg a [/30 subnet](https://www.petri.com/how-30-and-32-bit-ip-subnet-masks-can-help-with-cisco-networking))
- **AS Path** - a sequence of AS numbers that describe the route to the destination. The last number on the AS Path is known as the origin AS, which is the origin of the route advertisement.

Once inside an AS, **Internal BGP (iBGP)** is used to transmit routes *inside* an AS for **external** destinations. This is not to be confused with **Intra-domain routing Protocol (IGP)** which transmit routes *inside* an AS for **internal** destinations. 

To go from an origin inside AS1 to a destination in AS2:

- AS1 learns the route to AS2 with **eBGP**
- Router inside AS1 learns the route to AS2 with **iBGP**
- Origin router needs to use **IGP** to reach the **iBGP Next Hop** address inside AS1.

**BGP Route Selection**

Choosing between multiple routes to the same destination:

1. Prefer higher **local preference**
2. Shortest AS path length (fewer ASes to traverse the better)
3. Multi-Exit Discriminator (MED) - an AS tells you, of its multiple possible exits to your destination, which is preferred. MED values are not comparable between ASes.
4. Shortest IGP path - leads to "hot potato routing" where you basically want to spend as little time inside your own network as possible
5. Arbitrary Tiebreak based on stability or age or router with lowest router ID.

**Local Preference** is just a number a sysadmin can assign to a particular route. It's local so it isn't transmitted anywhere, but it allows sysadmins to explicitly state one route should be preferred over others. Useful for configuring primary and backup routes. Although local preferences are local, ASes can attach a **BGP Community** to their advertisements to that other ASes can configure their local prefs to work with them, based on prior agreement.

**Multi-Exit Discriminators** 

MED overrides "Hot Potato Routing" allowing an AS to explicitly specify that a neighboring AS carry the traffic *over its own network* rather than dumping the traffic at the closest egress and forcing traffic across the neighbor's backbone. This isn't a friendly move, this is a way to prevent an AS freeriding on another AS' backbone, for example when a transit provider peers with a content provider and doesn't want the content provider to get free transit.

## Interdomain Routing Business Models

**It's all about routing money.** There are two types of relationships:

- Customer-Provider: Money flows from customer to provider regardless of traffic flow direction
- Peering: An AS can exchange traffic with another free of charge (aka "settlement-free peering")

So ASes always prefer to:

1. route traffic through its **customer** because it makes money
2. route through **peers** because its free
3. last choice, route through its **provider** because it costs money

**Filtering/export**

Given that an AS learns a route from its neighbor, to whom should it re-advertise that route?

- A route from a customer should be advertised to everyone, because the AS makes money that way
- A route from a provider should only advertised to customers so you dont bleed money (eg dont have a route from a provider to another provider, you'd be a transit provider between the two providers and paying them both)

According to [Griffin, Shepherd and Wilfong's Safety paper](https://people.eecs.berkeley.edu/~istoica/classes/cs268/05/papers/GSW02.pdf), routing stability is guaranteed from these exact rules; without them, the route can oscillate indefinitely. Imagine 3 ASes with local preferences in a rock-paper-scissors setup (Varadhan Gorindam, Estrin (1996)). Business relationships like regional peering and paid peering can violate these conditions.

**More info on this topic can be found on this primer: [BGP Routing Policies in ISP networks](https://www.cs.princeton.edu/~jrex/papers/policies.pdf)**


## Next in our series

Hopefully this has been a good high level overview of why we need Switches and how they might work if we were to make our own Internet. I am planning more primers and would love your feedback and questions on:

- [Architecture and Principles](https://dev.to/swyx/networking-essentials-architecture-and-principles-2g5e)
- [Switching](https://dev.to/swyx/networking-essentials-switching-3eba)
- Routing
- Naming/Addressing/Forwarding
- DNS
- Congestion Control and Streaming
- Rate Limiting and Traffic Shaping
- Content Distribution
- Software Defined Networks
- Traffic Engineering
- Network Security
