---
layout: post
date: 2017-01-19
title: Where should "Business Logic" in Redux go?
categories: redux freecodecamp
tags: resigned
---

reddit has been very helpful in pointing out that to deep copy any object that doesnt have a method you just have to JSON.stringify and then JSON.parse. not elegant but it works. as a bonus you can easily perform equality checks which will come in useful when looking at locations.

the slice nature of reducers in redux is making me think weird things. my central problem is that my user, items, and enemies all have a location parameter that only makes sense in my game map. having a reducer for the map without those additional data is pretty pointless. Having the location parameter without the map is equally pointless. so that means i am having to relocate my location data all to the map, in a dict of locations and object ID's. this is a huge refactor from my previous implementation but it makes so much more sense when rendering. however it breaks my object orientation a bit because i feel like the user data should belong with the user object. but if you rethink this as a [normalized parent-child problem](http://redux.js.org/docs/recipes/reducers/NormalizingStateShape.html) then it becomes a lot easier to deal with. essentially the gamemap is the post and the items are the comments. these also allow me to create multiple items of a particular type just by specifying a diff location (enemies have persistent health so this approach would not be suitable)

lastly there is a bit of an internal debate about where to put "business logic". i realized yesterday that i should move my business logic out from mapStateToProps to reducers. but now i am finding that for even more meaningful controls I should probably put some intelligence in my action creators, before they flow to my reducers. it seems the commmunity is very split about this. there are some that think action creators should be as thin and as dumb as possible. there are others that think reducers should be dumb. in the end i think the most intelligent discussion was had here <https://github.com/reactjs/redux/issues/1171> where the compromise of impure = action creator, pure = reducer was made. i like that.

---

after some debate i have decided to just give up and put everything in the same reducer. so now all my user/items/enemies are belong to gameMap. basically if a group of objects require knowledge of each other they belong in the same reducer and i was trying too hard to fight this with no great reason.

here is the new plan:
---

Functions:

- placeInRandomFreeSpace
- constMap

Item and enemy id's have prefixes that identify their type for display purpose:

- id:"E-1",dmg:3,hp:10,xp:10,name:"grunt"
- id:"E-2",dmg:7,hp:30,xp:30,name:"orc"
- id:"B-1",dmg:10,hp:100,xp:80,name:"bossman"
- id:"H-1",hpOrDMG:10,name:"carrot"
- id:"H-2",hpOrDMG:30,name:"potato"
- id:"W-1",hpOrDMG:6,name:"paper cutter"
- id:"W-2",hpOrDMG:12,name:"shitty sword"
- id:"W-3",hpOrDMG:30,name:"gatling gun"

Reducers :

- **calcMap** // **gamemap: {viewedMap, user:{hp, level, weaponName, dmgLo, dmgHi, xp, x,y}, item:[{id,x,y,dmg, hp, xp, name}], enemy:[{id,x,y,hpOrDMG,name}]}**
- InitUser // **removed**
- InitEnemies // **removed**
- InitItems // **removed**
- UserMove // **removed**

InitMap tasks:

- if newLocation is item, remove item, modify user
- if newLocation is enemy, remove or modify enemy, modify user
- if newLocation is free space, modify userlocation
- if newLocation is wall, return state

MapStateToProp // **relegated to just passing the rendered map to the container**

Containers

- GameMap // translates the map code to tiles
- GameStatus // shows the status of the user

Action Creators

- Keypress // translate key press to **newLocation payload with knowledge of state**

---
