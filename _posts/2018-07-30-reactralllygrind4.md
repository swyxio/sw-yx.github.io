---
layout: post
date: 2018-07-30
tags: react
feelings: neutral
title: react rally grind 4
comments: true
description: solving a really gnarly bug
---

i had to rewrite things from an instantiate phase + render phase to just a straight render phase,

in order to regenerate the source stream for each new path render so that new sources can come online

and then i needed a stream of streams so that i can make new sources come online

jafar husain's reddit rxjs workshop thing was handy for getting me started thinking about 2d streams

however in my big refactor i messed up the scope of the render and that cost me a lot of time. listeners werent firing at all because they weren't even being added to the stream.

it took about 6 hours but i finally figured it out. stepping through code isn't as great as commenting out code to the smallest possible factor.
