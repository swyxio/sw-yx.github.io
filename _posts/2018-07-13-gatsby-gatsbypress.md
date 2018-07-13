---
layout: post
date: 2018-07-13
tags: gatsby
feelings: neutral
title: gatsbypress - call
comments: true
description: gatsby press call
---

how do most websites come to be

- themes
- nice stuff, aimed at specific use cases
- prebuilt mass manufactured
- cheaper to have expert designer
- drupal founder - now runes a diff company - SaaSification of the web
- what was custom built is now saas
- RE business 150bucks - website for you
- could find a local agency - would cost 5-10k and uncertain quality
- awesome onramp to using gatsby
- dont want to spend a ton of time configuring
- theme system
- wordpress / drupal agencies
- several base themes that are popular
- basis of tons of custom built wp instances
- take the design, write the css, html and js
- more polished base starters for gatsby
- establish baselines on top of gatsby
- ejecting?
  - a theme can ship with components
  - overrides for components
  - run a command line and list components
  - list included in themes that you want to override
  - override title
  - hmm
  - maybe extend instead "in the normal gatsby way"
  - to pull in random data for company
- gatsbypress have a well thought out component tree
  - straightfwd overriding
  - well documented
  - default gatsbypress uses emotion, global theme object (sass/less version too)
- overriding plugins is trickier (vs overriding components)
  - (not so keen) just using default is fine
  - ideally we dont do that
- define what it is, and what it is not
  - lock down - datastructure
  - copy wordpress
    - page type, post type, user
  - components dont need to write gql queries
    - assume what every component needs
  - editing page type
  - data layer handles translation
    - create content types on contentful
- gatsby config
  - specify backend
  - gatsbypress  = app or basetheme or themeconfig or themetype
  - theme <- supplies components
- gatsby theme creator
- defining core datatypes and core components
- other themes
  - blog theme first?
  - documentation theme
  - ecommerce theme
  - restaurant themes?

---

next part

- marketplace - browse - live immediately
- platform for saas type experiences
  - we take care of hosting/blding
  - so they only focus the theme
  - be part of that wave

technical qtn

- how does the theme define a content type
- how are the schemas enforced and propagated to the backend
- how does component overwriting work

basic stypes

- prototype something
- play around - dx is super important
- built out a few different themes
- starting to feel smooth
- launch with gatsbypress, documentation, and ecommerce
- then finetuning with agencies
- then building a bunch of polished themes
- a preview launch
- then a launch launch
- with 10 super nice themes that we launch with
- and you can install right away
- color presets
- typo presets
- 25 diff combos of color and typos.

---

agencies agencies agencies

- gbpress - really attractive entry pt into the gatsby ecosystem
- feel the constraints of gbpress - learn gb for real
- apple vs android - compare with high end enterprise webdev vs sqspace/wpress
- ridiculously entry into using gatsby. cool + fun + useful
