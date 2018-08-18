---
layout: post
date: 2018-08-18
tags: redux
feelings: neutral
title: a look at action creator creators
comments: true
description: looking at a thing ot add into redux-starter-kit
---

options:

1. https://redux-actions.js.org/api/createaction
2. https://github.com/christiangenco/createReducerActions (related: https://kickstarter.engineering/namespacing-actions-for-redux-d9b55a88b1b1)
3. https://github.com/granteagon/reduxr
4. https://github.com/ericelliott/autodux
5. https://github.com/cl1ck/redux-bits

---

# Standard Redux

```js
const initialState = []

const reducers = (state=initialState, action) => {
  switch (action.type) {
    case 'create':
      return [...state, action.payload ];
    case 'delete':
      return reject(state, action.payload);
    default:
      return state;
  }
}
```

### redux-actions

https://redux-actions.js.org/api/createaction

```js
const { createAction, handleAction } = require('ReduxActions');
const increment = createAction('INCREMENT');
const decrement = createAction('DECREMENT');
const reducer = handleActions(
  {
    [increment]: state => ({ ...state, counter: state.counter + 1 }),
    [decrement]: state => ({ ...state, counter: state.counter - 1 })
  },
  defaultState
);
const store = createStore(reducer, defaultState);

// dispatch
store.dispatch(increment()); // or store.dispatch(decrement());
```

with payloads:

```js
const { createActions, handleAction } = require('ReduxActions');
const { increment, decrement } = createActions({
  INCREMENT: (amount = 1) => ({ amount }),
  DECREMENT: (amount = 1) => ({ amount: -amount })
});
const reducer = handleActions(
  {
    [increment]: (state, { payload: { amount } }) => {
      return { ...state, counter: state.counter + amount };
    },
    [decrement]: (state, { payload: { amount } }) => {
      return { ...state, counter: state.counter + amount };
    }
  },
  defaultState
);
```

it also has a nice `combineActions` utility to reduce reducer repetition

### reduxr

https://github.com/granteagon/reduxr

reduxr takes keys mapped to a `function(state, payload)`:

```js
// Todo.js
import { createStore, combineReducers } from 'redux'
import { reduxr } from 'reduxr'

// define our reducers in a map
const todoReducer = {
  todoToggleVisibility: (state) => {
    return {
      ...state,
      visible: !state.visible  
    }
  },
  complete: (state, payload) => {
    return {
      ...state,
      complete: payload
    }
  }
}

// define our initial state
const initialState = {
  label: '',
  visible: false
}

// create
const Todo = reduxr(todoReducer, initialState)

// create store
const store = createStore(
  combineReducers({
    todo: Todo.reducer // load our reducer into the store
  })
)

// dispatching
store.dispatch(Todo.action.todoToggleVisibility({visible: true}))
```

### autodux

https://github.com/ericelliott/autodux

```js
export const {
  actions: {
    setUser, setUserName, setAvatar
  },
  selectors: {
    getUser, getUserName, getAvatar
  }
} = autodux({
  slice: 'user',
  initial: {
    userName: 'Anonymous',
    avatar: 'anon.png'
  }
});
```

### createReducerActions

https://github.com/christiangenco/createReducerActions

```js
// redux/counter.js
import createReducerActions from "create-reducer-actions";

const initialState = 0;
export const { reducer, actions } = createReducerActions(
  {
    increment: state => state + 1,
    decrement: state => state - 1,
    add: (state, { payload }) => state + payload,
    sub: (state, { payload }) => state - payload
  },
  initialState
);
```

### redux-bits

https://github.com/cl1ck/redux-bits

has too much magic, i'm not even going to bother
