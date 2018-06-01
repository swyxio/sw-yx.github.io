---
layout: post
date: 2018-05-31
tags: webpack
feelings: frustrated
title: webpack problems
comments: true
description: strugles with webpack trying to make a gatsby plugin
---

i wanted to help make a gatsby mdx prismajs plugin today to help the @reach/router docs use mdx but also have syntax highlighting.

it was easy enough to fork gatsby-plugin-mdx.

and the prismajs example i had uses the `module: rules` format:

```js
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /(node_modules)/,
        use: "babel-loader"
      },
      {
        test: /\.md$/,
        use: path.resolve("build/markdown-loader.js")
      }
    ]
  }
```

however the gatsby ecosystem uses [webpack-configurator](https://github.com/lewie9021/webpack-configurator) and it requires you to use "loaders".

There appears some confusion between webpack `loaders` vs `module.use` and they have switched back and forth confusingly: https://stackoverflow.com/questions/43002099/rules-vs-loaders-in-webpack-whats-the-difference/43805263

the docs don't really tell us any alternative way of configuring loaders - its just strings now: https://webpack.js.org/concepts/loaders/

they do have a section on [writing a loader](https://webpack.js.org/contribute/writing-a-loader/) and i dont think i have a problem with that.

---

i tried to use `config.merge()` like so:

```js

  config.merge(function(config) {
    config.module = {
      rules: [
        {
          test: /\.js$/,
          exclude: /(node_modules)/,
          use: 'babel-loader',
        },
        {
          text: mdFiles,
          use: path.resolve('markdown-loader.js'),
        },
      ],
    }
    console.log({ config })
    return config
  })
```


and this is the error i got - another not allowed error:

```bash
{ config:
   { context: '/Users/swyx/OpenSource/gatsby-plugin-mdx-prismjs/examples/basic',
     node: { __filename: true },
     entry:
      { main: '/Users/swyx/OpenSource/gatsby-plugin-mdx-prismjs/examples/basic/.cache/develop-static-entry' },
     debug: true,
     target: 'node',
     profile: false,
     devtool: 'source-map',
     output:
      { path: '/Users/swyx/OpenSource/gatsby-plugin-mdx-prismjs/examples/basic/public',
        filename: 'render-page.js',
        libraryTarget: 'umd',
        publicPath: '/' },
     resolveLoader: { root: [Array], modulesDirectories: [Array] },
     plugins:
      [ [StaticSiteGeneratorWebpackPlugin],
        [DefinePlugin],
        [ExtractTextPlugin] ],
     resolve: { extensions: [Array], modulesDirectories: [Array] },
     module: { rules: [Array] } } }
There were errors with your webpack config:
[1]
module.rules
object.allowUnknown , "rules" is not allowed


Your Webpack config does not appear to be valid. This could be because of
something you added or a plugin. If you don't recognize the invalid keys listed
above try removing plugins and rebuilding to identify the culprit.
```
