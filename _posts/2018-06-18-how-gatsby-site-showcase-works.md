---
layout: post
date: 2018-06-18
tags: gatsby
feelings: neutral
title: how gatsby site showcase works
comments: true
description: a walkthrough for kurt and a braindump for myself
---

relevant links:

- preview: <https://deploy-preview-5524--gatsbyjs.netlify.com/showcase/>
- PR: <https://github.com/gatsbyjs/gatsby/pull/5524> (rebased to <https://github.com/gatsbyjs/gatsby/pull/5941>)
- issue: <https://github.com/gatsbyjs/gatsby/issues/4392>

# backend/gatsby

we use a new plugin called [gatsby-transformer-screenshot](https://www.npmjs.com/package/gatsby-transformer-screenshot). this does a number of things:

- pulls data from a special file `docs/sites.yml` which contains all the sites for our showcase (there is some debate on best format for keeping the site data, we went with this so people can edit and we can keep a history)
- which creates an `allSitesYaml` field in the graphql query
- takes screenshots of all the sites and does image processing on them. in dev this seems to be done locally and takes a long time (for now). in prod this is done with an aws lambda function with caching for 2 weeks.  to solve the long build issue we are probably going to use [gatsby-plugin-netlify-cache](https://github.com/gatsbyjs/gatsby/pull/5862)
- the `sitesYaml` field bundles BOTH the screenshot data (in a `childScreenshot` field) AND the site metadata from `sites.yml` so it makes for a fairly easy query when rendering.

# layout

the layout is a little complex. the site showcase is "just" a master-detail view, but we also have a detail modal and mobile views.

https://github.com/gatsbyjs/gatsby/pull/5941/files#diff-d5343e3482876401d8bd4358271c3d9d

`www/src/components/layout.js` introduces a modal which is triggered when you pass a react-router state which has a state of `isModal: true`. when you navigate straight to a detail page this state won't be set, so you don't get the modal view.

vertical rhythm comes from [typography.js](https://github.com/gatsbyjs/gatsby/pull/5941/files#diff-4b4a6a4395be06bd74e68e1eddff59fbR7) and the responsiveness (desktop, tablet) comes from the site's [presets.js](https://github.com/gatsbyjs/gatsby/pull/5941/files#diff-d18b9fe9c1cb9db5af8ae610fee356eb)

# master view

the master view has a search filter which we use `fuse.js` for. there's some debate about this as typing more characters into "search sites" can have MORE instead of LESS search results because of the fuzzy search. we have chosen to go with it for now but may need to tweak the fuse.js options or switch to other formats (like algolia)

the category filter is "subtractive" instead of "additive". i.e. if 2 categories are picked there should be equal or less results than 1 category picked, not more results. that was another decision taking during building.

"Submit your Site" simply links to an issue template which will be created/maintained by shannon. it has instructions on how to format things for `sites.yml`.

as already discussed in "layout" clicking on any site in the master view opens the detail view in a Modal state [like this](https://github.com/gatsbyjs/gatsby/pull/5941/files#diff-c8c93d3f870b70bb6a9dd97cffddb96aR85)

featured packages are arbitrarily picked by us.


# detail view

so the detail view has a modal version and a full-page version. the other noteworthy thing is the concept of a "next" and a "previous" site. in practice this is just traversing the order of sites as specified in `sites.yml`. this is done through N+1 and N-1 queries done with [getNext()](https://github.com/gatsbyjs/gatsby/pull/5941/files#diff-4b4a6a4395be06bd74e68e1eddff59fbR47) and `getPrevious()`.
