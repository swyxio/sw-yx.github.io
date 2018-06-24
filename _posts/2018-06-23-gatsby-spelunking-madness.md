---
layout: post
date: 2018-06-23
tags: gatsby
feelings: determined
title: gatsby spelunking madness
comments: true
description: gatsby spelunking
---

after my recent image linking troubles (https://github.com/gatsbyjs/gatsby/issues/6112) i decided to dive into the gatsby source code and wrote this gist:

https://gist.github.com/sw-yx/09306ec03df7b4cd8e7469bb74c078fb

redux makes for fairly convoluted code and the lifecycles are split out all over the place. but its not impossible to patiently go through it. i guess. i am no closer to figuring out my answer but i know why my solutions so far have not worked. the stage i want (post gatsby-plugin-sharp) is not the stage i am operating at (source and transform nodes)
