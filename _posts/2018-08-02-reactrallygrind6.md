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
 and heres an interaction with settimeout: http://www.javascriptkit.com/javatutors/requestanimationframe.shtml
 
 
 ```js
 var adiv = document.getElementById('mydiv')
var starttime
 
function moveit(timestamp, el, dist, duration){
    //if browser doesn't support requestAnimationFrame, generate our own timestamp using Date:
    var timestamp = timestamp || new Date().getTime()
    var runtime = timestamp - starttime
    var progress = runtime / duration
    progress = Math.min(progress, 1)
    el.style.left = (dist * progress).toFixed(2) + 'px'
    if (runtime < duration){ // if duration not met yet
        requestAnimationFrame(function(timestamp){ // call requestAnimationFrame again with parameters
            moveit(timestamp, el, dist, duration)
        })
    }
}
 
requestAnimationFrame(function(timestamp){
    starttime = timestamp || new Date().getTime() //if browser doesn't support requestAnimationFrame, generate our own timestamp using Date
    moveit(timestamp, adiv, 400, 2000) // 400px over 1 second
})
```


---

the om style serialize state and free undo stuff is also seriously cool, i want it:

- https://circleci.com/blog/local-state-global-concerns/
- https://swannodette.github.io/2013/12/31/time-travel

---

3 aug

slidemaking mode. i am 20 slides in and i think i have the rough sequence of arguments, but i need diagrams to help argue the point and then i need some examples.

i also want to figure out if push-pull FRP is actually what we are talking about.

1.30am - done with "problem" flowcharts. now i need to look into push pull frp

ok i think i have all the charts i need. i am not going to claim push pull frp because i frankly dont super understand it but i like the idea of a reactive structure that is a "sequence of non reactive phases".

7.30am done https://www.beautiful.ai/-LIw2oTJmrrwRqvsoYgD/3
