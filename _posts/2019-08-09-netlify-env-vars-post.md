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

We usually use Environment Variables as (global) variables, but did you know they can totally configure your environments as well?

## Secret Globals, Global Secrets

Environment Variables occupy an interesting space in our collective mindset. They are [more secure than plaintext](https://stackoverflow.com/questions/12461484/is-it-secure-to-store-passwords-as-environment-variables-rather-than-as-plain-t), but also [Considered Harmful](https://news.ycombinator.com/item?id=8826024). As globals that we call by a more polite name, they are useful in small doses, and therefore have a funny way of spreading: They were [introduced to JavaScript by the Express backend framework](https://github.com/expressjs/express/commit/03b56d8140dc5c2b574d410bfeb63517a0430451) in 2010 and now [drive entire library build processes and development experience tradeoffs](https://overreacted.io/how-does-the-development-mode-work/) for all the major frontend frameworks (you can [make your own dev-only warnings with TSDX](https://github.com/palmerhq/tsdx#development-only-expressions--treeshaking)!).

### Making use of Environment Variables on the Frontend

Because unintentionally leaking API tokens to clients is a Very Bad Thing, we have evolved best practices like prefixing environment variables with [`REACT_APP_`](https://facebook.github.io/create-react-app/docs/adding-custom-environment-variables#docsNav) or [`VUE_APP_`](https://cli.vuejs.org/guide/mode-and-env.html#environment-variables) or [`GATSBY_`](https://www.gatsbyjs.org/docs/environment-variables/#client-side-javascript). If you aren't seeing an environment variable that you expected to see, this is probably the cause.

For hosted platforms like Netlify, we can also [use environment variables to communicate from the build environment to the frontend](https://www.netlify.com/docs/continuous-deployment/?utm_source=blog&utm_medium=scotchio&utm_campaign=devex#environment-variables), for the frontend to configure itself based on that information.

For example, Netlify automatically sets up [Continuous Deployments](https://www.netlify.com/docs/continuous-deployment/?utm_source=blog&utm_medium=scotchio&utm_campaign=devex) when you link your Git repo. Imagine you are hosting documentation for open source. People often want to "view source" for your site. Instead of hardcoding the Git repo, you can simply reference `process.env.REPOSITORY_URL` in your build process, and pass it to [your static site generator of choice](https://staticgen.com).

Granted you're not going to change your Repo URL that often. But this is one of many examples. For example, if you use [Deploy Previews](https://www.netlify.com/blog/2016/07/20/introducing-deploy-previews-in-netlify/) to work in progress PR's with collaborators and clients, you can check [`process.env.CONTEXT === 'deploy-preview'`](https://www.netlify.com/docs/continuous-deployment/?utm_source=blog&utm_medium=scotchio&utm_campaign=devex#environment-variables) and place a nice banner disclaiming that is not the production version of your site.

This kind of workflow with Environment Variables are key to how [Netlify's Split Testing](https://www.netlify.com/docs/split-testing/?utm_source=blog&utm_medium=scotchio&utm_campaign=devex) feature works. By basing your A/B test on git workflow, there is no A/B testing API to learn - just make a new git branch and send traffic that way. On the frontend, if you check the `BRANCH` context variable (again, with the caveat that some frontend frameworks may make you jump through some hoops to send this to the frontend).

## Using Environment Variables to Vary your Environment

However, intelligent runtime environments can also configure themselves around your variables, which is a very nice way to offer backward compatibility or flagged functionality for different codebases that all work on the same exact platform. Let's explore ideas for [how this can work on Netlify](https://www.netlify.com/docs/build-settings/?utm_source=blog&utm_medium=scotchio&utm_campaign=devex#build-environment-variables):

### Setting your Node Versions in Build Process and in Serverless Functions

To set your Node Version, you, well, [set `NODE_VERSION`](https://www.netlify.com/docs/continuous-deployment/?utm_source=blog&utm_medium=scotchio&utm_campaign=devex#node)! 🤯 

This variable only affects your build environment. By default it is `v10.16.2` at time of writing, but if, for example, your build process requires node 8 or node 12, you can set it to `8` or `12` accordingly. You can also [set a specific release version](https://www.netlify.com/docs/continuous-deployment/?utm_source=blog&utm_medium=scotchio&utm_campaign=devex#node) or use `.nvmrc` if you like.

The other important environment you may wish to vary is your serverless functions. [Netlify Functions](https://www.netlify.com/docs/functions/?utm_source=blog&utm_medium=scotchio&utm_campaign=devex) are a nice development experience layer atop AWS Lambda. By default, Node Netlify Functions run in Node v8.10, but you can set [`AWS_LAMBDA_JS_RUNTIME`](https://www.netlify.com/docs/functions/?utm_source=blog&utm_medium=scotchio&utm_campaign=devex#javascript-runtime-settings) to `nodejs10.x` ([or any other valid AWS Lambda runtime name](https://docs.aws.amazon.com/lambda/latest/dg/current-supported-versions.html)) to run in Node 10 for newer language features and better performance! (You can also [set Go function environment versions with the build image](https://www.netlify.com/docs/functions/?utm_source=blog&utm_medium=scotchio&utm_campaign=devex#building-go-functions-with-netlify-s-continuous-deployment).)

You can also set other important command line and environment versions as needed, for example `YARN_VERSION`, `YARN_FLAGS`, `NPM_VERSION`, and `NPM_TOKEN` (handy if you use a private npm instance).

## Power User Features

There are even more environment variables available meant for introspection, debugging, and power users, for example the `HOME` directory and `GOCACHE` directory environment variables. 

You can use variables like these to persist files between builds, to skip rebuilding files that already exist, and thereby massively speed up your static site builds by only building incremental new/changed pages. Check out [`cache-me-outside`](https://github.com/DavidWells/cache-me-outside#1-configure-the-files-you-want-to-cache) for an example of how to do this.

You can reference [the Big Fat List of Environment Variables here](https://gist.github.com/sw-yx/c53634e7e63f0015e43c16bc26832283), and it will be fully documented in an upcoming version of the Netlify Docs.

What else can you do with these environment variables that we aren't thinking about? [Let me know](https://twitter.com/swyx)!
