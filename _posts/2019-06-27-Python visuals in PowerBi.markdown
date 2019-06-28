---
layout: post
title:  Python visuals in PowerBi
date:   2019-06-27
description: How to use python to auto pull data and display in PowerBi # Add post description (optional)
img: pb.png # Add image post (optional)
tags: [PowerBi, Python, Business Intelligence]

datatable: true
author: # Add name author (optional)
---


Today I will discuss how to pull data live with a python script and then display that data in a powerBi python visual.
<br>
We will be pulling stock market data with the method, which accepts a start date, end date and stock ticker symbol.
<br>
<br>

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
    
pullstockdata('6/27/2017','6/27/2018','GOOG')
{% endhighlight %}   

<br>
<br>

First lets open powerBI, pull any data into the report.
![](/assets/img/Pb1.PNG)

Next get any dummy data you have and connect to it via csv file.
<br>
This data is not important because the real data will come from a pandas dataframe that we populate with our script.
<br>


Next drag the python visual to the canvas of the report.
![](/assets/img/Pb2.PNG)
<br>
You may be prompted to enabled scripting, click Enable.
![](/assets/img/Pb3.PNG)
<br>
Now drag any data field to the values field while the Python visual is highted.
![](/assets/img/Pb4.PNG)
<br>
Copy and paste the script to the code dialogue, you will need to ensure that each module is installed on your machine.
<br>
![](/assets/img/Pb4.PNG)
<br>
If you get errors just use pip to install the module it indicates as missing
<br>
![](/assets/img/Pb5.PNG)
<br>
Click the run button, you should see the data pulled and displayed in your Python visual!






