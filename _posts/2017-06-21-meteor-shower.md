---
layout: post
title: meteor shower
date: 2017-06-21
tags: meteor
feelings: happy
---

after a long but very productive day yesterday i hope to take a break from meteor while i wait for feedback from JR. I can either work on some VEMB (icebob) stuff or learn VulcanJS or Django.

---

somebody on the meteor chef channel asked:
```
Goodmorning guys. I have been struggling with a method for the past 2 days. I am calling an insertReport method on the client and all is well. Now however i want to go into another collection "strategies" find a strategy and append its id to my report before insertReport returns with the id report to the client. Does anybody know how to do this? Please help as i am drowning in wrapasync's, await's, fiber's and promises :w

I actually do not need to publish the strategies collection as I only need to access it on the server within the insertReport method. This way I am also avoiding to publish my strategies to the client side before the report is generated. Or is my reasoning completely wrong?

I am more looking for a mod of this. But I am still not sure if this is even possible...(?) https://medium.com/@robfallows/meteor-promises-fdd1f701caf1
```
the shitty solution was:

```javascript
import { Strategies } from '../strategies.js'; // my collection

export const insertReport = new ValidatedMethod({
  name: 'Reports.methods.insert',
  validate: Schemas.Report.validator(),
  run(report) {
    let selectedStrategies = [];
    let allStrategies = Strategies.find({}).fetch()
    
    for (var i = 0; i < 5; i++) {
        selectedStrategies.push(allStrategies[Math.floor(Math.random() * allStrategies.length)]._id)
      }
    report.strategies = selectedStrategies;

    return Reports.insert(report);
  },

});
```

the solution was:

```javascript
Meteor.methods({
  async insertReport(report){
    const pipeline = [
      {$sample: {size: 5}}
    ]

    // Using the meteorhacks:aggregate package to do a random search for 5 strategies in the Strategies
    // collection and append them to the report object. The aggregate package only works on the server.
    if(Meteor.isServer){
      // establish connection and wait for querry result
      const connection = await Strategies.aggregate(pipeline);
      // Once values are returned append to the report object
      report.strategies = await connection;
    }
    const result = await Reports.insert(report);
    return result;
  }
})
```
