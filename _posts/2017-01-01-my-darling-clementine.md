---
layout: post
title: "my darling clementine"
date: 2017-01-01
tags: javascript freecodecamp clementine
feelings: tired ambitious
---

Happy new year!

I am posting my daily coding adventures here. Let's hope it goes well.

Today I completed freecodecamp's d3 challenges. 

I need to learn React to complete the Data Viz certification but I think that is of secondary importance to gaining some expertise in full stack backend. 

I think if I can create clementine.js every day for January I will have this in my head.

[Here](http://www.clementinejs.com/tutorials/tutorial-beginner.html) is their very excellent tutorial. Use cloud9.io to test it out.

---

it is 8pm and i have spent the rest of the afternoon recreating clementine.js (including [passport.js](http://www.clementinejs.com/tutorials/tutorial-passport.html)) and deploying to heroku. heroku gave a lot of issues and i should note down the trip-ups here:

1. the top problem in deploying to heroku in search results is not changing the port. so instead of `app.listen(8080)` you should do var `app.listen(process.env.PORT || 8080)`. easy enough.
2.  i have had no perceptible success with deploying heroku configs, eg `heroku config:set MONGOLAB_URI=mongodb://swyx:Qt72nWPyNByw@ds111798.mlab.com:11798/heroku_p414n7r6`. i have defaulted to using `.env`
3. in passport.js there is a callback function that stores your URL. when switching from a development environment like cloud9 to a deployment environment like heroku you need to change that config.

I don't understand passport.js in much great detail having followed the tutorial in pretty much copy and paste fashion. But I was at leaast able to handle the js errors that appeared along the way.
