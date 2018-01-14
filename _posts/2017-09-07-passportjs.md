---
layout: post
date: 2017-09-07
title: passport js the missing manual
categories: passportjs
tags: determined
---

today i led a small group of students to go over auther, the express-session and passport.js workshop in the class that most of them had skipped. auth is my white whale and i decided to take it head on and also learn-by-teaching. i prepared by doing the authentication part of the boilermaker thing and then pretty much just led the class for 3 straight hours. people got exhausted and asked for two breaks and also we got hung up a lot on es6 and react-redux details. all in all it was a very very tough thing to do and could have used better preparation and rehearsal but i still got applause at the end and i think everyone walked away a little more confident. i also did a quick react native primer because why not.


i think auth is a big pain point and there is a lack of education around it. as unexciting as it is, i think it could be fertile ground for a video tutorial series.

---

# notes about passport

### What happens when I click login with google?

- Auther redirects me to google's login
- I login with google
- Google redirects me back to Auther (at the callback URL)
- Auther verifies with google
- Google passes my profile back to Auther
- When a user logs in, passport "serializes" them: stores some unique identifying information (often id) on the session.

"Deserialization" occurs on every request, it uses that piece of information to retrieve an actual user and then attaches that user to the request request.user.

In any middleware downstream of your passport session middleware, you will have access to request.user which represents the user making the request!

Because of how we've set it up, request.user will exist for those that logged in with OAuth or those that logged in locally.
