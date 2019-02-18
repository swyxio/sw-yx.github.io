---
layout: post
date: 2019-02-10
tags: react
feelings: fat
title: rreactjs podcast 5
comments: true
description: more podcasting for great goodness
---

## Discussions

- [This benchmark is indeed flawed](https://www.reddit.com/r/reactjs/comments/apopn3/this_benchmark_is_indeed_flawed_dan_abramov_medium/)
- [Early demo of the new React DevTools](https://www.reddit.com/r/reactjs/comments/aq1g6w/early_demo_of_the_new_react_devtools/_
- [React DevTools v4 will allow inspectable complex hook values](https://www.reddit.com/r/reactjs/comments/aqu26j/react_devtools_v4_will_allow_inspectable_complex/)
- [useDarkMode](https://www.reddit.com/r/reactjs/comments/apiy3t/usedarkmode_add_a_dark_mode_toggle_to_your/)
- [Video-tutorial about performance profiling using Profiler and Chrome Performance Tab](https://www.reddit.com/r/reactjs/comments/ar6ejo/videotutorial_about_performance_profiling_using/)
- [Creating a File Upload Component with React](https://www.reddit.com/r/reactjs/comments/aricfc/creating_a_file_upload_component_with_react/)

## Projects

- [Shards Dashboard React](https://www.reddit.com/r/reactjs/comments/aqk3yg/i_made_a_free_admin_dashboard_template_pack_using/)
- [VSCode React Refactor](https://www.reddit.com/r/reactjs/comments/apvc84/i_made_this_vscode_extension_that_makes_jsx_code/)
- [Windows 95 built with React Hooks](https://www.reddit.com/r/reactjs/comments/arbuf2/windows_95_built_with_react_hooks/)
- [Asperitas full-stack Reddit clone](https://www.reddit.com/r/reactjs/comments/arox51/i_made_a_barebones_fullstack_reddit_clone_to/)

---


Welcome to this week in /r/reactjs, where we read you the top stories of the week based the votes of 95,000 React developers. This podcast is never longer than 10 minutes, so lets get started.


## Discussions

- [This benchmark is indeed flawed](https://www.reddit.com/r/reactjs/comments/apopn3/this_benchmark_is_indeed_flawed_dan_abramov_medium/)

This was a wonderful reply by Dan Abramov to a medium article written by Arnel Enero that made a benchmark to compare the relative performance of Hooks vs HOCs. The benchmark tested rendering 10000 instances of a component with a side effect that sets state, and showed Hooks being almost twice as slow as HOCs. After correcting for a rookie mistake using the development build of React, Dan walks through why you can't blindly translate component lifecycles into the useEffect hook because of the different stages of painting and executing. This is also why the benchmarking methodology also has to consider the difference between when useEffect and when useLayoutEffect execute. In any case, rendering and updating 10000 components isn't reflective of common real life usage.

- [Early demo of the new React DevTools](https://www.reddit.com/r/reactjs/comments/aq1g6w/early_demo_of_the_new_react_devtools/_

user brian vaughn says: I've been unhappy with the state of this extension for a while now (both the codebase and the runtime performance). So I recently started working on a rewrite

There are mnany things I'm excited about with this rewrite– improved performance, more organized code, and some new features (most of which are just planned at this point) like exposing the "owners" tree


There was some discussion about the meaning of an owner in react, and request to look at complex data types like objects

- [React DevTools v4 will allow inspectable complex hook values](https://www.reddit.com/r/reactjs/comments/aqu26j/react_devtools_v4_will_allow_inspectable_complex/)

- [useDarkMode](https://www.reddit.com/r/reactjs/comments/apiy3t/usedarkmode_add_a_dark_mode_toggle_to_your/)

user gabe ragland said: I wrote this hook recipe because it seemed like a fun example and an opportunity to show off the power of hook composition. This hook composes two of the previous hook recipes I've posted (useMedia and useLocalStorage). Would love to hear your thoughts or ideas for improvement!

I took at look at the useMedia hook and it seems very handy for reading matching queries not just for screen width, but also with preferences like your dark mode preference.

- [Video-tutorial about performance profiling using Profiler and Chrome Performance Tab](https://www.reddit.com/r/reactjs/comments/ar6ejo/videotutorial_about_performance_profiling_using/)

This is an excellent introduction to profiling React App rendering performance by user Sergey Ryzhov starting with chrome performance devtools and talking about what all the crazy charts in there mean. and then talks you through the rest of the thought process using

📊@brian_d_vaughn's Profiler 
📓using React.memo for components
🎣 useMemo for d3 calculation
⏲️http://performance.now for analyzing different stages of a calculation

Super worthwhile 15mins (at 2x) for all your React perf needs

- [Creating a File Upload Component with React](https://www.reddit.com/r/reactjs/comments/aricfc/creating_a_file_upload_component_with_react/)

very newbie friendly, high quality blogpost from user lukas marx, explaining every dependency from server to client. i especially like that they took care of little details like implementing a progress bar which is a nice touch for file upload. the key part of the code is adding event listeners to XMLHttpRequest and syncing the progress and success and error states with the React UI.

## Projects

- [Shards Dashboard React](https://www.reddit.com/r/reactjs/comments/aqk3yg/i_made_a_free_admin_dashboard_template_pack_using/)

This was the top project of the week. 

user hiskio said:


Shards Dashboard is one of my latest projects that I designed and implemented using Sketch, Shards React and create-react-app. It comes with 7 free, beautiful and modern templates that can be extracted and easily integrated. Most of the templates are focused around blog applications, but can be easily adjusted for almost any type of application.

responsiveness and the fact that it looks beautiful and differnt from material design. its got components for dashboards, blogpost cards, adding new posts, forms, tables, and user profile. and you can buy a pro version for even more good stuff!

- [VSCode React Refactor](https://www.reddit.com/r/reactjs/comments/apvc84/i_made_this_vscode_extension_that_makes_jsx_code/)

nice vscode extension for refactoring components out into their own file, including their props. 


OP planbcoding says:I made this vscode extension that makes JSX code refactoring easier, also supports new hooks api


user erwindurzo said: Dude you just saved me so many minutes a week. Seriously, thank you

- [Windows 95 built with React Hooks](https://www.reddit.com/r/reactjs/comments/arbuf2/windows_95_built_with_react_hooks/)

it has a fully working minesweeper clone, movable windows, and all the graphics are appropriately low quality. when you hit the win 95 start button it even plays the startup sound!

OP onethousandHz said: It started as a job interview project (build a simple minesweeper app) that took on a life of its own. I'm still planning on adding more features, like the menus (gotta build some type of smart-positioning HoverCard component first), maximizing a window, more applications, a localstorage "file system".


user flying_head asked: Did you do write all the css and made the images yourself?

Yup, whole thing is from scratch. I made all the icon images from screenshots I took of an actual running instance of Windows 95. That was my reference for the whole project.
