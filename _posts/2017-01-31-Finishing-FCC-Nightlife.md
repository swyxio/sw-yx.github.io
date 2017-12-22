---
layout: post
date: 2017-01-31
title: Finishing FCC Nightlife
categories: freecodecamp react node
tags: full
---

4.00 here. The only way to break out of a nonproductive streak is to force your self to JUST DO IT. 

I [last left my project](http://swyx.io/blog/2017/01/27/No-Zero-Days) with a working Yelp api but no database/user model. I have implemented it in the prior attempt so I won't need to implement from scratch so here goes.

---

5.50 here. so the problems i have run into are

- the react-router api i use doesnt pass the Passport.js user authentication at all, so everything has to be placed as a parameter in the GET request. in order to do this I have had to put the userid in localStorage to be retrieved every time I make a request. It feels a little hacky but at the same time is probably no difference in security.
- Material-UI implements its own onClick functions and really doesnt like you trying to muscle in on its territory. So one has to use [onRowSelection](http://www.material-ui.com/#/components/table). Given this I have had to reconsider the design of the app in showing the selected bars - previously I was going to have selected bars separate from the bar list, but now I am considering just using the checkboxes native to Material-UI.

taking a pause.
