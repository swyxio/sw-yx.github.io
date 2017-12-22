---
layout: post
date: 2017-07-13
title: finishing packagejason
tags: projects
feelings: fat
---

Ok time to polish up and deploy this sumbitch. Today's tasks:

- prettify the stats output with some charting
- deploy (maybe heroku)

---

i prettified the stats but had some trouble with the tweet this buton which is surprisingly not so obvious when in react. I looked around and it seems <https://github.com/olahol/react-social> is the best method although the display isn't that great.

---

and we are done! <https://packagejason.herokuapp.com>

![demo](https://github.com/sw-yx/packageJason/blob/master/public/fulldemo1.gif?raw=true)

It is obviously not well designed (a slack acquaintance pointed out the margin issues which i couldnt quite resolve before pushing) but it is passable and has emojis to show scores!!! (that math involved using `tanh` functions and scaling things down from real number space to probability space for display, then to discrete integer space for the emoji mapping...)

Hope to get some feedback tonight.
