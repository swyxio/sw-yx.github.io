---
layout: post
date: 2018-12-03
tags: design
feelings: happy
title: design4devs
comments: true
description: notes from sarahs workshop
---



Design is a sore point for me so I am trying to get a bit better.

Here are notes from [Sarah Drasner's Design workshop](https://github.com/sdras/design-for-developers).

- Layout ([Slides](https://github.com/sdras/design-for-developers/blob/master/slides-pdf/Des4Dev2.pdf))
  - Balance: Break symmetry to do call to action
  - Shape: Use circles and other shapes to draw the eye. Remember to put circles a little "outside of lines". Diagonals are dynamic.
  - Scale: Scale an image and put it in background to make even a boring table interesting.
  - Grid: Practice drawing anchor lines on things. Use CSS Grid/Flexbox.
  - Also: Know when to break lines/grid, eg using `clip-path` and `shape-outside`. [Clippy CSS clip-path maker](https://bennettfeely.com/clippy/), [Handy guide to CSS/SVG masks](https://codepen.io/yoksel/full/fsdbu/) (and [when to use them](https://css-tricks.com/masking-vs-clipping-use/))
  - Book recommendation: **Making and Breaking the Grid**
  - Inspiration: https://gridbyexample.com/patterns/
  - Tools: PS, Illustrator, Sketch.
- Color ([Slides](https://github.com/sdras/design-for-developers/blob/master/slides-pdf/Des4Dev3.pdf))
  - Types of Combinations:
    - Monochromatic (clean, elegant eg shades of red, or gray)
    - Analogous (more dynamic, neighbor colors eg red and violet)
    - Complementary (cool vs warm color, eg blue and orange)
    - Triadic (3 equal spaced colors, vibrant, rich, eg red, blue, yellow)
    - Duotone (good for shadows)
  - HSLA is best
  - Check a11y contrast:
    - http://jxnblk.com/colorable/demos/text/
    - http://accessible-colors.com/
    - http://www.brandwood.com/a11y/
  - Process
    - Anchoring
    - Get your greys
    - Gather accents
  - Color picker tools
    - Kuler [color.adobe.com](https://color.adobe.com)
    - [Coolors](https://coolors.co)
    - [Paletton](https://paletton.com)
    - [Palettab](https://palettab.com)
    - [UI Gradients](https://uigradients.com)
    - [Cool Backgrounds](https://coolbackgrounds.io/)
    - [Web Gradients](https://webgradients.com/)
    - [CSS Gradient](https://cssgradient.io/)
    - [Gradient Animator](https://www.gradient-animator.com/)
    - Adobe Capture - take a "mood" on phone
    - [CSS Named colors game](http://codepo8.github.io/css-colour-names/)
  - Inspiration
    - https://dribbble.com/uixninja
    - Dribble - steal palette or search by color
  - More info
    - https://medium.com/@JustinMezzell/how-i-work-with-color-8439c98ae5ed
    - https://www.smashingmagazine.com/2012/10/the-code-side-of-color/
    - https://www.smashingmagazine.com/2010/02/color-theory-for-designer-part-3-creating-your-own-color-palettes/)
- Typography ([Slides](https://github.com/sdras/design-for-developers/blob/master/slides-pdf/Des4Dev4.pdf))
  - Terminology
    - serif vs sans serif
    - proportional vs monospaced
    - kerning (`letter-spacing: 0.03em;`) & leading (space between baselines e.g. `line-height: 1.4; vertical-align: baseline;`)
    - Widows and Orphans
    - Ligatures
  - Pair fonts
    - One display or serif
    - One sans-serif
  - Your reader is different from you:
    - shorter attn span
    - lower interest in topic
    - persuadable by other opinions
    - doesnt share same goals as you
  - Other rules
    - line length 45-90 chars
    - line height should respect "typographic color" (how does it look blurred?)
  - Perf:
    - https://www.zachleat.com/web/five-whys/
    - https://github.com/Munter/subfont
  - Tools:
    - Picking fonts
      - Free: https://fonts.google.com/
      - Free: https://www.fontsquirrel.com/
      - Spendier: Fonts.ocm, Hoefler&Co
    - Pairing fonts
      - manually https://fonts.google.com/
      - AI: https://fontjoy.com/
    - Responsive/Fluid Typography
      - https://twitter.com/MikeRiethmuller
  - More info
    - https://typographyforlawyers.com/
    - Typography terms cheatsheet: https://crmrkt.com/MNdAPQ
- Inspiration ([Slides](https://github.com/sdras/design-for-developers/blob/master/slides-pdf/Des4Dev5.pdf))
  - http://give-n-go.co/ Dribble shots coded up
  - [Shutterstock UI kits](https://www.shutterstock.com/search/ui+kit) off the shelf
  - [Codepen Design Patterns](https://codepen.io/patterns/)
  - [Codrops](https://tympanus.net/Development/PageFlipLayout/)
- Images ([Slides](https://github.com/sdras/design-for-developers/blob/master/slides-pdf/Des4Dev6.pdf))
  - Free
    - Unsplash
    - Google Image
    - Freepik
    - freeimages
    - pexels
  - Lowcost
    - creative market
    - adobe stock
  - Costly
    - shutterstock
    - istockphoto
  - different ways to do images
    - img tag
    - bg source
    - inline svg
    - sprites
    - full page bg img https://css-tricks.com/perfect-full-page-background-image/
  - svg tricks
    - Scaling svg: [preserveaspectratio = none](https://css-tricks.com/scale-svg/)
  - Tools
    - https://tinyjpg.com/
    - https://tinypng.com/
    - webpack plugins
    - https://jakearchibald.github.io/svgomg/
    - https://codepen.io/shshaw/full/LVKEdv
- Prototyping ([Slides](https://github.com/sdras/design-for-developers/blob/master/slides-pdf/Des4Dev7.pdf))
  - Use Loaders to reduce perceived wait
  - Feature reqs/user stories: as **_(specific user)_** I expect that \_**_(the task)_** so that **\_**(expected outcome)**\_**
  - [Storyboard/story map](https://medium.com/design-story/story-map-3cc64033128e)
  - Make End to end website flows
  - Drawing thumbnails
  - Storyboarding
  - Low-fi prototypes with just gray things [like this](https://codepen.io/yusufbkr/pen/ORBArk)
  - High-fi prototype that works
  - Screenshot and abs position things
  - Motion: Transforms

----

Animation:

- http://animista.net
- https://daneden.github.io/animate.css/
- http://tobiasahlin.com/spinkit/
- http://www.transformicons.com/docs.html

Design inspo

- https://tympanus.net/Development/DraggableDualViewSlideshow/

Bonus toys

- https://leaverou.github.io/css3patterns/
