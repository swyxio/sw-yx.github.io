---
layout: post
date: 2018-02-23
tags: rxjs
feelings: neutral
title: rxjs api according to me
comments: true
description: rxjs api according to me
---

# Key concepts

## The Multiple Push collection paradigm

Here is a [2x2 matrix of function paradigms](http://reactivex.io/rxjs/manual/overview.html#pull-versus-push):

- Single Pull: `Function`
- Multiple Pull: `Iterator` and `Generator functions`
- Single Push: `Promise`
- Multiple Push: `Observable`

In **Pull** systems the Consumer decides when it receives data from the Producer. The Producer is unaware of when the data will be delivered. **Functions** get consumed when their `return` values are assigned, and **Generators** get consumed when their `iterator.next()` is called.

In **Push** systems the Producer decides when to send data to the Consumer. The Consumer is unaware of when it will receive that data. **Promises** call their `.then`s when they want to, **Observables** push their data to `.subscribe`.

## Quick concepts

Observables are:

- lazy - if not `subscribe`d, it wont execute, just like a function.
- NOT asynchronous - `subscribe` is entirely synchronous, just like a function. 
- BUT it can deliver values either synchronously or asynchronously


# A categorized API

## Observable creators

### fromEvent

```js
var button = document.querySelector('button');
Rx.Observable.fromEvent(button, 'click')
  .subscribe(() => console.log('Clicked!'));
```

### create

```js
var observable = Rx.Observable.create(function (observer) {
  observer.next(1);
  observer.next(2);
  observer.next(3);
  setTimeout(() => {
    observer.next(4);
    observer.complete();
  }, 1000);
});

console.log('just before subscribe');
observable.subscribe({
  next: x => console.log('got value ' + x),
  error: err => console.error('something wrong occurred: ' + err),
  complete: () => console.log('done'),
});
console.log('just after subscribe');

// just before subscribe
// got value 1
// got value 2
// got value 3
// just after subscribe
// got value 4
// done
```

## Observable operators: Persistence

### scan

```js
var button = document.querySelector('button');
Rx.Observable.fromEvent(button, 'click')
  .scan(count => count + 1, 0) // works like 'reduce'
  .subscribe(count => console.log(`Clicked ${count} times`));
```

## Observable operators: Flow control

### throttleTime

```js
var button = document.querySelector('button');
Rx.Observable.fromEvent(button, 'click')
  .throttleTime(1000) // at most allow 1 click per second
  .scan(count => count + 1, 0)
  .subscribe(count => console.log(`Clicked ${count} times`));
```

### filter
### delay
### debounceTime
### take
### takeUntil
### distinct
### distinctUntilChanged

## Observable operators: Pure Transforms

### map

```js
var button = document.querySelector('button');
Rx.Observable.fromEvent(button, 'click')
  .throttleTime(1000)
  .map(event => event.clientX) // map means map :)
  .scan((count, clientX) => count + clientX, 0)
  .subscribe(count => console.log(count));
```

### pluck
### pairwise
### sample



