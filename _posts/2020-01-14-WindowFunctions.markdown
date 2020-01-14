---
layout: post
title:  Using SQL Window and Offset Window Functions
date:   2020-01-14
description: Using SQL Window and Offset Window Functions
img: sq.png # Add image post (optional)
tags: [SQL,Window Functions]

datatable: true
author: Ian Fogelman # Add name author (optional)
---
<br>
<br>
Hello There!
<br>
<br>
Today I was faced with a w task of diseminating sequence of events in a report, so I thought I would share how I approached to task.
<br>
<br>
Using window functions such as ROW_NUMBER(), RANK(), DENSE_RANK() and NTILE() we can gain insight into how data are ranked amounts a result set. Each of these functions have specific use cases, rank and dense rank treat ties amounts data slightly differently while ROW_NUMBER() is useful for assigning a ID field to a result set if for some reason one is not already present.

<br>
<br>
Have a look at the data below which has 4 fields, OrderId,CustId,Val and OrderDate. Typically you will be compring values based on a sequence of a date field. In this example we will apply first the window functions and secondly offset window functions.

<br>
<br>

  <div class="container-fluid">
    <table class="datatable table table-hover table-bordered">
      <thead>
        <tr>
          <th>OrderId</th>
          <th>CustId</th>
          <th>Val</th>
          <th>OrderDate</th>
        </tr>
      </thead>
      <tfoot>
      </tfoot>
      <tbody>
        <tr>
          <td>10782</td>
          <td>12</td>
          <td>18</td>
          <td>2017-01-01</td>
        </tr>
		    <tr>
          <td>10807</td>
          <td>12</td>
          <td>23</td>
          <td>2016-01-01</td>
        </tr>
        <tr>
          <td>10586</td>
          <td>12</td>
          <td>28</td>
          <td>2018-01-01</td>
        </tr>
        <tr>
          <td>10767</td>
          <td>28</td>
          <td>30</td>
          <td>2017-02-01</td>
        </tr>
        <tr>
          <td>10898</td>
          <td>28</td>
          <td>33</td>
          <td>2016-02-01</td>
        </tr>
        <tr>
          <td>10900</td>
          <td>28</td>
          <td>36</td>
          <td>2018-02-01</td>
        </tr>
        <tr>
          <td>10883</td>
          <td>36</td>
          <td>36</td>
          <td>2017-03-01</td>
        </tr>
        <tr>
          <td>11051</td>
          <td>36</td>
          <td>40</td>
          <td>2016-03-01</td>
        </tr>
        <tr>
          <td>10815</td>
          <td>36</td>
          <td>45</td>
          <td>2018-03-01</td>
        </tr>
      </tbody>
    </table>
  </div>

<br>
<br>
Lets first apply the window function query to our data, this will show how each treats data slightly differently.

<br>
<br>

{% highlight SQL %}

SELECT 
ROW_NUMBER() OVER(ORDER BY VAL) AS ROWNUM,
RANK() OVER(ORDER BY VAL) AS RANK,
DENSE_RANK() OVER(ORDER BY VAL) AS DENSE_RANK,
NTILE(5) OVER(ORDER BY VAL) AS NTILE,
VAL

FROM TEMP_ORDERVALUES
ORDER BY VAL
{% endhighlight %}

<br>
<br>

This will produce the result set

<br>
<br>

<div class="container-fluid">
    <table class="datatable table table-hover table-bordered">
      <thead>
        <tr>
          <th>RowNum</th>
          <th>Rank</th>
          <th>Dense_Rank</th>
          <th>Ntile</th>
	  <th>Val</th>
        </tr>
      </thead>
      <tfoot>
      </tfoot>
      <tbody>
        <tr>
          <td>1</td>
          <td>1</td>
          <td>1</td>
          <td>1</td>
	  <td>18</td>
        </tr>
	<tr>
          <td>2</td>
          <td>2</td>
          <td>2</td>
          <td>1</td>
	  <td>23</td>
        </tr>
	<tr>
          <td>3</td>
          <td>3</td>
          <td>3</td>
          <td>2</td>
	  <td>28</td>
        </tr>
	<tr>
          <td>4</td>
          <td>4</td>
          <td>4</td>
          <td>2</td>
	  <td>30</td>
        </tr>
	<tr>
          <td>5</td>
          <td>5</td>
          <td>5</td>
          <td>3</td>
	  <td>33</td>
        </tr>
	<tr>
          <td>6</td>
          <td>6</td>
          <td>6</td>
          <td>3</td>
	  <td>36</td>
        </tr>
	<tr>
          <td>7</td>
          <td>6</td>
          <td>6</td>
          <td>3</td>
	  <td>36</td>
        </tr>
	<tr>
          <td>8</td>
          <td>8</td>
          <td>7</td>
          <td>4</td>
	  <td>40</td>
        </tr>
	<tr>
          <td>9</td>
          <td>9</td>
          <td>8</td>
          <td>5</td>
	  <td>45</td>
        </tr>
      </tbody>
    </table>
  </div>

<br>
<br>

https://rextester.com/AVTOWD47990
