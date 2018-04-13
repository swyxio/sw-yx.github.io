---
layout: post
date: 2018-04-14
tags: react
feelings: happy
title: react amsterdam notes
comments: true
description: notes rom amsterdam
---

# Tracy Lee: Reactive Programming Demystified <a id="tracy"></a>

increasing adoption of reactive programming

- promises considered reactive??
- TC39 Observable proposal - added in apr 30 2015
- rxjs is at 8m monthly downloads now
- WHATWG Event Target Observable psoposed dec 12 2017  eg `button.on('click')` gives you an observable <https://github.com/whatwg/dom/issues/544>

Libraries

- RxJS: DSL for reacting to events, most popular FRP lib
- d3: reacting to the propagation of change - you describe what happens and it just happens under the hood
- react: reactive programming doesnt have to be pull based. calling `setState` causes reactions
- redux-observable
- mobx
- angular: rxjs is first class citizen.
  - onPush change detection strategy
  - ngrx
  - angular router
  - forms
- vuejs watchers

We see reactive programming patterns in cases where there is natrual fit for events to be modeled as values over time

- web sockets
- user events
- animations
- http

How to start thinking reactively?

- think of modeling everything as an event
- web apps are event driven
- everything can be represented as *a set of values* **over time**

code examples:

- drag and drop in react with a few lines of rxjs
- adding inputs and chaining with little refactoring in angular
- merging google vision + web speech + typed input to pipe into a search

No need to Rx all the things. Easy wins:

- click handlers
- ajax requests (easy cancellation)
- anything async

---

# Michele Bertoli: setState Machine <a id="michele"></a>

State is hard

- have to keep track of it over time
- make sure users can only perform valid operations

eg. Infinite Scroll

- attach event handler to scroll
- if scroll > 90% of page
- fetch data and append to end

problems

- keep track of whether you're fetching: `this.state.isFetching`
- no way to stop fetching if `data.hasMore` is falsy
- dealing with errors - `this.state.hasError`
- retry functionality
- nested ternaries of state flags

this is called the bottom-up approach

- focusing on linear transitions
- turns out, you need context
- edge cases can come from combinations of state flags that are impossible

solution: state machines!

- we want to constrain ourselves from infinite state turing machines to deterministic finite automata
- <Q: all states, Sigma: all inputs, delta: f(input) => state, q0: inital, F: final>
- visualize with state transition diagrams

If too many states, use Harel statecharts!

- clustering - wrapping shared transitions
- orthogonality - indepedent states
- guards - conditional blocking of transitions
- actions: side effects of entry/exit events

workflow: event -> statechart (evaluates guards etc) -> actions

"A statechart is a magic box; you tell it what happened, it tells you what to do" @lmatteis

Code example: 

- `yarn add xstate` :) i know this already
- `react-automata` - layer on top of xstate to work with it in React. 
  - infinite scroll example:
  - start -> ready -> fetching
  - fetching - SUCCESS[hasMore] - listening
  - listening - SCROLL[scrollPercentage>0.9] - fetching
  - listening - ERROR - fetching
  - fetching - SUCCESS[!hasMore] - finish
  
