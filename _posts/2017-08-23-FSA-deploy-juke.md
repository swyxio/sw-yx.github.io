---
layout: post
date: 2017-08-23
title: FSA Deploy Juke
categories: FSA react deploy
tags: determined
---

What's that? It's time for another Deployment tutorial! The last three ([trip planner](https://sw-yx.github.io/2017/FSA-deploy-Trip-planner/), [twitter.js](https://sw-yx.github.io/2017/FSA-deploy-twitter-js/) and [wikistack](https://sw-yx.github.io/2017/fsa-deploy-wikistack-to-heroku/)) went pretty well but here we have another challenge - seed media files on top of simply seeding a PostgreSQL database!


So the core instructions are sited at the [wikistack tutorial](https://sw-yx.github.io/2017/FSA-deploy-Trip-planner/). Do not push to heroku until i tell you to. Here I will focus on the incremental difference.

1. to configure `process.env.DATABASE_URL`, go to `server/db/db.js` and add it in there alongside `require(path.join(__dirname, '../env')).DATABASE_URI`
2. old `pg` dependency is gone
3. remember to create a table in SQL. this time, call it `juke`. refer to the wikistack tutorial for instructions
4. **ok so it gets tricky from here!** We aren't going to use webpack to rebuild the app on heroku so we need `public/bundle.js` to be served. delete that and `public/bundle.map.js` from the `.gitignore` so you will be able to push it to heroku. you may want to do this in a different branch (`git checkout -b herokubranch`) so you still have the right gitignore for github, but i didn't bother. (nothing secret in `public` anyway).
5. remove the `postinstall` process from `package.json`. you'll seed your database manually...
6. Time to push to heroku! `git add . && git commit -m "heroku deploy"`. MAKE SURE BUNDLE.JS is ADDED!! and now when you push here the difference is you are pushing a specific BRANCH instead of master. However, heroku only runs master. how to do that? `git push heroku <YOURBRANCHNAME>:master`.
6. ok hopefully that went well. to seed the db, `heroku run node ./bin/seed`
7. `heroku open`!

contact me @swyx on twitter if you run into any issues. My app is deployed here <https://swyx-juke.herokuapp.com/#/>


