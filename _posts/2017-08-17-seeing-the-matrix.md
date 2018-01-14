---
layout: post
title: seeing the matrix
date: 2017-08-17
categories: fsa
tags: happy
---

today i excitedly wrote on the codingblocks slack:

```
today was seriously one of the best days in the bootcamp so far. feel like i am seeing thru the matrix
```

its a little bit of an overstatement as today's exercise was kinda mundane but with my background understanding i saw what we were being guided towards. the task was simple - make the trip planner thing we did yesterday persistent via 1) ajax POSTing and GETting and 2) manipulate large chunks of DOM elements to swap in and out data sets. We also got an introduction to front-end routing via `location.hash` but it looks janky and I know we will have better ahead of us in `react-router`. However it was a pleasant surprise to see that even Gmail uses `location.hash` to do some of its routing.

### more importantly, (particularly with the optional Itinerary Days bonus task) we were literally but not explicitly being asked to build all the basic functionality of a React or Vue frontend. 

in particular the state change re-render caught a lot of my peers off guard (having built functionality incrementally upward with no view as to the end goal) but i saw this coming and structured my code for that. We start React on monday so I am looking forward to everyone being on the same page but it was really exciting to me to be able to do all this with vanilla JS. No `react` and `react-dom`. No `axios` or jQuery. just vanilla JS. if you can do that, you can build whatever you want. There is no spoon.

p.s. the heroku site is updated with persistence: <https://swyx-tripplanner.herokuapp.com/#SwyxAwesomeTrip>. obviously discovery and authentication are key features to add from here but i didnt bother for this toy app
