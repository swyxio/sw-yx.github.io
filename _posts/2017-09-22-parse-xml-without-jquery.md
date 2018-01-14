---
layout: post
date: 2017-09-22
tags: happy
categories: tips
title: parse xml without jquery
---

I am recovering from my sickness and working on the backend for my podcast player. this involves parsing XML feeds and a lot of the answers i foudn online use jquery. [this is a neat solution that doesnt need jquery](https://stackoverflow.com/a/41009103).

```
fetch('http://chatwithtraders.libsyn.com/rss').then(e=>e.text())
        .then(str => (new window.DOMParser()).parseFromString(str, "text/xml"))
        .then(e => console.log(e))
```
