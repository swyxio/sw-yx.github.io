---
layout: post
date: 2018-01-04
tags: advice
feelings: neutral
title: how to google your errors
comments: true
description: A blogpost prompted by a tweet "how to google your errors", xpost on dev.to
---

<https://dev.to/swyx/how-to-google-your-errors-2l6o>


So you're a newbie trying out a new technology, minding your own business when all of a sudden:

```bash
Uncaught (in promise) TypeError: _super.call is not a function
    at new ObservableQuery (b1f6a7d9f98d979758232d0dc3c394ce.js:26213)
    at QueryManager.watchQuery (b1f6a7d9f98d979758232d0dc3c394ce.js:27305)
    at b1f6a7d9f98d979758232d0dc3c394ce.js:27332
    at new Promise (<anonymous>)
    at QueryManager.query (b1f6a7d9f98d979758232d0dc3c394ce.js:27330)
    at ApolloClient.query (b1f6a7d9f98d979758232d0dc3c394ce.js:27981)
    at Object.require.2.react (b1f6a7d9f98d979758232d0dc3c394ce.js:29740)
    at newRequire (b1f6a7d9f98d979758232d0dc3c394ce.js:41)
    at require.39 (b1f6a7d9f98d979758232d0dc3c394ce.js:66)
    at b1f6a7d9f98d979758232d0dc3c394ce.js:71
```

You copy and paste the whole thing into Google, and a mere second later:

```
Your search - Uncaught (in promise) TypeError: _super.call is not a function - did not match any documents.

Suggestions:

    Make sure that all words are spelled correctly.
    Try different keywords.
    Try more general keywords.
    Try fewer keywords.
```

Or perhaps worse, you get tons of results, but none of them lead anywhere useful!

What now?

---

