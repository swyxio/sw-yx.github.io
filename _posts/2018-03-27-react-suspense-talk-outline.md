---
layout: post
date: 2018-03-27
tags: react
feelings: happy
title: react suspense talk prep
comments: true
description: react suspense talk prep
---

React Suspense for the rest of us

Basics

- Civilization advances by extending the number of important operations which we can perform without thinking about them.
  - Why do we like React
  - Build a webapp with Vanilla JS
  - Build the same thing with React
- What is Async Rendering?
- What is React Suspense?
- Who is behind React Suspense?
- When is it coming? Do I need to change anything?

outline for talk 1

- Why do we want React Suspense?
  - easy API, fetch in render
  - only switch when ready
    - "Transparent Asynchrony" aka Rendering child components without worrying if they're ready to be rendered
  - interactive AF
  - Precise control of loading states
  - Prefetching declaratively
  - No race conditions
- Code Examples
  - Data fetching
    - Code Splitting
    - Image loading
    - CSS loading
  - Debouncing
  - Streaming Server Rendering
  - Loading without Race conditions
  - Intentional loading states
  - Eager Loading/Prefetching
  - "Speculative Rendering" aka Rendering components before you actually need them
  - ... (we're still figuring out how this works)
  - all with less boilerplate!
- the hawaiian missile defence center

outline for talk 2

- How does it work?
  - Emoji app
  - High Level API vs Low Level
- Takeaways
  - React is evolving to Interact
  - Library innovation will be coming
  - We will need new devtools
- Pit of Success

---

# Q&A.

Disclaimer: I do not speak for the React team, this is just what I, a regular React user, have learned over dozens of hours of studying the information available.

- **Q: What Is React Suspense?**
    - A: [A generic way for components to suspend rendering while they load async data.](https://reactjs.org/blog/2018/03/01/sneak-peek-beyond-react-16.html)
- **Q: What Does That Really Mean?**
    - A: React is getting [2 new primitive](https://twitter.com/acdlite/status/969704631424053249) capabilities that give you a lot more control over loading *anything*, **while** staying interactive. A lot of boilerplatey things (e.g. fetching data without race conditions) will become much simpler to write, and some previously impossible things (e.g. partially rendering components while waiting for data) become easy.
- **Q: I'm fatigued already. Do I really have to learn this?**
    - A: No.
- **Q: Really?**
    - A: Yup. If you don't like any of this you don't have to change the way you do things one bit. [React really values API stability](https://reactjs.org/docs/design-principles.html#stability).
- **Q: Then why should I care?**
    - A: It will probably make your life a lot easier.
    - A: Also its frickin cool'. You like cool things, right?
- **Q: How does it make my life easier?**
    - A: Anytime you need to display something that needs to be loaded, you create a `fetcher` for it and just use the `fetcher` inside your `render` method.
        - If React tries to render something that hasn't been fetched, it stops the render and goes to fetch it. 
        - If something was previously fetched before, it is **cached** and renders immediately. 
    - If you want a `Placeholder` (eg spinner) to show if a fetch takes too long, thats just a couple lines of code. 
    - If you want to avoid cascading spinners as different things are fetched, that's how Suspense works by default. We really don't like cascading spinners.
    - btw by cascading spinners we mean [all unintentional loading states](https://twitter.com/acdlite/status/955160681208168448)
- **Q: Isn't that a job for Redux or a lifecycle method?**
    - A: Not anymore, if you don't want it. No need to use `componentDidMount` or dispatch a `Redux` action just to fetch data. No need to define extra state variables like `isLoading` or `pastDelay` and deal with the state explosion that can come from multiple things loading at various different times. And did I mention that your UI stays interactive throughout this whole thing?
- **Q: Doesn't `react-loadable` already do this?**
    - A: Only kind of. The focus on keeping things interactive while loading (by rendering on a "separate thread"... we'll explain later) cannot be done in "userland" and requires handling in React's (coming) `AsyncMode`.
- **Q: Cool so this `fetcher` and `Placeholder` are the new parts of React?**
    - A: Nope. That's the "High Level API", and you're going to see a bunch of these come out over the next year as people experiment with them. Don't worry about the low level APIs for now, those are unstable.
- **Q: Are you trying to make fetch happen? It's never going to happen.**
    - A: Everyone's already tired of [that joke](http://knowyourmeme.com/memes/stop-trying-to-make-fetch-happen) and its only March. Please be more original. Great movie tho.
- **Q: Why's it called Suspense?**
    - A: That's a reference to how it works internally. When you try to render something that requires data, your `fetcher` **`throw`s a promise** up to React, and React **suspends** the rendering (within [limits](https://twitter.com/dan_abramov/status/971187182621872128)) while the data is being fetched. While the fetch is ongoing, your UI is still interactive, as though the fetching is on a different thread (note: [not technically true](https://twitter.com/aweary/status/913619299087949824)).
- **Q: Is it safe to `throw` things willy nilly like that?**
    - A: It was [supposed to be controversial](https://twitter.com/acdlite/status/969172311067713537), but so far nobody's found a real objection to it. And extensive [tests](https://github.com/acdlite/react/blob/7166ce6d9b7973ddd5e06be9effdfaaeeff57ed6/packages/react-reconciler/src/tests/ReactSuspense-test.js) have been written. You'll see the words "algebraic effects" a lot, but for our purposes you can think of them as [resumable exceptions](https://www.youtube.com/watch?v=hrBq8R_kxI0). As a user, you'll probably never actually write this yourself.
- **Q: What's this about a cache? Is React getting a cache?**
    - A: Caching is great and kinda central to the whole "do I fetch or not" thing. React provides the ability for users to make caches, and we're gonna see [a whole lot of innovation](https://dev-blog.apollodata.com/a-first-look-at-async-react-apollo-10a82907b48e) there too. The React team has provided a [simple-cache-provider](https://github.com/facebook/react/blob/master/packages/simple-cache-provider/src/SimpleCacheProvider.js#L148) which will probably be very popular for basic apps, but they don't handle a lot of nuance, such as fine-grained cache invalidation. You may have heard that's a hard thing.
- **Q: Enough talk. Where do I see some code?**
    - A: First check out Dan's JSConf talk on Time Slicing + Suspense (the official video is [here](https://reactjs.org/blog/2018/03/01/sneak-peek-beyond-react-16.html) but I can't embed it):


- **Q: More!?**
    - A: Then check the followup where he just talked Suspense:


- **Q: More!??**
    - A: Then have a play of the Movie demo [here](https://github.com/facebook/react/pull/12279)
    - A: Then check out the [implementation pull request](https://github.com/facebook/react/pull/12279), the [tests](https://github.com/acdlite/react/blob/7166ce6d9b7973ddd5e06be9effdfaaeeff57ed6/packages/react-reconciler/src/tests/ReactSuspense-test.js) and the [async react demo](https://gist.github.com/acdlite/f31becd03e2f5feb9b4b22267a58bc1f) (check out the deployed app [here](https://build-mbfootjxoo.now.sh/), that's what made async rendering really click for me)
- **Q: Ok, too long, didn't read. Spell it out for me, what concretely can I use React Suspense for?**
    - A: **deep breath**
    - Data fetching
    - Code Splitting
    - Image loading
    - CSS loading
    - Debouncing
    - Streaming Server Rendering
    - Loading without Race conditions
    - Intentional loading states
    - Eager Loading/Prefetching
    - "Speculative Rendering" aka Rendering components before you actually need them
    - "Transparent Asynchrony" aka Rendering child components without worrying if they're ready to be rendered
    - ... (we're still figuring out how this works)
    - all with less boilerplate!
    - If that sounds like a lot, its because it is.
- **Q: Wow. How did Dan Abramov do all this?? He's a beast?**
    - A: Dan worked really, really hard on the (super awesome) demo. [The entire React team worked on React Suspense](https://twitter.com/dan_abramov/status/972856536073687040), with feedback from the community.
- **Q: SO COOL! I want to use it in production now!!**
    - A: That is a very bad idea and you should be ashamed of yourself.
- **Q: But when can I have it?**
    - A: Just mentally peg it "by end 2018" and you won't be too disappointed.
- **Q: What should I read next?**
    - A: If you want to walk through some code demos, [here's one I did of the (warning once again: Unstable!) low level API.](https://dev.to/swyx/a-walkthrough-of-that-react-suspense-demo--4j6a)



