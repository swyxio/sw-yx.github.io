---
layout: post
date: 2018-11-04
tags: react
feelings: happy
title: hooksifying react faunadb netlify app
comments: true
description: hooksifying react faunadb netlify app
---

Hi! Just thought I would refactor a demo site to get some practice and learn about hooks (just trying it out).

> ⚠️obligatory dont do this at home kids warning, react hooks are in alpha and you should read the docs over any third party content

This is the class based react project I started with: https://github.com/netlify/fauna-one-click

It sets up a React site with an automatically configured FaunaDB backend and serverless functions. Pretty sweet! But it uses classes. Could be sweeter!

This is the end result: https://github.com/sw-yx/hooks-suspense-faunadb-todo and it is deployed here: https://hooks-suspense-faunadb-todo.netlify.com/

this isn't meant to be "best practice" code or even my "best" code.. literally just playing around with a "breakable toy".

---

**Lessons Learned**

The first few steps are:

- convert all classes to functions accepting `props`
- delete `this.` anything
- all event handlers become "just functions", no binding
- all `render()` functions just get deleted, you only need what's inside

That's the first cut, but its not all of it. you still have to swap all lifecycle methods to `useEffect` or other hooks. Sometimes you get lucky and spot a chance to make a reusable hook. One example:


        componentDidMount() {
          // attach event listeners
          document.body.addEventListener('keydown', this.handleEscKey)
        }
        componentWillUnmount() {
          // remove event listeners
          document.body.removeEventListener('keydown', this.handleEscKey)
        }
        handleEscKey = (e) => {
          if (this.props.showMenu && e.which === 27) {
            this.props.handleModalClose()
          }
        }

this becomes:

    useKeydown('Escape', () => props.showMenu && props.handleModalClose());

Sometimes you're not so lucky and simply make a `componentDidMount` like [here](https://github.com/netlify/fauna-one-click/blob/master/src/App.js#L16) into a `useEffect` with an empty array second argument  like [here](https://github.com/sw-yx/hooks-suspense-faunadb-todo/blob/master/src/App.js#L21). No big deal.

Then you start breaking away from the old model and thinking about what hooks enable you to do that classes dont let you do. for me a non obvious win was not using objects in state.

From this: https://pbs.twimg.com/media/DrMvLSpVYAEjrJs.jpg

To this: https://pbs.twimg.com/media/DrMvkmbUwAAY0pj.jpg

Makes me wonder how we lived with objects as state for so long lol.

thats it! hoping to refactor the api stuff into a Suspense cache soon but that will involve writing my own cache for shits and giggles. thanks for reading.
