---
layout: post
date: 2019-01-22
tags: js
feelings: determined
title: immutablejs notes
comments: true
description: more notes!
---




## why we like immutability

- less moving parts

```js
export function foo(){
  var data = { key: "value" }
  touchFn(data)
  console.log(data.key) // ???
}
```

- solutions to answering "what changed": https://www.youtube.com/watch?v=I7IdS-PbEgI&feature=youtu.be
  - in UI we go from state A to B and to be performant we wanna know what changed
  - wrap in scope? do we re-render all of it?  change detection
  - problems with Object.observe - shallow? 
  - dirty bits - mark dirty? until you have to check part of the state
  - "you dont need an M because we have arrays and objects"
  - `===`. reference equality is free. for deep 
  - works nicely with memoization/sCU
  - om is faster. 
  - reconciliation is slower than persistent data. 
    - reconciliation is O(n), 
    - reconciliation minus unchanged legs is O(log N)
  - undo is easy
  - PIDS work because of strcutrual sharing, removing complexity
- copy on write is good https://en.wikipedia.org/wiki/Copy-on-write

If an object is immutable, it can be "copied" simply by making another reference to it instead of copying the entire object. Because a reference is much smaller than the object itself, this results in memory savings and a potential boost in execution speed for programs which rely on copies (such as an undo-stack).

## immutablejs api

```js
// creation
const map = Immutable.Map({
  todo1: {
    title: "Todo 1",
    value: "Make it happen"
  }
})

// alternatively
const map = Immutable.Map([["todo1", {title: "Todo 1"}]])

// access
map.get("todo1").title // "Todo 1"

// other methods: set(), delete(), clear(), update(), merge()
// other accessors: has(), include(), find()

// todos.slice(todos.size -2) doesnt takenegative
```

## The consequences of PIDS

- spreading api - we have 152 files that import immutable and 289 files that reference apis (1622 times). import once write 10x everywhere
- http://blog.theodo.fr/wp-content/uploads/2018/04/immutable-hero-console-no-formatter.png
- 

`(\.setIn\(|\.mergeDeep\(|\.get\(|\.getIn\(|\.has\(|\.fromJS\(|\.toJS\(|\Seq\()`

## why we like immer

- "just javascript" vs your own query language
- scope: we like mutability, but mutability is bad everywhere so use scope
- Immutability != persistent immutable data structures


----


scratch pad


```js
export function foo(){
  var data = Object.freeze({ key: "value" })
  touchFn(data)
  console.log(data.key) // "value"
}

// but this doesnt do nested values
// lets generalize it

export function foo(){
  var data = Immutable.Map({ key: { foo: "value" } })
  touchFn(data)
  console.log(data.getIn('key','foo')) // "value"
}

// how do you mutate tho?

export function foo(){
  var data = Immutable.Map({ key: { foo: "value" } })
  touchFn(data)
  console.log(data.getIn(['key','foo'])) // "value"
  data.setIn(['key','foo'], "newvalue"))
}

```


other references

https://blog.theodo.fr/2018/04/immutablejs-vs-spread-operators-part-1/
