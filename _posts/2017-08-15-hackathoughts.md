---
layout: post
title: hackathoughts and bufferclone work
data: 2017-08-15
categories: fsa
tags: meh
---

today we did a checkpoint on express sequelize and then did a hardware hackathon with <tessel.io>. i didnt really enjoy it but it was nice to work on something else other than webapps for a while.


---

I worked a bit on the bufferclone today. unfrotunately i did not get too far as I ran into a poorly configured passport issue <https://github.com/jaredhanson/passport-twitter/issues/86> which shows a hole in the documentation at [passport](http://passportjs.org/docs) and at [passport-twitter](https://github.com/jaredhanson/passport-twitter). I need to redo this from scratch and follow a proper passportjs tutorial. some examples:

- <http://mherman.org/blog/2015/01/31/local-authentication-with-passport-and-express-4/>
- <https://scotch.io/tutorials/easy-node-authentication-setup-and-local>
- <http://moonlitscript.com/post.cfm/how-to-use-oauth-and-twitter-in-your-node-js-expressjs-app/>
- This could be the simple one to try <http://blog.joeandrieu.com/2012/01/30/the-worlds-simplest-autotweeter-in-node-js/>

Twitter app: <https://apps.twitter.com/app/14108791/settings>
