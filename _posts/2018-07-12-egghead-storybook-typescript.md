---
layout: post
date: 2018-07-12
tags: egghead
feelings: happy
title: egghead storyboook typescript lesson planning
comments: true
description: A lesson plan for you know what
---


we are at lesson 6!

https://dev.to/swyx/quick-guide-to-setup-your-react--typescript-storybook-design-system-1c51

1. Add new files: touch .storybook/webpack.config.js tsconfig.json
2. add new deps: yarn add -D @types/react typescript awesome-typescript-loader
3. tsconfig:

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

3. Configure webpack: 

```js
const path = require('path');
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

3. turn button into tsx:

```tsx
import * as React from 'react';
import './Button.css';
export interface Props {
  children: React.ReactNode;
  onClick: () => void;
  disabled?: boolean;
}
const noop = () => {};
export const Button = (props: Props) => {
  const { children, onClick, disabled = false } = props;
  const disabledclass = disabled ? 'Button_disabled' : '';
  return (
    <div
      className={`Button ${disabledclass}`}
      onClick={!disabled ? onClick : noop}
    >
      <span>{children}</span>
    </div>
  );
};

```

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

---

docgen

1. `yarn add -D react-docgen-typescript-webpack-plugin`
2.  webpack config

`const TSDocgenPlugin = require('react-docgen-typescript-webpack-plugin');`

`defaultConfig.plugins.push(new TSDocgenPlugin());`

3. button.tsx

```tsx
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
```
