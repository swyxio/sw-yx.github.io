---
layout: post
date: 2018-08-21
tags: git
feelings: neutral
title: git upstream notes
comments: true
description: working on a fork of a moving target
---

# working on a fork of a moving target

i know this is something i need to do so here are some notes on it

this was super helpful https://stackoverflow.com/questions/8948803/what-does-git-remote-add-upstream-help-achieve

the precise instructions i use are:

```js
git remote add upstream https://github.com/netlify/netlify-www.git

git fetch upstream

git checkout master

git rebase upstream/master

git checkout 100-retweet-bugfix
```

i will alias the middle 3 as `gupdatefork`

---

# Visual Internal Documentation

> Making it easier to contribute to open source

Most documentation in open source is user facing. Whatever deficiencies we perceive in open source documentation (there isn't enough of it, for starters) is magnified 10x when it comes to documenting how it works for folks who want to contribute. Most CONTRIBUTING.md's read something like this:

```
1. Fork and clone the repo
2. Run `npm install` to install dependencies
3. Good luck!
```

Any time I see that I usually think of the after credits scene from Finding Nemo:

![fish asking "now what"](https://i.kym-cdn.com/photos/images/original/001/142/233/897.gif)

While time is of course scarce, I am always interested in simple ideas for making open source documentation easier, since it has a domino effect further down on whether people feel welcome to get involved.

Recently [Bret Comnes](https://twitter.com/uhhyeahbret) pointed me to [Sander Verweij](https://sverweij.github.io/)'s [dependency-cruiser](https://github.com/sverweij/dependency-cruiser) project, which describes itself like this:

> Validate and visualize dependencies. With your rules. JavaScript, TypeScript, CoffeeScript. ES6, CommonJS, AMD.

I think visualizing projects is really important when you want to do some [code spelunking](http://www.codespelunking.org/) - diving into large codebases you don't know anything about. If you're diving into a series of caves, wouldn't it be nice to have a map?

## Getting Started with `dependency-cruiser`

You can install `dependency-cruiser` globally with `npm i -g dependency-cruiser`. Then, in the folder of any project you care about, you can run `depcruise --exclude "^node_modules" --output-type dot src | dot -T svg > dependencygraph.svg` assuming the core of your code lives in the `src` sub folder and you have [Graphviz dot](https://www.graphviz.org/) installed (it comes installed in Netlify's buildbot so no fears about that).

Here's `dependency-cruiser` run on the `reactive-react` project I recently released at [React Rally](https://www.youtube.com/watch?v=SaO-7Lk5hZ8):

![dependencygraph.svg](https://github.com/sw-yx/reactive-react/raw/master/dependencygraph.svg?sanitize=true)

Here you can see that `index.js` is the highest level file, followed by `scheduler.js` and `reconciler.js`, while `swyxjs.js` seems like one of those "utils" files that is imported by everybody without importing anything else (it is). Also, `updateProperties.js` is never used. 

Pretty handy for a first glance! But what does it look like for a non-trivial project? 

## Visual code spelunking

Here's [Preact](https://preact.com)!

![preactgraph](https://user-images.githubusercontent.com/6764957/44438126-b7f31b00-a58b-11e8-932c-4456d0a47262.png)

This is slightly messier, but can still be followed. `render`, `component`, `render-queue`, `clone-element`, and `h` are top level imports, and `vdom` is an important submodule. The "terminal dependencies" like `constants`, `util`, `options`, and `vnode` also stand out as items that are frequently imported which themselves have no dependencies.

That's great! As I get into contributing to Preact I can do a breadth first scan of the top level stuff while keeping track of those files I have visited/should visit!

The arrows also suggest potential opportunities for less confusing file structures. `dependency-cruiser` highlights actual (invalid) circular dependencies, but some dependencies can be circular while importing completely independent pieces of code. Here, we see that:

- `src/component.js` imports 
- `src/vdom/component.js` which imports
- `src/vdom/component-recycler.js` which imports 
- `src/component.js` again.

This is valid code because `src/component` only wants `renderComponent` from `src/vdom/component`, while `src/vdom/component-recycler` only wants `Component` from `src/component`. This can be fine especially since Preact places a high priority on filesize, but you can see how this might get confusing quickly for projects of even marginally higher complexity.

## Visualizing massive projects

It is possible to go too far though. Here's the core React package (`packages/react`, without `react-dom`):

![don't zoom in, this is low res](https://user-images.githubusercontent.com/6764957/44438793-c4c53e00-a58e-11e8-85ce-8f1ddc2c3300.png)

I'd print that out and put it on my wall, but it's certainly not a very useful map! Here's where you run it with the `[max-depth](https://github.com/sverweij/dependency-cruiser/blob/develop/doc/cli.md#--max-depth)` flag:

`depcruise --max-depth 2 --exclude "^(node_modules|forks|__tests__)" --output-type dot packages/react/src | dot -T svg > dependencygraph.svg`

Which produces:

![depth2 react](https://user-images.githubusercontent.com/6764957/44439279-f0492800-a590-11e8-91d9-a0c15bd78c08.png)

Isn't that gorgeous?! But more importantly, if I am contributing to the top level files for React, I can look ahead to see where all my dependencies are in relation to what I am working on. Tweak the command line accordingly for exploring further

## Continuous Visual Documentation

Being Netlify, we of course want to explore how we can make this visualization continuously updated. Fortunately, you can install `dependency-cruiser` as a local dependency in your project, and then add an npm script to `package.json`:

```json
{
  "scripts": {
    "build:deptree": "depcruise --exclude '^node_modules' --output-type dot reactive-react | dot -T svg > dependencygraph.svg",
  }
}
```

Now if you include `build:deptree` in your final build script, your dependency graph updates **every time**!!! What? Documentation that doesn't go out of date!? Sign me up!

If you use a documentation system for your OSS project like [docusaurus](https://docusaurus.io/) (your friendly neighborhood dinosaur that just happens to be very passionate about great docs!) for example:

- `yarn add -D dependency-cruiser`
- In your `docusaurus` site folder, add build script, something like `"build:deptree": "depcruise --exclude '(node_modules|__tests__|dist)' --output-type dot ../packages | dot -T svg > static/img/dependencygraph.svg"`
  - `../packages` points to the source file for the OSS project which happens to sit next to the `docusaurus` site in file structure
- Now that the svg is in your static files, you can refer to it in your docs like so:

```
## Internal stucture

Here is a chart of how the package is set up:

![test](/img/dependencygraph.svg)
```

[Here it is live deployed in the wild!](https://babel-blade.netlify.com/docs/index.html#internal-structure)

## Next steps

I do think this is only the start of a bigger discussion around how we can promote better internal documentation for open source projects, and ironically visual documentation can be easier than keeping swathes of text up to date when code always moves faster. The svg generated has direct links to the files they represent, and it would be interesting to explore a deeper integration with `docusaurus` where you are able to click on any box and pull up the file right in the docs, or hover to see some top level comments.

There are a ton of possibilities with Visual Internal Documentation, and I hope this has given you an idea of how to get started!
