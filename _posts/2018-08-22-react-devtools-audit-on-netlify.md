---
layout: post
date: 2018-08-22
tags: react
feelings: happy
title: react devtools audit on netlify
comments: true
description: playing with brian's react devtools on a new codebase
---

attended reactnyc today. it was good, nice talk at the end by alex holachek presenting her react-flip-toolkit. so many toys so little time.

i got a walkthru of brian vaughn's [new devtools](https://github.com/facebook/react-devtools/issues/1099) at React Rally so i figured i could apply it on netlify's react app ui. irene pointed me to the log where it was hanging for big logs. i replicated the issue with [this repo](https://github.com/sw-yx/netlify-generate-big-log-for-perf-devtools/blob/master/index.js) and then fired up the react app ui and profiled. i found that the number of commits  was O(n) with the log length so that was obviously the problem. we need to debounce the thing somehow as it is streaming in piecemeal from firebase.
