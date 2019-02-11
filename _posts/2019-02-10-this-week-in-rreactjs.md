---
layout: post
date: 2019-02-10
tags: react
feelings: fat
title: rreactjs podcast 5
comments: true
description: more podcasting for great goodness
---

## Discussions

- [React 16.8 - the one with Hooks](https://www.reddit.com/r/reactjs/comments/anonoe/react_v168_the_one_with_hooks_react_blog/)
- [Do you still need Redux with the new Hook APIs](https://www.reddit.com/r/reactjs/comments/ap12uq/do_you_still_need_redux_with_the_new_hook_apis/)
- [Should I ever use classes now?](https://www.reddit.com/r/reactjs/comments/aoecpw/should_i_ever_use_classes_now/)
- [What React Hooks can do for you?](https://www.reddit.com/r/reactjs/comments/ap2cp6/what_react_hooks_can_do_for_you/)
- [Typescript, GraphQL, Nextjs youtube series](https://www.reddit.com/r/reactjs/comments/anf5h4/just_finished_making_a_series_on_typescript/)
- [Firebase + React Hooks Authentication](https://www.reddit.com/r/reactjs/comments/ap4aw7/firebase_react_hooks_authentication/)

## Projects

- [github history](https://www.reddit.com/r/reactjs/comments/anuj5z/browse_the_history_of_any_file_from_github_with/)
- [react RPG](https://www.reddit.com/r/reactjs/comments/anbts1/im_building_an_opensource_rpg_made_with_react/)
- [Diamonds Editor](https://www.reddit.com/r/reactjs/comments/aop9x7/my_first_personal_react_project_diamonds_editor/)
- [Linaria v1.0](https://www.reddit.com/r/reactjs/comments/aojnyd/announcing_linaria_10_zero_runtime_cssinjs_library/)
- [React Hooks Contest](https://www.reddit.com/r/reactjs/comments/anpxum/rreactjs_react_hooks_contest/)

---



Welcome to this week in /r/reactjs, where we read you the top stories of the week based the votes of 92,000 React developers. This podcast is never longer than 10 minutes, so lets get started.


## Discussions

- [React 16.8 - the one with Hooks](https://www.reddit.com/r/reactjs/comments/anonoe/react_v168_the_one_with_hooks_react_blog/)

questions about

- Rewriting
- Enzyme support
- Redux support

Followup posts
- [Do you still need Redux with the new Hook APIs](https://www.reddit.com/r/reactjs/comments/ap12uq/do_you_still_need_redux_with_the_new_hook_apis/)

user klabe2018 quotes: is there still a need for redux?

user dceddia quotes: 

Hooks makes it easier to use state in components, and group related state + side effects together. 
Hooks also make Context easier to use (with the useContext hook), and Context is what (React-)Redux uses to pass data around. 
Context is the secret sauce behind the Provider/connect components in react-redux. 
So if all you need out of Redux is the ability to pass data deeply through a component tree, Context might be good enough.

Redux brings along the debuggability with devtools, the extensibility from middleware (thunks, sagas, etc), 
and if you have complex state/business logic that you want to decouple from your components, 
Redux helps with that too. All of that could be reimplemented using hooks of course, 
but then, well... you may as well just use Redux 😄

user acemarke points you to 
React-Redux Roadmap: v6, Context, Subscriptions, and Performance
Redux Starter Kit package

- [Should I ever use classes now?](https://www.reddit.com/r/reactjs/comments/aoecpw/should_i_ever_use_classes_now/)
- [What React Hooks can do for you?](https://www.reddit.com/r/reactjs/comments/ap2cp6/what_react_hooks_can_do_for_you/)

User ghost20000 asks:

> I'm a new react developer, and have already worked on some personal projects with react.
> I've been pretty much avoiding function components, opting to just make component classes.
> However, with the introduction of hooks, I'm now wondering if there's any reason to use classes anymore.
> So are there any advantages to making functions instead of classes? And is there any reason to still make classes?

user SleepCircle-app says

> Short answer: use classes until you’re comfortable using hooks to replace them.

- [Typescript, GraphQL, Nextjs youtube series](https://www.reddit.com/r/reactjs/comments/anf5h4/just_finished_making_a_series_on_typescript/)

user benawad says

Haven't posted here in a while, figured it would be a good time after completing this series. And seems like a lot of people are starting to become interested in using Typescript with React.js

Here's what I cover:

setup Next.js project

setup Apollo

setup GraphQL Code Generator

register

handle and display errors

clean urls

login

authenticated routes

forgot/change password

logout and styled-components


## Projects

- [github history](https://www.reddit.com/r/reactjs/comments/anuj5z/browse_the_history_of_any_file_from_github_with/)

neat codesurfer project

user dance2die asks: Anyone know if he plans on sharing how it's implemented by chance?

user pomber replied with his prototype line slider animation built with popmotion, and also his own slider animation

- [react RPG](https://www.reddit.com/r/reactjs/comments/anbts1/im_building_an_opensource_rpg_made_with_react/)

user shaz_berries says:

Hey guys! I have been working on a turn-based dungeon-crawler, written completely in React.js (& Redux). It scales for all devices and I just got it in both the app stores. Check it out and let me know what you think!

http://react-rpg.com

Oh and it's open-source, so you can see all the code here:

https://github.com/ASteinheiser/react-rpg.com

Theres also a react native version and its all open source.

there were two other users who were also working on rpgs and roguelikes who shared their code

- [Diamonds Editor](https://www.reddit.com/r/reactjs/comments/aop9x7/my_first_personal_react_project_diamonds_editor/)

a math worksheet generator? you can generate answer key and print

- [Linaria v1.0](https://www.reddit.com/r/reactjs/comments/aojnyd/announcing_linaria_10_zero_runtime_cssinjs_library/)

Announcing Linaria 1.0 - Zero runtime CSS-in-JS library
🔥 New styled API for React

🌟 Prop based styles with CSS variables

😍 Support for CSS sourcemaps

🔮 Improved stylelint processor

discussion form styled components and emotion

- [React Hooks Contest](https://www.reddit.com/r/reactjs/comments/anpxum/rreactjs_react_hooks_contest/)

we hosted a hooks contest for people to make creative hooks!

- drcmda made a masonry grid that combines a useState, useMedia, useMeasure, useTransition, and useEffect hooks
- latviancoder made a real life cycling routes app that uses useMemo, useReducer, useContext, useFetcher, useRef, and useEffect
- mrleebo made conway's game of life encapsulating the life state machine and the timer in separate hooks
- charles stover wrote useDimensions for React Native, useFetch for Fetching and Suspending, useForceUpdate for backgward compatibility, useGlobal
for global singleton state, useReactRouter to replace the withRouter HOC from React Router

