---
layout: post
date: 2018-09-25
tags: omscs
feelings: stressed tired
title: omscs cn notes router design
comments: true
description: throwaway notes on router design
---

# Router design

*This is the fifth in a series of class notes as I go through the [free Udacity Computer Networking Basics course](https://www.udacity.com/course/computer-networking--ud436).*

Let's make a router!

## Basic Router Architecture

Here is the basic job of a Router:

1. Receive packet
2. Look at header to determine destination
3. **Look up** forwarding table to determine output interface
4. Modify header (e.g. Decrementing TTL - Time to Live field and updating header checksum)
5. Send packet to output interface

All this is done through a single **Line Card**, each of which has: a **lookup table**, ability to modify headers, and a buffer for packets coming in and out of the card. A router connects a bunch of **Line Cards** via an **Interconnection Fabric**. 

Two things of note:

- **Why One Table Per Card?** Each card has its own copy of the lookup table. We used to do the alternative which is a central table for the whole router, but it was a bottleneck as each card would have to communicate with the central table across a shared bus.
- **On the Interconnection Fabric**: We use a crossbar switch (aka switched backplane) to let nonconflicting input-output pairs to send data at the same time instead of a shared bus (which is limited to one input/output at a time). Every input port has a connection to every output port, which enables parallelism but requires some scheduling algorithm to ensure fair use.

## Maximal Matching in Crossbar Switching Algorithm

Maximal matching between N inputs and N outputs means:

- try to allocate a one-to-one input to output as far as possible
- remaining demands get put in a queue
- Many crossbars have a "speedup" where the interconnect is run 2x as fast as the line cards to allow 2x faster matching. This is common practice, however head of line blocking still can't be solved by speedups.

**Head of line blocking** is where there are a lot of packets destined for the same output ahead of another packet that ought to be sent to a different output (and therefore shouldn't wait for the others to clear). The solution is **Virtual Output Queues** where we establish one queue per output port.

## Scheduling and Fairness

Crossbar switching involves **scheduling** - the two goals here are:

- **Efficiency**: if there is available capacity to send traffic from input to output, it should be used up.
- **Fairness**: Each queue at each input should be scheduled "fairly" - typically an idea called **max-min fairness** which is like Pareto optimality where you can't make something better off without making something else worse off.

Max-min fairness can be achieved by:

- Round Robin Scheduling (although packets may have different sizes, and thus be unfair)
- Bit by bit scheduling (which can be impractical, how do you break packets up into bits)
- "Fair queueing" (compute "virtual finishing time" of each packet and serves the one with the minimal finishing time)

## Next in our series

Hopefully this has been a good high level overview of Routing protocols and BGP. I am planning more primers and would love your feedback and questions on:

- [Architecture and Principles](https://dev.to/swyx/networking-essentials-architecture-and-principles-2g5e)
- [Switching](https://dev.to/swyx/networking-essentials-switching-3eba)
- [Routing](https://dev.to/swyx/networking-essentials-routing-5gb7/)
- [Naming/Addressing/Forwarding](https://dev.to/swyx/networking-essentials-naming-addressing-and-forwarding-13kk)
- Router Design
- DNS
- Congestion Control and Streaming
- Rate Limiting and Traffic Shaping
- Content Distribution
- Software Defined Networks
- Traffic Engineering
- Network Security
