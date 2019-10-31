---
layout: post
title:  Python and AWS S3
date:   2019-10-31
description: Storing and retreiving files in S3 with Python
img: aws.png # Add image post (optional)
tags: [AWS,Python,Pandas]

datatable: true
author: Ian Fogelman # Add name author (optional)
---
<meta property="og:title" content="How to get started creating a SSAS tabular model + deployment">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">

<br>
<br>

Once you have SSDT installed, click new project and under analysis services click Tabular for the project type, give your project a descriptive name.
<br>
<br>

<br>
<br>
Make a note of the instance that you have your datawarehouse on we will need that information during the setup of our tabular model.
<br>
<br>

{% highlight SQL %}
VALUATE
SalesOrderDetail
{% endhighlight %}

<br>
<br>

![Features](/assets/img/SSASI010.png)
