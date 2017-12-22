---
layout: post
date: 2017-01-26
title: What to do when you hit a dead end
categories: freecodecamp react
tags: tired
---

still working on https://www.freecodecamp.com/challenges/build-a-nightlife-coordination-app

I haven't gotten any significant breakthrough overnight on my problem from yesterday. I have three plan B's:

- retry with no redirect for login
- retry with vue
- retry with gomix

Time to get to work.

---

3.00ish here. ok now this is a breakthrough! i ended up not pursuing any of my plan B's.

- step 1: i found this tutorial combining react and authentication: <https://vladimirponomarev.com/blog/authentication-in-react-apps-jwt>. I applied it mindlessly and i do owe it to myself to break this down as much as possible
- step 2: i deployed it on heroku using the knowledge I gained from [Fullstack React](https://www.fullstackreact.com/articles/using-create-react-app-with-a-server/) (running two servers) and my prior work in [the clementinejs tutorial](http://www.clementinejs.com/tutorials/tutorial-heroku.html) (hooking up an mlab database on heroku

with these two steps I think I have the missing pieces. I need to verify that I can deploy and access an API.

---

5.00 here. I have deployed the yelp api on top of the steps above and at this point i have everything i need to complete this thing. I just need to wire it up. the tricky thing here is that this method uses react router and so you cannot test the api's you set up on a separate development server. you need to run it together with the react app which isn't the worst thing in the world. its been a long day and i will tackle this tomorrow.

Here are some inspirations:

---

inspirations:

- http://worrydream.com/# ([Bret Victor's famous talk](https://vimeo.com/36579366) spawned [Light Table](http://www.chris-granger.com/lighttable/)) He has a reading list [here](https://gist.github.com/nickloewen/10565777)
- <https://github.com/arshdkhn1/foodiebay> deployed <https://foodiebay.herokuapp.com>
- <https://github.com/MarkoN95/Nightlife-App> deployed <https://food-out.herokuapp.com/>
