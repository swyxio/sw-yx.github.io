---
layout: post
title: Finishing Stock App
date: 2017-02-11
tags: freecodecamp gomix
feelings: fat
---

4.30 checking in here. job done! <https://swyx-fcc-stockchart.gomix.me> and <https://github.com/sw-yx/fcc-stockchart>

The issues faced: 

- integrating Socket.io with React caused me some confusion with knowing where to receive emitted messages on the client side (its in the constructor). Examples [here](http://www.thegreatcodeadventure.com/real-time-react-with-socket-io-building-a-pair-programming-app/) and [here (unvetted)](http://danialk.github.io/blog/2013/06/16/reactjs-and-socket-dot-io-chat-application/)
- getting Semantic-UI to run on GoMix so i just defaulted to bootstrap cards with [this remove card example](http://www.bootply.com/5Abr3Qa979#)
- getting [highcharts dark unica](http://www.highcharts.com/stock/demo/compare/dark-unica) to render on gomix (more or less solved by just pasting it into my react code, not pretty but it works
- realizing that socket.io basically replaces ajax; but I had been using jquery for ajax before and have up til now still not actually used [axios] which looks pretty simple.

inspiration:

- <https://github.com/open-source-society/computer-science>
