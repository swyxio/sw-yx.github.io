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
jslist = [ 'react', 'webpack', ' js', 'javascript','frontend', 'front-end', 'underscore','entscheidungsproblem', 'meteor']
osslist = [' oss', 'open source','maintainer']
designlist = ['css', 'designer', 'designing']
devlist = [' dev','web dev', 'webdev', 'code', 'coding',  'eng',  'software', 'full-stack', 'fullstack', 'backend', 'devops', 'graphql', 'programming',  'computer', 'scien']
makerlist = ['entrepreneur', 'hacker', 'maker', 'founder', 'internet', 'web']
def categorize(x):
    bio = unicode(x).lower()
    if any(s in bio for s in jslist):
        return 'js'
    elif any(s in bio for s in osslist):
        return 'oss'
    elif any(s in bio for s in designlist):
        return 'design'
    elif any(s in bio for s in devlist):
        return 'dev'
    elif any(s in bio for s in makerlist):
        return 'maker'
    else:
        return ''
cleanedfinal['cat'] = map(categorize,cleanedfinal['bios'])
print(len(cleanedfinal[cleanedfinal['cat'] == 'maker'])) # 573
print(len(cleanedfinal[cleanedfinal['cat'] == 'design'])) # 136
print(len(cleanedfinal[cleanedfinal['cat'] == 'oss'])) # 53
print(len(cleanedfinal[cleanedfinal['cat'] == 'js'])) # 355
print(len(cleanedfinal[cleanedfinal['cat'] == 'dev'])) # 758
```

this gives me 1875 categorized twitter accounts.

# gender

- https://github.com/tue-mdse/genderComputer # this did not work, filed an issue
- https://github.com/muatik/genderizer # this works!!!!
- https://www.kaggle.com/crowdflower/twitter-user-gender-classification # this is nice raw data for ML in future

# positivity

using textblob: https://pypi.python.org/pypi/textblob

you need this: https://stackoverflow.com/questions/26570944/resource-utokenizers-punkt-english-pickle-not-found
