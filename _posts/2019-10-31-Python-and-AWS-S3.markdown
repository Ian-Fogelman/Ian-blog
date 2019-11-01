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

Now lets create a bucket in S3...

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

Lets load our file into our bucket by reading the secret and access key we just loaded into our os enviorment variables.

<br>
<br>
{% highlight Python %}
import boto3
from botocore.exceptions import NoCredentialsError

ACCESS_KEY = str(os.environ['awsAccessKey'])
SECRET_KEY = str(os.environ['awsSecretKey'])


def upload_to_aws(local_file, bucket, s3_file):
    s3 = boto3.client('s3', aws_access_key_id=ACCESS_KEY,
                      aws_secret_access_key=SECRET_KEY)

    try:
        s3.upload_file(local_file, bucket, s3_file)
        print("Upload Successful")
        return True
    except FileNotFoundError:
        print("The file was not found")
        return False
    except NoCredentialsError:
        print("Credentials not available")
        return False
uploaded = upload_to_aws('filename.txt', 'bucketname', 'filename.txt')
{% endhighlight %}

<br>
<br>

{% highlight Python %}

import os
import boto3
import pandas as pd
import sys

if sys.version_info[0] < 3: 
    from StringIO import StringIO # Python 2.x
else:
    from io import StringIO # Python 3.x

client = boto3.client('s3', aws_access_key_id=ACCESS_KEY,
        aws_secret_access_key=SECRET_KEY)

bucket_name = 'bucket'

object_key = 'XXX.csv'
csv_obj = client.get_object(Bucket=bucket_name, Key=object_key)
body = csv_obj['Body']
csv_string = body.read().decode('utf-8')

df = pd.read_csv(StringIO(csv_string))
{% endhighlight %}


<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=UA-151332647-1"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'UA-151332647-1');
</script>
