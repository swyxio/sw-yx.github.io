---
layout: post
title: FSA deploy twitter js
date: 2017-08-01
categories: FSA
tags: sharing
---

I don't like leaving my projects in localhost. What's the point of being a web developer if you don't put your stuff on the web? Where possible I like to practice deploying things as this is often a source of extreme frustration with a lot of conflicting advice. also for stuff like socket.io you can't -really- test that you are emitting to different machines unless you host it somewhere that you can access (for example from your phone)

While containerized and dedicated box deployments are relatively straightforward, BaaS services like heroku are often free and great choices for hosting some things.

the Twitter.js FSA project, once done, can be easily hosted on Heroku. I am going to describe the steps to deploy to Heroku, assuming a complete Twitter.js project. This guide will assume you have the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli) installed and will be done entirely in commandline, although it does have a web interface.

# Step 1

Heroku supplies varying ports to the `PORT` env variable. adapt your port to take a supplied env variable, or default to your constant if not supplied:

```
const myport = process.env.PORT || 3000;
```

# Step 2

No nodemon in production! head to `package.json` and fix your `start` script:

```
  "scripts": {
    "start": "node app.js",
    "dev": "nodemon app.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```

# Step 3 - deal with https

Heroku will host your project with https which blocks any requests from `http` resources. this includes, in `layout.html`, these bootstrap files:

```
<link href="http://netdna.bootstrapcdn.com/bootstrap/3.0.3/css/bootstrap.min.css" rel="stylesheet">
...
<script src="http://netdna.bootstrapcdn.com/bootstrap/3.0.3/js/bootstrap.min.js"></script>
```
you need to switch these to https versions. good ones available at <https://www.bootstrapcdn.com/>

# Step 4 - DEPLOY!

```
git add .
git commit -m "deploy"
heroku login
heroku create myappname
git push heroku master

[wait for a few mins for the code to deploy...]

heroku open
```

optional - `heroku logs` to see what is going on with heroku. heroku takes a while to launch so some patience is advised.

my app is hosted here: <https://swyx-twitterjs.herokuapp.com/> and the code is <https://github.com/sw-yx/fsa-twitter-js>

# Relevant readings

- <https://devcenter.heroku.com/articles/deploying-nodejs>
- <https://scotch.io/tutorials/how-to-deploy-a-node-js-app-to-heroku>
