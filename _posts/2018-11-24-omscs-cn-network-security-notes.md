---
layout: post
date: 2018-11-24
tags: omscs
feelings: neutral
title: OMSCS CN Network Security Notes
comments: true
description: more than i want to know on security and attacks.
---

---
title: Networking Essentials: Network Security
published: false
description: What is Traffic Engineering?
tags: Networking
---

*This is the eleventh and last in a series of class notes as I go through the [free Udacity Computer Networking Basics course](https://www.udacity.com/course/computer-networking--ud436).*

---

This is the third of a 3 part miniseries on Network Operations and Management.


- [Software Defined Networking](https://dev.to/swyx/networking-essentials-software-defined-networking-35n9)
- [Traffic Engineering](https://dev.to/swyx/networking-essentials-traffic-engineering-13c4)
- [Network Security](https://dev.to/swyx/networking-essentials-network-security-1fcp)


---

## Why Network Security?

There are a wide variety of attacks on various parts of the infrastructure we have covered in this series (*I will not link them here for brevity but you can scroll to the bottom for the relevant topics we covered*):

- **Routing (BGP)**: In 2010, [China accidentally hijacked 50,000 IP prefixes from 170 countries for 18 minutes](https://arstechnica.com/information-technology/2010/11/how-china-swallowed-15-of-net-traffic-for-18-minutes/). This highlights the BGP's vulnerability where any AS can advertise an IP prefix to a neighboring AS and they will take it at face value and rebroadcast it to the rest of the Internet.
- **Naming (DNS)**: A **Reflection Attack** is a type of Distributed Denial of Service - generating very large amounts of traffic targeted at a victim. **Phishing** is also a common attack attempting to get the user's personal information on a rogue website.

The Internet's design is **fundamentally insecure**. It was design for simplicity over security, and a host is "on by default" or reachable by any other host, so if you have an insecure host, it is wide open to attack. This was fine when the Internet was a small network of trusted networks, but is no longer fine now. Its decentralized nature also makes it hard to coordinate defense.

## Resource Exhaustion Attacks

As we learned in the first post, **packet switching** allows multiple hosts to share the same link using **statistical multiplexing**, making it easy to achieve high utilization, but also making it easy to overload the link. These attacks target the **Availability** of a system.

## Confidentiality and Authenticity Attacks

We also want our systems to provide **Confidentiality**, **Authenticity**, and **Integrity**, for example when we execute a banking transaction we want it to be private and to make sure the origin of the information really is our bank and that it hasn't been modified in-flight ([a Man In The Middle attack](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)).

## The Negative Impacts of Attacks

Compromising **Availability**, **Confidentiality**, **Authenticity**, and **Integrity** can lead to:

- Theft of confidential info
- Unauthorized use of resources
- False information
- Disruption of legitimate services

## Routing Security

Focusing on BGP and control plane security, this involves authentication of messages being advertised by the routing protocol:

- **Session authentication**: protects point-to-point communication between routers
- **Path authentication**: protects the AS path
- **Origin authentication**: protects the *origin* AS, guaranteeing that the origin AS that advertises a prefix is actually the owner of that prefix. (e.g. preventing route hijacking)

## Origin Authentication: Route Hijacking

For a successful MITM attack, all traffic headed for the legitimate destination needs to be routed to the attacker, while the attacker's original connection with the legitimate location remains intact.

This is done by **AS path poisoning** - for the attacker's route to the legitimate location to be preserved, it prepends the path hosts onto its own AS advertisements so that they ignore the attacker's messages in an effort to avoid a loop.

This makes the MITM successful, however the circuitous route might be spotted when running a `traceroute`. The MITM can be hidden even from that, since a `traceroute` just consists of "ICMP time exceeded messages" from when a packet reaches a TTL of 0 - which normally would be decremented by each router along the way. If the routers in the attacker's network *never decrement the TTL*... then no messages would be generated, and the attacker AS would not be visible in a `traceroute`.

## Session Authentication

The session between two ASes is a TCP session, so we just use [TCP's MD5 authentication option](https://tools.ietf.org/html/rfc2385). Every message exchanged in the connection not only contains the message, but also the [MD5 hash](https://en.wikipedia.org/wiki/MD5) of the message with a shared secret key (manually distributed "out of band" (aka offline) between the AS1 and AS2 operator).

Another way to guard connections is to send packets with a TTL < 254. Because most eBGP sessions are only a single hop, and an attacker would be remote, it is not possible for the recipient AS to accept packets from a remote attacker. This is called [the TTL Hack Defense](https://bird.network.cz/pipermail/bird-users/2014-April/004276.html).

## Origin and Path Authentication

**If you can still read this, this post is still not ready. Check back in a bit!**

## Next in our series

Hopefully this has been a good high level overview of how Network Security works, and this entire series has been a useful introduction to you in some way. As a reminder, here are all the topics we have covered:

- [Architecture and Principles](https://dev.to/swyx/networking-essentials-architecture-and-principles-2g5e)
- [Switching](https://dev.to/swyx/networking-essentials-switching-3eba)
- [Routing](https://dev.to/swyx/networking-essentials-routing-5gb7/)
- [Naming/Addressing/Forwarding](https://dev.to/swyx/networking-essentials-naming-addressing-and-forwarding-13kk)
- [DNS](https://dev.to/swyx/networking-essentials-dns-1dl7)
- [Congestion Control and Streaming](https://dev.to/swyx/networking-essentials-congestion-control-26n2)
- [Rate Limiting and Traffic Shaping](https://dev.to/swyx/networking-essentials-rate-limiting-and-traffic-shaping-43ii)
- [Content Distribution](https://dev.to/swyx/networking-essentials-content-distribution-jag)
- [Software Defined Networks](https://dev.to/swyx/networking-essentials-software-defined-networking-35n9)
- [Traffic Engineering](https://dev.to/swyx/networking-essentials-traffic-engineering-13c4)
- [Network Security](https://dev.to/swyx/networking-essentials-network-security-1fcp)
