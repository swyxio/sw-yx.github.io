---
layout: post
title: Let's Make a Game
date: 2017-02-25
categories: game
tags: excited
---

it is refreshing to have a new project after getting stuck in the mud doing probably too much in Meteor. getting over the hump of my sickness probably also had something to do with it.

requirements of the game

- base game: show financials, allow user to enter price range, show result
- extension 1: enter multiples
- extension 2: view comps

---

10pm checking in. I started on codepen for inspiration and quickly found myself drawn out of my comfort zone. Look at this: <https://codepen.io/viciouskitten/pen/GNKOMW> done by <http://hannahch.com/#about> entirely in SASS and JQuery. And this guy does similar in entirely CSS and DOM JS <https://codepen.io/gdube/pen/yNWbLR>. While these do the job for the frontend I think it will be key to have a twitter signup method and once I get that set up I can glom on a prettier frontend. Their main contribution is in making the css look nice which is not my first step. I also want to build in the virality. so the new requirements are:

Base:
- play the game
- twitter account creation to save
- share with friends

i have soooo many ideas to extend this but they are all useless without desigining the base game. I am going to go with the higher-lower approach for now. mock the UI up in codepen and then implement locally. I have drawn out the rough mechanism so it is time to get to work.

---

11pm. the first thing i am doing is getting up to speed on bootstrap tables. The defaults dont include some stuff i love so I need to pick out properties I want [here are the docs](https://v4-alpha.getbootstrap.com/content/tables/) here are desirable properties:

- table-responsive
- table-sm
- table-hover
- table-bordered
- table-striped
- either table-inverse or thead-inverse/default

---

1.45 checking in. I have got as far as setting up the basic look of the table: <http://codepen.io/swyx/pen/aJzXmb> I will need to flesh out the findata, but independently I can flesh out the table and add the interactive elements. It's taken me far too long but I was rusty and today is my first full day of productivity in a week. I'm glad to be back.


---

inspiration:

- <https://www.youtube.com/watch?v=i_qE1iAmjFg> paul irish videos
