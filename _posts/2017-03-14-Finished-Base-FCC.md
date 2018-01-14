---
layout: post
title: Finished Base+FCC
date: 2017-03-14
categories: freecodecamp meteor
tags: productive
---

here we go again. I want to finish up my `base-fcc` boilerplate and then actually get started solving pinterest. 

I have been facing issues with CSP on fonts. this seems to be the hint: <https://themeteorchef.com/tutorials/using-the-browser-policy-package>. I fixed it by going into the `./meteor` folder and editing `browser-policy-common.js` to include:

```
var contentTypeOptions = BrowserPolicy.content &&
        BrowserPolicy.content.allowOriginForAll( 'fonts.gstatic.com' ) &&
        BrowserPolicy.content._xContentTypeOptions();
```

---

Other Sources

- main (with outdated buildpack): <https://medium.com/@leonardykris/how-to-run-a-meteor-js-application-on-heroku-in-10-steps-7aceb12de234#.czyfsba0w>
- probably the one to use going forward: <https://www.coshx.com/blog/2016/08/19/how-to-deploy-a-meteor-1-4-app-to-heroku/>
- also usable: <https://medium.com/@gge/deploy-a-meteor-1-3-application-to-heroku-cda1f68ca20a#.qtnc1pl0yhttps://medium.com/@gge/deploy-a-meteor-1-3-application-to-heroku-cda1f68ca20a#.qtnc1pl0y>
- myself: <https://github.com/sw-yx/sw-yx.github.io/blob/7de78a26e429b9f35aa051a1af211d218fd7af49/_posts/2017-02-13-Cliff-of-Confusio.md


problems I ran into:

- https://github.com/jordansissel/heroku-buildpack-meteor is now outdated i think. use https://github.com/AdmitHub/meteor-buildpack-horse

---

I am done. [https://base-fcc.herokuapp.com/](https://base-fcc.herokuapp.com/) is live and the source code is [here](https://github.com/sw-yx/base-fcc). Hopefully it helps others and my future self

I think there is one minor thing I have neglected which is this user story:

- User Story: As an unauthenticated user, I can browse other users walls of images.

I will try to implement this before i go to bed


---

Inspirations (VR Edition)
---

- <https://facebookincubator.github.io/react-vr/>
- <https://aframe.io/>
