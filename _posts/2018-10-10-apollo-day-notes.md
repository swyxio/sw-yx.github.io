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
- preview
  - streaming data instead of request/response

part 2 sneak peek

- questions - about Smarter Collaboration
  - static typing in workflow
  - will changes on the server break my client
  - apollo with tools like github and vscode
- graphql summit vscode extension
  - autocomplete features - pull schema down from engine
  - fragments
  - realtime p95 statistics of apollo engine
- ci workflow tools
  - warning for deprecating api's - right in github
  - diff visible inside engine as well - can see warnings and failures
  - operation validation
- better together
- circle ci config - use apollo cli to publish schema to apollo engine
- vscode also uses cli to  to pull down schema
- schema tags - to tag staging or dev or whatever
- q&a
  - distributed schemas is the way forward
  - fragment matchers
  - precompiled version of apollo just for your app?
  - you just need an engine api key - vscode will automatically connect to engine metrics for you
  - lifelong view into your graph of what is running - cant be from your dev server
  - dont allow arbitrary queries - the solution is engine which is a data poool
  - typescript - codegen - passed the type into query component and get the render prop with the type
  
query security

- questions
  - what risks do i take runnign a graph api?
  - who do i give access to my graph?
  - how can i secure my graph?
- demo 4: apollo query registry
  - an operation registry
  - new command - `npx apollo queries:register`
  - descopes your graph to only what your client is using
  - q4 api
    - integrated quotas in your schema itself
  
future hints

- code splitting in apollo client
