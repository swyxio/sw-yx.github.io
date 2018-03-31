---
layout: post
date: 2018-03-30
tags: react
feelings: nervous
title: react rally submissions
comments: true
description: my 2 talks
---

Why React is *not* Reactive

Functional-reactive libraries like RxJS make it easy to understand how data changes, giving us tools to declaratively handle events and manage state. But while our render methods *react* to state changes, React isn’t *reactive*. Instead, we write imperative event-handlers, and trip up on gotchas like async setState and race conditions. Why? In this talk we build a Reactive React to show the difference between the "push" and "pull" paradigms of data flow and understand why React chooses to manage Scheduling as a core Design Principle, enabling awesome features like async rendering and Suspense!

**Theme**: This talk is a deep dive into React's core design principle around scheduling. Instead of abstract discussion, we make it accessible and real by exploring an alternate universe in which React was an FRP library and actually running into the problems React solves.

Optional theme song: [Cinderella - Don't Know What You Got (Til It's Gone)](https://www.youtube.com/watch?v=i28UEoLXVFQ)

We follow a three-act structure where we:

- start off in an alternate universe where the prototype Reactive React I have built -is- the current React version, 
- and then talk about why we might want interoperability with imperative libraries and more control over scheduling to do things like batching and async rendering, 
- in order to propose switching Reactive React from a "push" based Functional Reactive Programming paradigm to a "pull" based approach where computations can be delayed until necessary.

In other words, we use a "what-if" scenario to better understand and appreciate the Design Principles behind React!

Outline:

- Recapping Reactive React
  - A gentle introduction to Functional Reactive Programming
  - Your UI as a stream
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
  

This talk was born out of my frustration at why it felt so unnatural to connect RxJS with React even though I intuited that they embody superficially similar paradigms: ultimately, stuff happens, state changes, and then the dom changes because the state changes. I was also inspired by [Paul O Shannessy's React Rally 2016 talk](https://www.youtube.com/watch?v=_MAD4Oly9yg) where he showed it was possible to "build react from scratch" and show all the key parts in a single talk. So I tried building a reactive version of React, and to my surprise, it wasn't hard. So it begged the question: why -isn't- React reactive?

And that is how I discovered the [Scheduling section](https://reactjs.org/docs/design-principles.html#scheduling) of React's Design Principles, where they explicitly joke that React should have been called "Schedule" because React does not want to be fully "reactive". The React team already thought about this, and *I had NO IDEA*.

I think a lot of React developers (like myself) often take the way things are for granted, and don't spend a lot of time understanding the reasons behind API choices and Design Principles like why React wants control over scheduling. This is why non obvious questions pop up like this [RFClarification: why is `setState` asynchronous](https://github.com/facebook/react/issues/11527). So maybe the best way to do it is explore the alternate universe and actually run head on into the issues that React's Design Principles solve.

I found this rabbit hole fascinating and I think others will too. By better understanding React's design principles, the React community can, on a philosophical level, 1) better understand how React works, 2) better design the broader React ecosystem to work with core React, and 3) better engage with the React core team when doing open source contributions.

https://www.youtube.com/watch?v=GWCcZ6fnpn4

ReactNYC - Contributing To React (added by Dan Abramov to https://reactjs.org/docs/how-to-contribute.html#introductory-video)

https://www.youtube.com/watch?v=rPuwZJEA-9U

ReactNYC - Never Bundle React Again (lightning talk with some ideas on bundling)

https://www.youtube.com/watch?v=R6pSelWatP0

ReactNYC - React Suspense: The Interactive Experience (livestream link on Mar 28 - the edited video will be up after CFP submission deadline)

---

React Mind, Beginner's Mind

As the React pool gets deeper, it gets easier to accidentally let newbies drown. But managing learning curve and DX is not just a matter of ergonomics: when we let the ecosystem ossify around the wrong abstractions, everyone pays the costs down the road. I went from Javascript newbie to React contributor in a year and podcasted dozens of interviews with beginners through the process of learning React and it's ecosystem. In this talk, we'll peek past the Github and Stackoverflow usernames to see what listening to the thousands of people that learn React everyday can teach us.

This is a talk about the technical choices we make and who we make them for.

We will do this with an exploration of the audio documentary I have been making for the past year about people learning React (and web dev in general), and go through what we, the React community, gain from keeping a beginner's mindset as well as some thoughts on how to do it.

Who are the React beginners?

  - None of the current React team is the original team
  - Beginners are React's future
  - Bootcamp grads now outnumber CS grads
  - Sample of Stories (Qualitative)
    - The English major
    - The HR recruiter
    - The Chef
    - The "Dinosaur"
  - Impostor Syndrome
  - React Beginners Survey (Quantitative)
    - How Experts think Beginners should Learn
    - How Beginners Actually Learn

Why Be a Beginner

  - The Cost of Wrong Abstractions
    - Examples to be fleshed out
  - The Best way to be a 10x Developer
  - First Principles Thinking
  - Code is read far more than it is written
  - Docs are read far more than they are updated
  - Solve Hard Problems > Bikeshedding
  - React Accessibility
  - The Design of React Things

Keeping a Beginners Mind

  - Use of the word "Just"
  - Quelling JS Fatigue
    - Comparing vs Using
    - Problem Solving > Github stars
  - Gatekeeping
    - What Too Much Thought Leading does to Beginners
    - Standards vs Inclusion
  - API choices
    - #0CJS
    - Adapters vs Layers
    - Babel Stage <4 Features
    - Boilerplates vs Toolkits
    - Render Props vs HOCs
    - CLIs
    - Monorepos
    - X-in-JS
  - Community
    - "How to Fight in Front of the Kids"
    - Github issues
    - Gitter/Slack/Discord/Forum
    - Twitter
    - Meetups & Conferences
    - Podcasts
    - Livestreams
  - Documentation
    - README.md
    - Documentation Sites
    - CONTRIBUTING.md
    - Blogs
    - Videos
  - Beginners Mind at Work
    - Hiring
    - Training
    - Managing
  - View Source
- Closing: Donella Meadows' 12 Points of Leverage in a System


This talk is relevant because React is getting more beginners than it has ever had, and at the same time we need to have a real discussion about how experts can keep a Beginner's Mind too. A lot of digital ink has been spilled and tempers lost because people have not kept a Beginner's Mind when dealing with each other, but even more pernicious is the hidden cost of the people who have been turned away because the far-reaching choices that we make in the React ecosystem haven't indicated that we welcome them.

My involvement is both as a recent beginner and as someone who has documented my own journey (through daily blogposts to myself) and that of others (through my podcast at impostor-syndrome.org). I hope a voice for the React beginners at React Rally :)


Infinite Learner and host of the Impostor Syndrome podcast. While I executed my career change, going from Javascript newbie to Software Engineer at age 30, I decided to live podcast the stories and voices of the increasingly diverse people joining the industry to light the path for others.
