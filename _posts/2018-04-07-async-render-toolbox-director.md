---
layout: post
date: 2018-04-07
tags: js
feelings: happy
title: async render toolbox, director edition
comments: true
description: doing research to implement the async render toolbox
---

launching the lag radar was easy but the other dev tool that dan abramov demoed at jsconf was the "director". pomber. implemented this with tight integration to his cache (clone [here](https://codesandbox.io/s/kk2v1op3m5), hitchcock director [here](https://github.com/pomber/hitchcock)) but i think it is doable without tying in to the cache.


the first method i looked into was using the chrome extension's ability to block requests:

- [tamper chrome](https://chrome.google.com/webstore/detail/tamper-chrome-extension/hifhgpdkfodlpnlmlnmhchnkepplebkb?hl=en) is full featured extension that does this. [requestly.in](https://www.requestly.in/home/) also does similar.
- [basically using the chrome webrequest.onbeforerequest.addlistener](https://stackoverflow.com/questions/30590428/chrome-extension-how-to-intercept-requested-urls?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa) method
- however the chrome sample is very sparse (just shows you how to substitute images with dog images). the documentation is available [here](https://developer.chrome.com/extensions/webRequest#event-onBeforeRequest)

the difficulty would have been commmunicating between my chrome extension and the injected js on the page (which is possible using the message api, or i could sidestep by just putting the extension inside the console).

but then this blogpost for apirequest.io [here](https://www.moesif.com/blog/technical/apirequest/How-We-Captured-AJAX-Requests-with-a-Chrome-Extension/) turned me on to the possibility that i could use simple xmlhttprequest to do this. so down the rabbit hole i go.

the above article was light on details (and the extension code is minified), this article shows exactly how to "monkey patch" xmlhttprequest's send: <http://qnimate.com/monitoring-ajax-requests/>. This gist does it in a IIFE manner <https://gist.github.com/suprememoocow/2823600>

I do need to determine what hooks i want to hook into. A bit of googling leads me to [the WHATWG spec](https://xhr.spec.whatwg.org/#events) and i think i pretty much have all the pieces I need.

---

plan of attack

- add logging into onloadstart and onreadystatechange and onprogress and onload
- do not pass loads until i want
- figure out the difference between load and send
- pause all loads on initiate
- have a button i can click to proceed with loading
- individual loading
- load pausing

---

i was foiled by the [execution environment](https://developer.chrome.com/extensions/content_scripts#execution-environment) of my content script. the xmlttprequest that i modify in my content script is just not the same as the one that operates on the main browser. im not sure i see a way around this, i might haave to go for a web worker or a browser onrequest extension.

---


you can sneak around the execution environment by [injecting a script tag](https://stackoverflow.com/questions/2660512/are-google-chrome-extension-content-scripts-sandboxed)... and then inject [a script tag inside the script tag](https://stackoverflow.com/questions/26183649/correct-simplest-way-of-calling-one-js-file-inside-another-js)... yo dawg i heard you like script tags

---

i decided i probably also want to use react since im building a UI, so basically i need to set up an ejected CRA to build without chunk hash names. https://medium.com/@yosevu/how-to-inject-a-react-app-into-a-chrome-extension-as-a-content-script-3a038f611067

---

yo and while ur at it can you go ahead and [add script tags inside your jsx](https://stackoverflow.com/questions/34424845/adding-script-tag-to-react-jsx?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa) k thx bye

---

then you pull the bundled jsx thru [raw git](https://stackoverflow.com/questions/17341122/link-and-execute-external-javascript-file-hosted-on-github)
