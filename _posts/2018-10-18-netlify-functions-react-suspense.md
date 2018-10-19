---
layout: post
date: 2018-10-18
tags: react
feelings: neutral
title: netlify functions + react suspense
comments: true
description: a piece to use functions with suspense
---


[React's upcoming Suspense feature](https://reactjs.org/blog/2018/03/01/sneak-peek-beyond-react-16.html) is the most exciting thing to happen in frontend development since... well, since React. However, it is tricky for frontend devs to try it out. [Netlify Functions](https://www.netlify.com/docs/functions/) could help.
## What's tricky about it?
A hurdle with trying out React Suspense is the need to "fake" a backend API to interact with, or to use a public API with possibly restrictive API keys that add signup steps and security concerns to your demo. This distracts a lot from the actual point of the demo.
With our open source `netlify-lambda` utility, you can run [Netlify Functions](https://www.netlify.com/docs/functions/) locally, essentially setting up a deployable serverless backend alongside your local frontend dev server. This lets you ping serverless Netlify Functions while in development, which will work the same when they are deployed on Netlify in production. (As an additional benefit, having your serverless functions checked into the same git repo as your frontend allows [Atomic Deploys](https://www.netlify.com/blog/2016/08/11/from-unstable-to-reliable-a-release-engineering-journey/) - which means your endpoints update together with your client!)
With our [create-react-app-lambda boilerplate](https://github.com/netlify/create-react-app-lambda), you were easily able to set up a locally running frontend and backend. In this post, we'll show you how to modify it to try out React Suspense against this same backend.
## Setting up the repo
We're going to clone the repo and upgrade our React versions to the React v16.6 canary versions:
```bash
git clone https://github.com/netlify/create-react-app-lambda.git
cd create-react-app-lambda
yarn
```
If you run `yarn start` now you should get a functional app on `localhost:3000`. It looks like `create-react-app` v2, but it has been modified so that if you hit the button you will ping the endpoint specified in `src/lambda/hello.js`. **Take some time to look around and familiarize yourself with the `src/App` and `src/lambda` code.**
> ⚠️Warning: **The exact APIs discussed below are unstable**, and will likely be different if you are reading this in 2019. We will revisit and update this article when the APIs are finalized.⚠️
## Converting to ConcurrentMode
Time to live life on the edge. Upgrade your React version or install `react-cache`:
```bash
# today, assuming React 16.6 hasn't been released yet
yarn add react@canary react-dom@canary react-cache@canary
# in future, when React 16.6+ is released
yarn add react-cache
```
Great! Now head to `src/App.js` and put our app in Concurrent mode:
```js
// src/App.js
import React, {
unstable_ConcurrentMode as ConcurrentMode, // new
unstable_Suspense as Suspense, // new
Component
} from 'react';
// ...
// wrap the JSX in line 38-46 in <ConcurrentMode>
// ...
class App extends Component {
render() {
return (
<ConcurrentMode>
<div className="App">
<header className="App-header">
<img src={logo} className="App-logo" alt="logo" />
<p>
Edit <code>src/App.js</code> and save to reload.
</p>
<Suspense maxDuration={1000} fallback={'Loading...'}>
<LambdaDemo />
</Suspense>
</header>
</div>
</ConcurrentMode>
);
}
}
```
You can see that alongside `<ConcurrentMode>`, we've added a `<Suspense>` component with `maxDuration={1000}` and a simple fallback UI of `'Loading...'` to wrap our central `<LambdaDemo />` component. All components that use resource fetchers must be wrapped in a `Suspense` component like this, to ensure that there is a fallback UI when rendering is suspended and `maxDuration` is exceeded.
## Creating Caches and Resources
`react-cache` is a new library maintained by the React team as a reference implementation for caches that could work with React Suspense. Alternative caches will be implemented by the community, including by the Facebook Relay and [Apollo GraphQL](https://github.com/peggyrayzis/react-europe-apollo) team.
`react-cache` itself exposes two low level APIs meant for very general use cases, so it is often easier to initialize your React Suspense cache as a singleton and pass it in automatically to your resource. Make a new `src/cache.js` file and fill it like so:
```js
// src/cache.js
import { createCache, createResource } from 'react-cache';
export let cache;
function initCache() {
cache = createCache(initCache);
}
initCache();
export function createFetcher(callback) {
const resource = createResource(callback);
return {
read(...args) {
return resource.read(cache, ...args);
}
};
}
```
Good, now we have everything we need to write suspenders*!
\*_note: "suspenders" aren't an official React concept, they are just what I call things that suspend._
# Writing Suspenders
In `src/App.js` we used 25 lines of code to write a class component to handle loading state, handle clicks, ping our `/.netlify/functions/hello` endpoint, and display the message from the backend:
```js
// currently inside src/App.js
class LambdaDemo extends Component {
constructor(props) {
super(props);
this.state = { loading: false, msg: null };
}
handleClick = e => {
e.preventDefault();
this.setState({ loading: true });
fetch('/.netlify/functions/hello')
.then(response => response.json())
.then(json => this.setState({ loading: false, msg: json.msg }));
};
render() {
const { loading, msg } = this.state;
return (
<p>
<button onClick={this.handleClick}>
{loading ? 'Loading...' : 'Call Lambda'}
</button>
<br />
<span>{msg}</span>
</p>
);
}
}
```
With our `createFetcher` utility, we can rip all that out and replace it with:
```js
// new code in src/App.js
import { createFetcher } from './cache';
const DataFetcher = createFetcher(() =>
fetch(`/.netlify/functions/hello`).then(x => x.json())
);
function LambdaDemo() {
const msg = DataFetcher.read().msg;
return <p>{msg}</p>;
}
```
And here you have written your first fetcher, which is a resource with a cache with a callback function we wrote with the browser `fetch` API returning a promise.
When `DataFetcher` is read, it suspends the render of the `LambdaDemo` component. When suspending, the `fallback` text of `'Loading...'` that we specified inside the `App` component shows, and when the `fetch` promise resolves, `LambdaDemo` completes its render and we get the final UI:
![Success with our React Suspense app](/img/blog/helloworld.png)
## Deploying with Netlify Functions
Push your new React Suspense + Netlify Functions app to a repo you own on Github (we have put ours on the [canary branch here](https://github.com/netlify/create-react-app-lambda/tree/canary)). Here there are a couple of options to deploy your new app: either head to <https://app.netlify.com> to [deploy from our UI](https://www.netlify.com/docs/continuous-deployment/), or [try our new CLI to deploy straight from your terminal!](https://www.netlify.com/docs/cli/)
We have ours deployed [here](https://festive-nightingale-47f180.netlify.com/) if you want to take a look. Now go make your new JAMStack React Suspense demos!
