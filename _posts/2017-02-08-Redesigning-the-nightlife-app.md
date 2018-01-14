---
layout: post
title: Redesigning the Nightlife App
date: 2017-02-08
categories: freecodecamp react
tags: determined
---

10.00 here. I have the API's sorted out but my lack of thoughtfulness in designing the nightlife app is coming to bite me now. React's component structure is very unforgiving when you belatedly realize you need to elevate one state item to a higher level so that other components can access it. I am going to bite the bullet now and refactor the app to get rid of the multiple dashboard components given by vladimir's tutorial case.

---

3.00 here. finally done with nightlife app: <https://quiet-mesa-37696.herokuapp.com/> and <https://github.com/sw-yx/fcc-nightlife> 

tricky pieces were:

- showing whether other users are going to the same place you are going (the data structure kind of folds in on itself twice which can get really confusing if you aren't careful)
- for the above point, doing async programming when querying an array of things that must be looked up from a database (hint: use a generator function to map over the array and then feed the array of promises to Promise.all)
- storing the right data so you can then get it out later (because i tested the simplest cases, I didnt store the right data and so took some time to 
- Mongoose query results are not json! <http://stackoverflow.com/questions/28157597/cant-add-a-new-property-to-an-object-returned-by-mongoose-query>

I cant remember what else was tricky but that just about sums up the 2-3 weeks it has taken me to do this app from start to finish. I am just so glad to be done.
