---
layout: post
title: adding dependency filter
date: 2017-07-11
tags: projects
feelings: determined
---

next feature. the dependency filter should be relatively easy to do but i will need to redo the inputs since I dont store them separately. tasks:

- remodel the input to save dependencies and devdependencies (let them join its ok).
- create an api to retrieve the dependencies (no need for count) `documents.dependencies`
- create the filtering api and modify in `documents.list`
- create the frontend to use the two api's - basically an autocomplete search bar to know what to include

---

remodel went well.

i was wrong to want to create a separate api to retrieve just the dependencies. [you cannot call the search on the same collection multiple times](https://docs.meteor.com/api/collections.html) which is mildly annoying (here is a SO question on [this big topic](https://stackoverflow.com/questions/19826804/understanding-meteor-publish-subscribe/21853298#21853298))

I am going to just do the next two points on the clientside.

---

2.5 hours later and this is done!

[boilerplate dependency search](https://twitter.com/swyx/status/884675880236306436)


I have also open sourced it <https://github.com/sw-yx/packageJason>

---

ok its 6.50pm and midday today i implemented the meteor dependency scraping just because i couldnt bear to go without it.

now i have to do boilerplate saving which is the second last "basic" feature I should do (the other feature being the parametric landing page that lets people just input any owner+repo combo and go straight to document adding)

---

i have implemented very basic hearting. it sits on the boilerplate collection instead of the user collection which i'm not super happy about but given that hearts are not private it is probably okay. (the difference is how you join the two in the frontend)

---

11pm. alright. now for the public api. this is a tricky thing to do because meteor doesnt do routes like a regular node/express app. Let's see:

- modify the front page to take owner/repo combinations without authentication
- clean up the score display to look nicer instead of some ugly numbers


2am. it took a long while to refactor the edit boilerplate page to have a separate boilerplateStats component without having the state communication messed up.

Now to get it onto the front page with owner/repo combinations. I can also knock out the github URL copy paste.
