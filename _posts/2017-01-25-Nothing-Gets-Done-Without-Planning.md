---
layout: post
title: Nothing Gets Done Without Planning
date: 2017-01-25
tags: freecodecamp react express node
feelings: optimistic
---

still working on <https://www.freecodecamp.com/challenges/build-a-nightlife-coordination-app>

As apps and data models get more complicated the need for proper planning increases. I spent a few hours in low-productivity fuckery incrementally adding things today until I realized it was getting a little out of hand and I need to sit down and do some proper planning.

It is interesting that there are two layers of planning now:

- Frontend planning: the structure of the clientside app implemented by React - what components are needed? what states are stored?
- Backend planning: the routing but also the structure of the api's that are needed.

after so much practice in the last few days frontend planning is quite second nature to me (at least for SPA's). Backend is where I am getting some confusion and so I will lay out the API structure needed.

Controllers

- viewUser: when user is logged in, show what he has clicked on. if new user, create user
- addBarToUser: when user is logged in, add to barIDs
- removeBarFromUser: when user is logged in, remove bar from barIDs
- getClicks: given a barID, scan all users and list the users that match.

Models

- User: userid, barIDs

Routing

- app.route('/api/clicks').get(clickHandler.getClicks) // params req.query.barid
- app.route('/api/clicks/add').get(clickHandler.addBarToUser) // params req.query.uid, barid
- app.route('/api/clicks/remove').get(clickHandler.removeBarFromUser) // params req.query.uid, barid
- app.route('/api/user').get(clickHandler.viewUser) // params  req.query.uid || req.user.twitter.username

i like this structure rather than the two-model setup i made in the voting app. do more work in the user view rather than the bar view.

---

so today was one of those brick wall days. core of it was problems with running create-react-app alongside express routing when I dont truly understand every step and the black magic bit hard today. I couldnt figure out how to server a login.html file alongside the index.html file I normally serve. I have logged my problems here: 

- https://github.com/fullstackreact/food-lookup-demo/issues/19
- http://stackoverflow.com/questions/41859154/webpack-somehow-bypasses-express-routing-completely

And these are the relevant resources I have found so far:

- https://blog.hellojs.org/setting-up-your-react-es6-development-environment-with-webpack-express-and-babel-e2a53994ade#.t8eq2inb8
- https://medium.com/@patriciolpezjuri/using-create-react-app-with-react-router-express-js-8fa658bf892d#.6y4rrl61q
