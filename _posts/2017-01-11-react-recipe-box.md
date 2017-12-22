---
layout: post
date: 2017-01-11
title: peeking at react recipe box
categories: building
tags: distracted
---

i have been procrastinating all night and probably wont get much done. lets crank out something. no zero days.

---

back and i have mapped out the basic [fcc recipe list](http://codepen.io/swyx/pen/KazMLQ) by hand. it was a great exercise showing i truly understand everything going on at the React level although I did have a few slip ups. in particular i had to understand that my consts go inside the render() because it is not a class member. boom. i think for this one i will really want to practice my bootstrap skills so I am leaving that as my exercise for tomorrow to pretty it up. then i have to code up:

- some functionality for editing individual recipes
- adding recipes
- deleting recipes
- show/hide recipes
- transitions
- finally - store in local browser storage to ensure persistence

the user was obviously meant to do this before the [voting app](swyx.io/blog/2017/01/03/its-voting-day), but I did this the other way round and so kept most of my logic on server side which I now realize was not a great way to do it. we grow and learn.
