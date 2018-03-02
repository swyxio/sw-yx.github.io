---
layout: post
date: 2018-03-01
tags: react
feelings: inspired
title: A Walkthrough of *that* React Suspense Demo
comments: true
description: Annotated commentary on the code behind the Movie search demo featuring React Suspense
---

posted in <dev.to/swyx/a-walkthrough-of-that-react-suspense-demo--4j6a>

---

Bottomline up front: In this walkthrough of the 300ish line Movie Search demo, we learn the various aspects of the React Suspense API:

- `simple-cache-provider.SimpleCache` - puts a `cache` in `createContext`
- `simple-cache-provider.createResource` - which takes a promise for your data
- How to delegate updates to a lower priority with `ReactDOM.unstable_deferredUpdates`
- How `createResource` loads data asynchronously by **throwing Promises** (!!!)
- `React.Timeout` - just gives you a boolean for flipping between children and fallback
- How to use `createResource` to do **async image loading** (!!!)

Read on if you want to learn React Suspense!

---

The Async React demo at JSConf Iceland lived up to the hype: Time Slicing and React Suspense are on the way! (See the [official blogpost](https://reactjs.org/blog/2018/03/01/sneak-peek-beyond-react-16.html), [video](https://www.youtube.com/watch?v=v6iR3Zk4oDY), and [HN discussion](https://news.ycombinator.com/item?id=16492973) for more). Dev Twitter was buzzing with prominent devs working through the implications of Async React for everything from [React-Loadable](https://twitter.com/jamiebuilds/status/969166935836344321?s=21) to [React Router](https://twitter.com/ryanflorence/status/969235637244018688?s=21) to [Redux](https://twitter.com/wsokra/status/969284466043703297?s=21), and the always-on-the-ball Apollo Team even pushed out [a demo app built with Async React and Apollo](https://twitter.com/apollographql/status/969313298440126466)!

Heady stuff. This is the culmination of a years-long process, starting with [this tweet from Jordan Walke in 2014](https://twitter.com/jordwalke/status/500587022890061824), to [Lin Clark's intro to React Fiber](https://www.youtube.com/watch?v=ZCuYPiUIONs) (where you see Time Slicing working almost a year ago), to [the actual React Fiber release](https://code.facebook.com/posts/1716776591680069/react-16-a-look-inside-an-api-compatible-rewrite-of-our-frontend-ui-library/) in Sept 2017, to [Sebastian coming up with the suspender API](https://twitter.com/acdlite/status/969172311067713537) in Dec 2017.

But if you're just a regular React-Joe like me, you're feeling a little bit left behind in all this (as it should be - this is advanced stuff and not even final yet, so if you're a React newbie STOP READING AND GO LEARN REACT).

I learn by doing, and am really bad at grokking abstract things just by talking about them.

Fortunately, Andrew Clark published [a version of the Movie search demo](https://codesandbox.io/s/5zk7x551vk) on CodeSandbox! So I figured I would walk through just this bit since it's really all the demo usage code we have (apart from the Apollo demo which is a fork of this Movie search demo) and I didn't feel up to walking through the entire source code (I also happen to be really sick right now, but learning makes me happy :)).

Finally, some disclaimers because people get very triggered sometimes:

1. I'm a [recent bootcamp grad](https://hackernoon.com/no-zero-days-my-path-from-code-newbie-to-full-stack-developer-in-12-months-214122a8948f). You're not reading the divinings of some thought leader here. I'm just some guy learning in public.
2. This API is EXTREMELY UNSTABLE AND SUBJECT TO CHANGE. So forget the specifics and just think about if the concepts make sense for you.
3. If you're a React newbie YOU DO NOT NEED TO KNOW THIS AT ALL. None of this needs to be in any sort of React beginner curriculum. I would put this -after- your learning Redux, and -after- learning the [React Context API](https://reactjs.org/docs/context.html)

But learning is fun! Without further ado:

# Diving into React Suspense

Please have [the demo](https://codesandbox.io/s/5zk7x551vk) open in another screen as you read this, it will make more sense that way.

once again for the people who are skimming:

# [OPEN THE DEMO BEFORE YOU READ ON](https://codesandbox.io/s/5zk7x551vk)

## Meet `simple-cache-provider.SimpleCache`

The majority of the app is contained in `index.js`, so that's where we start. I like diving into the tree from top level down, which in the code means you read from the bottom going up. Right off the bat in line 303, we see that the top container is wrapped with the `withCache` HOC. This is defined in `withCache.js`:

```js
import React from 'react';
import {SimpleCache} from 'simple-cache-provider';

export default function withCache(Component) {
  return props => (
    <SimpleCache.Consumer>
      {cache => <Component cache={cache} {...props} />}
    </SimpleCache.Consumer>
  );
}
```

Here we see the second React API to adopt the child render prop (see [Kent Dodds' recap for the first](https://dev.to/kentcdodds/reacts--new-context-api-dpi)), and it simply provides a `cache` prop to whatever Component is passed to it. [The source for simple-cache-provider](https://github.com/facebook/react/blob/master/packages/simple-cache-provider/src/SimpleCacheProvider.js) comes in just under 300 lines of Flow-typed code, and you can see it uses [createContext](https://github.com/facebook/react/blob/master/packages/simple-cache-provider/src/SimpleCacheProvider.js#L297) under the hood. You may have heard a lot of fuss about the "throw pattern", but this is all nicely abstracted for you in `simple-cache-provider` and you never actually have to use it in your own code.

Just because it really is pretty cool, you can check it out in [line 187](https://github.com/facebook/react/blob/master/packages/simple-cache-provider/src/SimpleCacheProvider.js#L187) where the promise is thrown and then called in the `load` function in [line 128](https://github.com/facebook/react/blob/master/packages/simple-cache-provider/src/SimpleCacheProvider.js#L128). We'll explore this further down.

## Side Effects in Render

The main meat of the Movie Search demo is in the `MoviesImpl` component:

```js
class MoviesImpl extends React.Component {
  state = {
    query: '',
    activeResult: null,
  };
  onQueryUpdate = query => this.setState({query});
  onActiveResultUpdate = activeResult => this.setState({activeResult});
  clearActiveResult = () => this.setState({activeResult: null});
  render() {
    const cache = this.props.cache;
    const state = this.state;
    return (
      <AsyncValue value={state} defaultValue={{query: '', activeResult: null}}>
      /*just renders more JSX here */
      </AsyncValue>
    );
  }
}
```

The first thing to notice is that there are no side effects outside of `render`. Pause to think about how you would normally do side effects in a React component - either do it in a lifecycle method like `componentDidMount` or `componentDidUpdate`, or in your event handlers like `onQueryUpdate` and `onActiveResultUpdate` above. How is this app updating as you type in queries in to the input box?

This is where things start to look really weird. The answer is in that AsyncValue component.

## Meet ReactDOM.unstable_deferredUpdates

The answer, as with everything, is 42. Specifically, scroll up to line 42 to find the source of `AsyncValue`:

```js
class AsyncValue extends React.Component {
  state = {asyncValue: this.props.defaultValue};
  componentDidMount() {
    ReactDOM.unstable_deferredUpdates(() => {
      this.setState((state, props) => ({asyncValue: props.value}));
    });
  }
  componentDidUpdate() {
    if (this.props.value !== this.state.asyncValue) {
      ReactDOM.unstable_deferredUpdates(() => {
        this.setState((state, props) => ({asyncValue: props.value}));
      });
    }
  }
  render() {
    return this.props.children(this.state.asyncValue);
  }
}
```

`ReactDOM.unstable_deferredUpdates` is an undocumented API but it is not new, going [as far back as Apr 2017](https://twitter.com/koba04/status/854926955463950337?lang=en) (along with [unstable_AsyncComponent](https://github.com/koba04/react-fiber-resources)). My uneducated guess is that this puts anything in `asyncValue` (namely, `query` and `activeResult`) as a lower priority update as compared to UI updating.

## Skipping MasterDetail, Header, and Search

Great! back to parsing the innards of `AsyncValue`.

```js
      <AsyncValue value={state} defaultValue={{query: '', activeResult: null}}>
        {asyncState => (
          <MasterDetail
            header={<Header />} // just a string: 'Movie search'
            search={ // just an input box, we will ignore
            }
            results={ // uses <Results />
            }
            details={ // uses <Details />
            }
            showDetails={asyncState.activeResult !== null}
          />
        )}
      </AsyncValue>
```

Nothing too controversial here, what we have here is a `MasterDetail` component with FOUR render props (yo dawg, I heard you like render props...). `MasterDetail` 's only job is CSS-in-JS, so we will skip it for now. `Header` is just a string, and `Search` is just an input box, so we can skip all that too. So the remaining components we care about are `Results` and `Details`.

## Digging into `simple-cache-provider.createResource`

It turns out that both use similar things under the hood. Here is `Results` on line 184:

```js
function Results({query, cache, onActiveResultUpdate, activeResult}) {
  if (query.trim() === '') {
    return 'Search for something';
  }
  const {results} = readMovieSearchResults(cache, query);
  return (
    <div css={{display: 'flex', flexDirection: 'column'}}>
       /* some stuff here */
    </div>
  );
}
```

The key bit is `readMovieSearchResults`, which is defined like so:

```js
import {createResource} from 'simple-cache-provider';

// lower down...

async function searchMovies(query) {
  const response = await fetch(
    `${TMDB_API_PATH}/search/movie?api_key=${TMDB_API_KEY}&query=${query}&include_adult=false`,
  );
  return await response.json();
}

const readMovieSearchResults = createResource(searchMovies);
```

Note that the `Results` component is still in the "render" part of the overall app. We are passing the `searchMovies` promise to the new `createResource` API, which is in the `simple-cache-provider` [source](https://github.com/facebook/react/blob/master/packages/simple-cache-provider/src/SimpleCacheProvider.js#L251) 

Now createResource uses some dark magic I don't totally understand and isnt strictly necessary for the demo, but indulge me. The rough process goes from

- [createResource defined in line 251](https://github.com/facebook/react/blob/master/packages/simple-cache-provider/src/SimpleCacheProvider.js#L251)
- [cache.read called in line 268](https://github.com/facebook/react/blob/master/packages/simple-cache-provider/src/SimpleCacheProvider.js#L268)
- [cache.read defined in line 175](https://github.com/facebook/react/blob/master/packages/simple-cache-provider/src/SimpleCacheProvider.js#L175)
- since the cache state is empty, [throw the suspender in line 187](https://github.com/facebook/react/blob/master/packages/simple-cache-provider/src/SimpleCacheProvider.js#L187)!!!
- We have a thrown promise! where do we catch it! 
- I have no. frigging. clue. There's no `catch` anywhere!
- At some point, the promise bubbles up to `createCache` (which we declared aallll the way up at the top level with `SimpleCache`) and `load` is called on the cache. How do I know this? [Line 128 is the only `.then` in the entire app](https://github.com/facebook/react/blob/master/packages/simple-cache-provider/src/SimpleCacheProvider.js#L128).
- From here, it gets easier. the cache is either in a `Resolved` or `Rejected` state. If `Resolved`, the [record.value](https://github.com/facebook/react/blob/master/packages/simple-cache-provider/src/SimpleCacheProvider.js#L192) is returned and emerges as the new `activeResult` in `AsyncValue` which re-renders the whole thing.

This circuitous roundabout method is the core innovation of React Suspense and you can tell it's just a bit above my level of understanding right now. But that is how you achieve side effects inside of your render (without causing an infinite loop). Apollo and others will have alternative cache implementations.

Yikes, that was gnarly! Let me know in the comments if there's something I got wrong. I'm learning too.

## The devil is in the Details

Actually, `Details` is just a thin wrapper around `MovieInfo`, which is defined on line 227:

```js
function MovieInfo({movie, cache, clearActiveResult}) {
  const fullResult = readMovie(cache, movie.id);
  return (
    <Fragment>
      <FullPoster cache={cache} movie={movie} />
      <h2>{movie.title}</h2>
      <div>{movie.overview}</div>
    </Fragment>
  );
}
```

`readMovie` is a similar cache call to `readMovieSearchResults`, it just calls that new `createResource` with a different URL to `fetch`. What I want to highlight is rather `FullPoster`:

```js
function FullPoster({cache, movie}) {
  const path = movie.poster_path;
  if (path === null) {
    return null;
  }
  const config = readConfig(cache);
  const size = config.images.poster_sizes[2];
  const baseURL =
    document.location.protocol === 'https:'
      ? config.images.secure_base_url
      : config.images.base_url;
  const width = size.replace(/\w/, '');
  const src = `${baseURL}/${size}/${movie.poster_path}`;
  return (
    <Timeout ms={2000}>
      <Img width={width} src={src} />
    </Timeout>
  );
}
```

Here we have a bunch of new things to deal with. `readConfig` is yet another cache call (see how we're just casually making all these calls as we need them in the render?), then we have some normal variable massaging before we end up using the `Timeout` and the `Img` components.

## Introducing `React.Timeout`

Here's `Timeout.js`:

```js
import React, {Fragment} from 'react';

function Timeout({ms, fallback, children}) {
  return (
    <React.Timeout ms={ms}>
      {didTimeout => (
        <Fragment>
          <span hidden={didTimeout}>{children}</span>
          {didTimeout ? fallback : null}
        </Fragment>
      )}
    </React.Timeout>
  );
}

export default Timeout;
```

Yes, this is new ([here's the PR to add it](https://github.com/facebook/react/pull/12279/files#diff-50d8f0a9fb4af9baa0c2d4b8905567a7), its mixed in with a bunch of other React Fiber code so explore at your own risk). But its intuitive: Feed in a `ms` prop, which then controls a boolean `didTimeout`, which if true hides the `children` and shows the `fallback`, or if false shows the `children` and hides the `fallback`. The third React API to use a render prop, for anyone keeping count! 

Pop quiz: why do this children/fallback behavior using `<span hidden>` rather than encapsulate the whole thing in `{didTimeout ? fallback : children}` and not have a `<span>` tag at all? Fun thing to consider if you haven't had to before (reply in the comments if you're not sure!)

On to the other thing.

## Async Image Loading, or, how to make just passing a string not boring

Here's `Img.js`:

```js
import React from 'react';
import {SimpleCache, createResource} from 'simple-cache-provider';
import withCache from './withCache';

function loadImage(src) {
  const image = new Image();
  return new Promise(resolve => {
    image.onload = () => resolve(src);
    image.src = src;
  });
}

const readImage = createResource(loadImage);

function Img({cache, src, ...props}) {
  return <img src={readImage(cache, src)} {...props} />;
}

export default withCache(Img);

```

What's this! We're creating another cache! Yes, there's no reason we cant have multiple caches attached to different components, since we're "just" using `createContext` under the hood as we already established. But what we are using it -for- is new: async image loading! w00t! To wit:

- use the `Image()` constructor (yea, I didn't know this was a thing either, [read the MDN and weep](https://developer.mozilla.org/en-US/docs/Web/API/HTMLImageElement/Image))
- wrap it in a `Promise` and set the `src`
- pass this `Promise` to `createResource` which does its thing (don't even ask.. just.. just scroll up, thats all I got for you)
- and when the loading is done, we pass it through to the `<img src`!

Take a moment to appreciate how creative this is. at the end of the day we are passing `src`, which is a string, to `<img src`, which takes a string. Couldn't be easier. But IN BETWEEN THAT we insert our whole crazy `createResource` process to load the image asynchronously, and in the meantime `<img src` just gets nothing to render so it shows nothing. BOOM MIC DROP.

Does this look familiar? Passing a string now has side effects. This is just the same as passing JSX to have side effects. **React Suspense lets you insert side effects into anything declarative, not just JSX!**


## Homework

There are only two more things to explore: `Result` and `PosterThumbnail`, but you should be able to recognize the code patterns from our analysis of `FullPoster` and `Img` now. I leave that as an exercise for the reader.

So taking a step back: What have we learned today?

- `simple-cache-provider.SimpleCache` - puts a `cache` in `createContext`
- `simple-cache-provider.createResource` - which takes a promise for your data
- How to delegate updates to a lower priority with `ReactDOM.unstable_deferredUpdates`
- How `createResource` loads data asynchronously by throwing Promises
- `React.Timeout` - just gives you a boolean for flipping between children and fallback
- How to use `createResource` to do async image loading

That is a LOT packed into 300 lines of code! Isn't that nuts? I certainly didn't get this from just watching the talk; I hope this has helped you process some of the finer details as well.

What have I missed or got wrong? please let me know below! Cheers!
