---
layout: post
date: 2018-03-22
tags: react
feelings: afraid
title: react rally cfp
comments: true
description: An attempt at submitting to react rally ¯\_(ツ)_/¯
---

# Talk 1

## Title: Why React is _not_ Reactive

## Abstract

**What is your talk about?**

Why DO we write imperative event handlers? You have to deal with an async `setState`, race conditions, and god forbid you forget to bind their context properly! Libraries like RxJS make async event streams first class citizens, giving us the tools to declaratively solve these issues. Since our `render` methods are just reactions to `state`, React *could* be reactive. In this talk we actually build a Reactive React to show the difference between the "push" and "pull" paradigms of data flow and *finally* understand why Interoperability and Control over Scheduling are core design principles of React.

## *for review committee*

## Details

**Explain the theme and flow of your talk. What are the intended audience takeaways? is there an outline you plan to follow?**

The theme is "Counterfactual History", and theme song: [Cinderella - Don't Know What You Got (Til It's Gone)](https://www.youtube.com/watch?v=i28UEoLXVFQ)

We follow a three-act structure where we:

- start off in an alternate universe where the prototype Reactive React I have built -is- the current React version, 
- and then talk about why we might want interoperability with imperative libraries and more control over scheduling to do things like batching and async rendering, 
- in order to propose switching Reactive React from a "push" based Functional Reactive Programming paradigm to a "pull" based approach where computations can be delayed until necessary.

In other words, we use a "what-if" scenario to better understand and appreciate the Design Principles behind React!

Outline:

- Recapping Reactive React
  - Building a basic Reactive Component Class and reconciler
  - Demonstrating usage to solve race conditions and other event streams
- Problems with Reactive React
  - Poor interoperability with imperative libraries
    - example with d3js
  - Lack of control over scheduling
    - too easy for the user to accidentally lock up the screen
    - async rendering eg time slicing work would be all in userland
- A "pull" based React
  - the Pull vs Push data flow paradigms
  - exploring what the "pull" paradigm brings side by side with our prior examples
    - interop with imperative libraries
    - async rendering example: time slicing
  - Proposing a complete rewrite of Reactive React ;)

## Pitch

**Why is this talk pertinent? What is your involvement in the topic? Explain why this talk should be considered and what makes you qualified to speak on the topic.**

This talk was born out of my frustration at why it felt so unnatural to connect RxJS with React even though I intuited that they embody superficially similar paradigms: ultimately, stuff happens, state changes, and then the dom changes because the state changes. I was also inspired by [Paul O Shannessy's React Rally 2016 talk](https://www.youtube.com/watch?v=_MAD4Oly9yg) where he showed it was possible to "build react from scratch" and show all the key parts in a single talk. So I tried building a reactive version of React, and to my surprise, it wasn't hard. So it begged the question: why -isn't- React reactive?

And that is how I discovered the [Scheduling section](https://reactjs.org/docs/design-principles.html#scheduling) of React's Design Principles, where they explicitly joke that React should have been called "Schedule" because React does not want to be fully "reactive". The React team already thought about this, and *I had no idea*.

I think a lot of React developers (like myself) often take the way things are for granted, and don't spend a lot of time understanding the reasons behind API choices and Design Principles like why React wants control over scheduling. This is why non obvious questions pop up like this [RFClarification: why is `setState` asynchronous](https://github.com/facebook/react/issues/11527). So maybe the best way to do it is explore the alternate universe and actually run head on into the issues that React's Design Principles solve.

I am far from the most qualified to speak on the topic, but I found this rabbit hole fascinating and I think others will too. By better understanding React's design principles, we can understand 1) what sets it apart from other libraries, 2) better design the broader React ecosystem to fit core React on a philosophical level, and 3) better engage with the React core team when doing open source contributions.

---


# Talk 2

## Title

React Mind, Beginner's Mind

## Abstract

**What is your talk about?**

In 

## *for review committee*

## Details

**Explain the theme and flow of your talk. What are the intended audience takeaways? is there an outline you plan to follow?**


## Pitch


**Why is this talk pertinent? What is your involvement in the topic? Explain why this talk should be considered and what makes you qualified to speak on the topic.**
