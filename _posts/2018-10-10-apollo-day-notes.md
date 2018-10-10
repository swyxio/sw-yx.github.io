---
layout: post
date: 2018-08-
tags: apollo
feelings: neutral
title: apollo day notes
comments: true
description: notes from apollo day
---

## intro talk

apollo client

- pagination, auth, data layer for react
- simple movie app
  - auth with apolloclient - clientState field
  - situation: getlikes and getmovies are from different databases
  - use react state to differentiate between the queries you want
  - fetchMore function to fetch more/sync automatically?
  - localstate? @client directive. `client.writeData()` to mutate
  - one source of truth for all your data

  
  
future hints

- code splitting in apollo client
