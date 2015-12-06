---
layout: post
title: "Sample post with images"
date: 2015-12-05 16:25:06 -0700
tags: images jekyll
description: Sample post showing how images would look like in posts
---

## Introduction

This theme supports two types of images:
 
- inline images: ![Battery Widget]({{ site.url | append:site.baseurl }}/images/batWid1.png)

{% highlight html %}
{% raw %}
![Battery Widget]({{ site.url | append:site.baseurl }}/images/batWid1.png)
{% endraw %}
{% endhighlight html %}

- centered images with caption (optionally):
 
![screenshot]({{ site.baseurl | prepend:site.url}}/images/iphone_landscape.PNG){: .center-image }*iPhone 5 landscape*

{% highlight html %}
{% raw %}
![screenshot]({{ site.baseurl | prepend:site.url}}/images/iphone_landscape.PNG){: .center-image }*iPhone 5 landscape*
{% endraw %}
{% endhighlight html %}


