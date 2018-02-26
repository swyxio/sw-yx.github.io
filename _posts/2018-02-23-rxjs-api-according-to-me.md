---
layout: post
date: 2018-02-23
tags: rxjs
feelings: neutral
title: rxjs api according to me
comments: true
description: rxjs api according to me
---

# Note to reader

Here is my cheatsheet while I am learning rxjs. I tend to prefer bullet points and short examples. I also like to work progressively from high level down to low level so that is how this is ordered. I am cribbing very heavily from [the rxjs manual](http://reactivex.io/rxjs/manual/overview.html) (the old docs) and [the new rjsdocs](https://github.com/reactivex/rxjs-docs).

I also care a lot about WHY to learn rxjs and the docs above should answer that better than me. But my initial impression is that it treats "time as a first class citizen", such that buffering and aggregating events become trivial declarative pure functions, instead of having to store results of imperative calculations in state.

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

---

## Observables vs Observers vs Subscriptions vs Subjects vs Operators

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

---

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

---

**Subscriptions** are:

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

---

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

Descriptions of Subject subtypes:

- [BehaviorSubject](http://reactivex.io/rxjs/manual/overview.html#behaviorsubject): BehaviorSubjects are useful for representing "values over time". For instance, an event stream of birthdays is a Subject, but the stream of a person's age would be a BehaviorSubject. `var subject = new Rx.BehaviorSubject(0); // 0 is the initial value`
- [ReplaySubject](http://reactivex.io/rxjs/manual/overview.html#replaysubject): A ReplaySubject records multiple values from the Observable execution and replays them to new subscribers. `var subject = new Rx.ReplaySubject(3); // buffer 3 values for new subscribers` or `var subject = new Rx.ReplaySubject(100, 500 /* windowTime */);`
- [AsyncSubject](http://reactivex.io/rxjs/manual/overview.html#asyncsubject): The AsyncSubject is similar to the last() operator, in that it waits for the complete notification in order to deliver a single value.

---

**Operators** are:

- pure methods on **Observables**
- return new **Observables** (instead of modifying the existing one)
- *static* (creates Observables) and *instance* (operates on Observables)

[Types of Operators](http://reactivex.io/rxjs/manual/overview.html#categories-of-operators):

- Creation 
- Transformation 
- Filtering
- Combination
- Multicasting
- Error Handling
- Utility
- Conditional/Boolean
- Mathematical and Aggregation

---

## (Advanced) Schedulers

[Schedulers](http://reactivex.io/rxjs/manual/overview.html#scheduler) are:

- a data structure: a priority queue for tasks
- an execution context: e.g. immediately, or in another callback mechanism such as setTimeout or process.nextTick, or the animation frame
- has a virtual clock: It provides a notion of "time" by a getter method now() on the scheduler.

There are 3 [types of Schedulers](http://reactivex.io/rxjs/manual/overview.html#scheduler-types) but RxJS will pick the " least concurrency scheduler" for you.

---

## The Observable lifecycle

Observables go through a rough lifecycle that can be described like this:

- Creation: e.g. `const observable = Rx.Observable.create(...)`
- Subscription: e.g. `const subscription = observable.subscribe(console.log)`
- Execution: the code inside `Rx.Observable.create(function (observer) {...})` which calls `observer.next()` and possibly `observer.error()` and `observer.complete()`. Essentially this is "manual" implementation of the observable, and could be supplanted by all the inbuilt operators rxjs provides.
- Disposal: e.g. `subscription.unsubscribe()` to cancel execution

---

# Stop reading and go learn operators

You really get good when you learn some basic [Types of Operators](http://reactivex.io/rxjs/manual/overview.html#categories-of-operators). Go!

---

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

## refCount()

<http://reactivex.io/rxjs/manual/overview.html#reference-counting>

You can also use `observable.multicast(subject)` but theres no discernable difference between [multicasting](http://reactivex.io/rxjs/manual/overview.html#multicasted-observables) and Subject.

## Manually written operator

<http://reactivex.io/rxjs/manual/overview.html#what-are-operators->

```js
function multiplyByTen(input) {
  var output = Rx.Observable.create(function subscribe(observer) {
    input.subscribe({
      next: (v) => observer.next(10 * v),
      error: (err) => observer.error(err),
      complete: () => observer.complete()
    });
  });
  return output;
}

var input = Rx.Observable.from([1, 2, 3, 4]);
var output = multiplyByTen(input);
output.subscribe(x => console.log(x));

// 10
// 20
// 30
// 40
```
