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

my first attempt at this involved using `passport-local-mongoose` but it kept dying horribly due to bad documentation so i ripped it out.

## Step 1

`npm i express-session passport passport-local bcrypt-nodejs` install `body-parser` if you need it

## Step 2

add `UserSchema.js`:

```js
const mongoose = require('mongoose');
var bcrypt   = require('bcrypt-nodejs'); // works better on windows https://scotch.io/tutorials/easy-node-authentication-setup-and-local
const Schema = mongoose.Schema;

const userSchema = new Schema({
  username    : String,
  password     : String
});

// methods ======================
// generating a hash
userSchema.methods.generateHash = function(password) {
  return bcrypt.hashSync(password, bcrypt.genSaltSync(8), null);
};

// checking if password is valid
userSchema.methods.validPassword = function(password) {
  return bcrypt.compareSync(password, this.password);
};


module.exports = mongoose.model('User', userSchema);
```

add `auth.js`:

```js
const passport = require('passport');
const LocalStrategy = require('passport-local');
const session      = require('express-session');
const router = require('express').Router();

const User = require('./UserSchema')

router.use(session({
  // this mandatory configuration ensures that session IDs are not predictable
  secret: 'SUPERSECRETSECRET', // or whatever you like
  // this option is recommended and reduces session concurrency issues
  resave: false,
  saveUninitialized: true,
  cookie: { secure: true } // requires HTTPS
}));
router.use(passport.initialize());
router.use(passport.session());

// http://www.passportjs.org/docs/username-password/
passport.use(new LocalStrategy(
  function(username, password, done) {
    User.findOne({ username: username }, function(err, user) {
      if (err) { return done(err); }
      if (!user) {
        return done(null, false, { message: 'Incorrect username.' });
      }
      console.log('user', user)
      if (!user.validPassword(password)) {
        return done(null, false, { message: 'Incorrect password.' });
      }
      return done(null, user);
    });
  }
));

passport.serializeUser(function(user, done) {
  console.log('serialized', user)
  done(null, user._id);
});

passport.deserializeUser(function(id, done) {
  User.findById(id, function(err, user) {
    console.log('deserialized', user)
    console.log('deserialized err', err)
    done(err, user);
  });
});

router.get('/auth/me', (req, res) => {
  console.log('req.user', req.user)
  res.json(req.user)
})

router.post('/auth/signup',
  async (req, res, next) => {
    const newUser = new User(req.body)
    newUser.password = newUser.generateHash(newUser.password)
    const result = await newUser.save()
      .then(user => req.login(user, err => err ? next(err) : res.json(user)))
      .catch(next)
  },
  passport.authenticate('local'),
  (req, res) => res.json(req.user)// success
);

router.post('/auth/login',
  passport.authenticate('local'),
  (req, res) => res.json(req.user) // success
);

router.post('/auth/logout',
  (req, res) => {
    req.logout()
    res.json(null)
  }
);
module.exports = router;
```

in `index.js`:

```js
app.use(require('./auth')) // needs to be after bodyParser
```

## Step 3

now for the clientside. a very simple implementation in React in App.js:

```js
import React, { Component } from "react";
import logo from "./logo.svg";
import { ToastContainer, toast } from 'react-toastify';
import "./App.css";

class App extends Component {
  state = { polls: null, message: "", newpollname: "", user: null };

  componentDidMount() {
    fetch("/api/hello")
      .then(res => res.json())
      .then(res => this.setState({ message: res.express }));

    fetch("/auth/me")
      .then(res => res.json())
      .then(user => this.setState({ user: user }))
      .catch(err => console.log('no current user ', err))

    this.getPolls()
  }
  getPolls = () => fetch("/api/polls")
    .then(res => res.json())
    .then(res => (res ? this.setState({ polls: res }) : null));

  clickHandler = poll => () => {
    console.log('poll', poll)
    fetch("/api/polls", { 
      method: "POST",
      body: JSON.stringify({poll}), 
      headers: new Headers({
        'Content-Type': 'application/json'
      })
    }).then(this.getPolls)
  };
  deletePoll = poll => () => {
    fetch("/api/polls?_id=" + poll._id, { method: "DELETE"});
    this.setState({polls: this.state.polls && this.state.polls.filter(p => p._id !== poll._id)})
    toast.success( "Deleted poll!")
  }
  textchange = e => {
    e.preventDefault()
    this.setState({newpollname: e.target.value})
  }
  addNewPoll = () => {
    this.setState({polls: this.state.polls.concat({
      name: this.state.newpollname,
      counter: 0,
      id: null
    })})
  }
  textHandler = fieldName => e => this.setState({[fieldName]: e.target.value})
  authHandler = authAction => () => {
    fetch('/auth/' + authAction, 
      {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
        body: JSON.stringify({
          username:this.state.username,
          password: this.state.password
        })
      }
    )
    .then(res => res.json())
    .then(user => {
      this.setState({ user })
      toast.success(authAction + " success!")
    })
    .catch(console.log)
  }
  render() {
    const {polls, user} = this.state
    return (
      <div className="App">
        <ToastContainer />
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1 className="App-title">Welcome to React</h1>
        </header>
        <hr />
        {user ?
          <div>Logged in as: {user.username} <button onClick={this.authHandler('logout')}>Logout</button></div>:
          <div>
              <h1>login</h1>
              <div>
                  <label>Username:</label>
                  <input type="text" name="username" onChange={this.textHandler("username")}/>
              </div>
              <div>
                  <label>Password:</label>
                  <input type="password" name="password" onChange={this.textHandler("password")}/>
              </div>
              <div>
                  <button onClick={this.authHandler('login')}>Log In</button>
                  <button onClick={this.authHandler('signup')}>Sign Up</button>
              </div>
          </div>
        }
        <hr />
        <p>Message from the server: {this.state.message}</p>
        <p className="App-intro">SunburstJS</p>
        <ul>
          {polls && polls.map((poll, i) => {
            return <li key={i}>{poll.name}: {poll.counter}
              <button onClick={this.clickHandler(poll)}>click me</button>
              <button onClick={this.deletePoll(poll)}>delete me</button>
              </li>
          })}
        </ul>
        <input onChange={this.textchange} value={this.state.newpollname} placeholder="name of poll" />
        <button onClick={this.addNewPoll}>add new poll</button>
        
      </div>
    );
  }
}

export default App

```
