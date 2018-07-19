---
layout: post
date: 2018-07-19
tags: react
feelings: happy
title: react debug thingy continued
comments: true
description: more debugging of the react thingy
---

so the response from last night:

```
i think i am stuck. probably have to make a change to react-error-overlay, but hopefully it wont break anything serious

 
i made the edits to the warning module no problem - but when `proxyConsole` triggers due to the `console.error` call, it first passes the stack to a "massageWarning" function

https://github.com/facebook/create-react-app/blob/19e0bb1881e24fb1ee3fe421413d4d87e67f68dd/packages/react-error-overlay/src/listenToRuntimeErrors.js#L66 …

 
the problem with this function (https://github.com/facebook/create-react-app/blob/19e0bb1881e24fb1ee3fe421413d4d87e67f68dd/packages/react-error-overlay/src/utils/warnings.js …) is that it expects frames instead, which is of type ReactFrame[]. we are instead passing it a string stack.

 this causes the parsing to fail - presumably you changed the types somewhere between 15 and 16 and they fell out of sync

 
am i allowed to modify `permanentRegisterConsole` in `react-error-overlay`? does anyone still use it? because i can just basically delete that one line and it would work

 think this is the only thing left i need to solve

```

response:

```
oh, I mean you could expose the "object" stack on ReactDebugCurrentFrame

the string we use wouldn't be sufficient because it doesn't include full filenames.. I think

 so it would be something like ReactDebugCurrentFrame.getStackFrames() which would return an object
```

---


this took a while to parse because `getStackFrames` doesnt exist. i basically had to figure out how to implement it!

it took maybe 1hr before i realized i could simply use `currentlyValidatingElement`. so i inserted my own `getStackFrames`.

---

well looks like i cant do that, `currentlyValidatingElement` is null at the time of warning. ugh. i would get it from `ReactCurrentOwner` but it is `null` at the time of warning as well.

here's the current injection process for `getStackAddendum`:

- `ReactDebugCurrentFrame.getCurrentStack`
- (inside `react-reconciler`) `getCurrentFiberStackInDev` (this has the `current` Fiber in scope) -> `getStackByFiberInDevAndProd` -> `describeFiber`

so what i need to do is make my own injection process. not super hard but i am exposing an internal thing which may not be so good. i don't think i have much of a choice atm.

things to do

- [x] import Fiber type into ReactDebugCurrentFrame
- [x] ReactDebugCurrentFrame.currentFiber = null | Fiber
- [x] add ReactDebugCurrentFrame.currentFiber to setCurrentFiber
- [ ] ReactDebugCurrentFrame.getStackFrames builds the list of frames if currentFiber exists

