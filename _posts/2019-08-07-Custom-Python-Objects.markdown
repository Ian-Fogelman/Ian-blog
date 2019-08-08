---
layout: post
title:  Working With custom objects and Soup
date:   2019-08-07
description: Beautifulsoup and custom object
img: py.png # Add image post (optional)
tags: [Python,Requests,Beautifulsoup,OOP]

datatable: true
author: Ian Fogelman # Add name author (optional)
---

Today we take a look at creating custom objects in Python.
<br />
https://anaconda.org/IanFogelman/working-with-python-classes-and-soup/notebook

<br />
<br />
<br />

{% highlight Python %}

from bs4 import BeautifulSoup
import urllib.request
import nltk
import matplotlib.pyplot as plt
import pyodbc
import re
import requests
import pandas as pd
from datetime import datetime



TableRowList = []
class TableRow:
    def __init__(self,OrderDate,Region,Rep,Item,Units,UnitCost,Total):
        self.OrderDate = OrderDate
        self.Region = Region
        self.Rep = Rep
        self.Item = Item
        self.Units = Units
        self.UnitCost = UnitCost
        self.Total = Total
        self.ScrapeTime = str(datetime.now().strftime('%Y-%m-%d %H:%M %p'))




r =requests.get('https://www.contextures.com/xlSampleData01.html')
soup = BeautifulSoup(r.text,'lxml')
table = soup.find('table')
rows = table.find_all('tr')
for row in rows:
    data = row.find_all('td')
    OD = data[0].text.strip()
    Region = data[1].text.strip()
    Rep = data[2].text.strip()
    Item = data[3].text.strip()
    Units = data[4].text.strip()
    UnitCost = data[5].text.strip()
    Total = data[6].text.strip()
    t = TableRow(OD,Region,Rep,Item,Units,UnitCost,Total)
    TableRowList.append(t)



i = 1
for x in TableRowList:
    if(i <= 5):
        print(x)
    i += 1


{% endhighlight %}
