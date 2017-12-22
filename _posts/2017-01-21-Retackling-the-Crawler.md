---
layout: post
title: Retackling the Crawler 
date: 2017-01-21
categories: freecodecamp react redux
tags: determined
---

4. after two straight days of abject failure i am a little worn down but i am grateful that i have a bunch of different tracks i can pursue when I get stuck in a rut (or waiting for a reply from stackoverflow).

today i am retackling the refactoring of the MVG which is entirely doable with my experience. i have already done it with the new format here:

<http://codepen.io/swyx/pen/QdvWYY>

so now i need to progressively add game elements as mapped out in <http://swyx.io/blog/2017/01/19/Where-Should-Business-Logic-In-Redux-Go>

let's go!

---

5.30. i have had a productive sprint. i now have a working mvg with items and a gamestatus: <http://codepen.io/swyx/pen/RKVNPM> and it wasn't too hard to add enemies although i haven't added the enemy interaction yet.

there is still much of the user stories I have not done.

Done:

- User Story: I have health, a level, and a weapon. I can pick up a better weapon. I can pick up health items.
- User Story: All the items and enemies on the map are arranged at random.
- User Story: I can move throughout a map, discovering items.
- User Story: I can move anywhere within the map's boundaries, but I can't move through an enemy until I've beaten it.

Undone:

- User Story: Much of the map is hidden. When I take a step, all spaces that are within a certain number of spaces from me are revealed.
- User Story: When I beat an enemy, the enemy goes away and I get XP, which eventually increases my level.
- User Story: When I fight an enemy, we take turns damaging each other until one of us loses. I do damage based off of my level and my weapon. The enemy does damage based off of its level. Damage is somewhat random within a range.
- User Story: When I find and beat the boss, I win.
- User Story: The game should be challenging, but theoretically winnable.

there is no mention here of having multiple maps as implemented in the example game and i am choosing not to follow it because i dont want to overspend time on this particular skill. however there was one non-working piece which I DO want to implement, which is humane notifications: <http://wavded.github.io/humane-js/>

so next up is humane notification and then i tackle enemy interactions.

---

6.20 checking in. enemy interactions were a little tricky but done! now i want to do humane interaction and then occlusion and i should be done.

---

8.40. back from dinner break. humane was a little unintuitive but this <http://codepen.io/rcastroestevez/pen/LRgAAr> was helpful to figure out that the theme implementation is in the css and not a setting.

10.30. took some distractions but i am DONE with the crawler! it even has humane notifications!!! <http://codepen.io/swyx/pen/ygbNLM?editors=0010>

---

as an epilogue to yesterday's mess - i am following this <https://blog.heroku.com/deploying-react-with-zero-configuration> to deploy on heroku. it seems using the right buildpack is extremely important or things just die.
