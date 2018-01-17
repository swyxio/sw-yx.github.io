---
layout: post
date: 2018-01-19
tags: python
feelings: happy
title: halite first try
comments: true
description: taking a crack at halite ai
---

i was thrown off course by an email reminding me that halite is ending in a week so i am putting some time into halite.

resources:
- <http://forums.halite.io/>
- <http://stats.halite.io/python-starterkit.html#module-hlt.entity>
- <https://pythonprogramming.net/custom-ai-halite-ii-artificial-intelligence-competition/?completed=/modify-starter-bot-halite-ii-artificial-intelligence-competition/>

my bash script needed some modification to get it to work:
```bash
./halite -d "240 160" "python3 MyBot.py" "python3 MyBot_v0.py"
```

this is run_game.bat which is then run with `bash run_game.bat` in the terminal.

i was able to get the basic bot up and running and am now working on an attack strategy.

---

ok so after i adopted the guy's attack strategy this one is a very interesting attacker bot:

<https://halite.io/play/?game_id=8237454&replay_class=0&replay_name=replay-20180117-180755%2B0000--3096119115-336-224-1516212450>

---

after implementing a "viable settlement" strategy i am now up to rank 1088.

example wins: <https://halite.io/play?game_id=8245459> and example loss: <https://halite.io/play?game_id=8245389>

so i am falling prey to these basic issues:

- self collision (bad!! hard to fix)
- only targeting nearest planet when i need to weight vs proximity of enemy
- no defense against rambo (solo ships sent out specifically to hunt ships that are being docked)
- no rambo (easy!)

---


