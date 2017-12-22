---
layout: post
date: 2017-01-22
title: Tweaking the Food Lookup Boilerplate
categories: react node mongoose express
tags: optimistic
---

10.40. So yesterday was a big success day with the resolution of the FCC Data Viz certification and I finally found a working deployable full stack React + Express boilerplate.

Just to recap my problem - it is easy to get stuff working on local machine but then there are all sorts of esoteric configurations you need to take care of when you deploy to heroku and just doing it by yourself is impossible So you start off going through these:

- <https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md#heroku
- <https://github.com/mars/create-react-app-buildpack>
- <https://www.akawebdesign.com/2016/11/30/combining-create-react-app-express/>

and then finally settle on

- <https://www.fullstackreact.com/articles/using-create-react-app-with-a-server/>

The next things to do in FCC are the backend projects but I am dragging my feet because I honestly dont think I have solidified the fullstack react environment and I need a smaller project that has less dependencies that I can deploy. So I am going to do my work analytics project for now.

see you in a bit

---

2.50. so i have digressed quite a bit and honestly its a miracle i got anything done because of all the extra reading i have been putting in. but i successfully injected my data and modified the base react template (from fullstackreact as above) to display and more importantly DEPLOY on heroku (after some confusion with git branches and merging and how being on a non master branch locally doesnt translate to the  master branch on heroku). anyway. I like my digressions because this journey is as much aboud the *trade* of being a coder as it is the raw skills. Dan Luu's blog <http://dannluu.com> was particularly interesting to me as was [RC Start](https://www.recurse.com/blog/99-free-one-on-one-mentorship-for-new-programmers) which is unforuntately closed but should reopen again. I think I am also due for a reality check regarding software engineering style and [Sourcemaking](https://sourcemaking.com/) cant be too wrong.

I have also discovered [duet display](https://www.duetdisplay.com) which turns the ipad into a second screen. great buy and it works out of the box. 
