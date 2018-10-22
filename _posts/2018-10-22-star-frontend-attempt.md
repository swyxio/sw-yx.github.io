---
layout: post
date: 2018-10-22
tags: writing
feelings: neutral
title: star frontends failed first attempt
comments: true
description: i couldnt muster up the gusto to finish this
---

i couldnt muster up the gusto to finish this. last minute i realized i wanted to include SSR/SSG frameworks like gatsby and nextjs but that didnt fit in STAR. back to the drawing board.


---
title: STAR Frontends: Styled-Components, Typescript, Apollo GraphQL, and React
published: false
description: Some approaches to styling, static typing, data fetching, and declarative components are gaining mindshare in the wonderfully competitive frontend world, especially among tech companies.
tags: styled-components, typescript, graphql, react
---

## What are STAR Frontends?

Recently, I was trying to sum up the state of the frontend world in 2018 to bootcamp graduates I was speaking to. 

Right now, I can identify four trends that tech companies are investing in:

- Localized **Styling**
- Static **Typing**
- GraphQL-based **Data** Layer, 
- **Reusable components** in a Design System

As it stands right now, the leading solutions for each of these 4 concerns are:

- [**S**tyled-Components](https://styled-components.com)
- [**T**ypescript](https://www.typescriptlang.org/)
- [**A**pollo GraphQL](http://apollographql.com)
- [**R**eact.js](https://reactjs.org)

These conveniently spell **STAR**, which makes for a catchy title :) but to be clear, I am writing more about the ideas behind the solution. For example, these are perfectly worthwhile alternatives:

- [Emotion.sh](http://emotion.sh)
- [Flow](https://flow.org/)
- [Relay](https://facebook.github.io/relay/)
- [Vue](http://vuejs.org)

But so long as you care about Styling, Typing, Data and Reusable components, you have a STAR frontend as far as I am concerned.

## Why care what others think?

In the conferences that I go to and the conversations I engage in on Hacker News, Reddit and Twitter, certain technologies keep popping up again and again. 

There are some that make for super fun blogposts and talks and demos and are perpetually "the next big thing", like VR and WebAssembly. Their time may yet come.

Then there are others where devs at large tech companies are saying **"no seriously folks we're adopting this thing right now and it's really good here's why"**.

We drink from a firehose of content these days, and it can be hard to remember what you read five minutes ago, but remember this: Those two kinds of content may take up the same airtime, but they are not made equal. 

Tally up the instances when companies say they are adopting a thing to get a sense of where the big investment is going. Where things have been tested, and are being put into production at scale. Where the jobs will be. Where your fellow managers and team leads are also investing their scarce engineering resources.

Monoculture, cargo-culting and groupthink are not healthy. At the same time, it is silly to avoid reading the room. Both extremes are good reasons for spending some time understanding why the consensus is what it is.



typescript
https://slack.engineering/typescript-at-slack-a81307fa288d
http://neugierig.org/software/blog/2018/09/typescript-at-google.html

apollo
https://medium.com/airbnb-engineering/reconciling-graphql-and-thrift-at-airbnb-a97e8d290712

react
https://eng.uber.com/tech-stack-part-two/

styled-components
https://engineering.coinbase.com/introducing-coinbase-open-source-fund-116617a1f6ec



## Disclaimer

I almost didn't write this piece.

For some reason it is hard to discuss technology adoption without also seeming to diss alternatives to that technology. Selective highlighting of fact is confused for straw-man opinion and blanket recommendation. This makes it very hard to have any sort of "real talk". 

If you want to proceed, please understand a few things:

- I care more about the ideas represented by a library than the library itself. The library may not last but the idea might. We learn a lot from understanding why the idea is taking hold, which is why it is worth discussing.
- I categorically do not recommend that everyone adopt this stack. Very valid and very good alternative approaches exist. This is an observation that these technologies are popular right now, and I am trying to explain how these things fit together. It is entirely reasonable that teams and use cases differ from company to company and therefore there is no one best stack for everyone. Duh, I know.
- Lastly: ***What follows is personal observation and not the view of my employer.***

Someday we might be able to discuss technology on the internet without these disclaimers. But this is not that day.
