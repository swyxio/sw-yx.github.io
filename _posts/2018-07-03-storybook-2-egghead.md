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

export const Button = ({ bg, children }) => (
  <button style={{ backgroundColor: bg }}> {children} </button>
);
```

3. set up Button.stories.js

```js
import React from 'react';

import { storiesOf } from '@storybook/react';
import { Button } from './Button';

storiesOf('Button', module).add('with background', () => (
  <Button bg="palegoldenrod">Hello Button</Button>
));
```

# add a Welcome Screen to your Storybook

define `welcomeStory.js`

```js
import React from 'react';
import { storiesOf } from '@storybook/react';

storiesOf('Welcome', module).addWithJSX(
  'to your new Storybook🎉',
  () => <h1>Welcome to your new Storybook!</h1>)
);
```

go to config:

```js
  require('./welcomeStory');
```

# Use the JSX addon in Storybook

`yarn add -D @storybook/addons storybook-addon-jsx`

`.storybook/addons.js`:

`import 'storybook-addon-jsx/register';`

in config.js:

```js
import { setAddon } from '@storybook/react';
import JSXAddon from 'storybook-addon-jsx';
setAddon(JSXAddon);

// addDecorator
// import { withKnobs, select } from '@storybook/addon-knobs/react';
// addDecorator(withKnobs);

```

use `addWithJSX`


# Use the Info addon to document your Storybook stories

`yarn add -D @storybook/addon-info`

use markdown

```
storiesOf('Component', module)
  .add('simple info',
    withInfo(`
      description or documentation about my component, supports markdown
    
      ~~~js
      <Button>Click Here</Button>
      ~~~
    
    `)(() =>
      <Component>Click the "?" mark at top-right to view the info.</Component>
    )
  )
```


```

storiesOf('Component', module)
  .add('simple info',
    withInfo({
      styles: {
        header: {
          h1: {
            color: 'red'
          }
        }
      },
      text: 'String or React Element with docs about my component', // Warning! This option's name will be likely renamed to "summary" in 3.3 release. Follow this PR #1501 for details
      // other possible options see in Global options section below
    })(() =>
      <Component>Click the "?" mark at top-right to view the info.</Component>
    )
  )
```

utils.js:

```js
import { withInfo } from '@storybook/addon-info';
const wInfoStyle = {
  header: {
    h1: {
      marginRight: '20px',
      fontSize: '25px',
      display: 'inline'
    },
    body: {
      paddingTop: 0,
      paddingBottom: 0
    },
    h2: {
      display: 'inline',
      color: '#999'
    }
  },
  infoBody: {
    backgroundColor: '#eee',
    padding: '0px 5px',
    lineHeight: '2'
  }
};
export const wInfo = text =>
  withInfo({ inline: true, source: false, styles: wInfoStyle, text: text });
```
