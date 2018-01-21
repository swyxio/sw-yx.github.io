---
layout: post
date: 2018-01-
tags: passportjs
feelings: neutral
title: passportjs local guide
comments: true
description: rewriting my passportjs guide
---

my last tutorial on [passportjs](https://sw-yx.github.io/2017/09/09/checklist-for-authentication-with-passportjs) had some glaring inadequacies so i am rewriting it now for a MERN app with local strategy with the help of the Auther workshop from FSA.

## Step 1

`npm i express-session passport passport-local passport-local-mongoose` install `body-parser` if you need it

## Step 2

add `UserSchema.js`:

```js
const mongoose = require('mongoose');
const Schema = mongoose.Schema;
const passportLocalMongoose = require('passport-local-mongoose');

const User = new Schema({});

User.plugin(passportLocalMongoose);

module.exports = mongoose.model('User', User);
```

add `auth.js`:

```js
const passport = require('passport');
const LocalStrategy = require('passport-local');
const router = require('express').Router();
// requires the model with Passport-Local Mongoose plugged in
const User = require('./UserSchema');
const router = require('express').Router();

router.use(session({
  // this mandatory configuration ensures that session IDs are not predictable
  secret: 'tongiscool', // or whatever you like
  // this option is recommended and reduces session concurrency issues
  resave: false
}));
router.use(passport.initialize());
router.use(passport.session());

// use static authenticate method of model in LocalStrategy
passport.use(User.createStrategy());
// google
// https://github.com/sw-yx/sw-yx.github.io/blob/master/_posts/2017-09-09-checklist-for-authentication-with-passportjs.md
// twitter

// use static serialize and deserialize of model for passport session support
passport.serializeUser(User.serializeUser());
passport.deserializeUser(User.deserializeUser());


router.post('/signup', async (req, res) => {
  console.log('req.body',req.body)
  const newUser = new User(req.body)
  await newUser.save()
  res.redirect('/login')
});

router.post('/login', (req, res, next) => {
    console.log('req.body',req.body)
    next()
  },
  passport.authenticate('local', { successRedirect: '/',
                                  failureRedirect: '/login',
                                  failureFlash: true }
                                )
);

module.exports = router;
```

