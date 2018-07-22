---
layout: post
date: 2018-07-22
tags: egghead
feelings: neutral
title: css fundamentals 1
comments: true
description: some notes for a first course
---

alright, time to learn in public

# Understand CSS Cascade

the Cascade is the core algorithm in CSS. Browsers use them to resolve conflicts between multiple CSS declarations that can apply to the same property of the same element. 

CSS first looks at the stylesheet origin, or which stylesheet the rule comes from.

If the conflicting rules come from the same stylesheet, then it looks at their selector specificity, and the higher specificity wins.

If the conflicting rules have the same specificity, then whichever comes later in order wins.

You can always look up the MDN docs for Cascade to see the 7 levels of stylesheet origins. You can halve these by adopting the rule of thumb to not use the !important tag.

A better way to understand what they are is to remember where they come from:

The user agent stylesheet is defined by the browser
The user stylesheet is defined by the user (this is not often used)
The CSS Animations and author stylesheet are  defined by the website author

As a website author most of your work will be on the author stylesheet, where you will be dealing with specificity.

Specificity has its own mini-cascade, going up from Tags and pseudo-elements like :before, to Classes, HTML attributes, and pseudoclasses like :hover, to IDs, then to inline styles which have the highest specificity of all. The browser literally counts the number of each of these selector types, and whichever has the highest number of types of the highest priority wins. Let's look at an example.

Notice here that I have set up some example HTML. Just pay attention to the structure here, where I have an H1 with an id and class, inside of a header with another class. You can see the result of the specificity calculation in the browser dev tools on the right.

Currently my header is purple because I only have one author stylesheet rule saying it is purple. Notice I have annotated the rule with its specificity of 0, 0, 4. This is the most common way to denote the counts of the selector types. Here it is saying there are 0 id's included here, 0 classes, and 4 tags: html, body, header, and h1.

Lets see what happens when we introduce a conflicting rule. This rule wants my header to be orange, and as you can see in the dev tools it supercedes the original rule saying it should be purple. Why does it have a higher specificity? Because it included one single class tag. 

When specificities are exactly the same, the rule order will determine the "winnner" of any conflicts. This often comes up in styling link pseudo classes. Here I've set up 4 rules with exactly the same specificity of [0, 1, 1]. You can see that I have turned off the user-agent stylesheet's default text-decoration of links, but re-enabled them on hover. This is a very common effect on websites. But when I switch the order of these rules, the underline stops appearing because the rule with text-decoration: none comes later in order and has the same specificity as my hover rule. I can fix this by lowering the specificity of the original rule.

A handy trick to debug pseudoclasses here is to force the states in your browser devtools. Here I can set a hover state without actually hovering my mouse over the link. Let's try turning on the visited and active state. You can see here the active pseudoclass wins (the link is red) because it comes later in order, and if I swap the order, the visited state wins (the link is purple).

---

```css
// 1. origin: author !important > author > user agent
// 2. specificity: ids > classes > tags
// 3. order
```

# rules of thumb

1. dont use IDs
2. dont use !important

# Control CSS Inheiritance

what properties are inheirited: text, list, table border
inheirit keyword
beware shorthand properties
