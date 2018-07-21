---
layout: post
date: 2018-07-20
tags: react
feelings: fat
title: warning day 3
comments: true
description: got feedback, time to fix
---

https://github.com/facebook/react/pull/13242#discussion_r204101288

i need to move this

```js
// import type {Fiber} from 'react-reconciler/src/ReactFiber';
{
  let frames = [];

  // Add an extra top frame while an element is being validated
  if (currentlyValidatingElement) {
    const _name = getComponentName(currentlyValidatingElement.type);
    const _source = currentlyValidatingElement._source;
    frames.push({
      fileName: _source && _source.fileName.replace(/^.*[\\\/]/, ''),
      lineNumber: _source && _source.lineNumber,
      name: _name,
    });
  }

  // renderer (react-reconciler) injects currentFiber alongside getCurrentStack
  if (ReactDebugCurrentFrame.currentFiber) {
    let node = ReactDebugCurrentFrame.currentFiber;
    do {
      const owner = node._debugOwner;
      const source = node._debugSource;
      frames.push({
        fileName: source && source.fileName.replace(/^.*[\\\/]/, ''),
        lineNumber: source && source.lineNumber,
        name: owner && getComponentName(owner.type),
      });
      node = node.return;
    } while (node);
  }
  return frames;
};
```

from ReactDebugCurrentFrame to somewhere inside the reconciler.

i did it for react-reconciler, but now the server renderer i am not sure about.

---

i did my best for server renderer but it is completely untested. what does stack frames even output to in this context? there is no r-e-o.

also the testing strategy for dom renderer is weird.

```js


describe('ReactStackFrameWarnings', () => {
  beforeEach(() => {
    jest.resetModules();
    ReactFeatureFlags = require('shared/ReactFeatureFlags');
    ReactFeatureFlags.debugRenderPhaseSideEffectsForStrictMode = false;
    React = require('react');
    ReactNoop = require('react-noop-renderer');
  });

  it('calls console.reactStack with warning frames if available', () => {
    function Foo() {
      return <div class="bar" />;
    }

    console.reactStack = () => {}
    spyOnDevAndProd(console, 'reactStack');

    ReactNoop.render(<Foo />);
    expect(console.reactStack).toHaveBeenCalledTimes(1); // doesnt work yet
      // expect(ReactNoop.flush).toWarnDev(
      //   'Warning: Stateless function components cannot be given refs. ' +
      //     'Attempts to access this ref will fail.\n\nCheck the render method ' +
      //     'of `Foo`.\n' +
      //     '    in FunctionalComponent (at **)\n' +
      //     '    in div (at **)\n' +
      //     '    in Foo (at **)',
      // );
    // expect(console.reactStack.calls.argsFor(0)[0].message).toEqual(
    //   ['mock frames', 'here', 'tbd'],
    // );
  });
});

```

but console.reactStack is not being called so something is wrong.
