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

{% highlight python %}
def persistence(n):
    trg = n
    rounds = 0
    while(int(len(str(trg))) != 1):
        try:
            c = int(str(trg)[2])
        except:
            c = 1
        trg = int(str(trg)[0]) * int(str(trg)[1]) * c
        rounds += 1
    return rounds
{% endhighlight %}  
