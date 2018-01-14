---
layout: post
date: 2017-12-02
tags: happy
categories: devfest
title: devfest nyc notes
---

pam selle @pamasaur || <https://thewebivore.com/> || <http://turing.cool>

serverless
---

no server is easier to manage than "no server"

Two types: 
- Functions as a service - run code on demand in response to defined trigger eg GCP, AWS Lambda, Azure, Webtask, Iron.io, Openwhisk
- Backend as a service - auth0, firebase, wufoo, filestack, pubnub

Fundations of serverless compute

- small building blocks (functions)
- autoscaling
- pay per usage (no idle time)
- availablility and fault tolerance (noOps)

How we got here:
- servers in datacenter
- servers in cloud - vms
- functions as a service - containers make serverless possible

Faas Pricing COst = Compute time + Invocations + Network

Unit of measure - GB-second = 1 second with 1 GB of memory provisioned. cost on lambda is $0.000001667.
AWS lambda free tier invocations 1m, compute time 400k gbs, invocations 0.2/million, compute time 0.000001667.

Deployment options
- serverless framework
- architecht
- aws serverless application model
- claudiajs (node)
- chalice (python, aws)
- zappa (python)

things to check out
- servers.lol

integrate operations into development model - think about what kind of observability you want

CHallenges
- managing state
- orchestration
- operations in a noops world
- testing
- networking



---

---
title: Medical Machine Learning in 30 Seconds
published: true
tags: inthirtyseconds, machinelearning, tensorflow, keras
---

