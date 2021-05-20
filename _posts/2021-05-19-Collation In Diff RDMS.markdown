---
layout: post
title:  Database Collations in Different RDMS
date:   2021-05-19
description: 2021-05-19-Database Collations in Different RDMS
img: SF.png # Add image post (optional)
tags: [SnowFlake,SQL,AWS,CRONTAB,CRON]

datatable: true
author: Ian Fogelman # Add name author (optional)
---

<meta property="og:title" content="Database Collations in Different RDMS">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">



In a best case scenario, you may never notice your databases collation at all. 

But lets take a step back and ask what actually is collation to begin with? 

Collation is the set of character encoding which your data is stored with. 

Collation can be applied on the column level particular ally with the VARCHAR data type.





Lets take a look at a few different implementations of none specified collation to see how different RDMS's handle this.



First we will test SQL server, not specified any type of collation with a column and observe the default behavior.



{% highlight SQL %}

CREATE TABLE #TEMP_A
(
TEST VARCHAR(50)
)

INSERT INTO #TEMP_A
VALUES('ABC')


SELECT * FROM #TEMP_A
WHERE TEST = 'Abc';

SELECT * FROM #TEMP_A
WHERE TEST = 'ABc'

SELECT * FROM #TEMP_A
WHERE TEST = 'ABC'

{% endhighlight %}

