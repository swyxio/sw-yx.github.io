---
layout: post
date: 2017-01-17
title: Planning The Dungeon Crawler
tags: freecodecamp react redux
feelings: full
---

So here are the user stories for the Crawler:

Reducer - initializers

1. User: health, level, weapon, dmgrange, xp, explored map area (modifiers: health items, better weapons)
2. Items: Location, type (weapon, health)
3. Enemies: Location, type, level (normal, boss)
4. Map boundaries

Reducers

1. ATTACK_ENEMY
2. ATTACK_BOSS
3. MOVE_EMPTY
4. PICK_ITEM

Containers

1. game map
2. status bar

Action creators

1. Keypress
2. Restart

---

But I think I am getting ahead of myself. I need a Minimum Viable Game. So I am just going to do this subset:

Reducer

1. User
2. Map Boundaries

Reducers

1. MOVE_EMPTY

Containers

1. game Map

Action Creators

1. Keypress
2. Restart

Let's go.

---

4.45am checking in. i had a brief moment of bliss (had a working version of the MVG described above but it was transposed) and then i edited a bunch of stuff and now i am running into a serious bug with regards to changing constants. filed it in SO as the guys there have been very helpful. fingers crossed. <http://stackoverflow.com/questions/41706625/why-is-redux-able-to-change-my-constant-output-from-an-initializing-reducer>

anyway here is the MVG Pen <http://codepen.io/swyx/pen/MJbezj?editors=1010>

