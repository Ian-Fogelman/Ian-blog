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
This question arose on the SO forum some months ago.
<br>
<br>
![Before](/assets/img/Stuff1.PNG)

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


{% highlight SQL %}

{% endhighlight %}

<br>
<br>

https://rextester.com/AVTOWD47990
