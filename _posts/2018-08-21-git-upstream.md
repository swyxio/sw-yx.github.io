---
layout: post
date: 2018-08-21
tags: git
feelings: neutral
title: git upstream notes
comments: true
description: working on a fork of a moving target
---

# working on a fork of a moving target

i know this is something i need to do so here are some notes on it

this was super helpful https://stackoverflow.com/questions/8948803/what-does-git-remote-add-upstream-help-achieve

the precise instructions i use are:

```js
git remote add upstream https://github.com/netlify/netlify-www.git

git fetch upstream

git checkout master

git rebase upstream/master

git checkout 100-retweet-bugfix
```

i will alias the middle 3 as `gupdatefork`

