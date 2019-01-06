---
layout: post
date: 2019-01-06
tags: podcast
feelings: happy
title: this week in rreactjs intro
comments: true
description: im starting a podcast!
---

Welcome to this week in r/reactjs, where we read you the top stories of the week based on subreddit votes. This podcast is never longer than 10 minutes, so lets get started.

---

- Everything I write in 2019 will be in TypeScript

This week Typescript and React kind of blew up on Twitter and Reddit. I'm not exactly sure why but this post probably sums up the zeitgeist. The top comment from user `georgeharveybone` said:

90% of the time it was pretty straightforward and I liked it, I've used flow so I knew the basic idea and some stuff to watch for. But I when i got stuck, it was brutal. I'd say the issues I had are the really poor documentation, it reminds me of the Immutable.js one, it's super abstract and unfriendly. It seems to me, more for existing C# devs than JS ones. Overall I'm not sure TS is "The Answer", but I definitely feel like it's on the right lines. Certainly worth learning!

user `LetterBoxSnatch` said: 

One mistake I see newcomers to TypeScript doing is adopting every single strict feature. You may think you're "doing it right" from the start, but actually TypeScript has many benefits even if you use a more "JavaScripty" and/or functional style. Some of the features only make sense in some shops. The strictest settings are probably most useful to, eg, someone coming from a C# background.

If you're a compositional kind of shop, DON'T use strict settings. any is ok. Instead, start learning how to use generics to allow the types to COMPOSE THEMSELVES OVER TIME 

user TheDarkSwordsman said:

I think TypeScript is great, but I can't see it actually being used that mainstream yet. 

user vinnl said:

I've been doing so for the past year, and it's been great. A lot of projects have added support for TypeScript in that year, often making it a matter of very little to even no configuration. The tipping point has most certainly been reached, and I feel like TypeScript adoption will continue to grow massively for quite a while in 2019 still.

Certainly feels like people are seriously considering Typescript, even if they're not fully on board yet.

- React Kawaii - Cute React SVG Components from user dance2die

user Kitefr said: 

> Nice. I’ll install some as Easter Eggs in the application at work. evil laugh

the author also gave a great talk at the recent React Conf https://m.youtube.com/watch?v=1gG8rtm-rq4

- The Elements of UI Engineering

Dan on:

- Consistency
- Responsiveness
- Latency
- Navigation
- Staleness
- Entropy (state explosion)
- Priority (over limited resources) - cooperating
- Accessibility by default
- i18n, rtl
- Delivery/Performance
- Resilience - what happens when bugs do crash
- Abstraction - reusability that hides impl details?

- FBT has been open sourced (framework used for React i18n in Facebook)

Inlined translatable text, Seamless text collection, Integrated translations

user nixblu said:

> Looks cool but I am seriously struggling to get my head around how to use it.

People were generally confused, comparing to react-intl, lingui, mozilla's fluent. As well as

---

Projects:

Our second segment is Projects, where we feature cool, open source projects shared by r/reactjs community members.


- Movies app lynks

⚛️Nice Movie demo app featuring:

⚓@reactjs Hooks
🐠@jxnblk's rebass
💅 @mxstbr's styled-components
🔥Firebase auth/database
📁@jaredpalmer's Formik/yup
🥅@Netlify hosting

Github: https://github.com/22mahmoud/movies-app-lynks …
Live Demo: https://movies-lynks.netlify.com 

user Leonardo_Da_PinchMe said:

> Really like your extracting all of the Providers into their own function. Makes your App.js render much cleaner.

usage of usePrevious

- react-katas

done by a react beginner

Katas:

HOC
Async data fetching
Composition
context
React router API

plans to add:

Embedded React
Portals and Refs
Render props
DefaultProps and PropTypes
React testing

user philhagger said:

> This is a really great idea. I like the principle of a kata. Repeat a task until you can do it from muscle memory.

---

And that was this Week in r/Reactjs! Did we miss anything? Come complain in the r/reactjs subreddit chat! See you next week!

