---
layout: post
date: 2017-07-10
title: boilerplate search 2
categories: meteor projects
tags: fat
---

I am done with the first trello feature, the "reputation score"

![now with ratio](https://pbs.twimg.com/media/DEW0933WsAAaZOH.jpg:large)

it took 3 hours and i nearly derailed myself trying to refactor a suboptimal part. i need to stay on track.

a key problem was getting the download count of github projects. if you look by release there is an `assets` thing which shows download count but a) projects aren't often released formally and b) `assets` dont always show up for whatever reason (permissioning or not having the file upload, cant be bothered to dig into why)

I elected to ignore it and added commit count and a commit age penalty instead.
