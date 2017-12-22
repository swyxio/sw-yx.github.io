---
layout: post
title: Workproject 2
date: 2017-03-21
categories: workproject node react
tags: fat
---

I only made it 30 minutes yesterday in vue. It became evident that I would need way more than 2 hours to learn all the necessary paths of vue. it will take the same time as React. So given I have less than a week until my SF trip I am going to just finish up workproject and then see what I can do in VR.

---

11.50 checking in. I am done with the locally hosted version of workproject and have to deploy it now. I think material-ui is pretty good but ultimately it is not mature enough for heavy duty dashboard use. for example, tables [cannot yet be sorted](https://github.com/callemall/material-ui/issues/1352) and i found [this bug](https://github.com/callemall/material-ui/issues/6413) when selecting all and deselecting.

---

deploying preloaded databases to heroku and mlab:

- create heroku
- allocate an mlab instance
- create user
- inside mLab, go to Tools -> import/export helper
- `mongoimport -h ds139430.mlab.com:39430 -d heroku_p8g173r6 -c userdata -u <user> -p <password> --file <input .csv file> --type csv --headerline`

done! <https://swyx-analytics-2.herokuapp.com/>
