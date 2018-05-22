---
layout: post
date: 2018-05-21
tags: talks
feelings: neutral
title: creating create react app
comments: true
description: notes for giving my talk
---

# 4. Poll the Audience

who has set up webpack+babel+react manually?
who's used CRA?
who clones a boilerplate? other?

# 1. Start with Why (Have a Goal)

hello world: https://twitter.com/thomasfuchs/status/708675139253174273
Javascript fatigue: https://medium.com/@ericclemmons/javascript-fatigue-48d4011b6fc4

convention <-> configuration
limited / rails <-> power / decision fatigue
framework <-> language

HN announcement: https://news.ycombinator.com/item?id=12144371
fb post: https://reactjs.org/blog/2016/07/22/create-apps-with-no-configuration.html
FB live: https://www.facebook.com/react/videos/1022638037814603/
tweet: https://twitter.com/dan_abramov/status/752863664290553856 and "blessed learning resources" https://twitter.com/dan_abramov/status/752283652982185984 and 
Ember CLI is really cool. Autodiscovery of dependencies and no config: React community could learn from this.

https://twitter.com/dan_abramov/status/750755537239871488
Pit of Success is not enough. Mount of Failure is just as important. Unmaintainable code should be hard to write.

react-boilerplate
https://twitter.com/dan_abramov/status/750332439856312320

useful searches?
from:dan_abramov since:2016-07-12 until:2016-07-20
from:vjeux since:2016-07-12 until:2016-07-20

previous:

react starter kit: https://twitter.com/dan_abramov/status/755169928412590083
react boilerplate dec 2015: https://news.ycombinator.com/item?id=10794502
nwb: https://github.com/insin/nwb
enclave: https://github.com/eanplatter/enclave
budo: https://github.com/mattdesl/budo
prot: https://github.com/mattdesl/prot
rackt-cli: https://github.com/mzabriskie/rackt-cli
rwb: https://github.com/petehunt/rwb


https://pbs.twimg.com/media/CouqBE9UsAASqrW.jpg
https://twitter.com/erikras/status/759887903006658561

create create react apps

# 2. Make Them Laugh (Entertain)

standards: https://imgs.xkcd.com/comics/standards.png

# 3. Show some facts (establish credibility, relevance)

“I very frequently get the question: 'What's going to change in the next 10 years?' And that is a very interesting question; it's a very common one. I almost never get the question: 'What's not going to change in the next 10 years?' And I submit to you that that second question is actually the more important of the two -- because you can build a business strategy around the things that are stable in time. ... [I]n our retail business, we know that customers want low prices, and I know that's going to be true 10 years from now. They want fast delivery; they want vast selection. It's impossible to imagine a future 10 years from now where a customer comes up and says, 'Jeff I love Amazon; I just wish the prices were a little higher,' [or] 'I love Amazon; I just wish you'd deliver a little more slowly.' Impossible. And so the effort we put into those things, spinning those things up, we know the energy we put into it today will still be paying off dividends for our customers 10 years from now. When you have something that you know is true, even over the long term, you can afford to put a lot of energy into it.”
 
Jeff Bezos https://www.goodreads.com/quotes/966699-i-very-frequently-get-the-question-what-s-going-to-change

CRA users:
- https://twitter.com/rachsmithtweets/status/768862577451343873
- https://twitter.com/wesbos/status/768966170095579136
- https://twitter.com/tkh44/status/776468426579701760 webstorm
- https://twitter.com/somebody32/status/764805492505997312
- https://twitter.com/MicheleBertoli/status/772827892099211265
- https://twitter.com/darryldexter/status/805599059205558272
- https://twitter.com/mxstbr/status/786494075247538176
- https://twitter.com/Elijah_Meeks/status/791362890431205376

how it feels to learn js: https://twitter.com/mipearson/status/783406757745786880


# 5. Answer the poll (more facts)

- tasks
 - cra.js
- packages
 - create-react-app
 - react-scripts
  - bin/react-scripts.js => '../scripts'
  - scripts/init.js
  - template/*
  - scripts/start.js
  - config/webpack.config.dev
  - scripts/build.js
  - config/webpack.config.prod.js
  - scripts/eject.js
 - react-error-overlay
 - babel-preset-react-app
 
PUBLISHING THE DAMN THING

- https://lernajs.io/

# 6. Show what they can do ( 💰 shot )

https://github.com/sw-yx/create-react-app-parcel

- tasks
 - cra.js
- packages
 - create-react-app
 - react-scripts
  - bin/react-scripts.js => '../scripts'
  - scripts/init.js
  - template/*
  - scripts/start.js
  - config/webpack.config.dev
  - scripts/build.js
  - config/webpack.config.prod.js
  - scripts/eject.js
 - react-error-overlay
 - babel-preset-react-app

# Parcel is awesome

faster?
zeroconfig?

# MAKING CRAP

- https://github.com/sw-yx/create-react-app-parcel/blob/master/packages/react-scripts-parcel/scripts/start.js#L106

--- 

7. Reach the Goal

stars: https://www.javascriptstuff.com/create-react-app/

8. Leave breadcrumbs

sunil pai: good dx is good ux: https://www.youtube.com/watch?v=WYWVGQKnz5M
five things about CRA: https://www.youtube.com/watch?v=9t2GWFegnkQ
