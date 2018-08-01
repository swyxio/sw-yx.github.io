---
layout: post
date: 2018-08-01
tags: react
feelings: neutral
title: react rally grind 5
comments: true
description: looking into synthetic events!
---

checked out Sorry To Bother You, really great trippy film. also moviepass might be dying so i am using them more haha.

i wanted to make some points to do with synthetic events so i looked into https://www.youtube.com/watch?v=dRo_egw7tBc

the reasons for having an event system are:

- xbrowser naming inconsistencies
  - mousescroll
  - some ie stuff
  - firefox function key keyup has an event...
- batching (eg keyUp can also generate a keyUpEvent and changeEvent so you want to batch those as one update)
- onChange <- is custom
- adding events to dom nodes is slow? (from @theKashey)
- mouseEvents
  - override button event for ie8 compatibility
  - pageX/Y - polyfill based on scrollposition + clientX/Y if not exists
- trycatch on events - to catch when your callback errors and you want to rethrow it later
  
TraverseTwophase

- accumulate directional event list
- traverseTwoPhase - > literally goes down and then up
- in dev, react actually creates and element of type 'react' and throws the event onto it! https://youtu.be/dRo_egw7tBc?t=58m22s wtf
