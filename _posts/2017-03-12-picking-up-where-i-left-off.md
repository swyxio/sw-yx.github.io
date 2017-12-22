---
layout: post
title: Picking Up Where I left Off
date: 2017-03-12
tags: freecodecamp
categories: determined fat
---

I am still working on creating a Meteor based boilerplate application in order to work up to the last FCC project: <https://www.freecodecamp.com/challenges/build-a-pinterest-clone>

Yesterday I got stuck where I was trying to modify something I am trying to return on the API side. it was failing because I was returning documents itself instead of returning the cursors. This was warned somewhat in David Weldon's really good post about Meteor pitfalls: <https://dweldon.silvrback.com/common-mistakes>. So my options are to actually buckle down and read the API documentation in meteor or to just reset my schema to include the data I really want. I'm going to start with the [publishing API documentation](https://guide.meteor.com/data-loading.html) first and see where that gets me.

Ok I have a so i was doing the above because I was asking the wrong question. What I really wanted to solve was getting access to user data on the client side so I could do some basic mapping and modifying of data. This is probably not a good thing security wise but I don't much care about that right now while I am learning how things work. The solve was to publish an array of cursors: https://guide.meteor.com/data-loading.html#publishing-relations giving both the user and the document data.

---

now that I am done with this I realized I left out one giant piece of the puzzle that is going to cause me more trouble than I had anticipated - Twitter oauth login. The [standard meteor tutorial](https://guide.meteor.com/accounts.html) only offers Blaze based twitter authentication buttons so I am hoping for react compatible ones. this one: <https://github.com/okgrow/accounts-ui-react> looks promising and I am trying out now

---

integration was pretty successful! this is probably the thing I love most about meteor. the learning curve is sucking and I am realizing that I probably cannot use Meteor long term but by god it makes including new authentication methods a breeze.

what I need to do now that this works is basically surgically remove the old signup/login thing that MeteorChef/Base had because this thing is way better.

---

I have done this and also got some help from MeteorChef on my question: <http://stackoverflow.com/questions/42754763/meteor-best-practice-for-modifying-document-data-with-user-data> but ultimately the stackoverflow one was right.
