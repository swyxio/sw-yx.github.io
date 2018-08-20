---
layout: post
date: 2018-08-19
tags: react
feelings: neutral
title: reactconf cfps
comments: true
description: even more cfps tho im not super feeling it
---

Title: Reacting Ahead of Time

Abstract: What's faster than O(1)? Step changes in performance and scalability come from paradigm changes rather than incremental optimizations. The inexorable march forward in "table stakes" product requirements  has forced developers to explore ways to push computation from client to server, from runtime to buildtime, from fat SPAs to code-split frameworks. Cutting edge UX is exploring how to prefetch and cache resources intelligently, trading off memory usage for compute time. Lastly, DX has also been pushed forward with fascinating ideas from hot reloading to GraphQL. Where is all this taking us?

Details for Review:

- How will you start?

I need to track down an exact quote but the basic inspiration for this comes from a Google TGIF where either Larry or Sergey were asked about the lower bound for search response time. The expectation was that 0 seconds was a hard lower bound, but their response was surprising - why should 0 be a lower bound for response time? Once we start aiming for negative response times (i.e. predicting what you will need) we get very interesting solutions. The solution itself may not be extremely practical, but the paradigm shift that results from asking absurd questions like that can be very instructive!

- Content

This isn't a web performance talk so much as a paradigm shift talk. What I see happening in the React and greater Javascript ecosystem is pushing around the center of gravity of compute and it is fascinating seeing the resulting practices and frameworks that rise up to help developers execute these ideas. I think the common thread is continually pushing computation  to Ahead-of-Time where possible, and I've never seen anyone compare the related strategies people are taking toward this path. 

I'll keep the talk very frontend focused since that's what the audience is most likely to care about.

Somewhere in the talk I will have to reference Outkast's Hey Ya! and ask "what's cooler than being cool?" The equivalent of "ice cold" for us is "blazing fast", of course. 

- What is your conclusion? 

I don't know my conclusion just yet - will spend some time researching and thinking about implications of current trends. Ideally I want to provide a list of fertile areas for developers to explore based on the trends we cover in the talk.

- If you have live demos, how will you make sure they go smoothly? No live demos, just gifs

Pitch:

This talk needs to happen because seeing common trends tying together the disparate pieces of news of frameworks and libraries popping up to solve DX and UX problems can be a helpful organizing mental model. It gives developers a common starting point to discuss paradigm shifts rather than incremental improvements. I am not the only right person but I'm -a- right person to give this talk because I absolutely love identifying current trends, putting them in context with our history, and identifying areas of opportunity. I've only spoken at React Rally before, but I am a frequent meetup speaker and am comfortable speaking.
