---
layout: post
title: Creating Base-FCC
date: 2017-03-11
tags: freecodecamp
categories: determined fat
---

So yesterday was a good reset. My assignment today is:

- modify Documents page to My Documents
- modify main page to All Documents (viewable unauthenticated)

I think I also need to modify the editing API to search for documents by `_id` instead of by title as this can mess up when titles are not unique. (edit: not necessary. now that i am rereading the code i have no idea where i got this impression)

---

5pm here. i have just discovered that <https://github.com/rgstephens/base> does a lot of what I just spent yesterday doing. but better of course. I have forked that and am going to keep developing on top of this. A tip to forking a single branch: <http://stackoverflow.com/questions/1778088/how-to-clone-a-single-branch-in-git> where you have to do `git clone <url> --branch <branch> --single-branch [<folder>]`

6pm here. so now we have restricted access documents i just need to modify the Index page to show All Documents.

modifications made:

- `/imports/api/documents/server/publications.js` to create "documents.listAll"
- create `/imports/ui/containers/AllDocuments.js` to call "documents.listAll"
- modify `/imports/ui/pages/Index.js` to display `AllDocuments`
