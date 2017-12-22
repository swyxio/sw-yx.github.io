---
layout: post
title: Learning Redux - Unintentional Mutations
date: 2017-01-18
tags: react freecodecamp redux
feelings: happy
---

[React-redux links](https://github.com/markerikson/react-redux-links) is going to prove handy if I go down this path. But at this point I am convinced that I need to spend a couple days each on Angular 2 and Ruby On Rails just to satisfy myself that I am going down a path that I like. Redux is already beginning to annoy me with its magic.

I resolved yesterday's problem today with a final understanding of javascript passing arrays by reference instead of by value as it does for everything else. and of course it doesnt have native deep copy. so boo. but lesson learned.

Here is how the MVG **actually** turned out compared to my plan yesterday:

Functions:

- placeInRandomFreeSpace
- constMap

Reducers:

- InitMap // produces constant map
- InitUser // initialize user with placeInRandomFreeSpace. then = state.location
- UserMove // i ended up moving the business logic all the way to mapStateToProps

MapStateToProp // a function i didnt even realize would play a major part became the most important function

- adds vector to location and sees if its an open area
- moves location if so
- superimposes the user's code (9) on user.location in the constant map

Containers

- GameMap // translates the map code to tiles

Action Creators

- Keypress // translate key press to vector
- // i ended up dropping the restart thing as its not really needed

<https://codepen.io/swyx/pen/JEbZJj> 

---

So the task now is to build out the full crawler. Here is the updated plan for the thing:


Functions:

- placeInRandomFreeSpace
- constMap

Reducers:

- InitMap // produces map with randomized items and enemies
- InitUser // initialize user location but also health, level, weapon, dmgrange, xp, explored map area
- InitEnemies // enemies, boss (dmg, health, xp)
- InitItems // health (3 - health) and weapons (4 - dmg)
- UserMove // i ended up moving the business logic all the way to mapStateToProps

MapStateToProp // a function i didnt even realize would play a major part became the most important function

- superimposes the items (3,4) on item.location in the constant map
- superimposes the enemies (5,6) on enemies.location in the constant map
- adds vector to location and sees if its an open area
- moves location if so
- superimposes the user's code (9) on user.location in the constant map

Containers

- GameMap // translates the map code to tiles
- GameStatus // shows the status of the user

Action Creators

- Keypress // translate key press to vector
- // i ended up dropping the restart thing as its not really needed


---

5.45am here. i got the health collection working but then ran into trouble with the updating of the GameStatus container. basically because I refused to learn how to properly use redux I was STILL unintentionally mutating the stuff in the store and when you do that bad things happen, like the status container not updating when it is supposed to. There is no convenient setState() call to do like in React; here you really just have to do things the right way. I was also advised to move all my business logic out of mapStateToProps which is a crying shame but is probably right (as it was a back door into just not using the redux infrastructure. 

So I stopped everything and just spent an hour reading the Redux docs (link below). I am now better for it and I have a crystal clear idea with examples how to structure the thing. It is too late now for me to finish the crawler because I am just refactoring everything properly. The key thing to note here is because there are dependent reducers you can no longer use combineReducers and so you have to create your own multi stage reducer features. I will map that out tomorrow. Below are some references that were helpful.


Hacky way to force Redux to assume state is updated every single time (not recommended) `connect(select, undefined, undefined, { pure: false })(NavBar);` from this <http://stackoverflow.com/questions/38058378/rerendering-react-components-after-redux-states-locale-has-updated>

Here is the redux docs that spell out HOW you are supposed to cannonically handle deep copying.
<http://redux.js.org/docs/recipes/StructuringReducers.html> 
