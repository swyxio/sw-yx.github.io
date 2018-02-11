---
layout: post
date: 2018-02-10
tags: python
feelings: happy
title: python twitter scraping
comments: true
description: some notes on how to do python twitter scraping
---

twitters's api sucks so i have to do selenium scraping. here are my notes.

```python
%matplotlib inline
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import Select
from selenium.webdriver.support.ui import WebDriverWait
from selenium.common.exceptions import TimeoutException
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import NoSuchElementException
from selenium.common.exceptions import NoAlertPresentException
import sys

import unittest, time, re
from BeautifulSoup import BeautifulSoup as bs
from dateutil import parser
import pandas as pd
import itertools
import matplotlib.pyplot as plt

soup=bs(dailyemail_links) #make BeautifulSoup
dailyemail_links2 = [{"url": x.a.attrs[0][1].replace("http://stratechery.com/20","https://stratechery.com/20"),
"title": x.a.text,
"time": parser.parse(x.time.attrs[1][1]) if x.time else x.time}
for x in soup.body.findAll('header', attrs={'class':'entry-header'})]
```

thats a good test case

# scraping my followings

```python
driver = webdriver.Firefox()
driver.base_url = "https://twitter.com/swyx/following"
driver.get(driver.base_url)
```

and then login manually


```python
for i in range(1,225):
  driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
  time.sleep(2)
  print i
html_source = driver.page_source
dailyemail_links = html_source.encode('utf-8')
soup=bs(dailyemail_links)
arr = [x.div['data-screen-name'] for x in soup.body.findAll('div', attrs={'data-item-type':'user'})]
bios = [x.p.text for x in soup.body.findAll('div', attrs={'data-item-type':'user'})]
```

that is a very basic scrape script for usernames and bios.

# follow's follows

now i have to scrape my follows' follows. the scolling will be smarter.

# follow's stats

now to scrape my follow's bios and stats.

```python
# now get the user's own timeline
main = pd.DataFrame(data = {
        'user': ['swyx'],
        'stats': [[1,2,3,4]],
        'text': [['test']],
        'mostrecentTimestamp': [1234],
        'engagements': [1234]
    })
for i in range(0,100):
    currentUser = arr[i]
    driver.base_url = "https://twitter.com/" + currentUser + '/with_replies'
    driver.get(driver.base_url)
    html_source = driver.page_source
    dailyemail_links = html_source.encode('utf-8')
    soup=bs(dailyemail_links)
    time.sleep(2)
    driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
    time.sleep(2)
    # stats
    stats = map(lambda x: int(x), [x['data-count'] for x in soup.body.findAll('span', attrs={'class':'ProfileNav-value'})[:4]])
    # all text
    text = [''.join(x.findAll(text=True)) for x in soup.body.findAll('p', 'tweet-text')]
    # most recent activity
    alltweets = soup.body.findAll('li', attrs={'data-item-type':'tweet'})
    mostrecentTimestamp = int(alltweets[0].findAll('span', '_timestamp')[0].get('data-time'))
    # engagements
    noretweets = [x.findAll('span', 'ProfileTweet-actionCount') for x in alltweets if not x.div.get('data-retweet-id')]
    templist = [x.findAll('span', 'ProfileTweet-actionCount') for x in alltweets if not x.div.get('data-retweet-id')]
    templist = [item for sublist in templist for item in sublist]
    engagements = sum([int(x.get('data-tweet-stat-count')) for x in templist if x.get('data-tweet-stat-count')])
    pd.concat([main, pd.DataFrame(data = {
        'user': [currentUser],
        'stats': [stats],
        'text': [text],
        'mostrecentTimestamp': [mostrecentTimestamp],
        'engagements': [engagements]
    })])
```


# helpful

- [bs3 docs](https://www.crummy.com/software/BeautifulSoup/bs3/documentation.html#The basic find method: findAll(name, attrs, recursive, text, limit, **kwargs))
- <https://stackoverflow.com/questions/28928068/scroll-down-to-bottom-of-infinite-page-with-phantomjs-in-python>
- maybe useful in future: <https://github.com/sebinsua/scrape-twitter>
- i have not yet looked at: <https://codeandculture.wordpress.com/2016/01/19/scraping-twitter-with-python/>
