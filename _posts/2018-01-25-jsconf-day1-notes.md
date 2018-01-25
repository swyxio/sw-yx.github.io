---
layout: post
date: 2018-01-25
tags: notes
feelings: happy
title: jsconf asia day 1 notes
comments: true
description: notes from jsconf day 1
---

una

- the various groups of mix-blend-mode properties. darken lighten etc
- back compatibility for browsers that dont support blend mode - put it behind (z-index: -1) and use luminosity?
- <https://una.im/CSSgram/>
- <https://lukyvj.github.io/colofilter.css/>
- knockout text
- use the `filter` property
- svg filter effects
- Consistency - browsers are still not consistent about applying their color transformations
- <https://kazzkiq.github.io/svg-color-filter/> for testing svg effects
- duo tone effect - use with css variables
- Performance - black and white filters have ~20% savings
-  black and white doesnt have to be boring!!
-  contrast swap technique (lower image contrast, up in css)


homework: how did she do the Show Layers page in the duotone section?

inian

- static analysis is faster and has better code coverage
- dynamic analysis can analyse obfuscated code, browser engine diffs, and dynamically loaded code
- dom based xss - can steal cookie by adding # without any bugs in backend code
- sources: window, document, cookies
- sinks: eval() or manipulating dom in some way (document.write)
- how to combat dombased xss: 
- method 1: marktaint(loc) - (this is bad)
- not sure where to insert eg in for loop where do you do it
- method 2: use comma operator to continue execution of code as per original but with evaluation alongside
- ugly because limited to expressions, have to use ternary
- method 3: use IIFE: no leak in variables, not limited to only expressions
- storing analysis information: use string object in place of string literal, add on properties
- overloading keywords like typeof - transforming to functions
- patching dom based xss

ferross

- a linter catches bugs/analyses code to catch suspicious constructs
- jslint -> jshint -> eslint
- hierarchy: programmer errors > best practices > style issues
- extend sharable config so that you dont pick 200 lines of things.

```
{
  "extends": ["standard"]
}
```
- but beware rule overrides. this is where standardjs comes in.
- use `npm i standard --save-dev` then `standard` and it runs. also `standard --fix` to fix.
- or in npm script just have `test: standard && node test.js` to run before other tests
- should you use eslint directly? gotchas: bikeshedding, duplication
- best practices: avoid the WATs but be pragmatic
- prettier is great but only addresses style issues
