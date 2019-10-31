---
layout: post
title:  Python and AWS S3
date:   2019-10-31
description: Storing and retreiving files in S3 with Python
img: AWS.png # Add image post (optional)
tags: [AWS,Python,Pandas]

datatable: true
author: Ian Fogelman # Add name author (optional)
---
<meta property="og:title" content="Storing and retreiving files in S3 with Python">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">

<br>
<br>

Amazon Web Services or (AWS) offers many fantastic services, maybe none so utilitarian as amazon S3. S3 stands for simple storage service, you can store just about anything in S3. Today I will give a qucik example of using the python package boto to do this.
<br>
<br>
I will assume that you already have an AWS account, to work with AWS programatically you will interact with the API and to do that we need to be authorized via client and secret...
<br>
<br>


<br>
<br>
Now that you have a client and secret, you can optionally store the keys in an envormental variable. This will allow you to reference the keys on your machine without hard coding the values.
<br>
<br>
{% highlight Python %}
import os

os.environ['awsAccessKey'] = 'xxxxxx'
os.environ['awsSecretKey'] = 'yyyyyy'

{% endhighlight %}

<br>
<br>

![Features](/assets/img/SSASI010.png)
