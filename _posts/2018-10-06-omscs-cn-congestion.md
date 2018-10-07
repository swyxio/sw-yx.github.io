---
layout: post
date: 2018-10-06
tags: omscs
feelings: neutral
title: omscs cn: congestion notes
comments: true
description: Bottlenecks inevitably arise in networks. How do we deal with them in TCP? How about in practical streaming applications like Youtube and Skype?
---

*This is the sixth in a series of class notes as I go through the [free Udacity Computer Networking Basics course](https://www.udacity.com/course/computer-networking--ud436).*

## Congestion Collapse 💩

Say we have a central switch S that is linked to a destination D with a connection capacity of 1 Mbps. However, we have two source hosts H1 and H2 connecting through L trying to send data to D at rates of 10 Mbps and 100 Mbps respectively. 

The exact numbers don't matter, but the point is H1 and H2 are trying to send data at a much higher rate than the S-D link can take it. The S-D link is a bottleneck.

By [Internet architecture design](https://dev.to/swyx/networking-essentials-architecture-and-principles-2g5e), the source hosts H1 and H2 are unaware of each other, and of the current state of the S-D link. As a result, they very inefficiently compete for resources on the bottleneck and this can be lead to lost packets and long delays, to the point where the S-D link isn't even used to its full capacity because of all the failures. This is called **congestion collapse**.

![https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSMCpIHEbLysGQSMdsWp9qRPhq45l8l2QPu_KkI_Zzyk-o6jD5f](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSMCpIHEbLysGQSMdsWp9qRPhq45l8l2QPu_KkI_Zzyk-o6jD5f)

**Congestion Collapse** is defined where an increase in load leads to a decrease in useful work. It can be caused by:

- spurious retransmission (of packets, due to how TCP works when packets are dropped and ACKs aren't received)
    - solution: better timers and TCP congestion control
- undelivered packets (packets consume resources but are dropped elsewhere in the network)
    - apply congestion control to ALL traffic

## Goals of Congestion Control 👼

So given the realities of different bandwidths in our network, we want to:

- Use network resources **efficiently**
- Preserve **fair** allocation of resources
- avoid congestion collapse

You can chart Fairness and Efficiency between two hosts with a phase plot:

![https://i.ytimg.com/vi/EfeAglStSd4/maxresdefault.jpg](https://i.ytimg.com/vi/EfeAglStSd4/maxresdefault.jpg)

You can accomplish these goals with **network-assisted congestion control** where routers provide feedback with a bit to indicate congestion (eg the [Explicit Congestion Notification](https://en.wikipedia.org/wiki/Explicit_Congestion_Notification) extension in TCP) but more commonly the congestion control is implicit, or **end-to-end**, where the network does not give feedback and congestion is inferred by looking at packet loss and delays.

## TCP Congestion Control

In TCP Congestion Control, senders slowly increase their rate until packets are dropped. Packet loss is interpreted as congestion and the rate is slowed down. The rate is increased again periodically to retest the network in case the congestion was temporary (eg due to a maxed out buffer in a router).

The increase/decrease algorithm can work in two ways.

- **Window-based** algorithms have a set number of packets out at any given time. New packets are sent when ACKs from old packets are received. So if extant packets drop, new packets don't get sent, and the effective send rate decreases, and vice versa. So the **window size** is a natural control. The *increase and decrease* of window size is a different matter: when a packet is successfully sent, the window size "additively" increases by one. When a packet fails, the window size is cut "multiplicatively" in half. This asymmetric increase/decrease algorithm is known as [Additive Increase, Multiplicative Decrease (AIMD)](https://en.wikipedia.org/wiki/Additive_increase/multiplicative_decrease).
- **Rate-based** alogithms monitor their loss rate instead, and use a timer to modulate their rate. This is less common.

On a phase plot, AIMD moves rates up parallel to the X1 = X2 "fair" line, and down according to its angle to the origin:

![http://omsnotes.com/6250/images/fig8.14.png](http://omsnotes.com/6250/images/fig8.14.png)

In terms of individual rate over time, AIMD also results in a "sawtooth" plot:

![http://omsnotes.com/6250/images/fig8.15.png](http://omsnotes.com/6250/images/fig8.15.png)

A TCP sender sends at an average rate of 3/4 of it's window size due to AIMD. 

To calculate **throughput**, you take the average rate divided by the RTT, or `3/4 * Wm/RTT`.

Also because of AIMD, the time between the troughs is given by `Wm/2 + 1` (the `Wm/2` being the time taken to increase the height of half the window 1 at a time, and the `1` being the time it takes to decrease it in half). 

To calculate **the loss rate**, Compare the area under the sawtooth (successful sends) to the total packets sent, i.e. `1/2 * Wm/2 * (Wm/2 + 1)` or basically `Wm^2 / 8`. 

**Throughput and LossRate** are related by `Throughput ~ k/(RTT * sqrt(Lossrate))`.

## The TCP Incast Problem

There is a special congestion problem that specifically happens in the conditions of data centers. **Incast** is a drastic reduction in application throughput that results when servers using TCP all simultaneously request data (sometimes with [Barrier Synchronization](https://docs.oracle.com/cd/E19120-01/open.solaris/816-5137/gfwek/index.html), leading to a gross underutilization of network capacity in many-to-one communication networks like a datacenter. This is due to:
    - Requirement of high bandwidth and low latency
    - Many parallel requests coming from servers
    - High "fan in" in a data center
    - Small buffers in switches
    - Filled up switch buffers result in bursty retransmissions cause by TCP timeouts that last hundreds of milliseconds
    - But (as above) the requirement of low latency typically on the order of <1ms.

Two solutions are:

- Fine grained TCP retransmission timers on the order of microseconds instead of milliseconds
- Clients acknowledging every other packet instead of every packet, reducing network load

Bottom line: Timers should operate on a granularity close to the RTT of the network, and in a Data Center that is <1ms.

## Multimedia and Streaming

The next half of this piece focuses on dealing with the unique problems of sending multimedia and streaming. Some characteristics of these situations are:

- Large volume of data (eg video with sound)
- Data volume varies over time (depending on compression algorithm)
- Low tolerance for delay variation
- Low tolerance for delay

The most important of these is delay variation. We can establish a clientside buffer to manage some variation and variance but delay variations will threaten the smooth playback especially if the buffer is starved.

TCP is not a good fit for streaming. It is meant to:

- deliver reliably - but the client may not care about that
- slow down upon loss - this leads to delay variation
- 20 byte header for every packet - significant overhead

[UDP](https://en.wikipedia.org/wiki/User_Datagram_Protocol) is better:

- no retransmission
- no sending rate adaptation
- small header

## Case studies

**Youtube (streaming video)**

All videos are converted to Flash, and every browser can play it with HTTP/TCP. Even though TCP isn't a good fit for streaming, the creators of Youtube preferred to keep it simple so it can leverage existing HTTP/TCP infrastructure like CDNs.

**Skype/VOIP**

Skype has a central login server but uses P2P to exchange data. It compresses to a fairly low bitrate (67 bytes/pkt, 140 pkts/sec or 40kbps), encrypted both ways. But it is still very susceptible to drops in network quality.

## Quality of Service

We can use explicit reservations, or marking and policing packet streams as higher priorities.

If we have a VOIP app and an FTP app both sharing the same bandwidth, we want to make sure the VOIP has a higher priority. We can mark the audio packets as they arrive at the router with a priority queue, and then use scheduling (eg [weighted fair queueing](https://en.wikipedia.org/wiki/Weighted_fair_queueing) where you simply serve the high-pri queue more often than the low-pri queue) 


## Next in our series

Hopefully this has been a good high level overview of the Congestion control and some of its practical implementations/applications. I am planning more primers and would love your feedback and questions on:

- [Architecture and Principles](https://dev.to/swyx/networking-essentials-architecture-and-principles-2g5e)
- [Switching](https://dev.to/swyx/networking-essentials-switching-3eba)
- [Routing](https://dev.to/swyx/networking-essentials-routing-5gb7/)
- [Naming/Addressing/Forwarding](https://dev.to/swyx/networking-essentials-naming-addressing-and-forwarding-13kk)
- [DNS](https://dev.to/swyx/networking-essentials-dns-1dl7)
- Congestion Control and Streaming
- Rate Limiting and Traffic Shaping
- Content Distribution
- Software Defined Networks
- Traffic Engineering
- Network Security
