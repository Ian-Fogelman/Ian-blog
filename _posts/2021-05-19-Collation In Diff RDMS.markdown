---
layout: post
title:  Database Collations in Different RDMS
date:   2021-05-19
description: 2021-05-19-Database Collations in Different RDMS
img: sq.png # Add image post (optional)
tags: [RDMS,SQL,Collation,SQL-Concepts]

datatable: true
author: Ian Fogelman # Add name author (optional)
---

<meta property="og:title" content="Database Collations in Different RDMS">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">



In a best case scenario, you may never notice your databases collation at all. But lets take a step back and ask what actually is collation to begin with? 

Collation is the set of character encoding which your data is stored with. Collation can be applied on the column level particular ally with the VARCHAR data type. Lets take a look at a few different implementations of none specified collation to see how different RDMS's handle this. First we will test SQL server, not specified any type of collation with a column and observe the default behavior.

<br>

Collation can also cause different experiences with regards to querying your data out of your database. Probably the most influential piece of a collation format is the case sensitivity indicator. Lets examine the following collation setting : Latin1_General_CI_AI.

Lets break down the above collation string :

Latin1_General  = Character encoding

CI = Case Insensitive, will dictate if query results will return based on column exact matching. I.E. if your case sensitive "Dog" <> "DoG"

AI = Accent Insensitive, will dictate if accents affect the query results I.E. "pinata" <> "P~nata".

WI = Width Insensitive, although not specified the lack of the WS in the format string dictates width insensitivity.

KS = Kanatype-sensitive, although not specified the lack of the KS in the format string dictates kanatype insensitivty.



<br>



A little more discription for Width and Kanatatype.



Width Sensitive - Distinguishes between the two types of Japanese kana characters:  Hiragana and Katakana. If this option is not selected, SQL Server considers Hiragana and  Katakana characters to be equal for sorting purposes

Kanatype Sensitive - Distinguishes between a single-byte character and the same character  when represented as a double-byte character. If this option is not selected, SQL Server considers the single-byte  and double-byte representation of the same character to be identical  for sorting purposes.



<br>



Collation levels can be set on multiple locations, this is agnostic between RDMS but the the general theme is a higherarchy of priority. The hierarchy detailed is as follows : Server > Database > Table > Query, with the caveat of the possibility of schema level collation.

<br>

![Model Results](/assets/img/Collation.png)

<br>

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

