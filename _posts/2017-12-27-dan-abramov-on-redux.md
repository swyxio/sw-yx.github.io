---
layout: post
date: 2017-12-27
tags: redux
feelings: learning
title: dan abramov on redux
description: notes from Dan's egghead.io courses
---

These are my notes from Dan's [egghead.io courses](https://egghead.io/redux). (_stuff in italics are my own opinions_). I skipped all the test related parts as there was a lot of unnecessary stuff for me. I am also familiar with redux and react-redux so there will be implicit assumptions here.

---

# Table of Contents
- [Course 1: Getting Started with Redux](#course-1---getting-started-with-redux--https---eggheadio-courses-getting-started-with-redux-)
  * [Principles of redux](#principles-of-redux)
  * [Reducer tips](#reducer-tips-)
  * [Three methods of createStore](#three-methods-of--createstore-)
  * [Reducer Composition](#reducer-composition)
  * [Combining React and Redux (crude method)](#combining-react-and-redux--crude-method-)
  * [Separating Presentational Components and Container Components (basic method)](#separating-presentational-components-and-container-components--basic-method-)
  * [Combining React and Redux (better method)](#combining-react-and-redux--better-method-)
  * [Passing store down using Context](#passing-store-down-using-context)
  * [Combining React and Redux (react-redux method)](#combining-react-and-redux--react-redux-method-)
  * [Action Creators](#action-creators)
- [Course 2: Building React Applications with Idiomatic Redux](#course-2---building-react-applications-with-idiomatic-redux--https---eggheadio-courses-building-react-applications-with-idiomatic-redux-)

---

# Course 1: [Getting Started with Redux](https://egghead.io/courses/getting-started-with-redux)

_This course starts with a basic introduction of Redux's APIs, then shows how to integrate React and Redux, concluding with using the `react-redux` library but showing the steps to get there including using React's Context API._

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

with this you no longer need to wrap your entire app with a `renderfn()` and a top-level `subscribe` as you saw in the crude example above.

_this updates individual components based on store subscription and is better than rerendering the entire app as you saw above. Individual components can also draw upon the redux state and no longer have to rely on getting state from parents._

## passing store down using Context

_note: the context api is currently being [changed](https://github.com/reactjs/rfcs/pull/2) so expect the specific API of the below to change but the `Context` concept should stay around._

```javascript
class Provider extends Component {
  getChildContext() {
    return {
      store: this.props.store
    }
  }
  render() {
    return this.props.children
  }
}
Provider.childContextTypes = { // currently must specify this to enable context passing
  store: React.PropTypes.object 
}
ReactDOM.render(
  <Provider store={createStore(rootReducer)}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

with all that done, now you can do `const {store} = this.context` in any descendent of `<App />` if it is a class component. If it is a functional component, you still have to declare `contextTypes`:

```javascript

// note we are using the SECOND argument here
const FunComponent = (props, { store }) => <div>{store.text}</div>
FunComponent.contextTypes = { // as above, must specify this to receive
  store: React.PropTypes.object
}
```

Context essentially allows global variables along the tree which goes against the core principles of React but can be handy for dependency injection like we are doing here. for more, read: <https://reactjs.org/docs/context.html>

## combining React and Redux (react-redux method)

`react-redux` exports a `Provider` (which we wrote above) and a `connect` method which helps you generate a container component wrapping a presentational component.

```javascript
import { Provider } from 'react-redux' // aka already written for you

ReactDOM.render(
  <Provider store={createStore(rootReducer)}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

so in a given component, you can do... 

```javascript
import { connect } from 'react-redux'
import React, { Component } from 'react'

class RawComponent extends Component {
// your component here ...
}

const mapStateToProps = (state, ownProps) => {
  return { // data you would get from context or redux store
    foo: state.foo,
    containerProp: ownProps.containerProp
  }
}
const mapDispatchToProps = dispatch => {
  return { // methods that would use store.dispatch
    bar: baz => dispatch({ type: 'SOME_ACTION', baz })
  }
}

const ConnectedComponent = connect(mapStateToProps, mapDispatchToProps)(RawComponent)
// or export default connect(mapStateToProps, mapDispatchToProps)(RawComponent)
```

You can supply `null` for either `mapStateToProps` or `mapDispatchToProps` if you don't need it. If you don't need both you can simply call `connect()(RawComponent)` and a `dispatch` prop will still be supplied if you want to use it for any message dispatching.

The container created from `connect` also passes on its `ownProps` as a second argument to either `mapStateToProps` or  `mapDispatchToProps` so the above container would work with `<ConnectedComponent containerProp="foobar" />`.

## Action Creators

Action Creators are literally helper functions to help you create actions. Again they are not required but are probably good practice for creating maintainable apps. You can split these out into their own file and/or export them with their accompanying reducer. Just a helpful pattern, not super important.

```javascript
const fooCreator = foo => {
  return {
    type: 'FOO_CREATED',
    foo
  }
}
```

This can seem like boilerplate but they help to document your software as it is basically describing what it is doing.

# Course 2: [Building React Applications with Idiomatic Redux](https://egghead.io/courses/building-react-applications-with-idiomatic-redux)

## Redux Pro Tips

1. Use ES6 to make your action creators and object methods more concise.
2. Encapsulate all the store setup into a function and only export `configureStore()`.

## Persisting Redux to localStorage

`createStore` has a _second_ argument for you to supply things like `persistedState`. If supplied, it overrides any initial states you specify in your reducers.

```javascript
const loadState = () => {
 try { // can fail if user privacy doesnt allow localStorage
  const serializedState = localStorage.getItem('state')
  if (serializedState === null) return undefined
  return JSON.parse(serializedState)
 } catch (err) {
  return undefined
 }
}
const saveState = state => {
 try {
  const serializedState = JSON.stringify(state);
  localStorage.setItem('state', serializedState);
 } catch (err) {
  // do stuff
 }
}
const persistedState = loadState();
const store = createStore(rootReducer, persistedState);
store.subscribe(() => {
 saveState(store.getState())
 // or saveState({foo: store.getState().foo}) if specific field
})
```

this preserves the state of the app across reloads. 

- Beware of a subtle bug if your rehydrated data doesn't include some counter data (for example) that isn't saved in redux store. you may find `v4` from `node-uuid` helpful.
- adding an expensive operation like `JSON.stringify` to `store.subscribe()` can hurt performance. may use `throttle(saveStateFn, 1000)` from `lodash/throttle` to debounce at most every second.

## Adding React Router

to be continued (this is 2 yrs old so will be outdated)

