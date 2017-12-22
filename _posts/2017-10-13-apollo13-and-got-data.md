---
layout: post
date: 2017-10-13
feelings: happy
tags: apollo graphql fsa
title: apollo 13 and got data
---

i listened the Apollo and Relay team duke it out on GraphQL radio yesterday and it was pretty cool. it seems they will reach an uneasy convergence.

meanwhile the apollo team's tooling is solid. i am messing around on their playground today: <https://launchpad.graphql.com/3xp1vx3lv>

---

[i forked graphiql to do some of my own tweaks](https://github.com/sw-yx/swyx-graphiql)! also PR'ed to fix a doc: <https://github.com/graphql/graphiql/issues/619> 

---

two awesome sidebar components:
- <https://github.com/balloob/react-sidebar/blob/master/package.json> small
- <https://github.com/negomi/react-burger-menu/blob/master/package.json> full featured

---

[GOT data](https://www.reddit.com/r/freefolk/comments/769hav/game_of_thrones_datasets/) thread:

Hey guys,

I'm doing a project that requires good GOT data and so I naturally turned to what everyone knows, [the API of Ice and Fire](https://anapioficeandfire.com). Unfortunately I soon found concerning holes in the data, for example most (but not all!) of the Mother, Father, and Spouse fields are blank even for the main characters. As a non-book reader I was also worried about double entry - apparently Daenerys is -not- the first of her name([first](https://anapioficeandfire.com/api/characters/271), [second](https://anapioficeandfire.com/api/characters/1303))? and other careless mistakes.

So while I appreciate AoIAF I am looking to supplement my data with more datasets. I don't have the time/skill to write scrapers for the [wikia](http://gameofthrones.wikia.com/wiki/Game_of_Thrones_Wiki) or the [wiki](http://awoiaf.westeros.org/index.php/House_Arryn). Anyway I decided to share the resources I found in case future people need this too, and also please chime in with anything I may have missed. I went five pages deep into two google searches and picked out useful looking stuff. Here goes.


- [Jeffrey Lancaster's all the things](https://github.com/jeffreylancaster/game-of-thrones) - [Writeup here](https://medium.com/@jeffrey.lancaster/the-ultimate-game-of-thrones-dataset-a100c0cf35fb) This thing is a fucking masterpiece - [Sankey diagram!!!](https://jeffreylancaster.github.io/game-of-thrones/) and has like 20 views. i want to buy shares in it.
- [neo4j data scrape from wikia/wikipedia](https://github.com/mneedham/neo4j-got/tree/master/data)
- [books data for network analysis](https://github.com/mathbeveridge/asoiaf) - blog [here](https://networkofthrones.wordpress.com/)
- [AoIaF Data cleaned by IBM](https://developer.ibm.com/clouddataservices/wp-content/uploads/sites/47/2016/04/gameofthrones_scrubbed-gameofthrones-004.txt)/[and here](https://developer.ibm.com/clouddataservices/wp-content/uploads/sites/47/2016/04/gameofthrones.txt) - writeup [here](https://developer.ibm.com/clouddataservices/2016/04/19/winter-is-coming-importing-game-of-thrones-data-into-cloudant-with-the-simple-search-service/)
- [AFFC Share Copy](https://docs.google.com/spreadsheets/d/1gEZUEo8GUP5b6Tup6wzUG90erpDPgOyGhbB5GfMrk7E) Character database, no family relationships only house relationships
- [OrientDB scrape of the Wikia](http://gog.orientdb.com/index.html#/download) - has a ton of relations which is great, but in a proprietary db format
- [GOT Transcripts](https://gameofthronesscripts.wordpress.com/) - what it says on the tin. dead simple. 
- [Season Recaps](https://duelingdata.blogspot.com/2017/07/game-of-thrones-season-6-recap.html) - number of lines in season 6 by character . Here are [show appearances over 5 seasons](https://duelingdata.blogspot.com/2016/04/game-of-thrones-analysis.html)
- [Character Data](https://github.com/girikuncoro/thrones) scraped from wikia up to season 5. may have usable python code
- [Screentime Data](https://data.world/aendrew/game-of-thrones-screen-times) from [IMDB](http://www.imdb.com/list/ls076752033/)
- [Battles, deaths, death predictions](https://www.kaggle.com/mylesoneill/game-of-thrones/data) only battle dataset available. some [handy Python crunching of this data explained here](https://datascienceplus.com/network-analysis-of-game-of-thrones/) and more [usage](http://justingosses.com/game-of-thrones-parallel-sets-data-visualization/) [here](http://justingosses.com/ParaSet_Oct2/) and [here](https://tbgraph.wordpress.com/2017/06/24/neo4j-game-of-thrones-part-1/)
- [Deaths](https://data.world/aendrew/game-of-thrones-deaths) - source from [Time](http://time.com/3924852/every-game-of-throne/)
- [Episode data](https://en.wikipedia.org/wiki/List_of_Game_of_Thrones_episodes) from wikipedia
- [Character Data in excel](https://trumpexcel.com/game-of-thrones-dashboard/) with screentime and status and actor and episode rating
- [Betrayals](https://docs.google.com/spreadsheets/d/1yfzwRZFY08EBgwfMYTY8BojB4GIfsOBLo0PQ2Hs8u5g/edit#gid=0) from [venngage](https://venngage.com/blog/game-of-thrones-infographic/)

3 [Looker](https://looker.com/blog/data-of-thrones-part-i) blogs were a good source for finding some of the above (weren't on google) and wanted to give credit.

Visualizations w/o raw data (may need extra work to chase down the raw data, im not interested in the pretty charts if I cant reproduce them/use them myself)

- <https://flowingdata.com/tag/game-of-thrones/> Collection of various things
- <https://blog.twitter.com/en_us/topics/insights/2016/game-of-thrones-viz.html> Tweet mentions
- <https://datasaurus-rex.com/datavisualization/game-of-thrones-interactive-viz-mkii> GoT Deaths (holy shit, this one is really good. Go [here](https://public.tableau.com/en-us/s/gallery/game-thrones-deaths) if your browser doesnt like the first link like me. [HAND CODED by THIS GUY](https://www.youtube.com/watch?v=ZwiFLZidcM8))
- <http://patorjk.com/vis/chaos-ladder/> appearances over time, up to season 4
- <http://news.northeastern.edu/2017/08/the-game-of-thrones-social-network/> GOT as social network
- <http://deathtimeline.com/> scrapable [example](https://ekerstein.com/gameofthrones/)
- <https://github.com/neo4j-examples/game-of-thrones> proprietary neo4j format but from AoIaF
- <https://www.nytimes.com/interactive/2017/08/09/upshot/game-of-thrones-chart.html> user looks rating
