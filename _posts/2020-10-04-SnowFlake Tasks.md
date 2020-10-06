---
layout: post
title:  Snowflake Tasks
date:   2020-10-04
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

![Model Results](/assets/img/CronMeta.PNG)

<br>
<br>
Snowflake has recently introduced this functionality so lets take a quick look at how to create our first task on our Snowflake instance.


<br>
<br>

First lets create a table to store some data into. This will be the table targeted in our Snowflake task.

{% highlight SQL %}
CREATE DATABASE MYTASKEXAMPLES;

USE DATABASE MYTASKEXAMPLES;

CREATE OR REPLACE TABLE LOGTABLE
(
LogDT datetime
)

{% endhighlight %}

<br>
<br>

Next lets create and run a insert statment.

{% highlight SQL %}
INSERT INTO LOGTABLE(LogDT) VALUES(CURRENT_TIMESTAMP);
{% endhighlight %}

<br>
<br>

Now lets create the task which will use the SQL and the statement.

<br>
<br>

{% highlight SQL %}

CREATE OR REPLACE TASK MINUTEINSERT
  WAREHOUSE = COMPUTE_WH
  SCHEDULE = 'USING CRON * * * * * America/New_York'
  TIMESTAMP_INPUT_FORMAT = 'YYYY-MM-DD HH24'
AS
INSERT INTO LOGTABLE(LogDT) VALUES(CURRENT_TIMESTAMP);

{% endhighlight %}
<br>
<br>

This task create the task in Snowflake, but it will be initated in a suspended state. To turn the command on we must alter the task.
<br>
<br>

{% highlight SQL %}
ALTER TASK MINUTEINSERT RESUME;
{% endhighlight %}
<br>
<br>
Now all we need to do is wait and scan the table, here we can see every minute the task has ran and inserted our data.

<br>
<br>
{% highlight SQL %}
SELECT * FROM LOGTABLE
{% endhighlight %}

<br>
<br>

![RESULTS](/assets/img/TASKQUERY.PNG)

<br>
<br>
Turn off the task so it does not run up compute charges!
{% highlight SQL %}
ALTER TASK MINUTEINSERT SUSPEND; 
{% endhighlight %}
<br>
<br>
This is an extremly basic example, I will do some more complicated implementations of Snowflake tasks using SnowPipe and stored procedures soon. 
You can also do preceding steps, notifications and many other nifty things with Snowflake tasks.

