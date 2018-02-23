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

## Bullet point concepts

**Observables** are:

- lazy - if not `subscribe`d, it wont execute, just like a function.
- NOT asynchronous - `subscribe` is entirely synchronous, just like a function. BUT it can deliver values either synchronously or asynchronously
- Multiple Push based - see below
- `observable.subscribe`s are NOT listeners like `addEventListener` - the `observable` has no knowledge of Observers, but event handlers DO maintain a list of attached listeners. (Except for **Subjects**.. see below)

They look like:

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

// do observable.subscribe somewhere down the line
```

**Observers** are:

- consumers of values delivered by an **observable**.
- an object with three method: `next`, `error` (optional), and `complete` (optional).

They look like: 

```js
var observer = {
  next: x => console.log('Observer got a next value: ' + x),
  error: err => console.error('Observer got an error: ' + err),
  complete: () => console.log('Observer got a complete notification'),
};
```

**Subsciptions** are:

- disposable resource, eg execution of an **observable**
- an object with one primary method: `unsubscribe`
- able to `add` and `remove` child subscriptions.

They look like:

```js
var observable = Rx.Observable.interval(1000);
var subscription = observable.subscribe(x => console.log(x));
// Later:
// This cancels the ongoing Observable execution which
// was started by calling subscribe with an Observer.
subscription.unsubscribe();
```

**Subjects** are:

- special types of **observable**
- can be **multicast** to many **observers**
- maintain a list of many listeners (similar to EventEmitters)
- can be used just like normal **Observers** too e.g. `observable.subscribe(subject)`
- have specializations `BehaviorSubject`, `ReplaySubject`, `AsyncSubject`

They look like:

```js
var subject = new Rx.Subject();

subject.subscribe({
  next: (v) => console.log('observerA: ' + v)
});
subject.subscribe({
  next: (v) => console.log('observerB: ' + v)
});

subject.next(1);
subject.next(2);

// observerA: 1
// observerB: 1
// observerA: 2
// observerB: 2
```

## The Multiple Push collection paradigm

Here is a [2x2 matrix of function paradigms](http://reactivex.io/rxjs/manual/overview.html#pull-versus-push):

- Single Pull: `Function`
- Multiple Pull: `Iterator` and `Generator functions`
- Single Push: `Promise`
- Multiple Push: `Observable`

In **Pull** systems the Consumer decides when it receives data from the Producer. The Producer is unaware of when the data will be delivered. **Functions** get consumed when their `return` values are assigned, and **Generators** get consumed when their `iterator.next()` is called.

In **Push** systems the Producer decides when to send data to the Consumer. The Consumer is unaware of when it will receive that data. **Promises** call their `.then`s when they want to, **Observables** push their data to `.subscribe`.

# A categorized API

## The Observable lifecycle

- Creation: e.g. `const observable = Rx.Observable.create(...)`
- Subscription: e.g. `const subscription = observable.subscribe(console.log)`
- Execution: the code inside `Rx.Observable.create(function (observer) {...})` which calls `observer.next()` and possibly `observer.error()` and `observer.complete()`. Essentially this is "manual" implementation of the observable, and could be supplanted by all the inbuilt operators rxjs provides.
- Disposal: e.g. `subscription.unsubscribe()` to cancel execution

## Observable creators

### of

### from

```js
var observable = Rx.Observable.from([10, 20, 30]);
var subscription = observable.subscribe(x => console.log(x));
```

### interval

### fromEvent

```js
var button = document.querySelector('button');
Rx.Observable.fromEvent(button, 'click')
  .subscribe(() => console.log('Clicked!'));
```

### create

```js
var observable = Rx.Observable.create(function (observer) {
  observer.next(1); // synchronous
  observer.next(2); // synchronous
  observer.next(3); // synchronous
  setTimeout(() => { // asynchronous
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




# Interesting examples

## An observable that returns both synchronously AND asynchronously

```js
var foo = Rx.Observable.create(function (observer) {
  console.log('Hello');
  observer.next(42);
  observer.next(100);
  observer.next(200);
  setTimeout(() => {
    observer.next(300); // happens asynchronously
  }, 1000);
});

console.log('before');
foo.subscribe(function (x) {
  console.log(x);
});
console.log('after');

// "before"
// "Hello"
// 42
// 100
// 200
// "after"
// 300
```

## Subscribe/Unsubscribe interface implemented in pure javascript (no rxjs)

```js
function subscribe(observer) {
  var intervalID = setInterval(() => {
    observer.next('hi');
  }, 1000);

  return function unsubscribe() {
    clearInterval(intervalID);
  };
}

var unsubscribe = subscribe({next: (x) => console.log(x)});

// Later:
unsubscribe(); // dispose the resources
```

So why use rxjs if it can be done in pure js? "The reason why we use Rx types like Observable, Observer, and Subscription is to get safety (such as the Observable Contract) and composability with Operators."

## adding/removing Child subscriptions

```js
var observable1 = Rx.Observable.interval(400);
var observable2 = Rx.Observable.interval(300);

var subscription = observable1.subscribe(x => console.log('first: ' + x));
var childSubscription = observable2.subscribe(x => console.log('second: ' + x));

subscription.add(childSubscription);

setTimeout(() => {
  // Unsubscribes BOTH subscription and childSubscription
  subscription.unsubscribe();
}, 1000);

// second: 0
// first: 0
// second: 1
// first: 1
// second: 2
```

# handy tricks

## observable.subscribe(next, err, complete)

instead of providing an observer function or object, you can just provide a `next`, `err`, and `complete` function to `observable.subscribe` and it will create the object for you:

```js
observable.subscribe(
  x => console.log('Observer got a next value: ' + x),
  err => console.error('Observer got an error: ' + err),
  () => console.log('Observer got a complete notification')
);
```

## convert unicast to multicast with Subject

```js
var subject = new Rx.Subject();

subject.subscribe({
  next: (v) => console.log('observerA: ' + v)
});
subject.subscribe({
  next: (v) => console.log('observerB: ' + v)
});

var observable = Rx.Observable.from([1, 2, 3]);

observable.subscribe(subject); // You can subscribe providing a Subject

// observerA: 1
// observerB: 1
// observerA: 2
// observerB: 2
// observerA: 3
// observerB: 3
```
