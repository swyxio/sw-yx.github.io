---
layout: post
date: 2018-07-25
tags: react
feelings: determined
title: react callstack pr contd contd
comments: true
description: even more work on the react pr
---

prior work here: https://sw-yx.github.io/2018/07/19/react-pr-warnings-contd

so dan got back with suggestions for minor rewrites, and that is done: https://github.com/facebook/react/pull/13242#pullrequestreview-139864483

however the core problem still isnt resolved - how to write good tests for this thing. dan didnt seem to understand me :(

i tried using other renderers other than reactnoop:

- reactdom - there is no `document`
- reacttestutils - there is no `document`

```js
/**
 * Copyright (c) 2013-present, Facebook, Inc.
 *
 * This source code is licensed under the MIT license found in the
 * LICENSE file in the root directory of this source tree.
 *
 * @emails react-core
 * @jest-environment node
 */

'use strict';

let React;
// let ReactFeatureFlags;
// let ReactNoop; 
// let ReactDOM; // doesnt work
// const ReactTestUtils = require('react-dom/test-utils'); // doesnt work

describe('ReactStackFrameWarnings', () => {
  beforeEach(() => {
    jest.resetModules();
    // ReactFeatureFlags = require('shared/ReactFeatureFlags');
    // ReactFeatureFlags.debugRenderPhaseSideEffectsForStrictMode = false;
    React = require('react');
    // ReactNoop = require('react-noop-renderer');
    // ReactDOM = require('react-dom');
  });

  it('calls console.reactStack with warning frames if available', () => {
    function Foo() {
      return <div class="bar" />;
    }

    console.reactStack = () => {};
    spyOnDev(console, 'reactStack');

    // ReactDOM.render(<Foo />);
    ReactTestUtils.renderIntoDocument(<Foo />);
    expect(console.reactStack).toHaveBeenCalledTimes(1); // doesnt work yet
    // // then check for the frames
    // expect(console.reactStack.calls.argsFor(0)[0].message).toEqual(
    //   ['mock frames', 'here', 'tbd'],
    // );
  });
});

```



