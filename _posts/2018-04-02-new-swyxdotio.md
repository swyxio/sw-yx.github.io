---
layout: post
date: 2018-04-02
tags: misc
feelings: neutral
title: new swyxdotio
comments: true
description: new swyxdotio
---

i tried using https://github.com/konsumer/gatsby-starter-bootstrap-netlify again and i keep running into this stupid error:

```
8:05:12 PM: error Building static HTML for pages failed
8:05:12 PM: See our docs page on debugging HTML builds for help https://goo.gl/yL9lND
8:05:12 PM:   15 | export default function Template({ data }) {
8:05:12 PM:   16 |   const { markdownRemark: post } = data;
8:05:12 PM: > 17 |   const related = post.frontmatter.related ? post.frontmatter.related.map(r => findNode(r.post, data)) : [];
8:05:12 PM:      |                        ^
8:05:12 PM:   18 |   return (
8:05:12 PM:   19 |     <div>
8:05:12 PM:   20 |       <Helmet title={`Blog | ${post.frontmatter.title}`}>
8:05:12 PM: 
8:05:12 PM:   WebpackError: Cannot read property 'frontmatter' of null
8:05:12 PM:   
8:05:12 PM:   - blog.js:17 Template
8:05:12 PM:     src/templates/blog.js:17:24
8:05:12 PM:   
8:05:12 PM:   - ReactCompositeComponent.js:306 ReactCompositeComponentWrapper._constructComp    onentWithoutOwner
8:05:12 PM:     ~/react-dom/lib/ReactCompositeComponent.js:306:1
8:05:12 PM:   
8:05:12 PM:   - ReactCompositeComponent.js:282 ReactCompositeComponentWrapper._constructComp    onent
8:05:12 PM:     ~/react-dom/lib/ReactCompositeComponent.js:282:1
8:05:12 PM:   
8:05:12 PM:   - ReactCompositeComponent.js:185 ReactCompositeComponentWrapper.mountComponent    ~/react-dom/lib/ReactCompositeComponent.js:185:1
8:05:12 PM:   
8:05:12 PM:   - ReactReconciler.js:43 Object.mountComponent
8:05:12 PM:     ~/react-dom/lib/ReactReconciler.js:43:1
8:05:12 PM:   
8:05:12 PM:   - ReactMultiChild.js:234 ReactDOMComponent.mountChildren
8:05:12 PM:     ~/react-dom/lib/ReactMultiChild.js:234:1
8:05:12 PM:   
8:05:12 PM:   - ReactDOMComponent.js:657 ReactDOMComponent._createContentMarkup
8:05:12 PM:     ~/react-dom/lib/ReactDOMComponent.js:657:1
8:05:12 PM:   
8:05:12 PM:   - ReactDOMComponent.js:524 ReactDOMComponent.mountComponent
8:05:12 PM:     ~/react-dom/lib/ReactDOMComponent.js:524:1
8:05:12 PM:   
8:05:12 PM:   - ReactReconciler.js:43 Object.mountComponent
8:05:12 PM:     ~/react-dom/lib/ReactReconciler.js:43:1
8:05:12 PM:   
8:05:12 PM:   - ReactMultiChild.js:234 ReactDOMComponent.mountChildren
8:05:12 PM:     ~/react-dom/lib/ReactMultiChild.js:234:1
8:05:12 PM:   
8:05:12 PM: 
```

so i have wasted enough hours at this point on netlifycms that its too janky of an integration. the config.yml and graphql and the react have to all line up.
