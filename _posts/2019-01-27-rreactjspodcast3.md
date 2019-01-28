---
layout: post
date: 2019-01-27
tags: react
feelings: fat
title: rreactjs podcast
comments: true
description: podcasting for great goodness
---

## Discussions

- [Hooks are now merged and enabled - release coming soon](https://www.reddit.com/r/reactjs/comments/aj4uzz/hooks_are_now_merged_and_enabled_release_coming/)
- [9 y/o React Developer Revel Carlberg West talks about React Hooks](https://www.reddit.com/r/reactjs/comments/aiqtb5/9_yo_react_developer_revel_carlberg_west_talks/)
- [How to get team of java developers comfortable with ReactJs?](https://www.reddit.com/r/reactjs/comments/ajqhco/how_to_get_team_of_java_developers_comfortable/)
- [An Intro to Functional Programming](https://www.reddit.com/r/reactjs/comments/aielk8/an_intro_to_functional_programming/)

## Projects

- [Tour, a drag-drop-based travel planning app](https://www.reddit.com/r/reactjs/comments/ak2sjo/after_falling_in_love_with_react_native_less_than/)
- [Build Youtube in React - a free epic length tutorial](https://www.reddit.com/r/reactjs/comments/ai7umk/build_youtube_in_react_a_free_epic_length_tutorial/)
- [The Periodic Table of Elements](https://www.reddit.com/r/reactjs/comments/ajd7i3/i_made_the_periodic_table_of_elements_with_css/)
- [rbx – a UI Framework based on Bulma](https://www.reddit.com/r/reactjs/comments/ait5h2/new_ui_framework_based_on_bulma_written_in/)

---

![https://user-images.githubusercontent.com/91933/51640156-98379880-1f17-11e9-86bf-79ba3bc5ccf1.png](https://user-images.githubusercontent.com/91933/51640156-98379880-1f17-11e9-86bf-79ba3bc5ccf1.png)


Welcome to this week in /r/reactjs, where we read you the top stories of the week based the subreddit votes of 90,000 React developers. This podcast is never longer than 10 minutes, so lets get started.


## Discussions

- [Hooks are now merged and enabled - release coming soon](https://www.reddit.com/r/reactjs/comments/aj4uzz/hooks_are_now_merged_and_enabled_release_coming/)

The React codebase uses feature flags that are turned on and off for all sorts of builds, for example building the react-dom and react native versions that most people use, as well as the alpha builds. This PR by Brian Vaughn simply removes the feature flag which means Hooks will be standard in all builds. The comments were mostly celebrations so I won't read them here but I appreciate all React memes. Relatedly, a React NYC talk by 9 year old React Developer Revel West was also posted this week. Wouldn't it be great if Hooks made React so easy that any 9 year old could pick it up?

- [9 y/o React Developer Revel Carlberg West talks about React Hooks](https://www.reddit.com/r/reactjs/comments/aiqtb5/9_yo_react_developer_revel_carlberg_west_talks/)

- [An Intro to Functional Programming](https://www.reddit.com/r/reactjs/comments/aielk8/an_intro_to_functional_programming/)

This is a guide to Functional Programming with React examples from Matthew Gerstman of Dropbox. He writes:

> In the past few years, React and Redux have generated a surge of Functional Programming which we often take for granted. However many of us never got a chance to learn the fundamentals. In this post, we’ll cover the fundamentals of Functional Programming and how they apply to modern JavaScript. We’ll also avoid unnecessary jargon like monads and functors and stick to concepts that will make our code better.

There was a great follow up discussion about dependency injection and learning functional programming which you should check out if you ever felt like you needed to go deeper on that topic.

- [How to get team of java developers comfortable with ReactJs?](https://www.reddit.com/r/reactjs/comments/ajqhco/how_to_get_team_of_java_developers_comfortable/)

This was a cry for help by user shyscope, who is the most junior dev on his team just a year out of college. He was asked to take lead on a React project, but the others on his team are Java devs all older than him by more than 10 years and are struggling especially with a lot of concepts in react-boilerplate.

User rodrigocfd said: 

> Make sure they understand the difference between React and Redux. As far as I've seen, most people who complain about React actually complain about Redux boilerplate, not React itself.

User dogancan21 said:

> What I saw in teams/developers coming from a fullstack background like this is they don't actually understand SPA. I'd say start with mentality by comparing react to any other mobile app. Once you understand that, separation of concerns is easier and you understand better where react sits.

> Second step is to put typescript in. Those guys will love it . UI won't be the ugly unpredictable js anymore.

User arcanisCz said:

- Java composition over inheritance -> react components
- Java immutable patterns -> immutable.js and redux
- Java 8 streams and lambdas -> javascript's function as a first class citizen

## Projects

- [Tour, a drag-drop-based travel planning app](https://www.reddit.com/r/reactjs/comments/ak2sjo/after_falling_in_love_with_react_native_less_than/)

This is now the top post of all time on r/reactjs. User VanaticalDesign and User 2001kraft, both 18 years old, came up with this after trying to plan a trip and finding that popular apps like Tripit and Google Trips did not have great UI/UX. The key part of it was using drag-and-drop to eliminate manual entry. This was very impressive for a first React Native project! THe rest of the stack includes:


Backend: Firebase.

Db: Firebase Firestore.

Store: Redux.

Maps: Mapbox.

Navigation: React-Navigation.

Updates: Code-push

Styling: Styled-Components

It is not open source but you can try the app in iOS testflight, and see a gif of the app in motion at the top of the thread. The developers also answered many questions about the details so check it out!

- [Build Youtube in React - a free epic length tutorial](https://www.reddit.com/r/reactjs/comments/ai7umk/build_youtube_in_react_a_free_epic_length_tutorial/)

This is the start of a long tutorial series with a fully open source end result for a Youtube frontend. User prodcoder said he would publish one tutorial per day at 3pm UTC so definitely come along for the ride. As a side note, it is very meta watching someone build Youtube inside youtube.

- [The Periodic Table of Elements](https://www.reddit.com/r/reactjs/comments/ajd7i3/i_made_the_periodic_table_of_elements_with_css/)

This is a much simpler project than the other two - a static github page with the periodic table made with React and CSS Grid (and Science!) User tamalweb made this to flex their skills, but Because it was open source, user JayDP123 pointed out that they didn't have to list out every single element individually - instead they could map over an array of data.

- [rbx – a UI Framework based on Bulma](https://www.reddit.com/r/reactjs/comments/ait5h2/new_ui_framework_based_on_bulma_written_in/)


