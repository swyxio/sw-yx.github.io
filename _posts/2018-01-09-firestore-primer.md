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
