---
layout: post
date: 2018-08-03
tags: react
feelings: neutral
title: why react is not reactive (fsa version)
comments: true
description: an essay form
---

# Recapping Reactive React

## A reintroduction to Functional Reactive Programming
## Your UI as a stream
## Building a basic Reactive Component Class and reconciler
## Demonstrating usage to solve race conditions and other event streams

---


# Problems with Reactive React

## Poor interoperability with imperative libraries
### example with d3js
## Lack of control over scheduling
### too easy for the user to accidentally lock up the screen
### async rendering eg time slicing work would be all in userland

---

# A "pull" based Reactive React

## the Pull vs Push data flow paradigms
## exploring what the "pull" paradigm brings side by side with our prior examples
### interop with imperative libraries
### async rendering example: time slicing
## Proposing a complete rewrite of Reactive React ;)
