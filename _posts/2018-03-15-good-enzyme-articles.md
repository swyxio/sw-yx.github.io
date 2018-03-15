---
layout: post
date: 2018-03-15
tags: enzyme
feelings: neutral
title: good enzyme articles
comments: true
description: a. link list
---

i faced some trouble today getting jest to work in my storybook + typescript setup.

these issues helped me out:

// this runs before every *.test.tsx file
- <>https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#initializing-test-environment>
- <https://github.com/wmonk/create-react-app-typescript/issues/185>

basically you have to set up a `src/setupTests.ts` file and then go

```js
import * as enzyme from 'enzyme';
// import * as Adapter from 'enzyme-adapter-react-16'; // doesnt work for now
const Adapter = require('enzyme-adapter-react-16');
enzyme.configure({ adapter: new Adapter() });
```

then you can test as per normal

```js
import * as React from 'react';
import { shallow } from 'enzyme';

import { PrimaryButton } from './Button';

it('renders the heading', () => {
  const result = shallow(
    <PrimaryButton label="hi" onClick={() => alert('hi')} />
  ).contains(<span>hi</span>);
  expect(result).toBeTruthy();
});

```

here are some other helpful tutorials i found

- https://blog.theodo.fr/2017/04/enzyme-fast-and-simple-react-testing/
- http://engineering.pivotal.io/post/react-integration-tests-with-enzyme/
- https://hackernoon.com/oh-snap-snapshots-unit-integration-testing-with-jest-enzyme-13cc18aecb7b

i am mindful that unit tests/shallow rendering is easiest but doesnt mean much as you end up just testing react. (see kent dodds' essays on this)
