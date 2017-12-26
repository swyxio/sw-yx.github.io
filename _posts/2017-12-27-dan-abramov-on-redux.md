---
layout: post
date: 2017-12-27
tags: redux
feelings: learning
title: dan abramov on redux
description: notes from Dan's egghead.io courses
---

These are notes from Dan's [egghead.io courses](https://egghead.io/redux). (_stuff in italics are my own opinions_). I skipped all the test related parts as there was a lot of unnecessary stuff for me. I am also familiar with redux and react-redux so there will be implicit assumptions here.

## Principles of redux

1. all **STATE** is in a POJO.
2. all mutations are triggered by an **ACTION**. (cannot mutate state directly)
3. all state mutations are described by a **REDUCER** which has to be a pure function.

## Reducer tips:

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
3. don't mutate arrays - use `...` spreads and `slice` instead.

Avoid mutating methods, for example:

- To add to an array, don't use `list.push(i)`, use `[...list, i]`.
- To remove from an array at `i`, don't use `list.splice(i,1)`, use `[...list.slice(0, i), ...list.slice(i + 1)]`
- you can use [`deepFreeze`](https://www.npmjs.com/package/deep-freeze) to prevent accidental mutation
- To modify an object use `Object.assign({}, oldobject, newobject)` (ES6) or `{...oldobject, newobject}` (ES7)

## Three methods of `createStore`

Assuming you have `const store = createStore(reducer)`:

1. `store.getState()` - run imperatively/synchronously to get state
2. `store.dispatch(action)` - run imperatively/synchronously to trigger reducer
3. `store.subscribe(callback)` - runs `callback` whenver state has been updated

_not really used at all once you have react-redux_

This API is super simple and can be implemented in 20 lines (but there is more than this in redux)

## Reducer Composition

Having a root reducer split out into smaller reducers. Can be done with arrays or objects. Objects are more scalable. e.g.

```javascript
  const rootReducer = (state = {}, action) => {
    return {
      foo: fooReducer(state.foo, action),
      bar: barReducer(state.bar, action)
    }
  }
```

Different people can now work on different reducers with guarantees that they will not conflict.
Instead of writing the above code by hand, you can simply use `combineReducers`:

```javascript
  import { combineReducers } from 'redux'
  const rootReducer = combineReducers({
    foo: fooReducer, 
    bar: barReducer
  })
```

_if `foo` and `fooReducer` are named the same you can make this even more concise with es6 object literal shorthand_

`combineReducers` is so simple you can implement it in 15 lines with an `Object.keys(reducers).reduce()`.

## combining React and Redux (crude method)

Very crudely, you can wrap a function around your entire `ReactDOM.render` and pass it as the callback to `store.subscribe`:

```javascript
const renderfn = () => {
  ReactDom.render(<App {...store.getState()} />, document.getElementById('app'))
}
store.subscribe(renderfn)
renderfn()
```

_This will connect your redux store to your react view. The React view can send actions to update the redux store, and when the redux store is updated, it will call `renderfn` to show the new state. This is hamfisted and we'll see a better way to do this._

## separating Presentational Components and Container Components (basic method)

Pretty straightforward for those familiar with `react-redux`.

```javascript
// Presentational components only deal with how things look, and take named event handlers as props
const FooComponent = ({clickHandler}) => {
  return ( // per es6, this return statement isn't required if we're not doing any other logic
    <div>
      <button
        onClick={e => clickHandler(e)}
      >
        some stuff goes here
      </button>
    </div>
  )
}

// container components specify the named event handlers and pass them to presentational components
render() {
  return (
    <div>
      <FooComponent clickHandler={e => store.dispatch({
        type: 'E_HAPPENED',
        foo: e
      })}/>
    </div>
  )
}
```

Separation like this is NOT required in redux but helps to move to other state management like Relay or other view layers like React Native without rewriting the unrelated code.

the problem with above approach is passing down too many props down the tree especially when you have intermediate components. This breaks encapsulation because parents need to know too much about what children will need.

You can disconnect this reliance on prop-passing by making the child component just read directly from `store.getState()` instead of getting its state through a prop from a parent.

_note: do not call them "smart components" and "dumb components"._

## combining React and Redux (better method)

use `store.subscribe()` with `React.Component.forceUpdate()`

```javascript
class Foo extends Component {
  componentDidMount() {
    this.unsubscribe = store.subscribe(() => this.forceUpdate())
  }
  componentWillUnmount() {
    this.unsubscribe() // the name 'unsubscribe' doesnt actually matter but is more readable
  }
  render() {
    const state = store.getState()
    return <div>{state.foobar}</div>
  }
}
```

_this updates individual components based on store subscription and is better than rerendering the entire app as you saw above. Individual components can also draw upon the redux state and no longer have to rely on getting state from parents._

