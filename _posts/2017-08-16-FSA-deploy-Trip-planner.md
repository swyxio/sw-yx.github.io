---
layout: post
date: 2017-08-16
title: FSA deploy Trip Planner (live)
categories: FSA
tags: ambitious
---

hello again Fullstackers! Just like with [twitter.js](https://sw-yx.github.io/2017/FSA-deploy-twitter-js/) and [wikistack](https://sw-yx.github.io/2017/fsa-deploy-wikistack-to-heroku/), it's time to deploy the Trip Planner project! Here goes!

# how to deploy trip planner to Heroku

1. Follow steps 1-10 of the [wikistack tutorial](https://sw-yx.github.io/2017/fsa-deploy-wikistack-to-heroku/). 
There are some minor differences to take note of - for example, your start script is now `"start": "node server/app.js"`, you probably won't have https issues, and the postgres table you create is `tripplanner` or whatever you name it in your code.
2. so now you should see a deployed Trip Planner when you hit `heroku open`. however, all the fields are empty! why? your database isn't seeded! Hit `heroku run node server/seed.js` to run the seedfile on the heroku server just like you did to run locally
3. (optional) if you are using ES7 features like `async/await`, you need to also tell heroku to use node 8 instead of node 6 by including `"engines": {"node": "8.4.0"}` in your package.json

## a word on API key security

there is something i did not do here which is worth mentioning. This project uses private API keys. while these are safe being published to heroku, they are not meant to be published to github where anyone with access to your repo can essentially steal your identity. The way to deal with this is to put your private api keys in a special config file which is `.gitignore`'d, while on the heroku side, you set your environment variables with `heroku config:set` - see [documentation here](https://devcenter.heroku.com/articles/config-vars). then in your code you either take your API keys from `process.env.CONFIGVARNAME || require('./gitignoredconfigfile')`

My trip planner is deployed at <https://swyx-tripplanner.herokuapp.com/> and the source code is at <https://github.com/sw-yx/fsa-tripplanner-live>

