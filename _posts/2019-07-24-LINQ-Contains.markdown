---
layout: post
title:  LINQ Contains
date:   2019-07-24
description: A quick look at how to restore a database from a mdf file. # Add post description (optional)
img: cs.png # Add image post (optional)
tags: [C#,LINQ]

datatable: true
author: Ian Fogelman # Add name author (optional)
---
<br>
<br>
An example of how to apply a IN() statement to a LINQ query.
Traditionally in SQL you can use the IN() operator to narrow in on a list of Ids or string values.
<br>
<br>

{% highlight c# %}
var ItemIds = new int[] {1001, 1002, 1003};

var a = 	    from x in Items
                where ItemIds.Contains(x.Id)
				orderby x.Id descending
                select new { x.Id, x.Ac,x.Name };
				
Console.WriteLine(string.Join(", ",a));
{% endhighlight %}   
