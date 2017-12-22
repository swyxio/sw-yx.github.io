---
layout: post
title: Tutorial - How to Host an Alexa Flash Briefing feed on AWS Lambda and get a Free Amazon Echo Dot
date: 2017-06-03
categories: python
tags: happy
---

posted with images at: <https://medium.com/@swyx/tutorial-how-to-host-an-alexa-flash-briefing-feed-on-aws-lambda-and-get-a-free-amazon-echo-dot-a50fa1c76a64>

note: this tutorial uses Python for convenience but could be adapted for Node/other languages with minor effort.
Amazon has always been shameless about getting developers onboard the Alexa Skills platform, prioritizing quantity over quality. The latest Alexa Skills promo got me excited as it started handing out FREE ECHO DOTS for June 2017:

i liked the shirts, but this is too good to pass up
Incidentally I have been unhappy with the standard Coinbase-based flash briefing because it doesnt provide enough info and it is slow so I had been thinking to write my own flash briefing. So this is perfect timing to do a new skill.
I couldn’t find anything on this topic from googling around (this unreadable tutorial focuses on Skill instead of Flash Briefing) and then spent the next 3 hours piecing together how to do it so I figured I would write it up here.
Setup Alexa Dev Console
First, Add a New Flash Briefing skill:

Once you fill in the name just hit Next until you hit the Feed configuration, then hit Add new feed. Strictly follow their suggested settings as they are weirdly anal about these things.

Please read the API Feed Reference before you continue. No, really, read it.
Your Lambda-hosted API will need to fit these requirements exactly or the Flash Briefing will fail frustratingly without explanation.
Setup AWS Lambda
Hokay. Now we have to setup a feed for your Flash Briefing to consume. No biggie. Open a new tab and head over to AWS Lambda to create a new function. AWS doesn’t yet have a blueprint for you to create a Flash Briefing so we’re gonna have to wing it:

After you select the nonexistent blueprint, configure an API Gateway Trigger:

You can set various security levels — for convenience you can go with Open, I don’t know enough about security to do the other stuff but you probably should if you are hosting critical functions here or dont want to be charged a bomb if hackers find your API. Hit Next.

i cheated here and went with the microservice-http-endpoint-python3 blueprint but you’ll figure it out
Coding
Now the coding fun begins! You will hopefully spend the most time here as this is your value-add. Here are a couple notes:
you can and should crib extensively from the other Alexa skill blueprints. just head back to Select blueprint and type in ‘alexa’ to see the options. However note that none of these are Triggered by hitting a feed URL, so they have limited value for your Flash Briefing use case. So go ahead and check out other blueprints too, in particular ‘microservice-http-endpoint-python3’
Lambda does support a number of languages including C#, Node, and Python, however the default library importing is very limited in Node and often means you will have to upload a zip file which isn’t too complicated but again kind of overkill for the very simple serverless thing we are trying to do here. Python is what I prefer as all the standard libraries like urllib2 are already included so I will use that going forward.
As I noted earlier you will need to fit your JSON response to lambda_handler to the Alexa API Feed requirements so be warned. In particular, a fun quirk is that you will have to make sure there is a “Z” at the end of your ISO datetime format because Alexa needs to know when your feed updated in UTC Zulu time.
Yeah.
Anyway here is minimal sample code for you to crib from (does not work but should save you a ton of time)
from __future__ import print_function
import json
import urllib2
import datetime
def respond(err, res=None):
 return {
 ‘statusCode’: ‘400’ if err else ‘200’,
 ‘body’: json.dumps({
 “uid”: “1234”,
 “updateDate”: datetime.datetime.utcnow().isoformat() + ‘Z’,
 “titleText”: “yourname”,
 “mainText”: err.message if err else json.dumps(res[‘message’]),
 “redirectionUrl”: “https://yoururl.com” 
 }),
 ‘headers’: {
 ‘Content-Type': ‘application/json’,
 },
}
def lambda_handler(event, context):
 response = urllib2.urlopen(‘https://somesource.com')
 html=response.read()
 rawdata = json.loads(html)
 reply = { “message”: rawdata,
 }
 return respond(None, reply)
Alright! thats that! If this is your first Lambda function you may have to set up a basic execution role (sorry, this was a long time ago for me so I dont know the steps anymore but this and this are good tutorials), and if you have it set up then you can select it:

Testing
Alrighty then! Save and Test it to your satisfaction, but note that your testing needs to take a few stages:
test in Lambda — this is the base test to see if your code is valid and executes
test API gateway in browser (described below) — this tests whether your code outputs valid JSON
Saving the feed URL in Alexa Dev Console (“Flash Briefing Feed Configuration”) — this tests whether your JSON fits their requirements
test in Alexa — this tests whether your Flash Briefing runs properly (including the tricky timezone format issue) — do this at the end
To test your API gateway, just open the URL in another browser window:

This is the same URL you will paste in the Flash Briefing Feed Configuration (this is the 3rd test step described above):

Hit Next and you can start testing this skill in your Echo (4th test step)!

if you have no Echo, tough luck, Alexa doesnt yet work on your phone (i think)
Publishing your Skill
At this point you actually have a functional Flash Briefing Skill for your own personal use. But that’s not the point of this. The point of this is to get your FREE ECHO DOT. So you must get your thing certified. Ugh!
Fill out basic publishing info…

Don’t forget the privacy stuff too

And send it in for certification! This process can take between 3 days and 3 weeks and may involve multiple rounds of back and forth rejections.
Boom! You’re done! Right??!?
No!
Head back to the first page and get your application ID

And then fill out the form here with your info.
Your Echo Dot is now on its way! 🔥

this is crossposted from my dev blog http://swyx.io/blog/ or chat with me @swyx.
