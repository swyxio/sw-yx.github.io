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
