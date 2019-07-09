---
layout: post
date: 2019-07-09
tags: netlify
feelings: determined
title: nextjs on netlify
comments: true
description: netlify options
---


dwells helped me out with a rabbit hole thing yesterday:

> thanks @DWells for digging me out of a pit of despair: first listening to my question, working through it, figuring out it was the wrong question, then finding the answer almost right away thru his googlefu. absolute legend.


https://community.netlify.com/t/serverless-next-js-9-on-netlify-functions/1956

now i am trying to see fi i can make a little cli as i think there is still holes in this experience

https://github.com/netlify-labs/netlify-nextjs

what [motto](https://github.com/mottox2-sandbox/next-on-netlify/tree/master/src/functions) did was:

- build nextjs -> it builds serverless and static stuff
- inject the serverless wrappers
- run `netlify-lambda` over each endpoint to bundle everything

we can imagine a two stage evolution of how a `netlify-nextjs` layer can go.

- all in one
  - everything in ONE function, copy the whole serverless pages folder into that one function
  - at the root, a handler for that function with the compat layer, that dynamically routes each request
  - in this case we copy our handler OUT
  - shit for performance but probably simpler and proves out demand
- one by one 
  - every function has its home
  - copy (adapt?) each compat handler to require each
  - possibly run `netlify-lambda` vif shared files are required
