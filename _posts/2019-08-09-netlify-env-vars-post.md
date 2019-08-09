---
layout: post
date: 2019-08-09
tags: netlify
feelings: determined
title: netlify env vars blogpost
comments: true
description: netlify env vars
---

# Netlify Environment Variables: The Cheat Codes of the Internet

Environment Variables occupy an interesting space in our collective mindset. They are [more secure than plaintext](https://stackoverflow.com/questions/12461484/is-it-secure-to-store-passwords-as-environment-variables-rather-than-as-plain-t), but also [Considered Harmful](https://news.ycombinator.com/item?id=8826024). As globals that we call by a more polite name, they have a funny way of spreading: They were [introduced to JavaScript by the Express backend framework](https://github.com/expressjs/express/commit/03b56d8140dc5c2b574d410bfeb63517a0430451) in 2010 and now [drive entire library build processes and development experience tradeoffs](https://overreacted.io/how-does-the-development-mode-work/) for all the major frontend frameworks (you can [make your own dev-only warnings with TSDX](https://github.com/palmerhq/tsdx#development-only-expressions--treeshaking)!). Because unintentionally leaking API tokens to clients is a Very Bad Thing, we then evolved best practices like prefixing environment variables with [`REACT_APP_`](https://facebook.github.io/create-react-app/docs/adding-custom-environment-variables#docsNav) or [`VUE_APP_`](https://cli.vuejs.org/guide/mode-and-env.html#environment-variables) or [`GATSBY_`](https://www.gatsbyjs.org/docs/environment-variables/#client-side-javascript).

However

