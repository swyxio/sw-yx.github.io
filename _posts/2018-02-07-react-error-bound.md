---
layout: post
date: 2018-02-07
tags: react
feelings: neutral
title: react error bound
comments: true
description: sketching out what a react error bound lib would look like
---


continuing prior work from https://github.com/sw-yx/sw-yx.github.io/blob/master/_posts/2018-02-05-react-catch-error-prelim-work.md

first i need to deconstruct react-error-overlay.

then i should check out react dev utils:
- <https://github.com/reactjs-academy/react-dev-utils-academy>
- <https://www.npmjs.com/package/react-dev-utils>

and other react issues:
- <https://github.com/facebook/react/issues/11409>


---

ok its 7.15pm and i am looking into `react-error-overlay`.

react error overlay deals with 2 kinds of errors: webpack build errors and runtime errors. at it's core is an `update` function that gets run whenever you touch one of the 5 api's below. it then [creates an iframe](https://github.com/facebook/create-react-app/blob/47d2d94118db8d8366ac8036469439885d3e1525/packages/react-error-overlay/src/index.js#L122) to render your thing, and then goes away when you tell it to.

the entire API is encapsulated within this [index.js](https://github.com/facebook/create-react-app/blob/next/packages/react-error-overlay/src/index.js):

- setEditorHandler
- dismissBuildError
- reportBuildError
- startReportingRuntimeErrors
- stopReportingRuntimeErrors

the only other dependency is [errorOverlayMiddleware](https://github.com/facebook/create-react-app/blob/3767d91886823349ec46288d9b7ea48d4149890a/packages/react-dev-utils/errorOverlayMiddleware.js) which is super small. i used to think this was a necessary part of react-error-overlay but this was wrong.

now to investigate the 5 api's. this is helped by having a clear usage example in [webpackHotDevClient](https://github.com/facebook/create-react-app/blob/3767d91886823349ec46288d9b7ea48d4149890a/packages/react-dev-utils/webpackHotDevClient.js). i will refer to this as WHDC.

`setEditorHandler` receives a callback. this callback will be called in `updateIframeContent()` but it can easily be null since in WHDC the only usage is to sync with errorOverlayMiddleware.

Then there are two build error api's.

`reportBuildError` is a synchronous function. you feed it a string and react-error-overlay displays it.

`dismissBuildError` undoes that and set it to null.

so when you use react-error-overlay, you can mock up whatever UI you want and then just control it with those two functions alone.

but build errors are not the only kind of errors.

`startReportingRuntimeErrors` takes an options object:

```
type RuntimeReportingOptions = {|
  onError: () => void,
  filename?: string,
|};
```

onError is a callback that runs if `listenToRuntimeErrors` <https://github.com/facebook/create-react-app/blob/next/packages/react-error-overlay/src/listenToRuntimeErrors.js> sees a crash (but this is all internal and you shouldnt have to set it up). WHDC just uses it to help forcibly reload the entire application (the comments point to [this issue](https://github.com/facebook/create-react-app/issues/3096) which i have not explored).

filename is the more interesting one. this looks like a pointer to the bundle with a sourcemap. it would be interesting to set this but it doesnt seem strictly necssary? we shall see.

`stopReportingRuntimeErrors` just helps you stop listening to stuff i guess. ("why do we need this" [comment](https://github.com/facebook/create-react-app/blob/3767d91886823349ec46288d9b7ea48d4149890a/packages/react-dev-utils/webpackHotDevClient.js#L55))

---

now on to try it in a sample CRA app.
