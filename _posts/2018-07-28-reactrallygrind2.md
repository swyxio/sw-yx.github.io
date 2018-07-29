---
layout: post
date: 2018-07-28
tags: react
feelings: determined
title: reactrally grind 2
comments: true
description: more grind
---

i can forsee myself needing a merge function so i wrote one:

```js
// let a = 1, b = 100
// let observable1 = new Observable(observer => {
//   // Emit a single value after 1 second
//   let timer = () => setTimeout(() => {
//     observer.next(a++);
//     timer()
//   }, 2000);
//   timer()

//   // On unsubscription, cancel the timer
//   return () => clearTimeout(timer);
// });
// let observable2 = new Observable(observer => {
//   // Emit a single value after 1 second
//   let timer = () => setTimeout(() => {
//     observer.next(b++);
//     timer()
//   }, 1000);
//   timer()

//   // On unsubscription, cancel the timer
//   return () => clearTimeout(timer);
// });
// // merge(observable1, observable2).subscribe(a => console.log('see', a))

// merge

function merge(...obsArgs) {
  return new Observable(observer => {
    const subs = obsArgs.map(obs => obs.subscribe(x => observer.next(x)))
    // todo: handle unsubscribe haha
    return () => subs.forEach(sub => sub()) // untested, hope it works
  })
}
```


i also sketched out a proto api for reactive react:

```js
class Abc extends Component {
  source($) {
    return Observable.of(1)
  }
  render($) {
    return <LabeledSlider />
  }
}

class LabeledSlider extends Component {
  constructor() {
    this.input = <input type="range" min={20} max={80} onInput={console.log} />
  }
  source($) {
    return this.input.onInput$
  }
  render($) {
    return <div>{this.input}</div>
  }
}
```

i definitely havent worked out how the values go in and out of each other yet.

---

12am briefly considered using functions as the component api. but didact's updateInstance is clearly triggered by passing a reconcile on the internalInstance so i dont see any reason to modify that. classes it is.

---

12.45 ok i have started working on `creatsource`. i have a great nearterm goal - just using zenobservable, make a vanilla js updating app.

---

1.15 needed a scan instead of a reduce, so i wrote one:

```js
function scan(obs, cb, initial) {
  const INIT = '__NO_INITIAL_VALUE'
  let sub, acc = INIT
  if (typeof initial !== 'undefined') acc = initial
  return new Observable(observer => {
    if (!sub) sub = obs.subscribe(val => {
      if (acc !== INIT) acc = cb(acc, val)
      observer.next(acc)
    })
  })
}
```

---

1.30 yay i have my first plain observable js app, a click counter:

```js
import Observable from 'zen-observable'

// create
const app = document.getElementById('app')
const div = document.createElement('div')
const btn = document.createElement('btn')
btn.innerText = 'click me'
div.appendChild(btn)
const div2 = document.createElement('div')
div2.innerText = '0'
div2.id = 'div2'
div.appendChild(div2)

if (app.firstChild) app.replaceChild(div, app.firstChild)
else app.appendChild(div)


// listen
scan(
  fromEvent(btn, 'click'),
  (prev, cur) => {
    document.getElementById('div2').innerText = prev + 1
    return prev + 1
  },
  0
).subscribe(x => console.log('Clicked! ' + x));

function fromEvent(el, eventType) {
  return new Observable(observer => {
    el.addEventListener(eventType, e => observer.next(e))
    // on unsub, remove event listener
    return () => console.log('not implemented yet')
  })
}

function scan(obs, cb, initial) {
  const INIT = Symbol('NO_INITIAL_VALUE')
  let sub, acc = INIT
  if (typeof initial !== 'undefined') acc = initial
  return new Observable(observer => {
    if (!sub) sub = obs.subscribe(val => {
      if (acc !== INIT) acc = cb(acc, val)
      observer.next(acc)
    })
  })
}
```

with this ground work i think i can start refashioning things into the core of the Creat reconciler.
