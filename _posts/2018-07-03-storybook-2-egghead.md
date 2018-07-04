---
layout: post
date: 2018-07-03
tags: egghead
feelings: neutral
title: storybook 2 egghead
comments: true
description: getting back on the egghead train
---

well my first egghead video was pretty much an unexpected success and i honestly didnt even try that hard. time to pour more into it.

what to teach now: 

# add a React component to your Storybook

1. set up config.js

```js
import { configure } from '@storybook/react';
const req = require.context('../src', true, /.stories.js$/);
function loadStories() {
  req.keys().forEach(filename => req(filename));
}

configure(loadStories, module);
```

2. set up Button.js

```js
import React from 'react';

export const Button = ({ bg }) => (
  <button style={{ backgroundColor: bg }}> I am a button </button>
);
```

3. set up Button.stories.js

```js
import React from 'react';

import { storiesOf } from '@storybook/react';
import { Button } from './Button';

storiesOf('Button', module).add('with PGR background', () => (
  <Button bg="palegoldenrod">Hello Button</Button>
));
```
