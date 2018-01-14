---
layout: post
title: Passport.js the Hard Way Made Easy(-ier)
categories: passportjs
tags: curious
date: 2017-09-09
---

Webapp authentication is important but complicated. This makes the process of documenting and teaching libraries like passport.js a difficult proposition.

If your goal is to sell someone on how easily you can do something, your documentation is going to be scant on details (particularly if context is needed). this is the problem with [the official passport.js docs](http://passportjs.org/docs). Any tutorial that says "authentication is easy" is lying to you and is virtually guaranteed to leave out some important bit of implementation detail you aren't going to come up with on your own.

If your goal is to comprehensively handhold somebody through the process of adding authentication on an existing webapp, your tutorial is going to have a load of extraneous detail about the structure (in particular, assumptions about the frontend and the database stack) and it is going to feel like it never ends. In case it helps, here are the usable passport.js tutorials I found. Note the wide variance in stack.:

- [Starting with Authentication](https://medium.com/of-all-things-tech-progress/starting-with-authentication-a-tutorial-with-node-js-and-mongodb-25d524ca0359)
- [Node.js Authentication using Passport.js](https://blog.risingstack.com/node-hero-node-js-authentication-passport-js/)
- [Authenticating Node.js Applications With Passport](https://code.tutsplus.com/tutorials/authenticating-nodejs-applications-with-passport--cms-21619)
- [Authentication Using PassportJS](https://danialk.github.io/blog/2013/02/23/authentication-using-passportjs/)
- [Easy Node Authentication: Setup and Local](https://scotch.io/tutorials/easy-node-authentication-setup-and-local) <- Best in class
- [Tutorial for Passport.js authentication in a Node.js Express application](https://www.jokecamp.com/tutorial-passportjs-authentication-in-nodejs/)
- [Node.js, Express.js, Mongoose.js and Passport.js Authentication](https://www.djamware.com/post/58bd823080aca7585c808ebf/nodejs-expressjs-mongoosejs-and-passportjs-authentication)
- [Passport org on Github has maintained minimalist examples](https://github.com/passport)

### In the rest of this article I'm going to try to write for the person who is roughly familiar with passport.js that just wants a reference as he/she implements on top of an existing Node/Express app. 

Table of Contents
=================

  * [0. Choices to make](#zero)
  * [1. NPM installs](#one)
  * [2. Require and configure on Express Server process](#two)
  * [3. Configure Passport Serialization](#three)
  * [4. Configure Passport Strategies](#four)
  * [5. Setup routes](#five)
  * [6. Frontend Joy](#six)
  * [7. Bonus](#seven)

---

In case this gets outdated, here is this tutorial's "package.json":

```javascript
{
    "axios": "^0.15.0",
    "express": "^4.13.3",
    "express-session": "^1.15.5",
    "passport": "^0.4.0",
    "passport-google-oauth": "^1.0.0",
    "passport-github": "^1.1.0",
    "passport-twitter": "^1.0.4",
    "pg": "^4.5.5",
    "react": "^15.3.2",
    "react-dom": "^15.3.2",
    "react-redux": "^4.4.5",
    "react-router": "^4.1.1",
    "react-router-dom": "^4.1.1",
    "redux": "^3.6.0",
    "redux-thunk": "^2.1.0",
    "sequelize": "^4.4.0",
    }
```

<a name="zero"/>

# 0. Choices to make

Take stock of how your app is set up.

1. Is it an SPA or a more static site? If it is an SPA, you will need to setup the AJAX functions to submit, rerender and redirect user data.
2. Is your user model already setup? What fields are you going to require on registration?
4. What strategies are you going to need? What [scope](http://passportjs.org/docs/oauth#scope) are you going to need for your webapp? ([Facebook](https://developers.facebook.com/docs/facebook-login/permissions), [Google](https://developers.google.com/identity/protocols/googlescopes#adexchangesellerv2.0))
4. What will be your success redirect and your failure redirect?
4. How will you handle errors and wrong user/passwords? Will you flash error messages or generate your own?
3. (Bonus) What is the "forgot password" flow? Are you going to verify emails on signup? How about social account linking?
1. (Bonus) do your users have different permission levels? What functionality is scoped to which user levels?

<a name="one"/>

# 1. NPM installs

Append `--save` if required (not required for npm 5+). 

Basic: `npm install passport express-session body-parser`

Strategies:

- Local: `npm install passport-local`
- DB specific helpers: `passport-local-mongoose` or [passport-local-sequelize](https://github.com/madhurjain/passport-local-sequelize)
- Provider Strategies: `npm install passport-github passport-twitter passport-google-oauth`

<a name="two"/>

# 2. Require and configure on Express Server process

This is pretty straightforward and doesn't have much flexibility. On your `server.js` or equivalent:

```javascript
var passport = require('passport');
var bodyParser   = require('body-parser');
var session      = require('express-session');

//...
//middleware section, after var app = express();
//...

app.use(bodyParser.json()); // get information from html forms
app.use(bodyParser.urlencoded({ extended: true }));
app.use(session({ secret: process.env.SESSION_SECRET })); // session secret
app.use(passport.initialize()); // must be after express-session is called)
app.use(passport.session()); // persistent login sessions
```

Note that `cookie-parser` is not needed since express 1.5.0.

Your session by default is only locally stored in memory, which means you will lose all sessions if your server dies. You can use other middleware like `connect-session-sequelize` to put it in a database so it persists.

```javascript
const session = require('express-session')
const SequelizeStore = require('connect-session-sequelize')(session.Store)
const db = require('./db') // connection to the sequelize URI
const sessionStore = new SequelizeStore({db})

// ...

// session middleware with passport
  app.use(session({
    secret: process.env.SESSION_SECRET || 'my best friend is Cody',
    store: sessionStore, // this is now a sequelize store
    resave: false,
    saveUninitialized: false
  }))
```

Optionally use `connect-flash` to be able to `req.flash` error messages:

```javascript
var flash    = require('connect-flash');
// ...
app.use(flash()); // use connect-flash for flash messages stored in session
```

<a name="three"/>

# 3. Configure Passport Serialization

This could be on `server.js`, but could also be split into a separate file and brought into the server file with `require('./config/passport')(passport)`

```javascript
// implement passport.serializeUser
// implement passport.deserializeUser

```

sample implementation for a standard `User` model from any ORM

```javascript
const passport = require('passport');
const User = require('../api/users/user.model');
const router = require('express').Router();

router.use(passport.initialize());
router.use(passport.session());

passport.serializeUser(function (user, done) {
  done(null, user.id);
});

passport.deserializeUser(function (id, done) {
  User.findById(id)
  .then(user => done(null, user))
  .catch(done);
});

module.exports = router;
```

if you are using [passport-local-mongoose](https://github.com/saintedlama/passport-local-mongoose) (see their docs for how to add the plugin to the `User` model) then you can simply do

```javascript
passport.serializeUser(User.serializeUser());
passport.deserializeUser(User.deserializeUser());
```

Ditto for [passport-local-sequelize](https://github.com/madhurjain/passport-local-sequelize). However this does seem somewhat more work than it's worth so I cannot wholeheartedly recommend.

<a name="four"/>

# 4. Configure Passport Strategies

Here there is a decent amount of discretion. You can declare these in the config file above, or in the `server.js` file, or (probable best practice) in strategy-specific files right next to their corresponding routes (to be covered in the next step).

I am also going to assume you are not new to registering your app on the respective platforms to get the Client ID's and Secrets but here are convenience links:

- [Facebook Developers Portal](https://developers.facebook.com/)
- [Twitter Portal](https://dev.twitter.com/)
- [Google Cloud Console](https://console.cloud.google.com/) ([instructions](https://support.google.com/cloud/answer/6158849?hl=en))
- [Github Developer Program](https://github.com/settings/applications/new)

```javascript
/* 
    CAN BE IMPLEMENTED IN STRATEGY SPECIFIC FILES NEXT TO THEIR RESPECTIVE ROUTES 
    You will probably want to import your user models to FindOrCreate users in your strategy callbacks below
*/

// const LocalStrategy = require('passport-local').Strategy; 
// implement passport.use('local-signup', new LocalStrategy(), function(req, email, password, done) {})
// or
// const TwitterStrategy = require('passport-twitter'); 
// implement passport.use(new TwitterStrategy(), function (token, refreshToken, profile, done) {})
// or
// var GoogleStrategy = require('passport-google-oauth').OAuth2Strategy;
// implement passport.use(new GoogleStrategy(), function (token, refreshToken, profile, done) {})

```

Sample implementation for google auth with a sequelize `User` model

```javascript
var GoogleStrategy = require('passport-google-oauth').OAuth2Strategy;
passport.use(
  new GoogleStrategy({
    clientID: process.env.GOOGLE_CLIENTID,
    clientSecret: process.env.GOOGLE_CLIENTSECRET,
    callbackURL: '/auth/google/callback'
  },
  function (token, refreshToken, profile, done) {
    User.findOrCreate({
        where: {
            googleId: profile.id // make sure user model has a field for googleId
        }, 
        defaults: {
            name: profile.displayName,
            photo: profile.photos ? profile.photos[0].value : undefined,
            email: profile.emails[0].value
        }
    }).spread(user => {
        done(null, user)
    })
    .catch(err => done(err, false))
  })
);

```

Sample implementation for google with a mongoose `User` model:

```javascript
passport.use(new GoogleStrategy({
        clientID        : configAuth.googleAuth.clientID,
        clientSecret    : configAuth.googleAuth.clientSecret,
        callbackURL     : configAuth.googleAuth.callbackURL,
    },
    function(token, refreshToken, profile, done) {

        // make the code asynchronous
        // User.findOne won't fire until we have all our data back from Google
        process.nextTick(function() {
            User.findOne({ 'google.id' : profile.id }, function(err, user) {
                if (err)
                    return done(err);
                if (user) {
                    // if a user is found, log them in
                    return done(null, user);
                } else {
                    // if the user isnt in our database, create a new user
                    var newUser          = new User();
                    // set all of the relevant information
                    newUser.google.id    = profile.id;
                    newUser.google.token = token;
                    newUser.google.name  = profile.displayName;
                    newUser.google.email = profile.emails[0].value; // pull the first email
                    // save the user
                    newUser.save(function(err) {
                        if (err)
                            throw err;
                        return done(null, newUser);
                    });
                }
            });
        });
    }));
```

<a name="five"/>

# 5. Setup routes

You will likely need to set up an `/auth/` or `/api/auth/` route file, potetntially one per strategy and combined with the strategy setup you see above. Here are the things to deal with:

1. Provider Authentication (e.g. `GET /auth/google`)
1. Provider Callback (e.g. `GET /auth/google/callback`)
1. Logout (e.g. `GET /logout`)
1. (Optional) Profile Page and/or API (e.g. `GET /profile` or `GET /api/me`)
1. (Optional) IsAuthenticated middleware

Here is a sample implementation of Provider Authentication and Callback. Notes:

- here is your first chance to define the [scope](http://passportjs.org/docs/oauth#scope) of your auth request)
- what will you do on successRedirect and on failureRedirect? Do you want to flash failure messages? Make sure to read the "Custom Callback" section of [the docs](http://passportjs.org/docs/authenticate) for the available options.

```javascript
    app.get('/auth/google', passport.authenticate('google', { scope : ['profile', 'email'] }));

    app.get('/auth/google/callback',
            passport.authenticate('google', {
                    successRedirect : '/profile',
                    failureRedirect : '/'
            }));
```

Sample logout implementation

```javascript
app.get('/logout', function(req, res){
  console.log('logging out');
  req.logout();
  res.redirect('/'); // or res.sendStatus(204);
});
```

Sample /me implementation

```javascript
router.get('/me', function (req, res, next) {
  res.send(req.user);
});
```

Sample [isAuthenticated](https://github.com/jaredhanson/passport/blob/master/lib/http/request.js#L83) middleware (this is undocumented; see [related SO question](https://stackoverflow.com/questions/14188834/documentation-for-ensureauthentication-isauthenticated-passports-functions/14301657#14301657))

```javascript
// route middleware to make sure a user is logged in
function isLoggedIn(req, res, next) {
    if (req.isAuthenticated()) // if user is authenticated in the session, carry on 
        return next();
    res.redirect('/');     // if they aren't redirect them to the home page
}

// MIDDLEWARE USAGE EXAMPLE
// we will want this protected so you have to be logged in to visit
// we will use route middleware to verify this (the isLoggedIn function)
app.get('/profile', isLoggedIn, function(req, res) {
    res.render('profile.ejs', {
        user : req.user // get the user out of session and pass to template
    });
});
```

<a name="six"/>

# 6. Frontend Joy

Now the degrees of freedom are really wide open. Quite simply you just need to make a form that submits login information (or simply just calls the oauth provider route that you set up above). Once the authentication is done the redirects you set up above will need to exist and the conditional rendering for the logged-in-state will depend on how you are doing your frontend: 

- If you are doing static rendering then you will pass along the `req.user` information along with the rest of your page's information
- If you are using react/redux to manage the API calls you will want to dispatch action creators to store the user info that can then be used elsewhere.

Consider also how you will handle [401](https://httpstatusdogs.com/401-unauthorized)/[403](https://httpstatusdogs.com/403-forbidden) error codes in your UI.

If using together with `react-router`, you may want to build a wrapper component [like in this SO example](https://stackoverflow.com/questions/43164554/how-to-implement-authenticated-routes-in-react-router-4) to redirect unauthenticated folks. This is also the recommended pattern in [the official example](https://reacttraining.com/react-router/web/example/auth-workflow).

As for Redux management... Here's a sample redux and redux-thunk auth file:

```javascript
import axios from 'axios';
import { create as createUser } from './users';
import { browserHistory } from 'react-router';

/* ------------------    ACTIONS    --------------------- */

const SET    = 'SET_CURRENT_USER';
const REMOVE = 'REMOVE_CURRENT_USER';

/* --------------    ACTION CREATORS    ----------------- */

const set     = user => ({ type: SET, user });
const remove  = () => ({ type: REMOVE });

/* ------------------    REDUCER    --------------------- */

export default function reducer (currentUser = null, action) {
  switch (action.type) {

    case SET:
      return action.user;

    case REMOVE:
      return null;

    default:
      return currentUser;
  }
}

/* ------------       DISPATCHERS     ------------------ */

/**
 * Dispatchers are just async action creators.
 * Action creators are supposed to emit actions.
 * Actions will be reduced to produce a new state.
 *
 * However, thunks can also do side effects, such as route to another location.
 * This could get fairly elaborate, by taking arguments as to where to go, or
 * whether to change routes at all. But we illustrate a simple case with some
 * composed dispatchers which also route to a specific page.
 *
 * If we wanted the calling code (component) to handle the result instead, we
 * would use the "simple" dispatcher and chain off the returned promise.
 * Components should probably know nothing about side effects, however.
 */

const resToData = res => res.data;

// a "simple" dispatcher which uses API, changes state, and returns a promise.
export const login = credentials => dispatch => {
  return axios.put('/api/auth/me', credentials)
  .then(resToData)
  .then(user => {
    dispatch(set(user));
    return user;
  });
};

// a "composed" dispatcher which uses the "simple" one, then routes to a page.
export const loginAndGoToUser = credentials => dispatch => {
  dispatch(login(credentials))
  .then(user => browserHistory.push(`/users/${user.id}`))
  .catch(err => console.error('Problem logging in:', err));
};

export const signup = credentials => dispatch => {
  return axios.post('/api/auth/me', credentials)
  .then(resToData)
  .then(user => {
    dispatch(createUser(user)); // so new user appears in our master list
    dispatch(set(user)); // set current user
    return user;
  });
};

export const signupAndGoToUser = credentials => dispatch => {
  dispatch(signup(credentials))
  .then(user => browserHistory.push(`/users/${user.id}`))
  .catch(err => console.error('Problem signing up:', err));
};

export const retrieveLoggedInUser = () => dispatch => {
  axios.get('/api/auth/me')
  .then(res => dispatch(set(res.data)))
  .catch(err => console.error('Problem fetching current user', err));
};

// optimistic
export const logout = () => dispatch => {
  dispatch(remove());
  axios.delete('/api/auth/me')
  .catch(err => console.error('logout unsuccessful', err));
};
```

<a name="seven"/>

# 7. Bonuses - Account Linking, Forgot password, Email verify, permissioning etc

To be honest I dont have any experience with this stuff. But I know it is important. So shoot me links and tips ([@swyx on twitter](http://twitter.com/swyx)) and I will include them here.

- <https://scotch.io/tutorials/easy-node-authentication-linking-all-accounts-together>
