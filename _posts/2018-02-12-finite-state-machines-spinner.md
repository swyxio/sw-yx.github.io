---
layout: post
date: 2018-02-12
tags: react
feelings: neutral
title: finite state machines - spinner example
comments: true
description: a journey with david khourshid's FSMs
---

my full journey today with finite state machines, including testing https://twitter.com/swyx/status/963157725998911488


i started with this:

```

// pseudocode logic:
// ---
// constructor:
//   if (isLoading): # this mainly takes care of case A and B
//     set timer250
//       after 250
//         if (isLoading):
//           set show = true and lock = true
//           set timer300
//             after 300
//               set lock = false and show = true/false depending if isLoading is true
//         else:
//           show = false
//   else:
//     show = false

// componentWillReceiveProps: # here we take care of case C
//   if (lock == false and nextprops.isLoading == false):
//     set show=false
// ---
```

i then went for code review and was told i need to handle cancel and restart cases so i started pottering around with this:

```


// behavior we want:

// isLoading goes from true => false
// ---
// A - if time < delay1: cancel delay1
// B - if time between delay1 & delay2:
//   -   if isLoading=true: lock=true and delay2 kicked off
//   -
// C - if time > delay2: spinner shows and disappears only when item loads

// delay1 ends
// ---
// if isLoading = false AND lock = false
//   set show = false
// else
//   kickoff delay2

// isLoading goes from false => true
// ---
// D - if spinner is showing: cancel delay1 and delay2
// E - if spinner not showing: kick off delay1
```

and its incomplete because i realized this was going out of control


i then moved to state machinese and it was so much easier. i started with this first attempt

![https://pbs.twimg.com/media/DV3Sp8lVoAE2ULl.jpg:large](https://pbs.twimg.com/media/DV3Sp8lVoAE2ULl.jpg:large)

and then realized this didnt handle stage C cancelling so i modified

![https://pbs.twimg.com/media/DV3sZIVX0AAo6Rn.jpg:large](https://pbs.twimg.com/media/DV3sZIVX0AAo6Rn.jpg:large)
