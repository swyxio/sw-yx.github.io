---
layout: post
date: 2019-02-03
tags: react
feelings: fat
title: rreactjs podcast 4
comments: true
description: more podcasting for great goodness
---

## Discussions

- [React 16.8 (The One Hopefully with Hooks) planned for Feb 4](https://www.reddit.com/r/reactjs/comments/al3zj7/react_168_the_one_hopefully_with_hooks_planned/)
- [Weekend Reads: React Docs on Hooks](https://www.reddit.com/r/reactjs/comments/amhr5y/weekend_reads_react_docs_on_hooks/)
- [React as a UI Runtime](https://www.reddit.com/r/reactjs/comments/aml427/react_as_a_ui_runtime/)
- [React-Redux Roadmap: v6, Context, Subscriptions, and Hooks](https://www.reddit.com/r/reactjs/comments/amuhwi/reactredux_roadmap_v6_context_subscriptions_and/)

## Projects

- [Nice and smooth! I've created a collection of animated burgers (HTML/CSS + React)](https://www.reddit.com/r/reactjs/comments/alevhs/nice_and_smooth_ive_created_a_collection_of/)
- [Fully functional WhatsApp Clone using React (Hooks+Suspense), GraphQL, Apollo, TypeScript and PostgreSQL](https://www.reddit.com/r/reactjs/comments/am58h2/fully_functional_whatsapp_clone_using_react/)
- [/dondonleroy's personal site](https://www.reddit.com/r/reactjs/comments/akvead/hey_guys_just_finished_my_personal_website_using/)
- [I had a real nostalgic attack so I created this library](https://www.reddit.com/r/reactjs/comments/alpo0q/i_had_a_real_nostalgic_attack_so_i_created_this/)
- [Introducing react-movable: Drag and drop for your lists and tables. 3.5kB gzipped. Accessible.](https://www.reddit.com/r/reactjs/comments/al1c1p/introducing_reactmovable_drag_and_drop_for_your/)
- [Announcing Overmind, reducing the pain of state management](https://www.reddit.com/r/reactjs/comments/alpjyc/announcing_overmind_reducing_the_pain_of_state/)
- [Road to React with Firebase](https://www.reddit.com/r/reactjs/comments/akr0ax/road_to_react_with_firebase/)

---



Welcome to this week in /r/reactjs, where we read you the top stories of the week based the votes of 92,000 React developers. This podcast is never longer than 10 minutes, so lets get started.


Youtube theme: https://www.youtube.com/watch?v=ckbhnKVRWUA&feature=youtu.be

## Discussions

- [React 16.8 (The One Hopefully with Hooks) planned for Feb 4](https://www.reddit.com/r/reactjs/comments/al3zj7/react_168_the_one_hopefully_with_hooks_planned/)
- [Weekend Reads: React Docs on Hooks](https://www.reddit.com/r/reactjs/comments/amhr5y/weekend_reads_react_docs_on_hooks/)

This is a pretty unusual move for the React team - Dan Abramov filed a PR to update the changelog for React 16.8.0 and set an expected release date of Feb 4. This includes updates to React, React-DOM, React Test Renderer, and a new ESLint plugin for the rules of Hooks. As of the time of recording, this is tomorrow, although of course this is still subject to change. The update will also include breaking changes since the initial alpha release, particularly to useReducer.

Its worth remembering that the majority of React users still have no idea what Hooks are or why we might want them, so its worth staying close to the intent of the release to avoid spreading uncertainty and doubt. In this weekend's read special, I summed up the must-read blogposts and interviews from the React team and speakers at React Conf, and of course you can always search r/reactjs for more Hooks content.

- [React as a UI Runtime](https://www.reddit.com/r/reactjs/comments/aml427/react_as_a_ui_runtime/)

This is a long deep dive into React's runtime by Dan Abramov on Overreacted.io. It covers what React does for you under the hood, like the concept of how keys and element types are used in Reconciliation, idempotence vs purity, and why we migh want principles like the inversion of control, lazy evaluation, and batching, but DON'T have a Reactivity system like Vue. There are many ways to view React other than "just a view library", and this posts guides you through the thought process of viewing React as a Runtime complete with a callstack. He also ends with thoughts on what he hasn't yet explained - for example multipass rendering, error handling, and Suspense.

- [React-Redux Roadmap: v6, Context, Subscriptions, and Hooks](https://www.reddit.com/r/reactjs/comments/amuhwi/reactredux_roadmap_v6_context_subscriptions_and/)

This is a fascinating review from Mark Erikson on lessons learned from releasing React-Redux v6 last month. Major rearchitecture was done for the implementations of the Provider and connect APIs, to use the new React Context, and it now seems to have been premature. I Quote:

```
TL;DR:

v6 switched from direct store subscriptions in components, to propagating store state updates via createContext
It works, but not as well as we'd hoped
React doesn't currently offer the primitives we need to ship useRedux() hooks that rely on context
Based on guidance from the React team, we're going to switch back to using direct subscriptions instead, but need to investigate an updated way to re-implement that behavior
As part of that, we need to improve our tests and benchmarks to ensure we're covering more use cases from throughout the community
Our ability to ship public hooks-based APIs depends on switching back to subscriptions, and how we update connect may require a major version bump.
We need volunteers and contributions from the React-Redux community to make all this happen!
```

So if you use Redux in your projects and want to contribute toward the future of Redux, please check out this issue. Its a rare opportunity to have impact on a massive open source project. Mark has been extremely transparent with all the nuances and considerations in the journey from v5 to v6 and needs your help to execute!

- [Q&A with the React Native Core Team (Reactiflux Transcript)](https://www.reddit.com/r/reactjs/comments/aml5qr/qa_with_the_react_native_core_team_reactiflux/)

This is pretty much what the title says and is the closest thing we have to a "state of React Native" so far this year. The React Native team has added a lot of great new people and Facebook's investment in both the core infrastructure (like React Fabric/TurboModule/Codegen) and in the community (with this Q&A) mean that React Native is stronger than ever.

- [What the Flow team has been up to – Flow – Medium](https://www.reddit.com/r/reactjs/comments/aksp3y/what_the_flow_team_has_been_up_to_flow_medium/ef832fg/?context=3)

```
Performance: Make Flow scale as O(size of edits) rather than O(size of codebase), which necessarily involves projects aiming for big-O improvements.
Reliability: Make Flow type analysis trustworthy with respect to run-time semantics, not only for product safety and developer efficiency but also to explore opportunities for runtime optimizations.
Language tooling: Make Flow the basis for unified JavaScript tooling to enable language evolution for performance and reliability.
```

user bequebed

> Flow has become the underdog in the land of statically typed JavaScript and I do not expect them to improve much in their public communication. BUT I do think that it is good to have two teams inventing in this space with different objectives. TypeScript and Flow both benefit from the other generating and investigating ideas.



## Projects

- [Nice and smooth! I've created a collection of animated burgers (HTML/CSS + React)](https://www.reddit.com/r/reactjs/comments/alevhs/nice_and_smooth_ive_created_a_collection_of/)

user march08

very simple plug and play solution

```
Hey guys, I've created some sleek burger animations. The burgers are available as an npm package for each style to minimize the size! e.g. @animated-burgers/burger-squeeze

Let me know what you think. Also this is my first time using lerna, so any comments on the code and the structure are welcome as well.
```


- [Fully functional WhatsApp Clone using React (Hooks+Suspense), GraphQL, Apollo, TypeScript and PostgreSQL](https://www.reddit.com/r/reactjs/comments/am58h2/fully_functional_whatsapp_clone_using_react/)

user Eytan Manor said:

> I’m happy to announce that a new version of the WhatsApp Clone is coming, and it’s based on React 16.7 (Hooks & Suspense), Styled-Components, Material-UI, TypeScript, Apollo, GraphQL-Subscriptions/Codegen/Modules, PostgreSQL and TypeORM.

```
Basic authentication.
Image uploading with Cloudinary.
Live updates with GraphQL Subscriptions.
100% function components with React Hooks.
GraphQL communication with react-apollo-hooks.
```

a lot to sink your teeth into and this project had the most calls to reddit's remindme bot i have ever seen, so a ton of interest in this front

- [/u/dondonleroy's personal site](https://www.reddit.com/r/reactjs/comments/akvead/hey_guys_just_finished_my_personal_website_using/)

This is a great personal site with a Windows 95 user interface! very fun and creative!

- [react-word art](https://www.reddit.com/r/reactjs/comments/alpo0q/i_had_a_real_nostalgic_attack_so_i_created_this/)

This project helps you make odl school 90's style wordart with really bad colors and text effects

User console dot noob said: 

> This as great as it is terrible!

- [Introducing react-movable: Drag and drop for your lists and tables. 3.5kB gzipped. Accessible.](https://www.reddit.com/r/reactjs/comments/al1c1p/introducing_reactmovable_drag_and_drop_for_your/)

tajo21


```
The main goal of this library is the smallest footprint possible while supporting main use-cases, a11y, mobile devices and keyboard controls. It's about 10x smaller than alternatives. Some other features:

No wrapping divs or additional markup

Simple single component, no providers or HoCs

Unopinionated styling, great for CSS in JS too

Full control over the dragged item, it's a portaled React component

Autoscrolling when dragging (both for containers and the window)

Scrolling with the mousewheel / trackpad when dragging

Typescript and Flow type definitions

Happy to get your feedback!
```

very nice alternative 3.5kb to react-beautiful-dnd 31kb.  

```
Horizontal sorting.
DnD between multiple list.
Combining items / multi drag support.
Supporting older versions of React. The minimum required version is 16.3 since the new createRef and createPortal APIs are used.
```

- [Announcing Overmind, reducing the pain of state management](https://www.reddit.com/r/reactjs/comments/alpjyc/announcing_overmind_reducing_the_pain_of_state/)

A new framework agnostic state management library by christianalfoni. it address pain points of:

- Typescript
- Flux architecture
- Connecting dependencies
- Imperative vs functional
- Props drilling and events
- Component vs application state

inspired by MobX and RxJS. it moves state management outside of components and has piping and debouncing like rxjs

THe release has a nice 30 minute tutorial and website so its worth checking out, but i have to say i'm not sold on it yet.

- [Road to React with Firebase](https://www.reddit.com/r/reactjs/comments/akr0ax/road_to_react_with_firebase/)

This is another amazing free book by Robin Wieruch diving into advanced react topics and building full stack products with a Firebase backend. I love working with Firebase and Robin's tutorial approach and Highly recommend checking this out if you havent given Firebase a shot. There is an optional paid video series to accompany the book.

- [Notable - The markdown-based note-taking app that doesn't suck](https://www.reddit.com/r/reactjs/comments/alf05t/notable_the_markdownbased_notetaking_app_that/)


it looks like notion, which is a compliment to its design, but its completely markdown based and open source and definitely needs more attention.

user  fabiospampinato said
 
> I made Notable because I couldn't find a note-taking app that ticked all the boxes I'm interested in: notes are written and rendered in GitHub-flavored Markdown, no WYSIWYG, no proprietary formats, I can run a search & replace across all notes, notes support attachments, the app isn't bloated, the app has a pretty interface, tags are indefinitely nestable and can import Evernote notes (because that's what I was using before).


