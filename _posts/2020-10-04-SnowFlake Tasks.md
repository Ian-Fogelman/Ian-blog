---
layout: post
title:  Snowflake Tasks
date:   2020-09-03
description: 2020-10-04-Snowflake Tasks
img: SF.png # Add image post (optional)
tags: [SnowFlake,SQL,AWS,CRONTAB,CRON]

datatable: true
author: Ian Fogelman # Add name author (optional)
---

<meta property="og:title" content="Snowflake tasks">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">

<br>
<br>

Today we take a look at a newish feature in Snowflake called Tasks.
Tasks allows you to create a SQL script or procedure and schedule it to run on your Snowflake instance.
The Snowflake task engine is a CRON variant and should look familar syntactically if you are an avid linux user.
CRON or CRONTAB is the linux version of windows task schedule. It is extermly simplified in regards to how it runs a job. It supports a few parameters and points to a .sh or other script file. The paramters control the frequency of the job being run, days of week and time.
<br>
<br>
| Column 1 Header | Column 2 Header | Column 3 Header |
| --------------- | --------------- | --------------- |
| Row 1 Column 1 | Row 1 Column 2 | Row 1 Column 3 |
| Row 2 Column 1 | Row 2 Column 2 | Row 2 Column 3 |
| Row 3 Column 1 | Row 3 Column 2 | Row 3 Column 3 |
<br>
<br>

MIN | HOUR | DAY | MON | WEEKDAY | Script| Description
--- | --- | --- | --- | --- | --- | --- |
* | * | * | * | * | script.sh |This task would run every minute,hour and day
0 | 17 | * | * | sun | SundayScript.sh| Every Sunday at 5 PM
* |10 | * | * | * | /scripts/EveryTenMinutes.sh | Every ten minutes

<br>
<br>

Snowflake has recently introduced this functionality so lets take a quick look at how to create our first task on our Snowflake instance.


<br>
<br>

First lets create a table to store some data into. This will be the table targeted in our Snowflake task.

{% highlight SQL %}
Create table...

{% endhighlight %}


 <table style="width:100%">
  <tr>
    <th>Firstname</th>
    <th>Lastname</th>
    <th>Age</th>
  </tr>
  <tr>
    <td>Jill</td>
    <td>Smith</td>
    <td>50</td>
  </tr>
  <tr>
    <td>Eve</td>
    <td>Jackson</td>
    <td>94</td>
  </tr>
</table> 
<br>
<br>

Next lets create and run a insert statment.

<br>
<br>

Now lets create the task which will use the SQL and the statement.

{% highlight SQL %}
CREATE TASK mytask_hour
  WAREHOUSE = mywh
  SCHEDULE = 'USING CRON 0 9-17 * * SUN America/Los_Angeles'
  TIMESTAMP_INPUT_FORMAT = 'YYYY-MM-DD HH24'
AS
INSERT INTO mytable(ts) VALUES(CURRENT_TIMESTAMP);

{% endhighlight %}

<br>
<br>

Now all we need to do is wait and scan the table, here we can see every minute the task has ran and inserted our data.

<br>
<br>

This is an extremly basic example, I will do some more complicated implementations of Snowflake tasks using SnowPipe and stored procedures soon. 
