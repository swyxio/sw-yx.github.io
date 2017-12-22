---
layout: post
date: 2017-06-18
title: Adopting Pup
categories: pup
tags: determined
---

I realized today that Meteor Chef has migrated to a new version (this week!) called [Pup](http://cleverbeagle.com/pup). It looks like it has some sensible defaults so I am downloading it now and checking it out.

---

# Meteor is fanatical about security

a small anecdote as I am still working on adapting Pup. I was trying to rapidly develop and that meant adding a ton of fields to my documents for this CRUD application. Meteor checks each and every single field coming in presumably for injection and type safety so when you try to save it without checking each field it fails. I lost about an hour because there is a case in which Meteor fails SILENTLY (edit: silent failure happens when you try to insert a document that does not have all the required fields)

```javascript
import { Meteor } from 'meteor/meteor';
import { check } from 'meteor/check';
import Documents from './Documents';
import rateLimit from '../../modules/rate-limit';

Meteor.methods({
  'documents.insert': function documentsInsert(doc) {
    // check(doc, { // commenting this out causes the second error below
    //   title: String,
    //   url: String,
    //   guid: String, // not including an exhaustive list of fields causes the first error below
    // });

    try {
      return Documents.insert({ owner: this.userId, ...doc });
    } catch (exception) {
      throw new Meteor.Error('500', exception);
    }
  },  

... and so on
```

command line output:

```
=> Meteor server restarted
I20170619-01:28:05.585(-4)? Exception while invoking method 'documents.insert' Error: Match error: Unknown key in field itunesImage
I20170619-01:28:05.587(-4)?     at exports.check (packages/check.js:56:15)
I20170619-01:28:05.588(-4)?     at [object Object].documentsInsert (imports/api/Documents/methods.js:8:5)
I20170619-01:28:05.588(-4)?     at packages/check.js:129:16
I20170619-01:28:05.589(-4)?     at [object Object]._.extend.withValue (packages/meteor.js:1122:17)
I20170619-01:28:05.589(-4)?     at Object.exports.Match._failIfArgumentsAreNotAllChecked (packages/check.js:128:41)
I20170619-01:28:05.590(-4)?     at maybeAuditArgumentChecks (packages/ddp-server/livedata_server.js:1734:18)
I20170619-01:28:05.590(-4)?     at packages/ddp-server/livedata_server.js:719:19
I20170619-01:28:05.591(-4)?     at [object Object]._.extend.withValue (packages/meteor.js:1122:17)
I20170619-01:28:05.591(-4)?     at packages/ddp-server/livedata_server.js:717:40
I20170619-01:28:05.591(-4)?     at [object Object]._.extend.withValue (packages/meteor.js:1122:17)
I20170619-01:28:05.592(-4)? Sanitized and reported to the client as: Match failed [400]
I20170619-01:28:05.593(-4)? 
W20170619-01:28:59.217(-4)? (STDERR) Note: you are using a pure-JavaScript implementation of bcrypt.
W20170619-01:28:59.218(-4)? (STDERR) While this implementation will work correctly, it is known to be
W20170619-01:28:59.218(-4)? (STDERR) approximately three times slower than the native implementation.
W20170619-01:28:59.219(-4)? (STDERR) In order to use the native implementation instead, run
W20170619-01:28:59.220(-4)? (STDERR) 
W20170619-01:28:59.220(-4)? (STDERR)   meteor npm install --save bcrypt
W20170619-01:28:59.221(-4)? (STDERR) 
W20170619-01:28:59.221(-4)? (STDERR) in the root directory of your application.
=> Meteor server restarted
I20170619-01:29:07.939(-4)? Exception while invoking method 'documents.insert' Error: Did not check() all arguments during call to 'documents.insert'
I20170619-01:29:07.940(-4)?     at [object Object]._.extend.throwUnlessAllArgumentsHaveBeenChecked (packages/check.js:484:13)
I20170619-01:29:07.940(-4)?     at Object.exports.Match._failIfArgumentsAreNotAllChecked (packages/check.js:132:16)
I20170619-01:29:07.940(-4)?     at maybeAuditArgumentChecks (packages/ddp-server/livedata_server.js:1734:18)
I20170619-01:29:07.941(-4)?     at packages/ddp-server/livedata_server.js:719:19
I20170619-01:29:07.941(-4)?     at [object Object]._.extend.withValue (packages/meteor.js:1122:17)
I20170619-01:29:07.942(-4)?     at packages/ddp-server/livedata_server.js:717:40
I20170619-01:29:07.942(-4)?     at [object Object]._.extend.withValue (packages/meteor.js:1122:17)
I20170619-01:29:07.942(-4)?     at packages/ddp-server/livedata_server.js:715:46
I20170619-01:29:07.943(-4)?     at [object Object]._.extend.protocol_handlers.method (packages/ddp-server/livedata_server.js:689:23)
I20170619-01:29:07.943(-4)?     at packages/ddp-server/livedata_server.js:559:43
```
