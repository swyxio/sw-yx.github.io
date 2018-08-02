---
layout: post
date: 2018-08-02
tags: react
feelings: neutral
title: react rally grind 6
comments: true
description: a detour into immediate mode and retained mode
---

spent some time reading into immediate mode GUIs based on [this discussion of react](https://games.greggman.com/game/react-and-redux-are-a-joke-right/). it seems React is retained mode because components retain state. but interestingly enough the browser is immediate mode. here's another discussion of that fact:

- https://gamedev.stackexchange.com/questions/24103/immediate-gui-yae-or-nay 
- https://www.gamedev.net/forums/topic/694192-when-did-immediate-mode-take-over-the-web/
- seb on vdom being immediate mode: https://twitter.com/sebmarkbage/status/530393349069750272
- great essay on the topic: http://www.johno.se/book/imgui.html

this is related to a broader group of essays on the fundamentals of react:

- https://jlongster.com/Removing-User-Interface-Complexity,-or-Why-React-is-Awesome
- https://github.com/reactjs/react-basic/blob/1512678469e04da02fe052ba884480a78f2e03ee/README.md
- react and bacon: https://joshbassett.info/2014/reactive-uis-with-react-and-bacon/

---

a cool idea for my demo would be using requestanimationframe to update: https://css-tricks.com/using-requestanimationframe/
