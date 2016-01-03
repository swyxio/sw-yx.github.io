---
layout: post
title: "Text formatting"
date: 2015-11-02 16:25:06
tags: jekyll
description: Text formatting examples.
toc: true
---

Some examples of text formating for some common text elements.

# Headers

# Header1

## Header2

### Header3

#### Header4

# Emphasis

Italics: `*asterisks*` -> *asterisks* or `_underscores_` -> _underscores_.

Bold: `**asterisks**` -> **asterisks** or `__underscores__` -> __underscores__.

You also can combine them: `**asterisks and _underscores_**` -> **asterisks and _underscores_**.

# Blockquotes and notes


{% highlight bash %}
>Blockquotes
{% endhighlight bash %}

>Blockquotes

Using very cool [feature](http://kramdown.gettalong.org/quickref.html#block-attributes) of kramdown which allows to assign any attribute to a block-level element I've added note a warning:

{% highlight bash %}
>Note 
{: .note}
{% endhighlight bash %}

>Note 
{: .note}

{% highlight bash %}
>Warning 
{: .note .warning}
{% endhighlight bash %}

>Warning 
{: .note .warning}