---
layout: post
title:  On a meeting with adventures
date:   2019-06-23
description: How to use python to auto pull data and display in PowerBi # Add post description (optional)
img: pb.png # Add image post (optional)
tags: [PowerBi, Python, Business Intelligence]

datatable: true
author: # Add name author (optional)
---


Today I will discuss how to pull data live with a python script and then display that data in a powerBi python visual.

{% highlight Python %}
import pandas as pd
import datetime
import pandas_datareader.data as web
import matplotlib.pyplot as plt
import matplotlib as mpl
from pandas import Series, DataFrame
from matplotlib import style
from datetime import datetime
import datetime

def pullstockdata(start,end,symbol):
    start = datetime.datetime.strptime(start, '%m/%d/%Y')
    end   = datetime.datetime.strptime(end, '%m/%d/%Y')
    stock = 'GOOG'
    df = web.DataReader(stock, 'yahoo', start, end)
    close_px = df['Adj Close']
    mavg100 = close_px.rolling(window=100).mean()
    mavg150 = close_px.rolling(window=150).mean()
    mpl.rc('figure', figsize=(8, 7))
    mpl.__version__
    # Adjusting the style of matplotlib
    style.use('ggplot')
    close_px.plot(label=stock)
    mavg100.plot(label='mavg100')
    mavg150.plot(label='mavg150')
    plt.legend()
    plt.show()
{% endhighlight %}   
