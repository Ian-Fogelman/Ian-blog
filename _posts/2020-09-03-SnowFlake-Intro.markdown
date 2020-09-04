---
layout: post
title:  SQL Server and Python Machine Learning
date:   2020-05-19
description: 2020-05-19-SQL Server and Python Machine Learning
img: SF.png # Add image post (optional)
tags: [Machine Learning, SQL Server, Python, Pandas, SciKitLearn]

datatable: true
author: Ian Fogelman # Add name author (optional)
---

<meta property="og:title" content="SQL Server and Python Machine Learning">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">

<br>
<br>
Today we will be utalizing a new feature in 2017+ SQL Server enviornments, machine learning via Python native script execution. In this tutorial we will recap how to load a Pickle file into a SQL server database stored as binary data. Retreive that file and load it as a machine learning model and apply the model predictions to our SQL Server data. The steps to achieve our predicts are as follows:

<br>
<br>
