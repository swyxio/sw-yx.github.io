---
layout: post
date: 2018-06-05
tags: graphql
feelings: inspired
title: rauchg graphql
comments: true
description: meeting an industry hero
---

rauchg talked about mdx at reactnyc today after i invited him on twitter. crazy how these things just happen.

he talked about mdx and it was helpful to get the ideas straight from him. i like that he dislikes the `import` statements at the top but i am not sure if i like his idea of taking a web-components dictionary so as not to insert js into mdx. i think that way lies a lot of config.

---

the after talk was MUUUCH more interesting. some notes

- typescript is a nice middle ground between untyped and typed languages. typescript is annotations, and that's new. typescript is a path to guide webdevs towards a strongly typed future, probably rust -> wasm.
- graphql can do a lot more to deliver on its promise: error monads, and error types.
- (im more excited about this) graphql modules (instead of es modules) so you have no difference between accessing a local library or pinging an external api
- server to server graphql (he didnt really have time to expand on this but he’s seeing stuff at now.sh that makes him think its a thing)

he's playing a "protocol" instead of a "framework" game. that seems like a very fundamental truth. added "work on protocols rather than frameworks rather than libraries" to my truth list.

he cares about dx, and talks about how google and coinbase are using now.sh even though they have the tech ability to use other things. i wonder if that is only fine for toys but not for larger products. whats the biggest app on now.sh? it seems like its Iota, the crypto platform.

pointed us to <https://www.youtube.com/watch?v=hrBq8R_kxI0&index=7&list=PLBnKlKpPeagmDknQV_PT9XP8YK-3T_tNT> and his Pure UI blog post from 3 yrs ago.

the main things he cares about dx are errors. building it into the language, making developers consider error states before it compiles instead of after. the enemy is jquery, the goal is react/rust.

react is winnign because of the care put into its api: static gDSFP is an example. also components are non leaky abstractions.

---

also i had tsi zoo garden party today. it was gorgeous.
