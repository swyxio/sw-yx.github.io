---
layout: post
date: 2017-01-15
title: Game of Life - Done!
tags: freecodecamp react
feelings: done
---

checking in on the other side of the flight. I am still battling a bad cold but decided to just knock out the Game of Life task today given that I realize it doesnt require redux and is just more of a math problem than anything else.

<http://codepen.io/swyx/pen/wgzGEQ?editors=1010>

I think I did the right thing with regards to the evaluation algorithm, but I ran into a particularly persistent bug in terms of understanding the relationship between `this.state.grid` and `grid`. it appears to be a two way binding instead of one way and that was surprising to me. According to [the official docs](https://facebook.github.io/react/docs/thinking-in-react.html) this is not supposed to be how it works but that is the only way I can explain it as when I broke the connection intentionally the thing was fixed. 

the smaller discovery is the differnce between using setInterval and setTimeout. in short, always use the latter.

Onward and forward. I just have the dungeon crawler left to get the dataviz certification. then I have multiple possible paths but I think here I should take a pause and figure out how to deploy react to heroku and redo my analytics dataviz. And then I learn redux and then carry on with the last of the backend certifications.
