---
layout: post
title:  SQL Many Rows To One XML And Stuff
date:   2019-10-05
description: How to combine rows in SQL example
img: py.png # Add image post (optional)
tags: [SQL,XML,STUFF]

datatable: true
author: Ian Fogelman # Add name author (optional)
---

This question arose on the SO forum some months ago.
<br>
<br>
![Before](/assets/img/Stuff1.PNG)
The poster had a number of rows per UserID and wanted a result set of one row per userId with all the roles that would be associated to them.
![After](/assets/img/Stuff2.PNG)
<br>
<br>

The question appears easy, but it does take a bit of critical thinking because any simple join on userId will result in many rows.

My solution was to use the STUFF and For XML trick to work for you.

