---
layout: post
date: 2017-10-05
tags: happy
categories: foss
title: hacktoberfest
---

going to github/DO's hacktoberfest thing today. i do many things for shirts.

resources
- <https://github.com/Microsoft/vscode-tips-and-tricks>
- [Firebase tutorial](https://codelabs.developers.google.com/codelabs/firebase-web/#0)

---

<https://github.com/sendgrid/sendgrid-nodejs/issues/463>

Deploy SendGrid mail to Heroku
---

We are thrilled you are ready to deploy your SendGridified app! Here are step by step instructions (assuming you have a NodeJS app running on localhost tracked by git):

- `heroku create` (you may need to have your `heroku login` ready)
- `git push heroku master`
- `heroku ps:scale web=1`
- `heroku config:set SENDGRID_API_KEY=SG.YOUR.OWN-API_KEY-HERE` (replace `SG.YOUR.OWN-API_KEY-HERE` with your own [api key from sendgrid](https://app.sendgrid.com/settings/api_keys)

If you run into any other non SendGrid related issues, don't forget to read through [Heroku's deployment documentation](https://devcenter.heroku.com/articles/getting-started-with-nodejs).
