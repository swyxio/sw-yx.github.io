---
layout: post
date: 2018-04-15
tags: graphql
feelings: rested
title: graphql day
comments: true
description: graphql day notes
---

# ken wheeler: intro to urql

why urql

- just for react
- less abstracted

why not apollo

- diff caching system
- easy setup (pre boost)
- multiple queries/mutations per connection component

easy way to use graphql - just use query string in `fetch`

why use client instead of fetch

- caching
- cache invalidation
- components
- updates

api overview

- `const client = new Client({ url: 'mygraphqlurl' })`
- `<Provider client={client}>`
- `<Connect query={query(myQuery, variables)}>` with FaaC or `ConnectHOC()()`

connect props

- see table
- interesting: typeinvalidation, shouldinvalidate

render args

- see table

caching - explanation into detail

- when query comes in you parse query in ast, inject __typename into queries
- stringify query and variables to create a hash
- store query results on that hash
- keep track of associated typenames (gankTypenamesFromResponse)
- invalidate when mutation affects a given dependent typename
- (aka using introspection to do cache invalidation)

you can create your own cache

- when you do a mutation you should return the thing that was mutated
