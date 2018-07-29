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


