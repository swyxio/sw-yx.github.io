---
layout: post
title: drawing network graphs
date: 2017-05-10
categories: dataviz
tags: hungry
---

i spent a lot of time in a hellhole today looking at network graphs ([tutorial(https://www.cl.cam.ac.uk/~cm542/teaching/2011/stna-pdfs/stna-lecture11.pdf)). 

this is still typical of the python space: <https://stackoverflow.com/questions/35830941/creating-network-graphs>

basically there are only a few options to do network graphs in python and none of them are good:

- networkx has good tutorials but are not interactive <# #https://networkx.readthedocs.io/en/networkx-1.11/tutorial/tutorial.html#multigraphs>
- plot.ly is interactive but their examples are shit <https://plot.ly/python/network-graphs/> and <http://www.techpoweredmath.com/constructing-social-graph-twitter-plotly/>
- igraph (now jgraph) has some weird conflict that prevents usage <https://plot.ly/python/igraph-networkx-comparison/>

i then tried desktop software: <https://gephi.org> which has a very weird nonintuitive UI but at least helps me consume my data.

so i turned to javascript

- sigma js: <https://github.com/jacomyal/sigma.js/releases/> requires you to position your nodes
- vis js <http://visjs.org/network_examples.html> is promising.

that's where im at right now.
