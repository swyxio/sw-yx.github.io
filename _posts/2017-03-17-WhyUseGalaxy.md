---
layout: post
title: I just finished FreeCodeCamp's Backend Certification
tags: meteor freecodecamp
feelings: happy
date: 2017-03-17
---

notes on For Implementing [masonry.js](http://masonry.desandro.com/) inside my meteor application:

- add css to `client/main.html` header
- add `<script src="/node_modules/masonry-layout/dist/masonry.pkgd.min.js"></script>` to react component (documentslist.js)
- use the inline html method as that is the most direct way to get it working

My backend requirements are completed!

- <https://wholesomepinterest.herokuapp.com/>
- <https://github.com/sw-yx/fcc-pinterest>

Boom. Now I sleep.

Misc
---

Points from meteorchef/koleok on why to use galaxy
```
koleok [10 hours ago] 
swyx: Basically just that with galaxy you:
- don’t need to set up a load balancer
- have built in rolling deployment automation
- basically get certificates/https abstracted away
- get as many `*.meteorapp.com` urls as you want _(which is what i use for review apps)_

But I have no stake in selling anyone on galaxy, you can do the gitlab ci/review-app thing with wherever you host including heroku
```
