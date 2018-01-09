---
layout: post
date: 2018-01-09
tags: firestore
feelings: neutral
title: firestore primer
comments: true
description: some notes on adopting firestore
---

there are bindings for react and firebase without redux.

however there are no bindings for react and firestore without redux. 

this is the best i've found: <https://github.com/prescottprue/redux-firestore>

So adopting needs to be tested to understand where redux goes in.

## instructions

1. set up a Create React App
2. `yarn add redux react-redux firebase redux-firestore react-redux-firebase@next`
3. add `firebase.js` in src as per their readme
4. add `firebaseConfig` from firebase console
5. copy in the component class from their readme
6. add it into App.js
7. npm start

this turned out not to work very well. i have to try the simple <https://github.com/unfold/react-firebase/issues/50> version

---

i managed to adapt react-firebase for firestore successfully. not the neatest code but it works. i also sent it in as a PR <https://github.com/unfold/react-firebase/issues/50> but dont think it will get accepted. still dont know how to make my prettier work with eslint.

time to look at auth rules.

---