This was the question [posed today by Brittany Storoz](https://twitter.com/brittanystoroz/status/948671247734407169). As a recent Javascript newbie myself I am pretty familiar with this. Javascript is not a typesafe language so it is particularly prone to cryptic errors. Over the past year I have done my fair share of Googling The Error with various degrees of success, so I am going to lay down some thoughts here (as well as combining the thoughts of others). Here goes!

![Alt text of image](https://geekifyinc.com/wp-content/uploads/2017/09/IMG_0333-1280.jpg)

## 1. Don't Panic

[Don't Panic](https://en.wikipedia.org/wiki/Phrases_from_The_Hitchhiker%27s_Guide_to_the_Galaxy#Don't_Panic)! 😂 Googling Errors is a rite of passage. In fact, treat it as a welcome opportunity to _practice_ researching errors because this is a key skill in your career. The process will likely turn up other bits of knowledge you didn't know you were missing!

## 2. Rubber Duck It

[Rubber Duck Debugging](https://en.wikipedia.org/wiki/Rubber_duck_debugging) is a time-tested method of software engineering. Basically, try to explain to an inanimate object (preferably cute, squeakiness optional) in your own words what you are trying to do, and what the error is. Your natural language description of the problem is likely how others are also going to describe it, so plug that into Google and see what happens. You might be surprised! 🦆

## 3. Go in Expanding Circles (remove irrelevant info)

Your error likely has a lot of App-specific info in it. For example:

```bash
$ node_modules/.bin/parcel watch app/client/entry.html --out-dir public/dist
🚨  Cannot read property 'type' of undefined
    at Bundler.createBundleTree (/home/ben/projects/dg/node_modules/parcel-bundler/src/Bundler.js:373:52) 
    at Bundler.createBundleTree (/home/ben/projects/dg/node_modules/parcel-bundler/src/Bundler.js:412:12) 
    at Bundler.createBundleTree (/home/ben/projects/dg/node_modules/parcel-bundler/src/Bundler.js:412:12) 
    at Bundler.buildQueuedAssets (/home/ben/projects/dg/node_modules/parcel-bundler/src/Bundler.js:245:23)                                                                                                           
    at <anonymous>                                   
    at process._tickCallback (internal/process/next_tick.js:188:7) 
```

Plugging all that into Google isn't going to help! Why? Because you've left a bunch of assumptions in there that only you are going to use, for example `app/client/entry.html` and `/home/ben/projects/dg`. So removing them and decreasing the specificity of your search is likely to improve the search results to find other people who had similar issues to you.

James Roe phrased this better than I can:

 twitter 948706229785739264 

So if you have an error code, Google that. If that doesn't work, Google the error message. If that doesn't work, Google the library you're using. and so on!

## 4. Give more Context (add relevant keywords)

Errors are also notable for what they _leave out_. In this case, there is an implicit assumption that you, the developer, know what language or library you are using. But there's no way for Google to know that! Help Google along by adding the tech stack you are using as keywords, for example, `parceljs Cannot read property 'type' of undefined`. Remember your end goal is to hopefully land on a Github Issue, Stackoverflow question, or blog post so give Google all the context you can to help it surface the answer you need!

Incidentally, **version numbers** are a big part of context too. If you are experiencing an error on D3.js v4, then answers from D3.js v3 won't be very helpful! If you don't have version numbers or think your error is more generic, throwing a date limit on your query (Google lets you restrict it to the past 1 year) is likely to turn up more recent results which are likely to be more relevant.

## 5. Use advanced search operators

Google's searchbox packs a lot of power if you know how to wield it. Check out [this cheatsheet](https://support.google.com/websearch/answer/2466433?hl=en) (or [others](https://www.lifewire.com/advanced-google-search-3482174) like [this](https://bynd.com/news-ideas/google-advanced-search-comprehensive-list-google-search-operators/)) for advanced search tips to search only for quoted content, or for all search words, so on and so forth. (Thanks [Guinivere Saenger](https://twitter.com/guincodes/status/948740376705187840)). Special bonus, this increases your Google-fu for your non-dev life too!

## 6. Don't Google!

Google isn't the only search engine out there. There are plenty of venues where people go for help - like Stackoverflow and Github - and they have plenty fine search functionality too!

Google is also not the most dev-friendly search engine. Quote [Quincy Larson](https://www.quora.com/How-do-I-master-the-art-of-Googling-as-a-programmer-I%E2%80%99m-currently-learning-bash-and-the-IT-seems-like-too-much-memorisation/answer/Quincy-Larson):

*"Another thing a lot of new programmers don’t realize is that Google omits most non-alphanumeric characters from its queries. Symbols that programmers use all the time like !@#$%^& and * aren’t searched. Neither are (){}[]."*

So if you have a lot of symbols in your search, use [DuckDuckGo](https://duckduckgo.com/)!

## 7. Think about your biggest unknown

Programming works in terms of layers of abstraction. At the biggest of the big picture level there is the [OSI model](https://en.wikipedia.org/wiki/OSI_model), but if you are an app developer then your layers can be something like:

- Language
- Environment
- Framework
- Library
- App

What do I mean by biggest unknown? Well, if you are new to a **language**, then when you have errors you are most likely making a simple syntactical error because you are not yet familiar with all the grammar and edge cases a new language can bring. 

Confident it's not a language problem? Then proceed to the **environment**. Languages are evolving specifications and the environment in which you are coding matters a LOT as to how you can actually use the language. As a Javascript developer I always see [people getting tripped up by whether they can or cannot use ES6 syntax](https://forum.freecodecamp.org/t/changing-js-version/166453/3). So they get errors that they then Google and get totally confused by.

You've ruled out environment as the cause? Proceed to **framework**. So on and so forth. You can even proceed in reverse order if that suits you. Whatever works.

This step isn't meant to take long, I just erred verbose to emphasize what I mean by "Think about your biggest unknown". Your biggest unknown is your biggest source of risk, and therefore the first (but not only!) place to look when you have errors.

## 8. Read the Docs

If your error has to do with a specific Framework or Library, it is quite possible that there is some concept or language you may not know about that is the hidden cause of your error. Go read the docs, and look at the examples and understand how you might differ from that. Pay attention to the docs' **specific choice of words** and add that as keywords to your Google search to see if it surfaces any better results.

## 9. Reproduce the Error

Start a whole new project and make it very small so that you can isolate your Error. Copy over the bare minimum from your existing project that you will reproduce the Error, or just try to code it up from scratch without all the extra fluff your main project has. 

If you cannot reproduce the Error, you will have found a HUGE clue as to what is going on with your error. 

If you CAN reproduce the Error, that's also great, because that sets you up for...


## 10. Asking for Help

Post your error EVERYWHERE. Github, Stackoverflow, Reddit, Twitter, Slack/Discord communities, you name it. 

Your minimally reproducible sample, if you have it from step 8, will help go a long way for people to figure out what is going on. 

Furthermore, it will help FUTURE people who have YOUR exact error be able to Google to find YOU. If we are ever going to have Googlable error solutions, someone has to start the ball rolling!


# More Resources

- Michael Hoffman's presentation on [How To Be Stuck](http://code-worrier.com/how-to-be-stuck)
- Qunicy Larson's answer on [How do I master the art of Googling as a programmer?](https://www.quora.com/How-do-I-master-the-art-of-Googling-as-a-programmer-I%E2%80%99m-currently-learning-bash-and-the-IT-seems-like-too-much-memorisation/answer/Quincy-Larson)

Good luck!
