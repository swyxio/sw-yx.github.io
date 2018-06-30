---
layout: post
date: 2018-06-29
tags: staticsitegen 
feelings: neutral
title: staticsitegen project
comments: true
description: THIS IS THE CRAZIEST INTERVIEW EVER
---


this is the craziest interview ever!!!!!!!!!!!!

netlify build hooks
typically
outgoing hooks

zapier hooks

---

sources:
staticgen.com
headlesscms.org

daily hook in zapier to trigger build
- stars and forks and tweets
- json file in a gist

contentful


build pipline
core transformation
get the tweet data
do all this 

---

---
title: A Glance through Docusaurus, Docz, and React-Static
published: true
description: a survey of new documentation/static site generators
tags: react
---

DocGens/[SSGs](https://www.staticgen.com/) are hard to evaluate because they all look similar on the surface and you have to really invest time before you understand important features and differences between them. I know [Gatsby fairly well](https://gist.github.com/sw-yx/09306ec03df7b4cd8e7469bb74c078fb) and have used Hugo/Jekyll and wanted to check out some of the new generation of React based site generators that have recently come out (yes 2 of the 3 have a specific documentation focus, I don't mind).

# Docusaurus

[Docusaurus](https://docusaurus.io/) is very focused on the docs usecase and is used for docs for every major Facebook project -except- React, which uses Gatsby. The install and spin up is very fast, but basic demo doesn't impress at first glance because it literally focuses on Markdown for blog and docs with some components in `/core` and pages in `/pages`. Comparable to a constrained Gatsby. The `siteConfig.js` and `sidebars.json` choices to configure things felt a bit ugly/arbitrary but unimportant. 

**Killer Features**: I think where Docusaurus shines is in [Search](https://docusaurus.io/docs/en/search), [i18n/l20n](https://docusaurus.io/docs/en/translation), and [versioning](https://docusaurus.io/docs/en/versioning). Here it benefits from having a very focused usecase and opinionated choices about partner/problem domain - it is as minimal config as it gets. I think the Versioned docs are a killer feature.

Docusaurus also ships with some [provided components](https://docusaurus.io/docs/en/api-pages#provided-components) that are helpful in docs, and ships with some inbuilt theming (basically colors that can be set through `siteConfig.js`). Prismjs is also included for zero-setup syntax highlighting. An interesting model that provides a lot of convenience without restricting you if you want to add custom React components or CSS.

The deploy story is also a nice touch, PARTICULARLY providing a [publish-gh-pages](https://docusaurus.io/docs/en/publishing#using-github-pages) script, which from experience is a sore pain point. Of course, Netlify is also present. If you `yarn build` and check out the build folder you can even see they include a `sitemap.xml` for you which is super sweet. (The blog also comes with `atom.xml` and `feed.xml` for RSS).

Docusaurus itself is impressively well documented, which I guess maybe isn't a surprise but I appreciate nonetheless.

**Cons?**: I honestly struggle to come up with real cons. There is no plugin system so it's not extensible. The config/sidebars is a bit unintuitive, and if you need to use more build processes like SASS you're on your own, but honestly those are just nitpicks. Extremely impressed.

**Makers**: Docusaurus is made and sponsored by Facebook - it seems Eric Nakagawa and Joel Marcey led the charge and you can check out the rest of the team via [their Twitter](https://twitter.com/docusaurus/following) or [their commits.](https://github.com/facebook/Docusaurus/graphs/contributors).

---

# Docz

[Docz](https://www.docz.site/)'s demo is slick - seriously go watch the video. The value proposition is immediately obvious - you can drop this in to an existing project to generate documentation by colocating [`mdx` files](https://www.docz.site/introduction/writing-mdx) next to your JSX files. 

This does mean that Docz is more constrained to the React ecosystem than Docusaurus is (though they are working on Preact/Vue support), but again that tradeoff enables the (optional) ability to use MDX, which is very nice. Together with [the supplied components APIs](https://www.docz.site/documentation/components-api), in particular `Playground` and `PropsTable` components (which are great ideas!!) it makes documenting a React component library extremely ridiculously easy. But it doesn't do much else than that for the time being :)

**Killer Features**: Zero config MDX docs, and the `Playground` and `PropsTable` components with Typescript support.

The ability to spin up the docz server just by doing `yarn docz dev` *without even adding in an npm script* is a very very nice touch. I didn't even know you could do that!

I feel I am not the target audience for Docz because about half the docs on Docz are spent on [Theming](https://www.docz.site/documentation/creating-themes), which I don't particularly care about. It's cool if you need it I guess.

The plugin story has a lot of potential, a bunch of well documented lifecycle hooks already exist. Although there aren't many plugins to boast of yet this project is still very young (only announced on Jun 11 2018).

A very nice touch is the console output - this thing looks designer - very sexy.

**Cons?**: It really is best suited for INTERNALLY documenting a React (in JS or Typescript) component library. Similar to Storybook it doesn't help you generate a nice looking landing page or blog or anything, it is literally a bunch of MDXes put together. So it's my top choice if I'm doing that, but not anything else. `yarn docz build` also doesn't build a static site, it just makes a production JS bundle to serve with `index.html`. (I didnt know this before i included Docz in the mix, too late now).

The nice thing about this extreme focus on generating docs for JSX components is that Docz can actually coexist with other static site gens and you can still get value out of it. So say your Docusaurus site has a reusable component library; you can use Docz to help develop and keep that in check.

**Makers**: [Pedro Nauck](https://twitter.com/pedronauck?lang=en) who has done a bunch of other interesting things like [react-adopt](https://github.com/pedronauck/react-adopt). Definitely one to keep an eye on.

---

# React-Static

[React-Static](https://react-static.js.org/) is in my mind -the- current Gatsby alternative, so I expect more degrees of freedom and perhaps complexity than the above two. (It is also older by a bit, and already at v6.0.0)

First thing to notice is the [stepped CLI experience](https://react-static.js.org/docs/#quick-start). This is a small touch of course but still a level up from Gatsby. There are a bunch of [super interesting offered templates](https://react-static.js.org/docs/#examples-and-templates) right within the CLI, which is nice. My eye was drawn to the "animated routes" one since I know that is a struggle with SSGs.

As someone who's contributed to Gatsby's docs, I'll just come right out and say it: React-Static's docs are super well written, particularly with [the introduction of core concepts](https://react-static.js.org/docs/concepts#overview). Dynamic routing is also [easier](https://react-static.js.org/docs/concepts#non-static-routing). [Template generation](https://react-static.js.org/docs/concepts/#pagination) feels somewhat similar to Gatsby's templates inside `gatsby-node.js` but perhaps with fewer files to wrangle. GraphqQL is no longer a first class citizen and I will have to play around with the data fetching to see how I feel about it.

**Killer feature**: It's hard to articulate this but React-Static is notable for what it -lacks- which are counterintuitively good features. [all data fetching](https://react-static.js.org/docs/concepts#code-data-and-prop-splitting) is done within [`static.config.js`](https://react-static.js.org/docs/config), no magic graphql components, heck no graphql at all. data comes in with the integrated [RouteData](https://react-static.js.org/docs/components#routedata). there aren't a bunch of other files to deal with, and much fewer lifecycle hooks. It doesn't support plugins, so presumably to "plug in" you just write something compatible with `static.config.js`. All in all, there is a lot less *magic*, and I never knew how much I appreciated that til now. Who knows if this is the right level of but it certainly feels like the appropriate balance of simplicity and functionality for the 80% of use cases.

Nice touch: [one-line config for Preact](https://react-static.js.org/docs/concepts#using-preact-in-production), [Components](https://react-static.js.org/docs/components) (react router components enhanced with static site concerns including data and scrollto) and [Methods](https://react-static.js.org/docs/methods)

**Cons?**: The lack of a plugin ecosystem means more custom work has to be done to setup/configure data sources to provide data for page generation. The starters/templates amount to a bunch of boilerplates which isn't very composable or reusable. I guess the tradeoff of having less magic is more work to make up for it.

**Makers**: Tanner Linsley of [Nozzle.io](https://nozzle.io/about). Origins are important and you should definitely check out Tanner's [Next vs Gatsby article](https://medium.com/@tannerlinsley/%EF%B8%8F-introducing-react-static-a-progressive-static-site-framework-for-react-3470d2a51ebc) to understand why he made React-Static. (much more indepth than my superficial survey - for example he pays attention to the JS shipped per-route, something I definitely didnt look into)

---

# Overall

I started out this research with only a vague idea of what each do, and I believe it would be irresponsible to pick any one of these over the other. They are apples and oranges and tomatoes, and they all address different problems in unique and interesting ways. The world is wide enough for a diversity of solutions to the wide array of problems, and I welcome these additions to my toolkit. 

I will note that probably the biggest positive surprise to me was Docusaurus, as I had no idea how easy some of these difficult problems in documentation are in Docusaurus.
