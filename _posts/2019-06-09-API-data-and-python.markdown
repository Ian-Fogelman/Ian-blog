---
layout: post
title:  API data and Python
date:   2019-06-09 13:32:20 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: py.png # Add image post (optional)
tags: [Python, Restful, JSON, API]

datatable: true
author: # Add name author (optional)
---

Today I will discuss how to retreive restful API data using python.

In particular we will make use of the following libraries:

requests,json and pandas all of the code in this post can be downloaded on my cloud anaconda page <here>.
    
First lets import our libraries, if you do not have these libraries, use the !pip install command to install them.

{% highlight python %}
import requests
import json
import pandas as pd
{% endhighlight %}  

For this demo we will use a free API that is hosted at http://api.icndb.com.
This particular API returns jokes about the famous western and kungfu guru Chuck Norris.
At its core we can achieve a simple get request as follows :

{% highlight python %}
url = """http://api.icndb.com/jokes/random?"""
result = requests.get(url)
print(result.text)
#{ "type": "success", "value": { "id": 467, "joke": "Chuck Norris can delete the Recycling Bin.", "categories": ["nerdy"] } }
{% endhighlight %}  

This is pretty good in terms of basic functionality and concepts, but what if we only care about the joke itself?
We can see it is nested inside the value portion of the return dictionary. For this we can enlist the help of the JSON library.

{% highlight python %}
url = """http://api.icndb.com/jokes/random?"""
r = requests.get(url)
print(result.text)
j=json.loads(r.text)
print(j['value']['joke'])
#Chuck Norris breaks RSA 128-bit encrypted codes in milliseconds.
{% endhighlight %}  

Here we get a more appealing result just the actual joke text returned from the API.

