---
layout: post
date: 2018-02-26
tags: rxjs
feelings: sick
title: rxjs cheatsheet
comments: true
description: an rxjs cheatsheet
---

## Observable creators (eg static operators)

### of

immediately emit all arguments provided

```js
var numbers = Rx.Observable.of(10, 20, 30);
```

### from

convert a promise or array or iterable or Observable-like object to Observable

```js
var observable = Rx.Observable.from([10, 20, 30]);
var subscription = observable.subscribe(x => console.log(x));
```

or 

```js
function* generateDoubles(seed) {
  var i = seed;
  while (true) {
    yield i;
    i = 2 * i; // double it
  }
}

var iterator = generateDoubles(3);
var result = Rx.Observable.from(iterator).take(10);
result.subscribe(x => console.log(x));

// Results in the following:
// 3 6 12 24 48 96 192 384 768 1536
```

### interval

```js
var observable = Rx.Observable.interval(1000 /* number of milliseconds */);
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

### merge

```js
var merged = Rx.Observable.merge(observable1, observable2);
```

more Creation Operators: <http://reactivex.io/rxjs/manual/overview.html#creation-operators>

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
