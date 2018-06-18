---
layout: post
date: 2018-06-17
tags: gatsby
feelings: neutral
title: abandoned gatsby feature request - imageSharp
comments: true
description: somethign i decided not to request
---

what follows is something i nearly sent in, but reconsidered. i think i will just stick to using allFiles for all the things. but it doesn't feel good.

----
## Summary

when using `imageSharp` i see a lot of writing of glue code just to match up the image with the associated file or other content. this feels like a very config heavy experience that probably should be easier. see: https://github.com/gatsbyjs/gatsby/issues/1749

what if instead of modifying `gatsby-node.js` every time we add an image, we make the `imageSharp` query usable purely within GraphQL?

I think the usability of gatsby-transformer-sharp'a `imageSharp` type could be greatly improved by exposing the metadata of the related file the image comes from. I want to be able to do this:

```graphql
    allImageSharp(filter: {
      relativePath: {
        regex:"/mypath/"
      }
    }) {
      edges {
        node {
          fixed {
            ...GatsbyImageFragment
          }
        }
      }
    }
```

instead of having to glue `AllFile` and `allImageSharp` together every time, or use `AllFile` and `child :(
