---
layout: post
date: 2018-03-26
tags: react
feelings: neutral
title: react suspense talk prep
comments: true
description: react suspense talk prep
---

just planning out my talk for react suspense. the format:

https://twitter.com/swyx/status/961082736252420096

1. Start with Why (Have a Goal)
2. Make Them Laugh (Entertain)
3. Show some facts (establish credibility, relevance)
4. Poll the Audience
5. Answer the poll (more facts)
6. Show what they can do ( 💰 shot )
7. Reach the Goal
8. Leave breadcrumbs

---

image https://pbs.twimg.com/media/DXyCoVdXcAIYa8a.jpg

Basic flow

- sierpinski triangle https://claudiopro.github.io/react-fiber-vs-stack-demo/

- Why We Dont Want to Think About Rendering
  - It is a profoundly erroneous truism, repeated by all copy-books and by eminent people when they are making speeches, that we should cultivate the habit of thinking of what we are doing. The precise opposite is the case. 
  - Civilization advances by extending the number of important operations which we can perform without thinking about them. 
  - Operations of thought are like cavalry charges in a battle — they are strictly limited in number, they require fresh horses, and must only be made at decisive moments.
- Think Less
- Whats the Laugh???
- Facts
  - Build a webapp with Vanilla JS
  - Build the same thing with React
  - React Family Feud - what do we like about React
- How it works
  - Throwing Promises
  - Caching
- The Released API of React Suspense
  - React.Timeout
  - React.unstable_AsyncMode
    - AsyncMode https://twitter.com/acdlite/status/977291318324948992 with pics https://pbs.twimg.com/media/DZAJMtzVoAAFMvT.jpg
  - ReactDOM.unstable_deferredUpdates
  - simple-cache-provider.SimpleCache
  - simple-cache-provider.createResource
  - <div hidden={true}>
- The '../future' API?
  - Placeholder
  - createFetcher
  - this.deferSetState
  - <div hidden={true}>
  - Custom Caches
    - Why simple is simple // https://github.com/facebook/react/blob/master/packages/simple-cache-provider/src/SimpleCacheProvider.js#L148
    - Apollo/relay
- Community Usage
- Definitions
  - Suspense is a fundamentally new capability that lets you render a component tree “in background” while components are fetching data, and display them only after the whole tree is ready. For slow connections, it gives you full control over where and when to show a placeholder. https://twitter.com/dan_abramov/status/972838329367584768
  - Suspense lets you *delay* rendering the content for a few seconds until the whole tree is ready. It *doesn’t* destroy the previous view while this is happening. https://twitter.com/dan_abramov/status/970363058030772225
  - Optional/Opt In
  - Who worked on Suspense: https://twitter.com/dan_abramov/status/972856536073687040
- Use cases
  - ignoring the first state deferred state change if a second deferred state is set before the first one has completed https://twitter.com/karlrokeeffe/status/973449089693122560
  - catch promises from .read() in child components https://twitter.com/pete_gleeson/status/972954356269047808
  - Waterfall requests - Preloaders
  - Code splitting
  - Eager loading
  - Image loading
  - any component can do this without explicit coordination with parents. I can make any leaf component depend on something, and that constraint will “just work” wherever it’s rendered. https://twitter.com/dan_abramov/status/970363058030772225
  - Debouncing https://github.com/acdlite/react/blob/7166ce6d9b7973ddd5e06be9effdfaaeeff57ed6/packages/react-reconciler/src/__tests__/ReactSuspense-test.js#L573
  - Fallback https://github.com/acdlite/react/blob/7166ce6d9b7973ddd5e06be9effdfaaeeff57ed6/packages/react-reconciler/src/__tests__/ReactSuspense-test.js#L69
  - AsyncValue https://github.com/acdlite/react/blob/7166ce6d9b7973ddd5e06be9effdfaaeeff57ed6/packages/react-reconciler/src/__tests__/ReactSuspense-test.js#L452
  - Loading https://medium.com/@pete_gleeson/creating-suspense-in-react-16-2-dcf4cb1a683f
- Keywords
  - Committing: The new capability here is that any component can “suspend” the update that caused its render from “committing”. At least for a while. This was never possible before. https://twitter.com/dan_abramov/status/970363058030772225
In React, if a component suspends, we keep rendering the siblings as deeply as we can. Suspend means "don't commit," not "don't reconcile."  https://twitter.com/acdlite/status/970495397125750784
  Commit = change the DOM. This is usually fast. Reconcile = call the render method, find out what should be rendered, call the next render method, etc. This can take a decent amount of time on a deep tree. https://twitter.com/dan_abramov/status/970683705768513536
  - Expiration Time: We assign an “expiration time” based on update priority. For events like clicks, it’s ~1 second. So you’ll get a placeholder after a second even if you “ask” for a bigger delay. But deferredUpdates() lets you opt into a longer expiration time (~5 sec). https://twitter.com/dan_abramov/status/971092374691766273 source for 5s expiration time https://github.com/facebook/react/blob/e1a106a071dff17737a7afb49a0b71899d60c248/packages/react-reconciler/src/ReactFiberScheduler.js#L998 suspend rendering has an expiration: real_delay = Math.min(delay_prop, expiration_time). Expiration time depends on the kind of the update (lo pri updates are 5 seconds, high pri are 1 second) https://twitter.com/dan_abramov/status/971187182621872128 react groups into buckets by expiration time - avoiding starvation (https://twitter.com/acdlite/status/978413091460272129)
  - Cooperative scheduling: https://twitter.com/aweary/status/913619299087949824 2 images https://pbs.twimg.com/media/DK3TUnwU8AAbL9c.jpg lin clark https://www.youtube.com/watch?v=ZCuYPiUIONs
  - Idempotency: 
