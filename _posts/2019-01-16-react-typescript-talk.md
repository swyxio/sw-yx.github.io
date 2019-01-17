---
layout: post
date: 2019-01-16
tags: react
feelings: happy
title: react typescript talk
comments: true
description: prep notes for react typescript talk
---

1. Create React App + TypeScript
2. Writing a Function component
3. Basic TypeScript types
4. Writing a Class component
5. React Hooks
6. Commenting and Docz

first fix package.json

```json
  "resolutions": {
    "ansi-styles": "^3.2.0"
  }
```

then `yarn add -D docz-theme-default docz@0.12.17`
then scripts `docz": "docz dev"`
then `doczrc.js`

```js
// doczrc.js
export default {
  typescript: true
};
```

and then write MDX

```mdx
---
name: MyComponent
route: /
---

import { PropsTable, Playground } from 'docz'
import {Button} from './Button'

## This is your Button component

Some text here

<Playground>
  <Button />
</Playground>

<PropsTable of={Button} />

More text here
```

8. Some advanced stuff - typeof, in/union types
7. DefinitelyTyped
8. Here be dragons
  - Linting: TSLint vs ESLint
  - Bikeshedding: Multiple ways to do things
  - Generic Type Logic
10. Why React + TypeScript?

Why do you test JS? Correctness.

11. People moving to TypeScript

- Jest
- Expo
- react-beautiful-dnd
- Storybook
- VueJS
- AirBnb
- Google
- Palantir
- Atlassian

https://twitter.com/swyx/status/1082290067937275904

