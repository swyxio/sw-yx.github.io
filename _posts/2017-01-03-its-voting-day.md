---
layout: post
title: "it's voting day!"
date: 2017-01-03
categories: javascript freecodecamp mongoose express
tags: ready tired excited
---

# intro

okay, it is time to do the voting app!!!

I ended things yesterday having bashed out the rough api I would need to get this thing working. my area of least confidence was getting the data model working in mongoose/mongodb. in particular, FCC gives you no direction as to having variable length fields in data models. and then incrementing them! oh goodness. google to the rescue: <http://stackoverflow.com/questions/10471350/increment-a-value-in-a-nested-object> and i got that working in the data model i laid out yesterday.

now i need to do the delete api and the add option api and I think I am set api wise. the rest is frontend/ajax and routing work.

yesterday i also (re)read <http://haseebq.com/farewell-app-academy-hello-airbnb-part-ii/> and it really hit home for the first time. It is becoming very clear that I need to spend a couple days figuring out Rails for myself, just to be sure of what I am missing. It has a high chance of not being useful for me (see <http://www.modulecounts.com/>) but I am curious enough at this point (especially with [DHH's](http://david.heinemeierhansson.com/) endorsement) that I have to check it out.

Then the plan is to take a break on backend work and learn React and finish the React dataviz path. this is so that I can use React for all remaining backend projects.

Let's go.

---

job is done after about 7hrs of straight work! <https://github.com/sw-yx/fcc-votingapp> it looks like shit but all the requirements are addressed. so the main learnings were:

- [Google charts](https://developers.google.com/chart/interactive/docs/examples) is probably better than charts.js (i dont have to worry about performance yet)
- MVC architecture really works.
- managing connected database items is very tricky. eg I had Users and Polls and Users contained a subset of Polls. When I deleted a Poll I needed to remember to delete its link from the User. ##I'm sure there is some way to automate this but I haven't found it yet##.
- Express routing basically splits down to private and public routing where you try to build all the business logic through api's and then use ajax to pull the required data from those api's. i'm not sure that this is the best way to do it but this is what I have observed so far.
- using [body-parser](http://stackoverflow.com/questions/5710358/how-to-retrieve-post-query-parameters/12008719#12008719) is very handy for post requests.
- embedding <input type="hidden" value=x> to enhance the metadata given in POST submissions.

i swear i had more to say but that is all lost in the sands of forgetfulness but I actually cannot wait to get started on ruby, react and meteor having dealt with all this web 1.0 style coding.
