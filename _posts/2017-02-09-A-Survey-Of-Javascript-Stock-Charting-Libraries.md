---
layout: post
title: A Survey of Javascript Stock Charting Libraries
date: 2017-02-09
tags: freecodecamp react
feelings: happy
---

on to the FCC stock chart project: <https://www.freecodecamp.com/challenges/chart-the-stock-market>

there is much less backend work to do here as we are just querying an API and i have access to my own APIs which I will be using.

However that leaves the dataviz problem and the requirements [here](http://watchstocks.herokuapp.com/) are pretty advanced:

- Multiple stock lines
- Select time period
- tooltip drawing multiple lines based on x coordinate

Here are the resources I know and my assessments:

- Google charts: <http://rakannimer.github.io/react-google-charts/#/examples/SteppedAreaChart?_k=cpip61> The base configuration isn't good enough for us (no multi lines), but digging into the settings should be helpful here: <https://developers.google.com/chart/interactive/docs/lines>
- React-stockcharts: <http://rrag.github.io/react-stockcharts/documentation.html#/line_scatter> has a nice zoom inbuilt and example of multilines. documentation is pretty weak but there is some example of tooltips [here](http://rrag.github.io/react-stockcharts/documentation.html#/hover_tooltip)
- D3FC: <http://blog.scottlogic.com/2015/07/22/yahoo-finance-chart-part-two.html> too complicated but the chart looks good.
- Raw D3: <http://blog.scottlogic.com/2014/09/26/an-interactive-stock-comparison-chart-with-d3.html>
- Chart.js: <http://www.chartjs.org/docs/#line-chart-introduction> very promising, good on animations
- Highcharts: <http://www.highcharts.com/stock/demo/compare> EXATLY WHAT WE WANT!!!! free for noncommercial. Popular on [Quora](https://www.quora.com/Is-D3-js-or-Highchart-js-better)
- dygraphs: <http://dygraphs.com/> looks good but the other charts kinda blow it out of the water
- anycharts: <http://docs.anychart.com/Stock_Charts/Series/Line> (Paid)
- MetricsGraphics: <http://metricsgraphicsjs.org/examples.htm#multilines> Looks really lightweight and simple.
- TechanJS: <http://techanjs.org/> looks really professional but only does single stock
- NVD3: <http://nvd3.org/examples/cumulativeLine.html> very interesting library, looks lightweight and looks good
- Rickshaw: <http://code.shutterstock.com/rickshaw/examples/> looks promising but a little simple

Meh:

- dc.js: <https://dc-js.github.io/dc.js/>
- plot.ly: <https://plot.ly/javascript/> possibly paid... but its also open source. very confusing.
- koolchart: <http://www.koolchart.com/> paid, didnt look into it too much
- jenscript: <http://jenscript.io/stock/8/> too fancy
- c3.js: <http://c3js.org/> looks like it is no longer actively supported and examples too simple
- chartist: <http://gionkunz.github.io/chartist-js/examples.html#line-scatter-random> lightweight but looks like crap

So the conclusion from all this is I should use highcharts.

---

inspirations:

- Analytics: <https://philipwalton.com/articles/the-google-analytics-setup-i-use-on-every-site-i-build/>
- Game programming: <http://gameprogrammingpatterns.com/contents.html>, <https://unity3d.com/learn/tutorials/projects/2d-roguelike-tutorial>
- d3 inspiration lists: <http://christopheviau.com/d3list/>, <https://libraries.io/github/wbkd/awesome-d3>
- <http://timelyportfolio.github.io/gridSVG_d3_multline/d3mult_line.html> interesting way to visualize govt bond runs
