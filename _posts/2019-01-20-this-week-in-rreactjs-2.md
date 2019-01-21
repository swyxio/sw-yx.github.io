---
layout: post
date: 2019-01-20
tags: react
feelings: determined
title: this week in rreactjs 2
comments: true
description: continuing to record so i have mp3
---

i spent this afternoon fixing minor issues in react-static-podcast template. i need to clean up the code but it at least generates valid RSS now.

---



Welcome to this week in r/reactjs, where we read you the top stories of the week based on subreddit votes. This podcast is never longer than 10 minutes, so lets get started.

## [Hooks support added in DevTools v3.6](https://www.reddit.com/r/reactjs/comments/ag1gye/hooks_support_added_in_devtools_v36/)

If you have React DevTools installed, you should already have received this update. Running any app with the latest React alpha and selecting a component with Hooks in it should show the hooks it uses in the Devtool (minus the "use" prefix, so a "useState" becomes just "State"). One caveat right now is that objects in State dont seem to display properly. 

Custom Hooks also follow this format, and you can click to expand the custom hook to expose the primitive React Hook used within. Hook libraries can make use of a new `useDebugValue` primitive hook to add labels for devtools display.

User Baryn said:

> Would really love to see Redux DevTools-style features when calling the setState and dispatch functions, spawned from the useState and useReducer hooks, respectively.

User brianvaughn replied:

> This is just an MVP release. I plan a major overhaul/rewrite of DevTools soon at which point we'll probably look at adding some more advanced hooks features.

## [Tech Choices I Regret at Spectrum](https://mxstbr.com/thoughts/tech-choice-regrets-at-spectrum/)

- Regret 1: Not using react-native-web
  - react-native reuse
  - mobile first
  - Lesson 1: Building a good product is all about experimentation and momentum. Optimize for iteration speed and flexibility.
- Regret 2: Not using Next.js
  - needed SSR for SEO
  - chose to build own SSR instead of switching to Nextjs
  - but DX and UX for SSR is tough
  - Lesson 2: Use existing solutions for technological problems where possible, particularly ones you do not understand deeply.
- Regret 3: Using RethinkDB
  - chosen because of changeFeeds, but suffered due to lack of documentation and community
  - Lesson 3: Carefully choose core technologies that are hard to change later.
  - Lesson 4: Prioritize community size and active maintenance, especially in unfamiliar territory.
- Regret 4: Using DraftJS and WYSIWYG editing
  - thought he needed WYSIWYG so picked Draftjs
  - but Draft.js was buggy, big, and lacked xbrowser support
  - couldve picked something else but also just didnt really need WYSIWYG
  - Lesson 5: Deliberately assess cutting edge technologies, bias towards conservative choices.
  - Lesson 6: Be open with your roadmap to learn about your users priorities.
  
User paulstronaut who works on the Twitter PWA says:

>  I recently started using RNW (personally) with next.js and the combination is absolutely fantastic. Blazingly fast!

He links to an example on the Next.js repo for Max's benefit

User twigboy brought up another discussion on Draft.js which linked to [another r/reactjs post from a month ago](https://www.reddit.com/r/reactjs/comments/a4ysxh/) with related drawbacks like tables or nested formatting.. 

User toolate said:

> There are some fair points here. But If your needs are simple Draft is still a good pick. 

Lots of nuanced points brought up by people who used the technologies in question, so a very good read if you have related problems.


---

Projects:

Our second segment is Projects, where we feature cool, open source projects shared by r/reactjs community members.

## [React Best Practices](https://www.reddit.com/r/reactjs/comments/agpo04/react_best_practices/)

a list of good ideas for everything from:
- functional programming
- state structure
- folder structure (a "lib" folder vs a "ducks" style)
- naming convention
- styling
- testing
- build tools
- libraries for forms

It is a little bit dated. 

user KerberosKomondor said

> There's a lot of useful things there. I'm not sure is recommend redux forms though. The author has created a new form library, react final form. It's much nicer imo.

user imanuglyperson said

> Agree, we use redux form at work. 100% do not recommend. It does a lot of wonky things. You’re better off using state unless you have an application that truly utilized redux forms power. Otherwise, it’s just like throwing blockchain into a Minecraft server.

interesting.


## [A Pokedex using PokeAPI](https://www.reddit.com/r/reactjs/comments/ahtquu/learning_react_heres_my_first_go_at_it_a_pure_css/)

user SiliconUnicorn said:  I would love any and all feedback to help me get better!

user Enigman said: You might prefer using something like CodeSandbox and you should look into object destructuring assignment.

person who made pokeapi and someone who used to work at pokemon USA. also someone who made a similar pokedex shared their source code.

---

And that was this Week in r/Reactjs! Did we miss anything? Come complain in the r/reactjs subreddit chat! See you next week!

