---
layout: post
date: 2018-02-11
tags: python
feelings: happy
title: categorizing twitter accounts
comments: true
description: now i have the data, to figure out what to do with it.
---

i have a lot of raw data but i need to categorize it for it to be useful.

i think i want to go with just three dimensions of categorization

- tech subindustry
- gender
- positivity

# tech categorization

tech categorization is going to be really imperfect but i made a bag of words based on my 4000 twitter follows

```python
jslist = [ 'react', 'webpack', ' js', 'javascript','frontend', 'front-end', 'underscore','entscheidungsproblem', 'meteor', 'google']
osslist = [' oss', 'open source','maintainer']
designlist = ['css', 'designer', 'designing']
devlist = ['dev', 'code', 'coding',  'eng',  'software', 'full-stack', 'fullstack', 'backend', 'devops', 'graphql', 'programming',  'computer', 'scien']
makerlist = ['entrepreneur', 'hacker', 'maker']
def categorize(x):
    bio = unicode(x).lower()
    if any(s in bio for s in makerlist):
        return 'maker'
    elif any(s in bio for s in designlist):
        return 'design'
    elif any(s in bio for s in osslist):
        return 'oss'
    elif any(s in bio for s in jslist):
        return 'js'
    elif any(s in bio for s in devlist):
        return 'dev'
    else:
        return ''
cleanedfinal['cat'] = map(categorize,cleanedfinal['bios'])
print(len(cleanedfinal[cleanedfinal['cat'] == 'maker'])) # 254
print(len(cleanedfinal[cleanedfinal['cat'] == 'design'])) # 152
print(len(cleanedfinal[cleanedfinal['cat'] == 'oss'])) # 73
print(len(cleanedfinal[cleanedfinal['cat'] == 'js'])) # 371
print(len(cleanedfinal[cleanedfinal['cat'] == 'dev'])) # 706
```

this gives me 1556 categorized twitter accounts.
