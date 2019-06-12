---
layout: post
title:  SQLite-And-Python
date:   2019-06-11
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: py.jpg # Add image post (optional)
tags: [SQL, SQLite, Python, Pandas]

datatable: true
author: # Add name author (optional)
---

Today I will be covering using SQLite in conjuction with Python you can find this notebook <here>.
SQLite is pretty much the most light weight database backend around, it is easy to setup and use and dosent come with much of the overhead of other DBMS. It has a limited number of data types available, but if you can be creative in you storage requirements it can be a great solution. I recomend downloading a SQLite browser tool <here> to help strucutre of what we will be creating.

< br >
< br >

 ![SQLite Browser](/assets/screenshot.jpg)
 
< br >
< br >

Firstly lets import our modules:

{% highlight python %}
import requests
import pandas as pd
import json
import sqlite3

{% endhighlight %}  

< br >
< br >

I should mention we will be using cursors throughout the demo, think of a curosr as a worker thread, a layer between you and your database. In order to retrieve or create objects or data in your database you will need to use the cursor to handle those operations.

< br >
< br >

First lets define the name of our database, in this case I am going to use "example.db" as the database name.

{% highlight python %}
conn = sqlite3.connect('example.db')
{% endhighlight %}  

< br >
< br >

This conn will be used in conjection with a cursor to perform work in our example.db.
Next lets create a table inside our database called User

{% highlight python %}
c = conn.cursor()
c.execute('''CREATE TABLE USER (
 User_id INTEGER PRIMARY KEY AUTOINCREMENT,
 first_name TEXT NOT NULL,
 last_name TEXT NOT NULL);''')
conn.commit()
conn.close()
{% endhighlight %} 

< br >
< br >  

The table has been created, lets insert our first row of data.

< br >
< br >  

{% highlight python %}
c = conn.cursor()
c.execute("INSERT INTO USER (first_name, last_name) VALUES ('Ian','Fogelman')")
conn.commit()
{% endhighlight %} 

< br >
< br >  

Notice the <strong> conn.commit() </strong> this will be required before any operations can be saved to a database.

< br >
< br > 

If we go back to our database we can see the record inside.



