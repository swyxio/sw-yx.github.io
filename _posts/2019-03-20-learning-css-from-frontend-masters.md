---
layout: post
date: 2019-03-20
tags: css
feelings: learning
title: learning css from frontendmasters
comments: true
description: css masters
---


my own list of css tricks

```css
// a11y
img:not([alt]) {
  border: 3px solid red;
} 
// // show URL for ext. links!
a[href^='http']:after {
  content: ' (' attr(href) ')';
}

// // show PDF!
a[href$='.pdf']:after {
  content: ' (PDF)';
}

// // change css of label associated w checkbox
// doesnt work
// label input[type='checkbox']:checked {
// doesnt work (no support?)
// label.has(input[type='checkbox']:checked) {
// works
input[type='checkbox']:checked + label {
  color: red;
}

// required input
input:required {
  border: 5px solid red;
}

// // css counters?!?!
body {couter-reset: invalidCount}
:invalid {
  background-color: pink;
  counter-increment: invalidCount
}
p:before {
  content: "you have " counter(invalidCount) "invalid els"
}

// // using counter for sections
body {counter-reset: sections;}
header h1.sectiontitle:before{
  content: "Part " counter(sections) ": ";
  counter-increment: sections;
}


// tell people they have all blank textfields
:blank {
  // something here
}

// // fancy drop caps

p:first-of-type::first-letter {
  position: relative;
  top: 8px;
  float: left;
  font-size: 3em;
  line-height: 1;
  color: hsl(205, 87%, 50%);
  padding: 0 4px 2px 0;
  font-weight: bold;
}

// custom selection css w pseudoelement

p::selection {
  background-color: orange;
}

// // show URL on hover

a[href^='http']:hover {
  position: relative; // only on hover, not always, bc stacking context
}
a[href^='http']:hover:after {
  content: attr(href);
  position: absolute;
  top: 1em;
  left: 0;
  background-color: black;
  color: white;
  padding: 3px;
  line-height: 1;
}

// // custom quote tag open/close

:lang(en) > q {
  quotes: '\275D''\275E''<' '>';
}
:lang(fr) > q {
  quotes: '<' '>' '[' ']';
}
q::before {
  content: open-quote;
}
q::after {
  content: close-quote;
}

// custom unicode before content

a[href^='mailto:']:before {
  content: '\2709'; // email
}
a[href^='tel:']:before {
  content: '\2706'; // phone
}
a[href$='vcs']:before {
  content: '\2709'; // watch
}

// // styling quotes with speech tags
q {
  border-radius: 10px;
  position: relative;
  padding: 20px;
  left: 40px;
  background-color: red;
  color: white;
  float: left;
}
q:after {
  position: absolute;
  content: '';
  width: 0;
  height: 0;
  border: 20px solid blue;
  border-color: transparent red transparent transparent;
  top: 20px;
  left: -40px;
}

// // media queries

@media screen and (max-width: 60em) // better than px width
@media screen and (orientation: portrait) 
@media screen and (orientation: landscape) 

// rarely used but cool. there's tons more.
@media screen and (min-aspect-ratio: width)
@media screen and (min-resolution: 72dpi) // or dpcm or dppx
@media screen and (min-device-pixel-ratio: 1.5) // higher res screens
// media dependent imports! 
<link rel='stylesheet' 
  media='screen and (min-width: 320px) and (max-width: 480px)'
  href='css/smartphone.css' />


// using grammar
media="only screen and (orientation: portrait)"
media="not screen and (color)"
media="print, screen and (min-width: 480px)"

// // @supports
@supports (display: flex) {
  // rules for browsers supporting unprefixed flexbox
}

// // @viewport
// dont use user-scalable: no or maximum-scale=1 etc
<meta name="viewport" content="width=device-width, initial-scale=1" />
@viewport {
  width: device-width;
  zoom: 0.5
}

// // hyphenate for small screens. can also use <WBR> hint
@media screen and (max-width: 38em) {
  p {
    hyphens: auto
  }
}

// // 2 ways to center text
// media query
@media screen and (min-width: 38em) {
  #content { padding: 0 21%}
}
// margin auto
p {max-width: 44em; margin: auto}

// // CSS columns
p { columns: 8em 3;} // equally distribute text in 3 cols at least 8em wide
h1 {column-span: all;} // spans all columns

// // opacity vs alpha transparency
// opacity makes whole thing disappear
// alpha still preserves textshadow
// use transparency on text shadows so it looks nice overlapping stuff
box-shadow: -10px 10px rgba(0,0,0, 0.3)
text-shadow: 0 21px 1px rgba(0,0,0, 0.3)

// // styling tables
// you can style tables
table {border: 1px solid; border-collapse: collapse}
// you can style columns
col.week {background-color: pink}
col.player {font-size: 2em} // doesnt work tho
// you can style rows
tbody tr:hover {background-color: slategrey}
// you can style captions
caption { padding: 5px 0 10px; font-weight: bold}
// you can style theads with underline
thead tr th {border-bottom: 1px solid; background-color: #dedede}
// you can pad columns
td, th {padding: 10px 5px 10px 15px}
// you can stripe rows
tbody tr:nth-of-type(even) {background-color: #00000010}

```
