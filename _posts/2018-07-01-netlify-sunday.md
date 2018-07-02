---
layout: post
date: 2018-07-01
tags: netlify
feelings: neutral
title: netlify sunday
comments: true
description: netlify sunday
---

second wind

- i have a couple of paths i can pursue now. i dont like the separate declaration of paths. but i also probably want to have a data injection model, and thats probably the meatier thing to tackle rather than nitpicking about dev ergonomics.
- create-jamstack-app could be interesting - netlifycms is really good to show off the power.
- ok well lets do this i guess
- <http://npm.im/create-jamstack-app>
- tired out now. i have to think about converting the jamstack-scripts node process into a babelified thing. also would be good to import other assets including css.

---

sun pm session

- i need to make the jamstack-scripts node process be babelified.
- lost an hour on trying to make @babel/register work
- using babel-register, and now the ignore: false is biting me. logan again: https://github.com/babel/babel/issues/6130#issuecomment-324495961
- babel-register is a complete failure
- i am going to try inserting a straight `babel` build step into create-jamstack-app
- filled out https://github.com/sw-yx/boring-ssg 's readme at matt's request
- so i got the BoringShell transpiled in `jamstack-scripts`. i should rip out the current CJA bundler and put in BoringShell
- CJA assumes an index.html. i might regret my decision to target index.js as entry instead of index.html.
- ok i did a first rewrite of boringshell with babel to hook in to the `yarn start` process. v0.0.4 published. hope it doesnt blow up in my face.
- LOL

```
✝  netlify/create-jamstack-app/plsdelete   master  yarn start
yarn run v1.6.0
$ jamstack-scripts start
Cannot find module '../BoringShell'
error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
```

- ah. i need to whitelist the files i publish... and the template dependencies thing didnt work for @reach/router
- filed [this issue](https://github.com/facebook/create-react-app/issues/4717)
- minor [babel](https://stackoverflow.com/questions/38056498/babel-es7-async-regeneratorruntime-is-not-defined) hiccup nbd
- now i have `yarn start` running but because i prebabeled my stuff i basically punted on doing it for SSR?
- this is a big problem, potetntial showstopper. i dont know how to do this.


stuck
___

so i am having trouble with my babel strategy - talking out loud in case anyone here has ideas/advice

- basically for static site generation i need babel running in node to SSR the react app. I know how to do this as a standalone app, its just `babel-node`. 
- However now i’m making a library to do the same thing and i can’t use `babel-node` or `babel-register`. 
- so in my library i compile down using regular `babel`. however now this es5 js inside the library doesn’t know how to work with the esnext js in the app!

---

after being in a bit of a funk i am retrying babel-register

- taking serious inspiration from react-statics setup with the separate js file.
- well ok i gave the babel config a try and it did not work. trying it in a slightly different place, and with a "binhelper"
- now it wants to have a top level .babelrc. i am also forced to install babel-preset-react-app top level. lets see.
- ok. i have hit the limit. i cant get the babel node thing to work without seriously ripping up a lot of this infrastructure. time to scale down my ambitions, and just work on last mile for boring-ssg.

----

sun evening

- so back to boring ssg. time to brush up the DX a bit and actually do a bit of data injection
- took in the files from gatsby starter netlifycms
- now i have a basic markdown plugin but its not injected
- the data injection is very complex. not sure i have enough time to do it.
- drew some state charts, then decided that while the route/split data was possible, it wasnt ever going to be finished by me on time. need to be less ambitious.
- big ball of data approach.


# miscellany

---

today i learned how npm's bin works. in your package.json you have this:

```
  "bin": {
    "jamstack-scripts": "./bin/jamstack-scripts.js"
  },
```

upon install that file (if it exists) gets copied to your top level `node_modules/.bin` thingy. and thats how you can run `jamstack-scripts foobar` in the command line within that folder.


---



---

misc thoughts

- "more projects than people" is a great comment
- "answer a different question" when facing a tough/boring question
