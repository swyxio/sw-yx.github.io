---
layout: post
date: 2017-02-10
title: Sockets With Persistence
tags: freecodecamp gomix socketio
feelings: preempting
---

So the other part of the stock chart challenge is implementing sockets. This is native within create-react-app and meteor but I wanted to do a bit of my own cooking as it is too easy to lean on meteor.

I did want to try gomix to see what a different deployment experience might look like. So the challenges I am setting myself here are deploy on gomix and implement socket.io first before laying on the stock charts which should be the simplest layer now that I have found some libraries.

Sockets tutorials:

- basic tutorial <http://thejackalofjavascript.com/websockets-and-sockets-io/>
- heroku's guide to socket deployment <https://devcenter.heroku.com/articles/node-websockets> and some SO issues <http://stackoverflow.com/questions/37950349/socket-io-chat-example-heroku>
- very good SO thread on sockets vs websockets: <http://stackoverflow.com/questions/10112178/differences-between-socket-io-and-websockets>
- full chat app <https://dzone.com/articles/using-redis-with-nodejs-and-socketio>
- the inevitable "you should just do websockets" <https://medium.com/@ivanderbyl/why-you-don-t-need-socket-io-6848f1c871cd#.2274as6o7> and <https://davidwalsh.name/websocket>

---

10.40 checking in. So doing an MVP wasn't too difficult although I did run into some issues with [synchronous programming with fiber](https://github.com/alexeypetrushin/synchronize/issues/23). i just switched to async for now as I wasn't able to figure out what the issue was.

<https://gomix.com/#!/project/persistent-chat> stored in <https://github.com/sw-yx/gomix-persistent-chat/tree/gomix> is a project that takes the gomix standard projects: [async storage](https://gomix.com/#!/project/mongodb-async) and [socketio chat](https://gomix.com/#!/project/socketio-chat) to create a persistent chat project.

I will then use this to create a separate ticker-chat project where i will layer on the api pull and the charting library and finish this FCC project.

---

inspirations

- full multiplayer tutorial <http://buildnewgames.com/real-time-multiplayer/>
