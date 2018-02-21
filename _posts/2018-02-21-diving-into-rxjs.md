---
layout: post
date: 2018-02-21
tags: rxjs
feelings: neutral
title: diving into rxjs
comments: true
description: diving into rxjs
---

<https://gist.github.com/btroncone/d6cf141d6f2c00dc6b35>
<https://gist.github.com/staltz/868e7e9bc2a7b8c1f754>
<https://www.learnrxjs.io/>

for react usage: 

https://github.com/acdlite/recompose/blob/master/docs/API.md#componentfromstreamwithconfig

---

# basic hello world

```js
import React from "react";
import { componentFromStream } from "recompose";

const Potato = componentFromStream(props$ =>
  props$.map(props => <div>{props.message}</div>)
);
export default () => <Potato message="hello world" />;
```

# a very basic rxreact timer using Observable.interval

```js
import React from "react";
import { setObservableConfig, componentFromStream } from "recompose";
import rxjsConfig from "recompose/rxjsObservableConfig";
import { Observable } from "rxjs";

setObservableConfig(rxjsConfig);

const Potato = componentFromStream(props =>
  Observable.interval(1000).map(count => <h1>{count}</h1>)
);

export default Potato;
```

# typewriter effect example using Observable.zip, .from, .interval, .scan, .switchMap and .map

```js
import React from "react";
import rxjsConfig from "recompose/rxjsObservableConfig";
import { Observable } from "rxjs";
import { setObservableConfig, componentFromStream } from "recompose";

setObservableConfig(rxjsConfig);

const createTypewriter = message =>
  Observable.zip(
    Observable.from(message),
    Observable.interval(100),
    letter => letter
  ).scan((acc, curr) => acc + curr);

const Potato = props => <div>{props.message}</div>;

const AppStream = componentFromStream(props$ =>
  props$
    .switchMap(props => createTypewriter(props.message))
    .map(message => ({ message }))
    .map(Potato)
);

export default () => <AppStream message="hello world" />;
```

