---
layout: post
title: Finishing book trading project
date: 2017-02-28
categories: freecodecamp react
tags: moved
---

today I moved to my new digs. everything is nice and clean and I am feeling happy and ready to be productive. today I will plug in the books object to the interface and allow the user to add books.

There is some debate as to how to structure these kinds of owned objects in nosql setups. i like the benefits of normalization (being able to present data later on in other formats eg in the All Books format) and so I am just writing the Books to a different object and NOT creating a sublist under User. this is certainly not the only way to do it but the cost of doing this will be de minimis. for more reading see <http://stackoverflow.com/questions/5841681/mongodb-normalization-foreign-key-and-joining>

