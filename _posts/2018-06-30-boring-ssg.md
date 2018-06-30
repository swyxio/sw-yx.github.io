---
layout: post
date: 2018-06-30
tags: netlify
feelings: neutral
title: boring ssg
comments: true
description: a boring ssg
---

more notes on boring ssg

- so i am taking fixtures/ssr as my reference and making a [boring-SSG](https://github.com/sw-yx/boring-SSG)
- babel-register shoudl work but doesnt, i dont know what i am doing wrong. [filed a bug](https://github.com/babel/babel/issues/8241) but dont have high hopes
- just adding an extra "env" preset fixed it i think. i was going to try [this starter file method](https://timonweb.com/posts/how-to-enable-es6-imports-in-nodejs/) next. [filed a followup comment](https://github.com/babel/babel/issues/8241)
- **ok so i can output HTML! woo!** 🎉
- the hydrate doesnt work because i need to send it along as javascript separate from the HTML.
- now adding parcel to the bundle to make the js bundle
