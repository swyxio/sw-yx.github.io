---
layout: post
date: 2018-07-29
tags: react
feelings: neutral
title: reactrally grind 3 
comments: true
description: react rally grind 3
---

ok i have a basic vdom implementation in and lets see if i can write a render function that works

```js
// mount the vdom on to the dom and 
// set up the runtime from sources and
// patch the vdom
// ---
// returns an unsubscribe method you can use to unmount
export function mount(element, container) {
  const source$ = constructStream(element)
  const rootNode = render(element)
  container.appendChild(rootNode)
  return scan(
    source$, 
    (prevDOM, next) => {
        const vDOM = render(element, next)
        patch(rootNode, diff(prevDOM, vDOM))
        return vDOM
    },
    rootNode
  ).subscribe()
}
```

constructStream: 

```js
// traverse all children and collect a stream of all sources
// this means you cant hot swap streams for now which is impt for a real app
function constructStream(rootEl) {
  // this is the first ping of data throughout the app
  let source$ = Observable.of(INITIALSOURCE) 
  traverse(source$, source => {
    // visit each source and merge with source$
    if (source) return source$ = merge(source, source$)
  })(rootEl)
  return source$
  /* alternate approach abandoned for now */
  // // this is the first ping of data throughout the app
  // let data$ = Observable.of(INITIALSOURCE) 
  // // start with null view
  // let view$ = Observable.of(null) 
  // return {data$, view$}
}

function traverse(source$, addToStream) {
  return function traverseWithStream(element){
    const { type, props } = element;
    const isDomElement = typeof type === "string";
  
    if (isDomElement) {
      // updateDomProperties(dom, [], props); // TODO: add event listeners
      const childElements = props.children || [];
      childElements.map(traverseWithStream); // recursion
    } else {
      // Instantiate component element
      const instance = {};
      const publicInstance = createPublicInstance(element, instance);
      addToStream(publicInstance.source(source$));
      const childElement = publicInstance.render();
      traverseWithStream(childElement);
    }
  }
}

```

now i just need to render a bunch of h's

---

ended up dipping into vtext and vnode instead of using hyperscript but it workd!

# Hello world!

```js
class App extends Component {
  source($) {
    return scan(
      Ticker(), // tick every second
      x => x+1, // count up
      0         // from zero
    )
  }
  render(state, prevState) {
    const elapsed = state === INITIALSOURCE ? 0 : state
    return <div> number of seconds elapsed: {elapsed} </div>
  }
}

mount(<App />, document.getElementById('app'))

```

this produces a steadily ticking counter next to hello world!

---

6pm ok so looking at last nights todo list:

- make every dom el a source: not done yet but doable under my static stream restriction
- figure out if sinks matter? maybe
- render takes observable? not sure what this mmeans haha

now my new wishlist:

- local stream state based on the instance
- trying to show the rough edges based on this new thingy...

---

3am long detour but i took in jafar husain's awesome rxjs course on FEM and also explored recompose and reasonreact from egghead. i feel very close to something here.

Reason react looks like this:

```re
type state = {count: int};

type action =
  | Increment
  | Decrement;

let component = ReasonReact.reducerComponent("Counter");

let string = ReasonReact.stringToElement;

let make = _children => {
  ...component,
  initialState: () => {count: 0},
  reducer: (action, state) =>
    switch action {
    | Increment => ReasonReact.Update({count: state.count + 1})
    | Decrement => ReasonReact.Update({count: state.count - 1})
    },
  render: ({send, state}) => {
    let msg = "Current Count:  " ++ string_of_int(state.count);
    <div>
      <h2> (string(msg)) </h2>
      <button onClick=(_event => send(Increment))> (string("+")) </button>
      <button onClick=(_event => send(Decrement))> (string("-")) </button>
    </div>;
  }
};
```

so having the `send` function is a pretty good idea i should steal.

theres an alternative using `self.handle`...

```re
let handleClick = (_event, _self) => Js.log("click!");

let make = (~message, _children) => {
  ...component,
  render: self =>
    <div onClick=(self.handle(handleClick))>
      (ReasonReact.stringToElement(message))
    </div>
};
```

i can adapt this idea.

`recompose` also has a nice section: https://github.com/acdlite/recompose/blob/v0.26.0/docs/API.md#componentfromstream

with a `createEventHandler` i should probably use:

```js
const Counter = componentFromStream(props$ => {
  const { handler: increment, stream: increment$ } = createEventHandler()
  const { handler: decrement, stream: decrement$ } = createEventHandler()
  const count$ = Observable.merge(
      increment$.mapTo(1),
      decrement$.mapTo(-1)
    )
    .startWith(0)
    .scan((count, n) => count + n, 0)

  return props$.combineLatest(
    count$,
    (props, count) =>
      <div {...props}>
        Count: {count}
        <button onClick={increment}>+</button>
        <button onClick={decrement}>-</button>
      </div>
  )
})
```

---

4am i took componentFromStream and adapted it for my use now to implement:

```js
class Counter extends Component {
  constructor() {
    this.$ = {
      increment: createEventHandler(),
      decrement: createEventHandler()
    }
  }
  source($) {
    const {increment, decrement} = this.$
    const state$ = scan(
      startWith(
        merge(
          mapToConstant(increment.$, 1), 
          mapToConstant(decrement.$, -1)
        ),
        0
      ),
      (acc, n) => acc + n, 0 // scan args
    )
    return state$
  }
  render(state) {
    const {increment, decrement} = this.$
    return <div {...props}>
        Count: {state}
        <button onClick={increment}>+</button>
        <button onClick={decrement}>-</button>
      </div>
  }
}
```
