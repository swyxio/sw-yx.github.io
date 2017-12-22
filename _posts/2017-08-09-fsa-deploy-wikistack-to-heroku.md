---
layout: post
date: 2017-08-09
title: FSA deploy wikistack to heroku
tags: FSA
feelings: ambitious
---

my last post on [deploying the FSA Twitter.js project](https://sw-yx.github.io/2017/FSA-deploy-twitter-js/) was well received. well, here's the FSA wikistack version!

prerequisites: go through the official heroku guide here to get a simple overview of what we will be doing: <https://devcenter.heroku.com/articles/getting-started-with-nodejs>

another good general instruction guide is on the `sequelize` documentation itself! <https://sequelize.readthedocs.io/en/1.7.0/articles/heroku/>

---

The following specifically deals with deploying FSA wikistack.

# Steps 
_assuming you have started from a [clean wikistack local build](https://github.com/sw-yx/fsa-wikistack-deploy/tree/37d8146b8e40a46802243ffcac1cbc6c2b0ce2c2):_

1. `heroku login`
2. `heroku create myAppName`
3. get rid of nodemon in your package.json scripts: `"start": "node index.js"`
4. replace your port with `process.env.PORT` in `index.js`: `app.listen(process.env.PORT || 3001, function () {`
4. `heroku addons:create heroku-postgresql:hobby-dev` - explanation here: <https://devcenter.heroku.com/articles/getting-started-with-nodejs#provision-a-database> and you can check the URL with `heroku config`
5. replace your db connection with `process.env.DATABASE_URL` in models/index.js: `var db = new Sequelize(process.env.DATABASE_URL || 'postgres://localhost:5432/wikistack', { logging: false });`
6. ***this is a temporary wikistack issue*** - downgrade your `pg` requirement in package.json to `"pg": "^4.5.3",`
8. deal with https issues: replace your boostrap js and css with https versions <https://www.bootstrapcdn.com/>
7. `heroku pg:psql` and `create table wikistack` to initialize the table. exit with `\q` (what is it with CLI designers and shitty quit commands)
8. `git commit -am "deploy to heroku"` and `git push heroku master`

I deployed my example at <https://swyx-wikistack.herokuapp.com>

---

i faced a problem with `git commit -am` and `git add . && git commit -m`. wont repeat that again.
