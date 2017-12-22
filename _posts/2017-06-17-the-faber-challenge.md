---
layout: post
date: 2017-06-17
title: the faber challenge
tags: projects
feelings: fat
---

# Weekend sprint! 
Meb Faber [threw down the gauntlet](http://mebfaber.com/2017/06/07/owe-160000-mba-get-paid-receive-masters-investing/) for one of my most passionate projects. I don't feel ready but the time is right to do this and this is going to happen with or without me.

Basically, the problem is the balance between consumption (entropy) and creation (order).

As a broad rule of thumb, it is generally better to create something than it is to consume something. Not only is it better for the world, it is also better for you. This is a high-class problem that many intelligent people face. [This question on Hacker news](https://news.ycombinator.com/item?id=14386394&source=techstories.org). Content shock is a real thing and the pace of (spammy) creation is [broadly outpacing consumption](https://www.businessesgrow.com/2014/01/06/content-shock/) in many media fronts.

The solution for this on the low end is a social layer to crowdsource and filter and surface quality content, and on the high end a done-for-you service that summarizes the content and makes it searchable/structured.

To get traction though, I hope to get Meb's help. So I must tailor my mvp to his criteria.

## Here are the specs he has laid out:
1. Rank each podcast on a scale of 1-10.
2. Identify the best sections from all the episodes (FYI, a great clip could come from an episode that you didn’t rate highly overall). Be sure to label the exact minute markers (start and end times) of the quality content. A great clip could be just a few seconds, or far longer (there isn’t a time cut-off – the issue is quality).
3. For each podcast section you identify as ‘best,” write-up a short, 1-3 bullet-point sentence description, identifying the subject matter, and what makes it worthy of listening to.
4. Each week, you’ll throw in one fun/interesting/miscellaneous snippet from a podcast of your own discovery. It can be financial, though it doesn’t have to be – the point is for this snippet to be interesting/helpful/or somehow beneficial for the listener. Be creative.
5. You’ll send all this information to Jeff each week. We’ll provide a template to candidates on how to do this later.

I aim to create software to solve 1, 2, 4, and 5.

## Meb also outlined some details on twitter:
1. [hire and pay about 5 quality and motivated people to listen, rate, and summarize about 20 weekly podcasts.](https://twitter.com/MebFaber/status/875764151674585088)
2. [if there is a better solution for paying on per week basis and utilizing all 150?](https://twitter.com/MebFaber/status/875764528637648897)
3. [his list of 16 podcasts he definitely listens to](https://twitter.com/MebFaber/status/870686357089165317)


---

# Technical choices

There is ideally an admin page to do the assignment uploads and to view (even play) results but that is actually not necessary for the MVP.

I have a few options here:
- VulcanJS
- KeystoneJS - dead, sadly
- Django - unexplored
- Meteor/base - i am most familiar and have made my own version, [Base-FCC](https://github.com/sw-yx/sw-yx.github.io/blob/master/_posts/2017-03-14-Finished-Base-FCC.md)
- react-starter-kit
- [allcount](https://allcountjs.com/) - no longer maintained but god damn its good 
- [Feathers](https://github.com/feathersjs/feathers) - very promising
- <https://github.com/apogeu/apogeu> poor docs but promising
- <http://sailsjs.com/> well supported, i need to learn this at some point

Angular solutions i dont consider:
- <https://github.com/meanjs/mean>
- <https://github.com/linnovate/mean>

---

after needless hours of surveying I think I am going to go with Base-FCC, but I definitely need to explore VulcanJS, Feathers, SailsJS, and then at some point Django.

# Building the MVP

Here is what I will need to do for the MVP.
- Display a list of podcast episodes for each regular user
- Click the episode to go to a single episode page where it shows rank and has a UI for adding start-end-summary blocks
- ICYMI bit down below on main page

# Nice to haves

- time saved progress bar
- ad blocks
- guest and other structured data
- user podcast submissions
- top5 voting system
- post submission discussion area
