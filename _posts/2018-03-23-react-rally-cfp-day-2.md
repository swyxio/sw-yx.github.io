---
layout: post
date: 2018-03-23
tags: react
feelings: excited
title: react rally cfp day 2
comments: true
description: react rally cfp day 2
---

I am starting to reap some benefits from engaging on twitter.

- i stalked people and managed to get EXTENSIVE feedback from Josh comeau, and an offer from Kurtis (who I will pass the IS CFP to)
- the react rally team RT'ed my request for feedback and i got offers from nader dabit and kyle welch (https://twitter.com/swyx/status/977033816400453632)

here's v2 of the talk

Title: Why React is not Reactive
Abstract
Functional-reactive libraries like RxJS make it really easy to understand how data changes, giving us tools to declaratively handle events and manage state. But while our render methods react to state changes, React isn’t fully reactive. Instead, we write imperative event-handlers, and constantly trip up on gotchas like async setState and race conditions. Why? In this talk we actually build a Reactive React to show the difference between the "push" and "pull" paradigms of data flow and understand why React chooses to manage Scheduling as a core Design Principle.
for review committee
Details
Explain the theme and flow of your talk. What are the intended audience takeaways? is there an outline you plan to follow?
Theme: This talk is a deep dive into React's core design principle around scheduling. Instead of abstract discussion, we make it accessible and real by exploring an alternate universe in which React was an FRP library and actually running into the problems React solves.
Optional Theme song: Cinderella - Don't Know What You Got (Til It's Gone)
We follow a three-act structure where we:
start off in an alternate universe where the prototype Reactive React I have built -is- the current React version,
and then talk about why we might want interoperability with imperative libraries like d3js, and more control over scheduling to do things like batching and async rendering,
in order to propose switching Reactive React from a "push" based Functional Reactive Programming paradigm to a "pull" based approach where computations can be delayed until necessary.
In other words, we use a "what-if" scenario to better understand and appreciate the Design Principles behind React!
Outline:
Recapping Reactive React
A reintroduction to Functional Reactive Programming
Your UI as a stream
Building a basic Reactive Component Class and reconciler
Demonstrating usage to solve race conditions and other event streams
Problems with Reactive React
Poor interoperability with imperative libraries
example with d3js
Lack of control over scheduling
too easy for the user to accidentally lock up the screen
async rendering eg time slicing work would be all in userland
A "pull" based Reactive React
the Pull vs Push data flow paradigms
exploring what the "pull" paradigm brings side by side with our prior examples
interop with imperative libraries
async rendering example: time slicing
Proposing a complete rewrite of Reactive React ;)
Pitch
Why is this talk pertinent? What is your involvement in the topic? Explain why this talk should be considered and what makes you qualified to speak on the topic.
This talk was born out of my frustration at why it felt so unnatural to connect RxJS with React even though I felt like they shared superficially similar paradigms: ultimately, stuff happens, state changes, and then the DOM changes because the state changes. I was also inspired by Paul Shannessy's React Rally 2016 talk where he showed it was possible to "build react from scratch" and show all the key parts in a single talk. So I tried building a reactive version of React, and to my surprise, it wasn't hard. So it begged the question: why -isn't- React reactive?
And that is how I discovered the Scheduling section of React's Design Principles, where they explicitly joke that React should have been called "Schedule" because React does not want to be fully "reactive". The React team already thought about this, and I had no idea.
I think a lot of React developers (like myself) often take the way things are for granted, and don't spend a lot of time understanding the reasons behind API choices and Design Principles like why React wants control over scheduling. This is why non obvious questions pop up like this RFClarification: why is setState asynchronous. By exploring the alternate universe, we can run head on into the issues that React's Design Principles solve.
I found this rabbit hole fascinating and I think others will too. By better understanding React's design principles, the React community can, on a philosophical level, 1) better understand how React works, 2) better design the broader React ecosystem to work with core React, and 3) better engage with the React core team when doing open source contributions.

