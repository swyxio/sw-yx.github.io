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
- 
