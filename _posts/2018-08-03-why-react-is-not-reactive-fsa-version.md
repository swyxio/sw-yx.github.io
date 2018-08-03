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

## Programming for Humans

I'm a career changer. My first real job out of school was trading interest rate and currency volatility derivatives at an investment bank. At investment banks there is a division between the front, middle, and back office. Basically, the closer you are to "the money", the more "front" you are. So front office folks were the salespeople and traders, middle office were grizzled risk managers, and back office were compliance and operations folks. As a rule, you made more money the closer you are TO "the money". Makes sense, right? Even if not exactly fair.

We also had quants and computer scientists on staff who were some of the best Haskellers in the world. More than 1 PhD on average per capita. They would create awesome automated programs for our back office reconciliation, and fancy valuation models and risk reports for our middle office.

You'd think that with billions of dollars at stake, the front office systems would the most top notch, right? Super slick custom programs, numbers flying all over the place, ten monitors, millisecond latency, all that jazz?

Nope. We used Excel.

Why Excel? Well, first of all, we all know it. Spreadsheets are one of the earliest business applications ever developed in computing. When you write an Excel program, every step is declarative, and debuggable by splitting out intermediate steps. You can create visual representations and transformations of your data with virtually no programming ability at all. Fantastic!

```
// TODO: demo of subscriptions in a sheet
```

Excel is also reactive. At any given time, every cell on a spreadsheet represents a consistent state with every other cell. If you plug your spreadsheet in to a realtime datasource like Bloomberg, you turn your spreadsheet into a dashboard! This is important in a fast moving market, and being able to customize your UI according to your needs without having to go to a separate developer is huge. Faster iteration cycles are more important than fighting for limited resources. And because software is eating the world, every trader like myself eventually became a developer. First I specialized in number crunching because Math is indisputable and if only I can crunch numbers better than the next guy I'd get paid. Only problem: no one cared. Eventually I realized the truth: If no one can see or play around with your Math, it might as well not exist. So I started making UI's.

```
// TODO: demo of live updating in a sheet
```

The reason I start with this story is to remind you of the "human" part of programming. As UI developers what we really want is to optimize for iteration speed and declarative consistency with realtime, asynchronous inputs. When we worry about writing things like imperative code, or state machines, or memory management, or dom manipulation, it's because the machines aren't good enough yet at keeping up with -us-. Fortunately, today we have the tools to match our programming paradigm with our usage paradigm: Reactive Programming.

## A reintroduction to Reactive Programming

[Reactive programming is programming with asynchronous data streams](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754). In a user interface, that can be anything from HTTP requests to stock data through a web socket, but the most important data stream of all is -you-, the user. This is why when we make our DOM interactive we use addEventListener(). This is the most widely known reactive API for web developers, but we also have a long list of other reactive APIs:

- dom events
- websockets
- server-sent events
- node streams
- service workers
- xmlhttprequest
- setInterval

However just supplying callbacks to addEventListener isn't very scalable, so we have the Observer design pattern. This pattern has two parts, Observables and Observers. You can think of Observables like a podcast. When you subscribe to a podcast, you are an Observer of the podcast. When the podcast has a new episode, the Observers get notified and download the episode. If you, the Observer, decide to unsubscribe, you can do it anytime you like, without the Observable's knowledge.

You can implement one like this (drastically simplified from the spec):

```js
function Observable(cb) {
  const subscribe = next => cb({ next })
  return { subscribe }
}
```

usage

```js
const saysWorld = new Observable(observer => observer.next('world!'))
saysWorld.subscribe(x => console.log('hello', x)) // 'hello world!'
```

Why is this a good fit for user interfaces? Imagine you had a realtime data source sending you data:

```js
function LiveData(tick = 1000) {
  return new Observable(observer => {
    let timer = () => setTimeout(() => {
      observer.next(90 + Math.round(Math.random() * 10000, 2)/100);
      timer()
    }, tick);
    timer()
    return () => clearTimeout(timer);
  })
}
```

You can now subscribe to it from all over your application just like in your Excel spreadsheet!

```js
const source = new LiveData()
source.subscribe(x => console.log('sheet 0', x)
source.subscribe(y => console.log('sheet 1', y)
// nuance: this woudld be a hot observable in real life
```

This is also very sad but true - the podcast host has no real idea how many people are subscribed! This is a feature as well as a weakness - it is an advantage that the parent has no knowledge of the child, but it also makes optimization of the child's experience harder.

```
// TODO: more examples
```

## Your UI as a stream

How would frontend development change if the Observable proposal became a part of Javascript? We might build an entire user interface library just based on it!

`v$=>  f(d$)`

Your view as a stream of your data

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


---

notes

more 2x2 diagrams

- one and sync: T
- many and sync: arrays
- one and async: promises
- many and async: observable
- https://channel9.msdn.com/Events/Lang-NEXT/Lang-NEXT-2014/Keynote-Duality

---

- Haskell GUI https://www.stackbuilders.com/tutorials/haskell/gui-application/

full observable


```js
function Observable(cb) {
  const subscribe = (
    next, 
    error = () => {}, 
    complete = () => {}
    ) => cb({ next, error, complete })
  return { subscribe }
}
```

event handler

```js
export function fromEvent(el, eventType) {
  return new Observable(observer => {
    const handler = e => observer.next(e)
    el.addEventListener(eventType, handler)
    return () => el.removeEventListener(eventType, handler)
  })
}
```
