---
layout: post
date: 2018-01-31
tags: storybook
feelings: neutral
title: digging into storybook
comments: true
description: today i dig into storybook
---

my task today is to add react-error-overlay into my storybook setup.

it involves modifying `webpack.config.js` inside `.storybook` which is easy. <https://github.com/facebook/create-react-app/issues/2344>

but then i also have to insert middleware into the dev server run by storybook, which is the tricky bit. this was moved: <https://github.com/facebook/create-react-app/issues/3097> which causes minor confusion and some googling.

now to track the dependencies

- @storybook/react/src/server/index has the line for `app.use(storybook(configDir))`
- `configDir` is simply `.storybook`
- `storybook` comes from `./middleware` which is an export default
- `./middleware` has the `Router` i tried using it in `webpack.config.js` and it doesnt work.
- so instead i focus on `middlewareFn(router)`
- `middlewareFn =  getMiddleware(configDir)`, `getMiddleware` is from `./utils`
- `getMiddleware` in `utils.js` pulls from `configDir/middleware.js`.

and that led me to this: <https://github.com/storybooks/storybook/issues/641> which showed how to do custom middleware. it was dead simple.

just have `.storybook/middleware.js`:

```js

const errorOverlayMiddleware = require('react-dev-utils/errorOverlayMiddleware');
module.exports = errorOverlayMiddleware;
```

----

i filed this: <https://github.com/storybooks/storybook/issues/2890>



helpful issues:
- <https://github.com/facebook/create-react-app/issues/3097>
- <https://github.com/facebook/create-react-app/issues/2344>