Today at [DevFestNYC](https://devfestnyc.com/), [Josh Gordon](https://twitter.com/random_forests) explained how Tensorflow is being used to [detect cancer](https://research.googleblog.com/2017/03/assisting-pathologists-in-detecting.html) and [diabetes](https://research.googleblog.com/2016/11/deep-learning-for-detection-of-diabetic.html). What's amazing is that it is not much different from training a neural network for recognizing cats and dogs! Here is a brief summary of the talk:

1. **Data** - Get a lot of data. You need millions of images of whatever you are training on.
2. **MORE Data** - no really, you need more. Sometimes your data can be bad, for example if your doctor panel disagrees with each other, or worse still, disagrees with themselves. (humans, who needs them?)
3. **Setup** - Tensorflow now comes with Keras (_awesome_), and Keras has inbuilt [applications](https://keras.io/applications/) of which *InceptionV3* is pretty good (_awesomer_), although Josh also shouted out to NasNet [(a type of AutoML)](https://research.googleblog.com/2017/11/automl-for-large-scale-image.html) as a neural network that trains itself (_galactic brain exploding_).
4. **Fuzz** - Here is the art form and area of active research. Josh explained some key ideas, from using a [sliding window](https://medium.com/@ageitgey/machine-learning-is-fun-part-3-deep-learning-and-convolutional-neural-networks-f40359318721) to applying rotation/contrast/other filters to milk all you can out of the image data so you can, for example, recognize the same thing it is trained to recognize, even if it was flipped upside down. The most interesting part here was how they looked at how real doctors look at slides in the microscope, zooming in and out to get different contexts, and achieved amazing results by replicating that behavior simply by copying and pasting their code 4 times and running their model at 4 different zoom levels on the same dataset!!
5. **Train** - this takes on the order of 2 days (Google Cloud) to a week (local machine with 10 GPUs). If you are a researcher, Google offers 1000 TPUs FOR FREE to you to use if [you apply here](https://www.tensorflow.org/tfrc/).
6. **Deploy** - This is actually the hardest thing, which is making your models useful for regular nontechnical people to use. 

The goal is for Machine Learning should be so routine it is boring. Hopefully I've made that boringness interesting!


---

---
title: Firebase Analytics in 30 Seconds
published: true
tags: inthirtyseconds, firebase, analytics
---

[Stacy Devino](https://twitter.com/doesitpew) gave an excellent talk on Firebase Analytics today ([slides here](https://docs.google.com/presentation/d/1akpEegWCXHkMjU-GBS-3yZ1wHvO7YATU_zHP0kuqDhU/edit#slide=id.p), [github here](https://github.com/childofthehorn/AndroidFirebaseAnalyticsWorkshop), [youtube intro here](https://www.youtube.com/watch?v=8iZpH7O6zXo)). There were a lot of aha moments:

1. Integrating the Firebase SDK ([Android](https://firebase.google.com/docs/android/setup) and [iOS](https://firebase.google.com/docs/ios/setup) and [web](https://firebase.google.com/docs/web/setup)) is just a few lines of code!

There are **pre-configured** events (send standard stuff like login attempts):

```
Bundle bundle = new Bundle();
bundle.putString(FirebaseAnalytics.Param.ITEM_NAME, name);
bundle.putString(FirebaseAnalytics.Param.CONTENT_TYPE, "image");
mFirebaseAnalytics.logEvent(
    FirebaseAnalytics.Event.SELECT_CONTENT, bundle);
```

and **custom** events (send whatever you want)

```
Bundle bundle = new Bundle();
bundle.putString("image_name", name);
mFirebaseAnalytics.logEvent("profile_image_select", bundle);
```

2. User grouping (so you can study how different groups of your users behave differently) is similarly just a few lines:

```
mFirebaseAnalytics = FirebaseAnalytics.getInstance(this);
mFirebaseAnalytics.setUserProperty("preferred_pet", 
    petSelector);
mFirebaseAnalytics.setUserId("userIdString");
```

3. Once your app is out in the wild, head to the [Dashboard](https://console.firebase.google.com) to see insights!

4. Within the Dashboard, [Streamview](https://support.google.com/firebase/answer/7229836?hl=en) is particularly awesome as you can see the "clickstream" of user actions in sequence and visualize what they are doing (or failing to do) on your app!

5. Everything above is completely free and unlimited for you to use. To do -really big- number crunching, you'll have to export the data outside of [Firebase Analytics to Google BigQuery](https://cloud.google.com/solutions/mobile/mobile-firebase-analytics-big-query). There you will be able to query all your historical user data with plain SQL!

BigQuery is a freemium product so you'll have to have a credit card attached but the financial risk is low for most use cases: "[Realistically, most people wouldn’t burn through the free GCP $ in the first year. 20k users and normal use <$5/month](https://youtu.be/Ki_F6VCOtXU?t=86)".

Having messed around with a few analytics products in my time as a product manager this looks extraordinarily easy to setup and get insights on, and I am looking forward to deploying this in ALL my [Firebase hosted](https://firebase.google.com/docs/hosting/) and [serverless function](https://firebase.google.com/docs/functions/) projects!

---

---
title: Are we human? Or are we reCAPTCHA?
published: true
tags: til, recaptcha, machine learning
---

# "The 2 squiggly word captcha that you know and hate will die by 3/31/2018."

The Web is dark and full of bots, and there is one undisputed leader in defending against them. You probably use reCAPTCHA every day but you don't even know it! [Aaron Malenfant](https://www.linkedin.com/in/aaronmalenfant/) is the lead software engineer for [reCAPTCHA](https://www.google.com/recaptcha) and he explained its past, present, and future at GDG DevFest NYC. reCAPTCHA is secretive by its very nature, so it is a rare look into how this essential piece of web technology works.

# Part 1: High level details

## What I Learned

You can sign up for reCAPTCHA at <https://www.google.com/recaptcha> and learn more with the CodeLab [here](http://g.co/recaptcha/codelab).

### Volume

ReCAPTCHA
- 2 million weekly active sites
- 1 billion CAPTCHA solutions a week
- Nocaptcha saves millions of hours a day

### Difficulty levels

The reCAPTCHA Machine learning engine categorizes incoming requests on a spectrum of difficulty levels from "just a checkbox" to "select all images with cars" (image classification) to "select all squares with vehicles" (image localization) to "ok you're definitely a bot".

### Integrating into -your- site

Head to <https://www.google.com/recaptcha/admin#list> and answer a few simple questions!

You will have a few options:

- Visible: Script tag and a div
- Invisible: script tag and a button with a callback
- Invisible: script tag with a div to have control when you execute

Yes, there is such a thing as Invisible reCAPTCHA! more below. Also look up more docs at the [DevGuide](http://g.co/recaptcha/devguide).

Don't forget to integrate with serverside

- make HTTP POST to <www.google.com/recaptcha/api/siteverify> with POST params of `secret` and `response` you get from reCAPTCHA

# Part 2: Past, present and future

### RIP 2 word Captcha (reCAPTCHA v1)

**The 2 word captcha that you know and hate will die by 3/31/2018.** ([Source](https://www.programmableweb.com/news/google-recaptcha-v1-api-shutting-down-march-2018/brief/2017/10/24) and on the [FAQ](https://developers.google.com/recaptcha/docs/faq#what-happens-to-recaptcha-v1))

AI has advanced to the point that [it can solve the hardest CAPTCHAs at 99.8% accuracy, but humans can only solve them 33% of the time](http://www.zdnet.com/article/google-algorithm-busts-captcha-with-99-8-percent-accuracy/). So it is time to put it to bed.

### [reCAPTCHA v2](https://developers.google.com/recaptcha/docs/display)

the "i am a human" checkbox you've clicked dozens of times - this is actually called the "[NoCAPTCHA](https://www.theverge.com/2014/12/3/7325925/google-is-killing-captcha-as-we-know-it)" - for more details, see implementation options in Part 1 above.

### [Invisible reCAPTCHA](https://developers.google.com/recaptcha/docs/invisible) - launched on 3/8/2017

For low risk traffic, no user interaction is required at all to detect if you are a bot!

### reCAPTCHA Android API

Included as part of [Google Play Services SafetyNet](https://developer.android.com/training/safetynet/recaptcha.html) - again, no user interaction required to verify you are human.

### Future of reCAPTCHA (v3)

v3 is in Closed Beta now:
- puts you in control of when we show a challenge
- integration siilar to V2 Invisible
- In admin console, get a view into the riskiness of your traffic

Signup for reCAPTCHA v3 beta announcements at <http://g.co/recaptcha/v3>!

---

---
title: Serverless Machine Learning at Google
published: true
description: Serverless Machine Learning at Google
tags: serverless, machine learning
---

### Google can tell dogs from mops. Can you?

![dog or mop](http://cin.h-cdn.co/assets/16/14/1460113736-screen-shot-2016-04-08-at-43127-pm.png)

Bret McGowen presented on Serverless machine learning at Google. You can watch his full talk [here](https://www.youtube.com/watch?v=Fu8Sdh_wkQM) but here are my notes.

# Serverless

Four principles:
- no need to manage/think about servers
- no upfront provisioning, scale as you go (can't be wrong about having enough capacity)
- pay per use
- stateless/ephemeral

Serverless at Google:
- Background functions: Cloud Storage, Cloud Pub/Sub
- HTTP functions: API, Webhooks, Browser

# Machine Learning

Machine learning is using many **examples** to **answer questions**.

Machine Learning at Google:
- Use your own data: TensorFlow and Cloud Machine Learning Engine
- Pretrained ML models: Cloud (Vision, Speech, Natural Language, Translation) API, Cloud Video Intelligence

Specifics on capabilities of Cloud Vision API: 
- Label detection ([dog or mop](http://www.cosmopolitan.in/life/news/a5821/is-it-a-dog-or-mop-kitten-or-ice-cream-these-photographs-will-definitely-confuse-you/)?)
- Face detection (within the photo, here is the location of the face)
- OCR (read text from photos)
- Explicit content detection (violence/adult)
- Landmark detection (that's the Eiffel tower!)
- Local Detection (not sure)

Other Cloud Vision features:
- crop hints - suggested crop dimensions
- web annotations - suggested other metadata to search about your page - eg from a photo of an iconic car, it can tell you the model of car, what film it was from, where it probably is. And can give you other matching images to back it up.

### Cloud event trigger walkthrough

Cloud storage -> Cloud Functions -> Cloud vision API

NLP: extract entities from a sentence, sentiment analysis, syntax analysis (parse sentence to a lemma so you can see the parts of speech dependency graph)

### Speech API

Speech to text transcription in 110 languages.

Azar - uses cloud speech api and cloud translation api to talk
Also gives timestamp of each word on top of transcript.

### Video Intelligence API

Look through the whole video to label things.

### Links

- <http://github.com/bretmcg/functions-resize>
- <http://cloud.google.com>
