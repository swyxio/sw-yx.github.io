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

```python
driver = webdriver.Firefox()
driver.base_url = "https://twitter.com/swyx/following"
driver.get(driver.base_url)
```

and then login manually and get the arr ready


```python
df = pd.read_csv('BASICDATA.csv', encoding = "ISO-8859-1")
arr = df.usernames
```

then


```python
for i in range(100,500):
    currentUser = arr[i]
    print('now doing user ' + str(i) + ': ' + currentUser)
    driver.base_url = "https://twitter.com/" + currentUser + "/following"
    driver.get(driver.base_url)
    time.sleep(3) # first load
    loopCounter = 0
    lastHeight = driver.execute_script("return document.body.scrollHeight")
    while True:
        if loopCounter > 499:
            break;
        if loopCounter > 0 and loopCounter % 50 == 0:
            print(loopCounter)
        driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
        time.sleep(2)
        newHeight = driver.execute_script("return document.body.scrollHeight")
        if newHeight == lastHeight:
            break
        lastHeight = newHeight
        loopCounter = loopCounter + 1
    print('ended at: ' + str(loopCounter))
    html_source = driver.page_source
    dailyemail_links = html_source.encode('utf-8')
    soup=bs(dailyemail_links)
    temparr = [x.div['data-screen-name'] for x in soup.body.findAll('div', attrs={'data-item-type':'user'})]
    tempbios = [x.p.text for x in soup.body.findAll('div', attrs={'data-item-type':'user'})]
    d = {'usernames': temparr, 'bios': tempbios}
    df = pd.DataFrame(data=d)
    df.to_csv('data/' + currentUser + '.csv')
```


# follow's stats

now to scrape my follow's bios and stats. this was run/edited multiple times before i finally got all the stats i wanted

```python
# now get the user's own timeline
main = pd.DataFrame(data = {
        'user': ['swyx],
        'text': ['text'],
        'mostrecentTimestamp': ['mostrecentTimestamp'],
        'engagements': ['engagements'],
        'name': ['name'],
        'loc': ['loc'],
        'url': ['url'],
        'stats_tweets': ['stats_tweets'],
        'stats_following': ['stats_following'],
        'stats_followers': ['stats_followers'],
        'stats_favorites': ['stats_favorites'],
    })
# now get the user's own timeline
for i in range(0,len(arr)):
    currentUser = arr[i]
    print('doing user:' + str(i) + ' ' + currentUser)
    driver.base_url = "https://twitter.com/" + currentUser + '/with_replies'
    driver.get(driver.base_url)
    html_source = driver.page_source
    dailyemail_links = html_source.encode('utf-8')
    soup=bs(dailyemail_links)
    time.sleep(2)
    driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
    time.sleep(1)
    # name
    name = soup.find('a', "ProfileHeaderCard-nameLink").text
    # loc
    temp = soup.find('span', 'ProfileHeaderCard-locationText')
    temp = temp.text if temp else ''
    loc = temp.strip() if temp else ''
    # url
    temp = soup.find('span', 'ProfileHeaderCard-urlText')
    temp = temp.a if temp else None
    url = temp['title'] if temp else ''
    # stats
#     stats = [x.get('data-count') for x in soup.body.findAll('span', attrs={'class':'ProfileNav-value'})[:4]]
    temp = soup.find('a',{'data-nav': 'tweets'})
    stats_tweets = temp.find('span', 'ProfileNav-value')['data-count'] if temp else 0
    temp = soup.find('a',{'data-nav': 'following'})
    stats_following = temp.find('span', 'ProfileNav-value')['data-count'] if temp else 0
    temp = soup.find('a',{'data-nav': 'followers'})
    stats_followers = temp.find('span', 'ProfileNav-value')['data-count'] if temp else 0
    temp = soup.find('a',{'data-nav': 'favorites'})
    stats_favorites = temp.find('span', 'ProfileNav-value')['data-count'] if temp else 0
    # all text
    text = [''.join(x.findAll(text=True)) for x in soup.body.findAll('p', 'tweet-text')]
    # most recent activity
    alltweets = soup.body.findAll('li', attrs={'data-item-type':'tweet'})
    mostrecentTimestamp = int(alltweets[0].findAll('span', '_timestamp')[0].get('data-time')) if len(alltweets) > 0 else 0
    # engagements
    noretweets = [x.findAll('span', 'ProfileTweet-actionCount') for x in alltweets if not x.div.get('data-retweet-id')]
    templist = [x.findAll('span', 'ProfileTweet-actionCount') for x in alltweets if not x.div.get('data-retweet-id')]
    templist = [item for sublist in templist for item in sublist]
    engagements = sum([int(x.get('data-tweet-stat-count')) for x in templist if x.get('data-tweet-stat-count')])
    main = pd.concat([main, pd.DataFrame(data = {
        'user': [currentUser],
        'text': [text],
        'mostrecentTimestamp': [mostrecentTimestamp],
        'engagements': [engagements],
        'name': [name],
        'loc': [loc],
        'url': [url],
        'stats_tweets': [stats_tweets],
        'stats_following': [stats_following],
        'stats_followers': [stats_followers],
        'stats_favorites': [stats_favorites],
    })])
    main.to_csv('data/BASICDATA_profiles.csv')
```

data cleaning:

```python
namesandbios = df.set_index('usernames')
final = main[1:].set_index('user')
final['bios'] = namesandbios['bios']
final = final.reset_index().drop(['Unnamed: 0'], axis=1)
final['stats'] = list(map(lambda x: eval(x), final['stats'])) # parse from string
final['stats-followers'] = list(map(lambda x: (int(x[2]) if x[2] != None else 0) if len(x) > 2 else 0, final['stats']))
final['stats-follows'] = list(map(lambda x: (int(x[1]) if x[1] != None else 0) if len(x) > 1 else 0, final['stats']))
cleanedfinal = final.sort_values(by = ['stats-followers'])[170:]
```


# helpful

- [bs3 docs](https://www.crummy.com/software/BeautifulSoup/bs3/documentation.html#The basic find method: findAll(name, attrs, recursive, text, limit, **kwargs))
- <https://stackoverflow.com/questions/28928068/scroll-down-to-bottom-of-infinite-page-with-phantomjs-in-python>
- maybe useful in future: <https://github.com/sebinsua/scrape-twitter>
- i have not yet looked at: <https://codeandculture.wordpress.com/2016/01/19/scraping-twitter-with-python/>
