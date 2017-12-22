---
layout: post
title: Cliff of Confusion
date: 2017-02-13
categories: meteor freecodecamp
tags: motivated
---

I have had some encouraging chats on the <http://jrdevmentoring.com> slack channel (that Zac of <http://thereactionary.net/> turned me on to), basically describing the [Cliff of Confusion](https://www.vikingcodeschool.com/posts/why-learning-to-code-is-so-damn-hard) I am about to face with the impending end of Freecodecamp. It is a very real and troubling thing but I will power through it.

Todos
---
yesterday I just about managed to recreate the meteor-react tutorial which wasn't really progress but it is a start. time to adapt meteor to booktrading needs.

as a reminder, deploying meteor to heroku requires these steps: <https://medium.com/@leonardykris/how-to-run-a-meteor-js-application-on-heroku-in-10-steps-7aceb12de234#.rhrildcw5> I forgot to add the buildpack before instantiating the heroku instance, and had to figure out how to bump the package version to force the rebuild in heroku and `npm version patch` doesn't seem to work for me. I dont know why but just manually editing minor things in the files worked to force the rebuild.

---

10.30 here. I have deployed the meteor app to heroku and that took embarrassingly long because I had forgotten so much. Now I am ready to start mutating it.

User Stories from <https://www.freecodecamp.com/challenges/manage-a-book-trading-club>

- User Story: I can view all books posted by every user.
- User Story: I can add a new book.
- User Story: I can update my settings to store my full name, city, and state.
- User Story: I can propose a trade and wait for the other user to accept the trade.

First off, this demo app sucks (what happens when a trade is accepted? god knows) so I am not going to bother replicating this UI or UX. But it will do me good to think about the data model and the clientside needs. Here are the implementation levels I am thinking:

- MVP: User signup with settings to store name, city, state, and user books.
- Stage 2: View all books.
- Stage 3: Propose trade.
- Stage 4: Accept trade.

---

1.40 here. meteor turned out to be far deeper than i thought. I started on this good video series: <https://www.youtube.com/watch?v=iH-Np5WuyUw&list=PLLnpHn493BHFYZUSK62aVycgcAouqBt7V&index=11> before realising he was teaching Blaze, and then did a bunhc of reading into Blaze vs React on Meteor (which is a rabbit hole of [this](https://www.discovermeteor.com/blog/blaze-react-meteor/), [that](https://themeteorchef.com/blog/react-vs-blaze), and ["react is the worst"](https://www.pandastrike.com/posts/20150311-react-bad-idea)

Inspirations
---
- React founder talking react: <https://www.youtube.com/watch?v=x7cQ3mrcKaY>
- Meteor Chef looks great: <https://themeteorchef.com/membership>
- proving Zuck wrong on HTML5: <https://www.sencha.com/blog/the-making-of-fastbook-an-html5-love-story/>
