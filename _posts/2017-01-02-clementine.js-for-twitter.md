---
layout: post
title: "clementine.js for twitter"
date: 2017-01-02
tags: javascript freecodecamp clementine
feelings: optimistic
---

Today's task is to adapt FCC's out of the box version of clementine.js to take twitter auth. 

This is in preparation for tackling the voting app challenge, but also just in general I foresee using passport.js heavily.

I failed horribly the last time based on not undertanding express or passport. Hopefully that is better now.

---

surprisingly it only took an hour! i have enough knowledge now to debug the multiple little mistakes I made during the transition. Here are some things to look out for when adapting passport.js from github to twitter:

* .env: give your TWITTER_KEY and TWITTER_SECRET
* app/config/auth.js: change `clientID` and `clientSecret` to `consumerKey` and `consumerSecret`. the structure of this can be found [here](http://passportjs.org/docs/twitter). also change the callback as appropriate. change `newUser.github.publicRepos = profile._json.public_repos;` to `newUser.twitter.followcount = profile._json.followers_count`
* similarly users.js will have to be updated  with followcount: Number
* routine search and replace for "github" to "twitter": clickHandler.server.js, index.js, login.html
* search and replace for publicRepos to followcount: profile.html, userController.client.js

i may have missed out some stuff but that and incomplete search-and-replacing were my major problems.

---

# tackling the voting app

The key thing to note is what users can do WITHOUT authentication:

* User Story: As an unauthenticated or authenticated user, I can see and vote on everyone's polls.
* User Story: As an unauthenticated or authenticated user, I can see the results of polls in chart form. (This could be implemented using Chart.js or Google Charts.)

This means the addclick and getclick api's are completely unrestricted. this is not a problem with the existing clementine framework. however there is a puzzle as to how to add new tags:

* User Story: As an authenticated user, if I don't like the options on a poll, I can create a new option.

this is the tricky bit we will have to deal with later

# architecture

## models
User

1. user-id = string
2. displayName = string
3. polls = array of poll id's = string

Poll

1. poll-id = string
2. poll-name = string
3. poll-options = array of options and votes
4. date-creation

## controllers
* unauthenticated
  * get poll results
  * vote on poll
* authenticated
  * user create poll
  * user delete poll
  * user sees own polls
  
## views
- my polls
- new poll
- login
- index <- list of polls
- polls/:pollid

# peerset inspiration
other attempts that i have found

* [1hella](https://github.com/1hella/freecodecamp-voting-app) i like the click-to vote feature and the registration form including facebook. uses grunt and karma as devdependencies. 1yr old so bit out of date
* [saintpeter](https://github.com/SaintPeter/fcc-basejump-voting-app) grunt, karma, and socketio integration for realtime update. worth exploring. 1yr old so bit out of date
* [scottwestover](https://github.com/scottwestover/freecodecamp-voting-app) implements express-stormpath and it is annoying as fuck if you dont want to add yet another password. oauth is so incredibly important. i'm ignoring body-parser for now but looks useful. i've seen [ejs](https://scotch.io/tutorials/use-ejs-to-template-your-node-application) a couple times but i'm still too enamored with Pug
* [rafase282](https://github.com/Rafase282/My-FreeCodeCamp-Code/wiki/Build-a-Voting-App) - uses gulp and react and morgan and emailjs. chartjs and [palettejs](https://github.com/google/palette.js/tree/master) for data vis
* [ubershib](https://ubershibs-voting-app.herokuapp.com/about) just really good. uses addthis. has a search feature.
