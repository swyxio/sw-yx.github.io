---
layout: post
date: 2018-07-18
tags: react
feelings: happy
title: react pr warnings
comments: true
description: an assignment from dan
---

one of my guiltiest things is that i have only contributed to react once (i have 2 outstanding PRs) and dan has hinted to me repeatedly that i should get more involved.

today he just straight up assigned this to me so i'm doing it

---

>I recently made it so that [warning() calls automatically attach the stack](https://github.com/facebook/react/commit/82c7ca4cca90976cd1230e03ea1a4372a4276e67). It’s available right in that module

<https://github.com/facebook/react/blob/master/packages/shared/warning.js>

> Now that we always have stack info, we can expose it to other tools consistently too

> In fact Create React App’s error overlay already intercepts console.error calls if a special method called console.reactStack was called before

> React 15 made console.reactStack calls only for one warning, to test the idea. We never implemented it for other warnings and then didn’t implement it at all in 16

> Suggestion: take a look at the API that React 15 was calling (search the UMD bundle for console.reactStack calls). Try doing the same in 16, except now in the centralised place (warning module). Use Create React App to check if it worked. When it works it should start showing the error overlay for warnings too, and include component stack as actual frames in the overlay

---

Let's do it...

- so the first thing i need is to search react 15 UMD for console.reactStack.
- i found it. it looks like this:

```js
// pushing warning onto stack
    if (typeof console.reactStack !== 'function') {
      return;
    }
    // blah    
    console.reactStack(stack);

// popping warning off stack
    if (typeof console.reactStackEnd !== 'function') {
      return;
    }
    console.reactStackEnd();
```

- `pushNonStandardWarningStack`/`popNonStandardWarningStack` is only called for `createElement` warning in React 15.
- ok now what? I dont know what `warning.js` does!!
- well thats not true. "warning() calls automatically attach the stack". i can kinda see that - it passes an extra `stack` arg at the end to `warningWithoutStack`.
- let's see how `errorOverlay` intercepts `reactStack`.
- ah so it DOESNT intercept reactStack - [it DEFINES it!](https://github.com/facebook/create-react-app/blob/8db5e336b14bf9c61bd34901ce52d2c0963ea439/packages/react-error-overlay/src/effects/proxyConsole.js#L29). boom.
- so i just insert `console.reactStack` and `console.reactStackEnd` calls around the `warningWithoutStack` call and thats it it looks like lol
- now to actually test this mofo. urghghhghghg. how to npm link....
- installing google closure compiler and java jdk..
- turns out [CRA symlinks](https://github.com/facebook/create-react-app/issues/3883) have also been a longstanding issue
- so it wasnt too bad. i just did [what it says on the docs](https://reactjs.org/docs/how-to-contribute.html#development-workflow)
- but the problem is now the stack warning that i expect to come up doesnt come up. lets dig into it.
- ok i used `debugger` and i found that `warningWithoutStack` just doesnt take arguments at all so passing the stack doesnt mean anything.

![image](https://user-images.githubusercontent.com/6764957/42921698-2cc1ca64-8aeb-11e8-951f-f2091bacadd7.png)

- everything looks like it works. the problem is the god damn overlay not coming up.
- i am putting things in `reactFrameStack` but its not even printing or anything. time to rethink.

ok

- so somehow the stack is getting nuked somewhere in the process. i need to double check how the stack is printed? called?
- intentionally making error of setting style as a string so that i can observe how the stack is thrown

ok so heres what a real error looks like

```
temp1.message
"The `style` prop expects a mapping from style properties to values, not a string. For example, style={{marginRight: spacing + 'em'}} when using JSX.
    in div (at test.js:8)
    in App (at App.js:17)
    in div (at App.js:9)
    in App (at index.js:7)"
temp1.stack
"Error: The `style` prop expects a mapping from style properties to values, not a string. For example, style={{marginRight: spacing + 'em'}} when using JSX.
    in div (at test.js:8)
    in App (at App.js:17)
    in div (at App.js:9)
    in App (at index.js:7)
    at invariant (http://localhost:3000/static/js/bundle.js:791:15)
    at assertValidProps (http://localhost:3000/static/js/bundle.js:7369:63)
    at setInitialProperties$1 (http://localhost:3000/static/js/bundle.js:8528:3)
    at finalizeInitialChildren (http://localhost:3000/static/js/bundle.js:9540:3)
    at completeWork (http://localhost:3000/static/js/bundle.js:15272:17)
    at completeUnitOfWork (http://localhost:3000/static/js/bundle.js:16965:24)
    at performUnitOfWork (http://localhost:3000/static/js/bundle.js:17143:12)
    at workLoop (http://localhost:3000/static/js/bundle.js:17155:24)
    at renderRoot (http://localhost:3000/static/js/bundle.js:17194:7)
    at performWorkOnRoot (http://localhost:3000/static/js/bundle.js:17969:7)
    at performWork (http://localhost:3000/static/js/bundle.js:17876:7)
    at performSyncWork (http://localhost:3000/static/js/bundle.js:17844:3)
    at requestWork (http://localhost:3000/static/js/bundle.js:17753:5)
    at scheduleWork$1 (http://localhost:3000/static/js/bundle.js:17545:5)
    at scheduleRootUpdate (http://localhost:3000/static/js/bundle.js:18212:3)
    at updateContainerAtExpirationTime (http://localhost:3000/static/js/bundle.js:18239:10)
    at updateContainer (http://localhost:3000/static/js/bundle.js:18266:10)
    at ReactRoot.../react/build/node_modules/react-dom/cjs/react-dom.development.js.ReactRoot.render (http://localhost:3000/static/js/bundle.js:18551:3)
    at http://localhost:3000/static/js/bundle.js:18691:14
    at unbatchedUpdates (http://localhost:3000/static/js/bundle.js:18106:10)
    at legacyRenderSubtreeIntoContainer (http://localhost:3000/static/js/bundle.js:18687:5)
    at Object.render (http://localhost:3000/static/js/bundle.js:18746:12)
    at Object../src/index.js (http://localhost:3000/static/js/bundle.js:37613:51)
    at __webpack_require__ (http://localhost:3000/static/js/bundle.js:679:30)
    at fn (http://localhost:3000/static/js/bundle.js:89:20)
    at Object.0 (http://localhost:3000/static/js/bundle.js:37819:18)
    at __webpack_require__ (http://localhost:3000/static/js/bundle.js:679:30)
    at http://localhost:3000/static/js/bundle.js:725:37
    at http://localhost:3000/static/js/bundle.js:728:10"
temp1.name
"Invariant Violation"
temp1.framesToPop
1
```

it throws to `catch(thrownValue)` which does:
- `resetCurrentlyProcessingQueue`
- `replayFailedUnitOfWorkWithInvokeGuardedCallback`
- `replayUnitOfWork`
- `invokeGuardedCallback$2`
- which dispatches the "fake event" - `react-invokeguardedcallback`

ok that was too in the weeds. i need to just know how to form an error object, and throw the stack with it. that should probably be enough.

so the error is being made like this:

```js
function invariant(condition, format, a, b, c, d, e, f) {
  validateFormat(format);

  if (!condition) {
    var error = void 0;
    if (format === undefined) {
      error = new Error('Minified exception occurred; use the non-minified dev environment ' + 'for the full error message and additional helpful warnings.');
    } else {
      var args = [a, b, c, d, e, f];
      var argIndex = 0;
      error = new Error(format.replace(/%s/g, function () {
        return args[argIndex++];
      }));
      error.name = 'Invariant Violation';
    }

    error.framesToPop = 1; // we don't care about invariant's own frame
    throw error;
  }
}
```

this is the key piece. i should be able to throw it. is that what he wants? whats the diff between an invariant and a warning then?

- he says "Create React App’s error overlay already intercepts console.error calls if a special method called console.reactStack was called before". This doesnt seem to be true.
- ok so i think i have it. `parsedFrames` inside `getStackFrames` inside `crashWithFrames` isn't parsing anything because the `error.stack` it has is just `""`. This is because the stack is being nuked. one more time to capture it and fix the process.

the flow

- `console.reactStack(stack)` - this is fine
- `warningWithoutStack` - this triggers `console.error`
- which triggers `react-dev-utils`' `proxyConsole`
- which calls its `callback` - this is the function that is broken somehow
- this is inside "utils warnings" which calls a "massage" function
- `massage` expects a `ReactFrame[]` for `frames` but just gets a string
