---
layout: post
title:  Managing Conda Environments for Jupyter Notebooks
date:   2020-09-14
description: How to build and load conda environments in Jupyter notebooks
img: AN.png # Add image post (optional)
tags: [Anaconda,conda,Environments]

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

In this example I am starting in Python 3.7 as my base or root environment, I will build a conda enviornment of 3.6 and emulate that Python version in jupyter notebook.

<br>
<br>

conda env list
jupyter kernelspec list

jupyter kernelspec uninstall unwanted-kernel

#1

conda install nb_conda
yes
<br>
<br>

#2
conda create -name Python_36 python==3.6
yes -> yes

<br>
<br>

#3
activate Python_36
conda install -c anaconda ipykernel
yes

<br>
<br>


Next you can add your virtual environment to Jupyter by typing:

python -m ipykernel install --user --name=Python_36


#4
Now restart your jupyter notebook instance by closing the current CMD that is running it and issue "jupyter notebooks" command.








