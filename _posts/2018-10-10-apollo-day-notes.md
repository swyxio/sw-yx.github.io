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
- apollo 3
  - benchmarks: https://twitter.com/benjamn/status/1045344757990526978
  - merging apollo boost api into core
  - bundle size reduction
  - active invalidation and cache scoping
  - mostly cleaning up point
- @defer support
- stability - stage process - showcase of how it works - test new language extensions - support process - want to make it more practical

apollo server

- concerns
  - "api change"
  - "how do i make my app fast"
  - "is my server working?" telemetry
- apollo server 2
  - "schema first design"
  - give typedefs and resolvers, build a production ready server for you, 
  - now can also add mocks - can build out UI without waiting for backend to be built
  - data sources: solution for how you model the request
    - we used to have models and connectors and resolvers
    - model and connector part was basically the same thing - a lot of manual implementation
    - `apollo-datasource-rest` - middleware to make the transform in a clean way
      - call `.get` to call the REST API
      - if has TTL it will start caching
  - tracing and full support for apollo engine
    - slowest thing is the second Movies in array
    - partial query caching
    - demo in Playground to show the second read has caching
- recap
  - schema first development
  - built in structure to get data plumbed
  - designed for a pit of speed
  


  
  
future hints

- code splitting in apollo client
