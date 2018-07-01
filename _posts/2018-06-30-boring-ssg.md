---
layout: post
date: 2018-06-30
tags: netlify
feelings: neutral
title: boring ssg
comments: true
description: a boring ssg
---

more notes on boring ssg

- so i am taking fixtures/ssr as my reference and making a [boring-SSG](https://github.com/sw-yx/boring-SSG)
- babel-register shoudl work but doesnt, i dont know what i am doing wrong. [filed a bug](https://github.com/babel/babel/issues/8241) but dont have high hopes
- just adding an extra "env" preset fixed it i think. i was going to try [this starter file method](https://timonweb.com/posts/how-to-enable-es6-imports-in-nodejs/) next. [filed a followup comment](https://github.com/babel/babel/issues/8241)
- **ok so i can output HTML! woo!** 🎉
- the hydrate doesnt work because i need to send it along as javascript separate from the HTML.
- now adding parcel to the bundle to make the js bundle
- had an [issue with class properties](https://github.com/parcel-bundler/parcel/issues/867) definitely doesnt make me feel great about parcel yet. but it bundles!
- the problem is parcel bundles to specific hashed chunks which i cant predeict when i send it out. so i need to somehow inject the chunk name into my html render
- async/await to the rescue. im just popping off the name of the first childBundle which is the js but i know that is going to be volatile. we'll see how to fix that
- ok i got the js loading to work from html but now parcel's assumptions are coming to bite me: "Uncaught TypeError: Super expression must either be null or a function, not undefined"
- that seems to be a bug in parcel :(  changing the Chrome class to a function fixes it
- aafter a bit more tweaking I HAVE A REHYDRATING APP!! WOO!!

pm session

- caught in a weird memory crash/loop when i comment out the assetManifest script which doesnt seem important at all. the commenting out may be unrelated
- i may have found an infinite loop/memory leak. i dont know how this is my fault.
- started stepping through react but honestly im probably not gonna get anywhere with that. what to do?
- nuked the directory and retried
- it works! now for routing
- reading <https://reacttraining.com/react-router/web/guides/server-rendering>
- reading <https://reach.tech/router/server-rendering>
- tried adding reach/router into my app. not much luck, i get a weird "Uncaught TypeError: Cannot read property 'createContext' of undefined" error.
- find related issue, file PR to maybe bump react version <https://github.com/reach/router/pull/92>
- try it myself, nope.
- try it on C-R-A-P - reach/router works fine. damn.
- attempted filing an issue on reach/router or on parcel, balked.
- ok i think its a reach/router rollup issue. just filed it <https://github.com/reach/router/issues/93> no hopes of being answered
- i monkey patched it in node modules but the problems are chaining. not good. now I have "Uncaught TypeError: Cannot read property 'ReactCurrentOwner' of undefined"
- i tried just using "straight parcel" and my app works. so its definitely something to do with me SSRing an app. potentially with babel-register.
- trying to upgrade to babel 7 and got a variety of errors.
- trying babel node after seeing artsy run into this: http://artsy.github.io/blog/2017/11/27/Babel-7-and-TypeScript/
- had a few issues but [this note from logan](https://github.com/babel/babel/issues/6130#issuecomment-324495961) helped me get over the hump?
- YAY I HAVE A REHYDRATING REACH ROUTER APP

evening session

- ok so even though the router works on one page, i should really try to generate multiple pages. at this point tho the tech risk is going way down because i know i can programmatically generate pages, and the logic is well within my control rather than with libraries i don't know.
- the thing about parcel is it starts with an entry index.html. we may have to flex that if we ever want it to produce different pages. (we dont technically have to - there is another way where we let it chunk everything and we figure out the events to hook into to generate pages)
- actually rereading the [getting started](https://parceljs.org/getting_started.html) it doesnt have to start with html...
- ok so `index.js` is now my entry asset and i moved it into dist. i could very conceivably make a lot more different entry assets with that.
- wasted some time looking into how suspense helps with loading. i really wish i could use this now. but i will have zero help if i run into a bug.
- time to shift everything into a pages mentality. i am using "test1" as a de facto library.
- its apretty big move. i think i screwed up, i dont know where i'm supposed to bundle the hydrater
- parcel insists on having a file to bundle. can i convince it otherwise?
- i could do some nasty file manipulation stuff but i dont want to. i am going to try combining the hydrater
- no it can't be done. the dynamic important i would need for it to work can't be statically analyzed and so can't be bundled for rehydration. i get a `'../../src/pages/index.js' module not found` type error. so i need to have a cahce and i need to do file manipulation
- ok i am doing a big rewrite of the node process so that i can acommodate the dynamic page generation. i am also using react-static's config so that i dont have to come up with something new for now. BORING IS GOOD.
- got the writeout working
- i have to write out a js file to bundle
- ok i had some issues there but it works, now to bundle
- now to write the html
- i have things rendering and rehydrating, but my routes are broken
- i am putting in a Layout component, but i need my components to know what path they are on
- i think i had the wrong model of SSRing - i was SSRing individual pages, but the right way is to SSR one page with different routes being rendered. i was going for one route one bundle, when i should be going for multiple routes one bundle. this makes a ton more sense, i dont know what i was thinking
- i did the rewrite, deleted a lot of code. it works now but my testing environment uses `serve` which gives 404 when i refresh on a url.
- `serve` is not picking up `serve.json` so i am going to use an express server
- YES i can actually demo refreshes!
- ok i worked out some minor kinks with index and now i am happy with the refresh behavior.

