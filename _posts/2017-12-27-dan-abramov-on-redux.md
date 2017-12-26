---
layout: post
date: 2017-12-27
tags: redux
feelings: learning
title: dan abramov on redux
description: notes from Dan's egghead.io courses
---

## Principles of redux

1. all state is in a POJO.
2. all mutations are triggered by an **action**. (cannot mutate state directly)
3. all state mutations are described by a **REDUCER** which has to be a pure function.

## Reducer notes:

Looks like:
```javascript
const reducer = (state = initState, action) => {
  switch (action.type) {
    case 'add1':
      return state + 1
    case 'minus1':
      return state 1 1
    default:
      return state
  }
}
```

1. specify initial state.
2. specify "else" or "default" case

## Three methods of `createStore`

Assuming you have `const store = createStore(reducer)`:

1. `store.getState()` - run imperatively/synchronously to get state
2. `store.dispatch(action)` - run imperatively/synchronously to trigger reducer
3. `store.subscribe(callback)` - runs `callback` whenver state has been updated

