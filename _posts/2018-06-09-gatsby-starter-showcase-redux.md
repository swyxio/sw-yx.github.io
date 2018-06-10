---
layout: post
date: 2018-06-09
tags: gatsby
feelings: neutral
title: gatsby starter showcase redux
comments: true
description: collecting thoughts on redux
---

my prior work on this is here: https://github.com/sw-yx/gatsby-starter-search

today i am starting work on the starter showcase.

quote myself: 

> digging into gatsby-source-github right now to see if i can crawl dependencies within gatsby instead of outside it (which is how I currently do it). it seems slightly problematic with security in particular (have to supply a github personal access token, which I assume we have to host in netlify somewhere) and we dont really need much beyond just grabbing a package.json. I think my node-based approach is faster and simpler, but less automated.
> there’s reasonable doubt as to whether my approach is best (given we want to show off gatsby’s capabilities) but for the timeframe I have I should probably just port over my existing approach. happy to completely throw it out and rewrite if we decide we want to make full automation a priority (i suspect it’s not)

we went with the crawl-outside approach.

so for integrating starter-search into gatsby's www i need to:

- figure out where to place the source data
- figure out where to place the images
- copy over the main showcase list page
- copy over the showcase detail page
- adapt to show and filter dependencies
- add route for dependencies so that we can link from other sites
- write the automation script last

