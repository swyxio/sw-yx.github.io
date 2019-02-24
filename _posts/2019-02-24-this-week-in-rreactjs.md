---
layout: post
date: 2019-02-24
tags: react
feelings: fat
title: rreactjs podcast 6
comments: true
description: even more podcasting for great goodness
---

## Discussions

- [Progressive React](https://www.reddit.com/r/reactjs/comments/at7uh4/a_brain_dump_of_all_the_things_you_can_do_to_make/)
- [List of open source React Production Codebases](https://www.reddit.com/r/reactjs/comments/atq0uh/what_are_some_nice_open_source_reactjs_production/)
- [Collection of 300+ React Hooks](https://www.reddit.com/r/reactjs/comments/at1gfz/collection_of_300_react_hooks/)
- [How to use WordPress with React](https://www.reddit.com/r/reactjs/comments/arxu7e/how_to_use_wordpress_with_react/)
- [Beginners guide to route level auth](https://www.reddit.com/r/reactjs/comments/atvy6t/a_beginners_guide_to_route_level_authentication/)
- [Phasing out styled components?](https://www.reddit.com/r/reactjs/comments/atlaln/fairly_mature_product_phasing_out_styled/)
- [RFC: createElement changes and surrounding deprecations ](https://www.reddit.com/r/reactjs/comments/atfq5u/rfc_createelement_changes_and_surrounding/)

## Projects

- [Asperitas - a barebones Full-stack Reddit Clone](https://www.reddit.com/r/reactjs/comments/arox51/i_made_a_barebones_fullstack_reddit_clone_to/)
- [react-spotify-api](https://www.reddit.com/r/reactjs/comments/aslt9y/reactspotifyapi_easily_fetch_data_from_the/)
- [React Resources](https://www.reddit.com/r/reactjs/comments/at59kz/collection_of_2200_react_resources_in_138_topics/)
- [ReactN - state management](https://www.reddit.com/r/reactjs/comments/arsgia/this_weekend_my_project_reactn_a_package_for/)
- [Navi - An async <Router /> with Suspense and Hooks](https://www.reddit.com/r/reactjs/comments/as0g0e/navi_an_async_router_with_suspense_and_hooks/)

---


Welcome to this week in /r/reactjs, where we read you the top stories of the week based the votes of 97,000 React developers. This podcast is never longer than 10 minutes, so lets get started.


## Discussions

- [Progressive React](https://www.reddit.com/r/reactjs/comments/at7uh4/a_brain_dump_of_all_the_things_you_can_do_to_make/)

This is a fantastic blogpost from Houssein Djirdeh of the Chrome Devrel team, listing in detail all the things you can do to make your React app more performant. He even includes a TL;DR upfront, which is worth internalizing:

1. Measure component-level rendering performance with either of the following:
- Chrome DevTools Performance panel
- React DevTools profiler
2. Minimize unecessary component re-renders
- Override shouldComponentUpdate where applicable
- Use PureComponent for class components
- Use React.memo for functional components
- Memoize Redux selectors (with reselect for example)
- Virtualize super long lists (with react-window for example)
3. Measure app-level performance with Lighthouse
4. Improve app-level performance
- If you are not server-side rendering, split components with React.lazy
- If you are server-side rendering, split components with a library like loadable-components 

he even mentions `react-loadable-visibility` to code split and load components based on whether they are visible on the screen

- Use a service worker to cache files that are worth caching. Workbox will make your life easier.
- If you are server-side rendering, use streams instead of strings (with renderToNodeStream and renderToStaticNodeStream)
- Can't SSR? Pre-render instead. Libraries like react-snap can help.
- Extract critical styles if you are using a CSS-in-JS library, or use an alternative to CSS in JS like astroturf or linaria
- Make sure your application is accessible. Consider using libraries like React A11y and react-axe.
- Add a web app manifest if you think users would like to access your site through their device homescreen.

I also posted a link to the talk that he gave at React Boston if you prefer to watch a video rather than read.

user eliseumds says:

- Rendering React synchronously on the server blocks the main thread, and it's tough to get streaming working properly in a big app. Make sure that you spawn multiple server processes to overcome that. Nginx and pm2 can help with that.
- Memoize heavy functions that get called too often, but always measure first since Javascript engines have a lot of optimizations baked in (don't go too far, though, measure memory consumption as well)
- Debounce event handlers to prevent too many dead operations

- [List of open source React Production Codebases](https://www.reddit.com/r/reactjs/comments/atq0uh/what_are_some_nice_open_source_reactjs_production/)


- [How to use WordPress with React](https://www.reddit.com/r/reactjs/comments/arxu7e/how_to_use_wordpress_with_react/)

This is a great guide to using wordpress as a headless CMS with React.


user tgsmith489 explains:

> I’ve been a WordPress developer for a while and really like how easy it is for my clients to use. I also really enjoy building things with React. This post explains how the two can be used together to make performant sites without giving up a popular CMS option.

he provides a starter project for you to get started and ends by commenting how you can use Gatsby and Nextjs together with Wordpress for even faster sites.

user isowolf sais:

> I have used this stack lately twice, but the WP Rest API is nagging me a lot. It loads all hooks and filters so basically each request to the WP REST API is loading the complete mess that a WP site can be. So, I decided to create a completely custom API for WP that loads only the necessary stuff and no plugins (although you can load plugins if u want but I don't see the point). The API loads around 2-3 times faster (I went from ~900ms to ~350ms load time), the only downside is that you have to do more custom coding.

and there was a lot of interest in this project. I'm sure we will hear more about it in future weeks.

- [Beginners guide to route level auth](https://www.reddit.com/r/reactjs/comments/atvy6t/a_beginners_guide_to_route_level_authentication/)

This is a 20 minute live coding tutorial on adding authetication to a react router app using context. This is clearly a pain point for a lot of beginners: 

User TheDarkSwordsman:

> I AM WATCHING THIS RIGHT NOW.
> Dear God I've been stuck on authentication for three weeks because we moved from an old method to using the appstore binded to context, but we have had race conditions everywhere because of it.

- [RFC: createElement changes and surrounding deprecations ](https://www.reddit.com/r/reactjs/comments/atfq5u/rfc_createelement_changes_and_surrounding/)

This is an oficial react RFC by sebastian markbage to change how React.createElement works and ultimately lets us remove the need for forwardRef.

* Deprecate "module pattern" components.
* Deprecate defaultProps on function components.
* Deprecate spreading `key` from objects.
* Deprecate string refs (and remove production mode `_owner` field).
* Move `ref` extraction to class render time and `forwardRef` render time.
* Move `defaultProps` resolution to class render time.
* Change JSX transpilers to use a new element creation method.
  
  * Always pass children as props.
  * Pass `key` separately from other props.
  * In DEV,
    
    * Pass a flag determining if it was static or not.
    * Pass `__source` and `__self` separately from other props.
    
Be sure to browse the comments for some great thoughts from Dominic Gannaway who also discussed adding flags to the jsx function calls to contain information about the virtual node, and great discussions with the Preact core team

## Projects

- [Asperitas - a barebones Full-stack Reddit Clone](https://www.reddit.com/r/reactjs/comments/arox51/i_made_a_barebones_fullstack_reddit_clone_to/)

User d3zb6z says:

> I made a barebones full-stack Reddit clone to learn more about React (and a lot more)!
> Wanted to try my hand at creating a bigger project than I am used to using Node.js, mongoose, and React+Redux. This is my first experience with writing unit tests, continuous integration, and deployment as well, and I would love to hear what people think of it. I would appreciate it very very much if you could give me any constructive criticism at all.

user 49Ivories says:

> The code looks clean and is very well organized. I would like to see the card view for a feature release. Everything looks really great!

User arsamsarabi says:

> Hey dude, this is pretty cool. You're way better than any junior developers I've ever worked with. I don't know where you're from but here in the UK you wouldnt have any difficulties finding a job with these skills.

- [react-spotify-api](https://www.reddit.com/r/reactjs/comments/aslt9y/reactspotifyapi_easily_fetch_data_from_the/)

user idanlo says:

> Hello everyone, this is a package I made a few months ago and decided to upgrade a little bit. With this library you can fetch data from the Spotify API very easily.
> Most of the components use the render props method to pass the data but I'm starting to work on hooks and there are some that are published. There are full docs in the npm page.

The documentation is undersold, it really is very complete and encouraging for users of the Spotify api.

user pilarenko says: 

> This looks great! The complexity of the Spotify API was the reason why I dropped one personal project a couple of months ago, but with this I can actually create what I wanted without a huge time investment.


- [React Resources](https://www.reddit.com/r/reactjs/comments/at59kz/collection_of_2200_react_resources_in_138_topics/)

poster reacttricks says:

> We just pushed a big new update to https://reactresources.com

> There are now 950 articles/tutorials, 600 videos, 40 books, 175 courses, 185 podcast episodes, 216 libraries and 34 jobs.

> Lots of cool stuff is coming soon, including the ability to contribute resources.

- [Collection of 300+ React Hooks](https://www.reddit.com/r/reactjs/comments/at1gfz/collection_of_300_react_hooks/)

This is a great collection of hooks by Nik Graf, more for inspiration than for importing a million random hook libraries into your node_modules. the point is to look at the code, learn the principles, and be able to apply them anywhere and everywhere you have a similar problem.

- [ReactN - state management](https://www.reddit.com/r/reactjs/comments/arsgia/this_weekend_my_project_reactn_a_package_for/)

user charles_stover says: This weekend, my project ReactN -- a package for managing global state in React without boilerplate -- reached 500 GitHub stars and launched 1.0.

user HappinessFactory asks: So what are the pros/cons for using this over something like redux?

charles: says

> The pros are no boilerplate and more intuitive API. The end goal is to mimic the idea that global state is baked into React itself, much like local state is. No copy pasting reducers or googling state setup. The interface is intuitive because it mimics local state design. this.state and this.setState become this.global and this.setGlobal. The global state hooks also match the same return types as local state hooks. The end result is no developer time wasted with steep learning curves or difficult implementation.

- [Navi - An async <Router /> with Suspense and Hooks](https://www.reddit.com/r/reactjs/comments/as0g0e/navi_an_async_router_with_suspense_and_hooks/)


