---
layout: post
date: 2018-07-18
tags: react
feelings: happy
title: react pr warnings
comments: true
description: an assignment from dan
---

one of my guiltiest things is that i have only contributed to react once (i have 2 outstanding PRs) and dan has hinted to me repeatedly that i should get more involved.

today he just straight up assigned this to me so i'm doing it

---

>I recently made it so that warning() calls automatically attach the stack. It’s available right in that module

> Now that we always have stack info, we can expose it to other tools consistently too

> In fact Create React App’s error overlay already intercepts console.error calls if a special method called console.reactStack was called before

> React 15 made console.reactStack calls only for one warning, to test the idea. We never implemented it for other warnings and then didn’t implement it at all in 16
> Suggestion: take a look at the API that React 15 was calling (search the UMD bundle for console.reactStack calls). Try doing the same in 16, except now in the centralised place (warning module). Use Create React App to check if it worked. When it works it should start showing the error overlay for warnings too, and include component stack as actual frames in the overlay

---

Let's do it...

