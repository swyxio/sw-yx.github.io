---
layout: post
date: 2017-07-12
title: polishing off package jason
categories: projects
tags: tired
---

1am. time to do the last feature, which is an open eval api, before i can upload and get it hosted. i spent yesterday refactoring the edit page and that sucked but it is more or less done now. Now I have to:

- get it on the index page
- prettify
- set up a public unauthenticated route to take any boilerplate

---

problems i faced

1. set up route params on the index route. Solution: copy the `ResetPassword` component pattern that simply extracts the `match` variable from `props`.
2. center the owner/repo components in the new index page. Solution: [stackoverflow says `display table`](https://stackoverflow.com/questions/13586171/css-vertical-align-text-bottom)
