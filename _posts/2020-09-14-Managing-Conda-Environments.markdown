---
layout: post
title:  Managing Conda Environments for Jupyter Notebooks
date:   2020-09-14
description: How to build and load conda environments in Jupyter notebooks
img: AN.png # Add image post (optional)
tags: [Machine Learning, SQL Server, Python, Pandas, SciKitLearn]

datatable: true
author: Ian Fogelman # Add name author (optional)
---

<meta property="og:title" content="Managing Conda Environments for Jupyter Notebooks">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">

<br>
<br>

Today we take a look at anaconda, not only is anaconda a flavor of Python distribution but it also a powerful enviornment management tool.

<br>
<br>

I am well aware that you can create and manage Python enviornments and install packages into virtual Python enviornments.
Recently however the need for me to run my Jupyter notebook instance in a different version of Python arose.
This can happen do to a particular package not being updated or compaitable with the most recent versions of Python.
In my case this conflict was caused from the difference in Python 3.7 -> 3.8.

<br>
<br>

Now I knew that I could just download the previous version of Anaconda so that Python would be 3.7, but this is not the correct answer!
There is a lot of misleading information or incomplete steps into loading the Anaconda enviornment so that your jupter notebook instance can run it.
Today I detail how this can be accomplished step by step.

<br>
<br>
