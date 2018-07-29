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