- Big Picture
  - We dont have to think about the Render
    - Crucially, React guarantees that the final state is always deterministic based on the order of updates. Intermediate states may vary according to resources, but eventually everything resolves to a predictable state. https://twitter.com/acdlite/status/978413273027526656
  - Coordinating Loading while staying interactive
  - Data shouldn't live in Component State
  - Imperative Boilerplate -> Declarative Framework
  - Coroutines https://twitter.com/acdlite/status/972542669040865280
    - coroutines vs generators https://medium.com/@tjholowaychuk/callbacks-vs-coroutines-174f1fe66127: Generators are sometimes referred to as “semicoroutines”, a more limited form of coroutine that may only yield to its caller. (more http://2ality.com/2015/03/es6-generators.html)
    - coroutines vs threads https://stackoverflow.com/questions/1934715/difference-between-a-coroutine-and-a-thread?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa
  - The new React API https://twitter.com/acdlite/status/974437383939743746 picture https://pbs.twimg.com/media/DYXlkUnVQAE--gD.jpg
  - Civilization advances by extending the number of important operations which we can perform without thinking about them. 
  
Resources
- Dan's Videos
  - JSConf: https://www.youtube.com/watch?v=v6iR3Zk4oDY
  - ReactFest: https://www.youtube.com/watch?v=6g3g0Q_XVb4
- Code
  - https://github.com/facebook/react/pull/12279
  - React suspense tests: https://github.com/acdlite/react/blob/7166ce6d9b7973ddd5e06be9effdfaaeeff57ed6/packages/react-reconciler/src/__tests__/ReactSuspense-test.js
  - Async React With Non React views https://gist.github.com/acdlite/f31becd03e2f5feb9b4b22267a58bc1f demo here https://build-mbfootjxoo.now.sh/
- Talk Responses from the team
  - https://www.reddit.com/r/reactjs/comments/814huj/so_what_do_you_all_think_about_react_suspense/
  - https://twitter.com/dan_abramov/status/970363058030772225
- community code
  - Demo Clone 1 https://github.com/karl/react-async-io-testbed
  - Demo Clone 2 https://github.com/BenoitZugmeyer/react-suspense-demo
  - Trial usage: https://github.com/BlackBoxVision/react-suspense-playground
  - Trial usage: https://github.com/birkir/react-suspense-demo
  - Trial usage: https://github.com/Baransu/react-suspense-demo
  - Trial usage: https://codesandbox.io/s/github/jaredpalmer/react-suspense-playground
  - Trial usage: https://github.com/horaciosystem/react-suspense-pokedex
  - Async React + Apollo: https://dev-blog.apollodata.com/a-first-look-at-async-react-apollo-10a82907b48e
  - Auth0: https://auth0.com/blog/time-slice-suspense-react16/
  - Userland Suspense: https://github.com/petegleeson/create-create-fetcher/blob/master/src/future/index.js
  There is: rendering doesn’t proceed with other siblings once you have the first error. But we need to render as much as possible. @acdlite rewrote error handling logic a week ago for that reason. It also forced deletion of all children instead of pausing. https://twitter.com/dan_abramov/status/969214785194688512
  - HOC: https://twitter.com/dan_abramov/status/970363058030772225
- suspense ready libs
  - https://github.com/pomber/hitchcock (not yet released)
  - https://github.com/didierfranc/drum-roll (note: problems - https://twitter.com/dan_abramov/status/972214161043271681)
  - https://github.com/reactions/fetch
- community 
  - https://medium.com/@lmatteis/react-suspense-for-the-layman-caae7f48686f
  - https://dev.to/swyx/a-walkthrough-of-that-react-suspense-demo--4j6a
  - https://medium.com/@baphemot/understanding-react-suspense-1c73b4b0b1e6
  - https://medium.com/@pete_gleeson/creating-suspense-in-react-16-2-dcf4cb1a683f
  - https://auth0.com/blog/time-slice-suspense-react16/
  - Userland Suspense https://twitter.com/pomber/status/969287602078736384
  - Harry Wolff https://www.youtube.com/watch?v=U1CpNtVdxM4
  - Ryan Florence https://www.youtube.com/watch?v=KyKvlnNGDxk
  - Preloaders https://www.youtube.com/watch?v=KyKvlnNGDxk
  - SSR https://blogg.svt.se/svti/react-suspense-server-rendering/
  - Redux https://github.com/reactjs/react-redux/issues/890#issuecomment-370521609
