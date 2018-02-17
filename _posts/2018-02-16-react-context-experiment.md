---
layout: post
date: 2018-02-16
tags: react
feelings: neutral
title: react context experiment
comments: true
description: an experiment with react context
---


two new developments this week got me excited about experimenting:

- parcel is now [0 dependency for bundling react](https://medium.com/@devongovett/parcel-v1-6-0-46f4a2514668)
- ryan florence dropped his [declarative component api](https://github.com/ryanflorence/react-component-component)

and with the freakout over redux recently i got to wondering if there might be a better way to do react and redux.

steps to experiment

- have parcel 1.6 installed
- `npm init -y`
- `touch index.html index.js`
- `npm i react-dom react@16.3.0-alpha.1`
- `npm install --save-dev babel-plugin-transform-object-rest-spread`
- `npm install --save-dev babel-plugin-transform-class-properties`
- and then do these files...

---

i ran into pretty bad context troubles. these didnt work:

- <https://medium.com/@baphemot/whats-new-in-react-16-3-d2c9b7b6193b>
- <https://tinyletter.com/kentcdodds/letters/react-s-new-context-api>
- <https://medium.com/dailyjs/reacts-%EF%B8%8F-new-context-api-70c9fe01596b>
- <https://github.com/reactjs/rfcs/blob/master/text/0002-new-version-of-context.md>


<https://github.com/facebook/react/issues/5927> so i stopped there.

anyway lets just prototype freely here

```js
import React from 'react';
import Component from './RCC'

export default ({ name }) => (<div>
<h1>Hello {name}!</h1>
  <Component
    initialState={{ gists: null }}
    didMount={({ setState }) => {
      fetch("https://api.github.com/gists")
        .then(res => res.json())
        .then(gists => setState({ gists }));
    }}
  >
    {({ state }) =>
      state.gists ? (
        <ul>
          {state.gists.map(gist => (
            <li key={gist.id}>{gist.description}</li>
          ))}
        </ul>
      ) : (
          <div>Loading...</div>
        )
    }
  </Component>
</div>);

```

a reducer looks like:
```js 
export default (state = 0, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state - 1
    default:
      return state
  }
}
```

so you can do `createStore(counter)` on it and use it

```js

  <Counter
    value={store.getState()}
    onIncrement={() => store.dispatch({ type: 'INCREMENT' })}
    onDecrement={() => store.dispatch({ type: 'DECREMENT' })}
  />
```
