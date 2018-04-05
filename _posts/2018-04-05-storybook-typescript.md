---
layout: post
date: 2018-04-05
tags: storybook react
feelings: happy
title: Quick Guide to setup your React + Typescript Storybook Design System
comments: true
description: Quick Guide to setup your React + Typescript Storybook Design System
---

today i [contributed to the storybook docs](https://github.com/storybooks/storybook/pull/3361) and wrote [a companion instruction document](https://dev.to/swyx/quick-guide-to-setup-your-react--typescript-storybook-design-system-1c51)

---
title: Quick Guide to setup your React + Typescript Storybook Design System
published: true
description: a no-bs guide to set up your new React and Typescript Design System powered by Storybook!
tags: react, typescript, storybook
---

Design systems are all the rage these days - here's how to make your own.

Because React is built on a plug and play component philosophy, every company has rushed to build and open source their component libraries, which are both displayed on a hot reloadable Storybook as well as importable as an npm library. [Look at all these companies!!!](https://storybook.js.org/examples/)

![image](https://user-images.githubusercontent.com/35976578/38384106-4ec86d9a-38dc-11e8-952b-ee542cf14ef5.png)

Because companies also care about maintainability, they also like creating Design Systems in Typescript. The prop typing that Typescript enforces helps us autogenerate documentation for our design systems, so it is a win-win!

Today we are going to walk through how to build and ship a React + Typescript Storybook Design System with handy addons for documentation. The end result looks like this:

![image](https://user-images.githubusercontent.com/35976578/38384106-4ec86d9a-38dc-11e8-952b-ee542cf14ef5.png)

Ready? Lets go!

Assuming you are in an empty folder:

```bash
yarn init -y
yarn add -D @storybook/react @storybook/addon-info @storybook/addon-knobs storybook-addon-jsx @types/react babel-core typescript awesome-typescript-loader react-docgen-typescript-webpack-plugin jest "@types/jest" ts-jest 
yarn add react react-dom
mkdir .storybook
touch .storybook/config.js .storybook/addons.js .storybook/welcomeStory.js utils.js
```

I have gone for a "colocated stories" setup where your story for a component lives next to the component. There is another setup where the stories are in a totally separate stories folder. I find this to be extra hassle when working on a component and its associated Story. So we will set up the rest of this app with colocated stories.

To have a runnable storybook, add this npm script to your `package.json`:

```json
{
  "scripts": {
    "storybook": "start-storybook -p 6006 -c .storybook"
  }
}
```

There's no strong reason why we want to run storybook on port 9001, it's just what they have on the docs.

In `.storybook/config.js`:

```js
import { configure } from '@storybook/react';
import { setAddon, addDecorator } from '@storybook/react';
import JSXAddon from 'storybook-addon-jsx';
import { withKnobs, select } from '@storybook/addon-knobs/react';
addDecorator(withKnobs);
setAddon(JSXAddon);

// automatically import all files ending in *.stories.js
const req = require.context('../src', true, /.stories.js$/);
function loadStories() {
  require('./welcomeStory');
  req.keys().forEach(filename => req(filename));
}

configure(loadStories, module);
```

In `.storybook/addons.js`:

```js
import '@storybook/addon-knobs/register';
import 'storybook-addon-jsx/register';
```

In `utils.js`:

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

In `.storybook/welcomeStory.js`:

```js
import React from 'react';

import { storiesOf } from '@storybook/react';
import { wInfo } from '../utils';

storiesOf('Welcome', module).addWithJSX(
  'to your new Storybook🎊',
  wInfo(`


    ### Notes

    Hello world!:

    ### Usage
    ~~~js
    <div>This is an example component</div>
    ~~~

    ### To use this Storybook

    Explore the panels on the left.
  `)(() => <div>This is an example component</div>)
);
```

Let's see it work! `npm run storybook`:

![image](https://user-images.githubusercontent.com/35976578/38380223-18b5a282-38d1-11e8-8c44-8395015435a1.png)

# Your first Typescript component

Time to make a Typescript component.

```bash
mkdir .storybook src src/Button
touch src/Button/Button.tsx src/Button/Button.css src/Button/Button.stories.js
```

In `src/Button/Button.tsx`:

```tsx
import * as React from 'react';
import './Button.css';
export interface Props {
  /** this dictates what the button will say  */
  label: string;
  /** this dictates what the button will do  */
  onClick: () => void;
  /**
   * Disables onclick
   *
   * @default false
   **/
  disabled?: boolean;
}
const noop = () => {}; // tslint:disable-line
export const Button = (props: Props) => {
  const { label, onClick, disabled = false } = props;
  const disabledclass = disabled ? 'Button_disabled' : '';
  return (
    <div
      className={`Button ${disabledclass}`}
      onClick={!disabled ? onClick : noop}
    >
      <span>{label}</span>
    </div>
  );
};

```

In `src/Button/Button.css`:

```css
.Button span {
  margin: auto;
  font-size: 16px;
  font-weight: bold;
  text-align: center;
  color: #fff;
  text-transform: uppercase;
}
.Button {
  padding: 0px 20px;
  height: 49px;
  border-radius: 2px;
  border: 2px solid var(--ui-bkgd, #3d5567);
  display: inline-flex;
  background-color: var(--ui-bkgd, #3d5567);
}

.Button:hover:not(.Button_disabled) {
  cursor: pointer;
}

.Button_disabled {
  --ui-bkgd: rgba(61, 85, 103, 0.3);
}
```


In `src/Button/Button.stories.js`:

```js
import React from 'react';

import { storiesOf } from '@storybook/react';
import { Button } from './Button';
import { wInfo } from '../../utils';
import { text, boolean } from '@storybook/addon-knobs/react';

storiesOf('Components/Button', module).addWithJSX(
  'basic Button',
  wInfo(`

  ### Notes

  This is a button

  ### Usage
  ~~~js
  <Button
    label={'Enroll'}
    disabled={false}
    onClick={() => alert('hello there')}
  />
  ~~~`
)(() => (
    <Button
      label={text('label', 'Enroll')}
      disabled={boolean('disabled', false)}
      onClick={() => alert('hello there')}
    />
  ))
);
```

We also have to make Storybook speak typescript:

```bash
touch .storybook/webpack.config.js tsconfig.json
```

In `webpack.config.js`:

```js
const path = require('path');
const TSDocgenPlugin = require('react-docgen-typescript-webpack-plugin');
module.exports = (baseConfig, env, defaultConfig) => {
  defaultConfig.module.rules.push({
    test: /\.(ts|tsx)$/,
    loader: require.resolve('awesome-typescript-loader')
  });
  defaultConfig.plugins.push(new TSDocgenPlugin());
  defaultConfig.resolve.extensions.push('.ts', '.tsx');
  return defaultConfig;
};
```

Note - you may have seen old instructions from `const genDefaultConfig = require('@storybook/react/dist/server/config/defaults/webpack.config.js');` but that is now deprecated. [We are using Full control mode + default instead.](https://storybook.js.org/configurations/custom-webpack-config/#full-control-mode--default)

In `tsconfig.json`:

```json
{
  "compilerOptions": {
    "outDir": "build/lib",
    "module": "commonjs",
    "target": "es5",
    "lib": ["es5", "es6", "es7", "es2017", "dom"],
    "sourceMap": true,
    "allowJs": false,
    "jsx": "react",
    "moduleResolution": "node",
    "rootDir": "src",
    "baseUrl": "src",
    "forceConsistentCasingInFileNames": true,
    "noImplicitReturns": true,
    "noImplicitThis": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "suppressImplicitAnyIndexErrors": true,
    "noUnusedLocals": true,
    "declaration": true,
    "allowSyntheticDefaultImports": true,
    "experimentalDecorators": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "build", "scripts"]
}
```

Ok that should be it. `npm run storybook` again!

Boom!

![image](https://user-images.githubusercontent.com/35976578/38381904-51eb3c6a-38d6-11e8-8ebd-800c29d12258.png)


# Time to build and ship your (one-Button) Design System

Typescript is only responsible for your Typescript-to-JS compiled code, but you're also going to want to ship CSS and other assets. So you have to do an extra copy process when you build your storybook:

```bash
yarn add -D cpx
touch src/index.tsx
echo "node_modules" >> .gitignore
git init # version control is good for you
```

In your `package.json`, add:

```json
{
  "main": "build/lib/index.js",
  "types": "build/lib/index.d.ts",
  "files": [
    "build/lib"
  ],
  "scripts": {
    "storybook": "start-storybook -p 6006 -c .storybook",
    "build": "npm run build-lib && build-storybook",
    "build-lib": "tsc && npm run copy-css-to-lib",
    "build-storybook": "build-storybook",
    "copy-css-to-lib": "cpx \"./src/**/*.css\" ./build/lib"
  },
}
```

Note that you already have a `main` from your init, so overwrite it.

In `src/index.tsx`:

```json
export {Button} from './Button/Button'
```

This is where you re-export all your components in one file so that you can import them all together. [This is known as the Barrel pattern](https://basarat.gitbooks.io/typescript/docs/tips/barrel.html)

Now when you run `npm run build`, it builds just your Design system in `build` without any of the storybook stuff, AND when you run `npm run build-storybook`, it builds a static page storybook you can host anywhere!

Did I leave out anything? let me know!
