---
layout: post
date: 2018-04-23
tags: chrome
feelings: frustration
title: async render toolbox woes
comments: true
description: urghghgh
---

i am trying to inject my script but running into gnarly csp issues (i need to eval my script to get around the sandbox issue).

Uncaught EvalError: Refused to evaluate a string as JavaScript because 'unsafe-eval' is not an allowed source of script in the following Content Security Policy directive: "script-src 'report-sample' 'sha256-6gLjSWp3GRKZCUFvRX5aGHtECD1wVRgJOJp7r0ZQjV0=' 'unsafe-inline' static.licdn.com s.c.lnkd.licdn.com static-fstl.licdn.com static-src.linkedin.com https://www.linkedin.com/voyager/service-worker-push.js https://platform.linkedin.com/js/analytics.js static-exp1.licdn.com static-exp2.licdn.com s.c.exp1.licdn.com s.c.exp2.licdn.com static-lcdn.licdn.com s.c.lcdn.licdn.com https://www.linkedin.com/sc/ https://www.linkedin.com/scds/ https://qprod.www.linkedin.com/sc/ https://www.linkedin.com/sw.js https://www.linkedin.com/voyager/abp-detection.js".

urgh. i added all sorts of shit onto my manifest.json.

it looks to be a dead end.
