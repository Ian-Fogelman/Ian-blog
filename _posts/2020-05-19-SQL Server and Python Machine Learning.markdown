---
layout: post
title:  SQL Server and Python Machine Learning
date:   2020-05-19
description: 2020-05-19-SQL Server and Python Machine Learning
img: py.png # Add image post (optional)
tags: [Machine Learning, SQL Server, Python, Pandas, SciKitLearn]

datatable: true
author: Ian Fogelman # Add name author (optional)
---

<meta property="og:title" content="Dynamic Python">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">

<br>
<br>
<br>
Today we will be utalizing the new feature in 2017+ SQL Server enviornments, machine learning via Python native script execution. In this tutorial we will recap how to load a Pickle file into a SQL server database stored as binary data. Retreive that file and load it as a machine learning model and apply the model predictions to our SQL Server data. So the steps to achieve our predicts are as follows:

I. Create a Table to hold our binary Models<br>
II. Create a table to hold our machine learning data<br>
III. Create a procedure to load our pickled model into the ML table<br>
IV. Fire stored procedure to load the pickle file<br>
V. Create another stored procedure to take in our SQL server data and model and return a result set<br>
VI. Compare the result set from the model to our data
<br>
<br>
<br>

Files : Iris.csv
	ML Init.sql
<br>
<br>
<br>

{% highlight SQL %}

DECLARE @DYNSQL VARCHAR(MAX)
DECLARE @TABLENAME VARCHAR(MAX)

IF OBJECT_ID('TEMPDB..#TEMP_A') IS NOT NULL DROP TABLE #TEMP_A
CREATE TABLE #TEMP_A
(
TABLENAME VARCHAR(MAX)
)


INSERT INTO #TEMP_A
SELECT 'SYS.all_objects'
UNION
SELECT 'SYS.all_views'
UNION
SELECT 'SYS.all_columns'



WHILE EXISTS(SELECT * FROM #TEMP_A)
BEGIN
	SET @TABLENAME = (SELECT TOP 1  TABLENAME  FROM #TEMP_A)
	SET @DYNSQL = 'SELECT * FROM ' +  @TABLENAME 
	EXEC(@DYNSQL)
	DELETE FROM #TEMP_A WHERE TABLENAME = @TABLENAME
END

DROP TABLE #TEMP_A

{% endhighlight %}

<br>
<br>
<br>

Here we can see that we have a static part of a query 'SELECT * FROM ' and the dynamic part is the table name.
<br>
<br>
The beauty of this approach is we can achieve very complex query criteria and reduce our line of code counts by utalizing CURSORs and while loops, I prefer the ladder.
<br>
<br>

This same functionality is achievable in Python, here is a code sample :
<br>
<br>

{% highlight Python %}

prog = 'print("The sum of 5 and 10 is", (5+10))'
exec(prog)
#The sum of 5 and 10 is 15
{% endhighlight %}

<br>
<br>
