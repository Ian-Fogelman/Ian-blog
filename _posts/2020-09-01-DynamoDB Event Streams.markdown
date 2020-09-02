---
layout: post
title:  DynamoDB Event Streams
date:   2020-09-01
description: 2020-09-01-DynamoDB Event Streams
img: AWS.png # Add image post (optional)
tags: [Python, AWS, DynamoDB, EventStreams,Lambda]

datatable: true
author: Ian Fogelman # Add name author (optional)
---

<meta property="og:title" content="DynamoDB Event Streams">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">

<br>
<br>

Intro to DynamoDB Event Streams

Problem
You are looking for a database endpoint that has a very quick response time is schema less and can alert you when data changes. For this tip we will implement this solution using AWS – DynamoDB, DynamoDB is AWS’s schema less database solution. In This tip we take a look at how to implement a DynamoDB table, build a lambda function that will alert our slack channel when data in that table is changed.
<br>
<br>

Project Workflow
<br>
<br>

![](/assets/img/DES1_image1.png)

For this project we will be working in 3 main workspaces, Slack, your personal work station and AWS.
To complete this project, you will need:
<br>
<br>

•	A free tier Slack account
•	An AWS Free tier account
•	Python installed on your work station
 
 

Solution – Step 1 Create the Slack Channel
This same logic can be implemented with MS Teams or other webhook supported technologies, but in this example, I will be implementing with slack. If you do not have a slack account you can sign up for free here: https://slack.com/get-started#/ . Once you have an account, we need to create a channel, click the + button next to channels >> Create a 
channel. 

 ![](/assets/img/DES1_image2.png)

Name your channel and click create 

![](/assets/img/DES1_image3.png)
 

Solution – Step 2 Enable app Webhook
Now that we have the channel created, we need to add an “app” to the channel. Click the Add an App link in the newly created channel header:

![](/assets/img/DES1_image4.png)

In the search bar type webhook, the one we want is Incoming WebHook for our channel, click install: 

![](/assets/img/DES1_image5.png)






Click Add to Slack
 

![](/assets/img/DES1_image6.png)

Select the channel you want the notifications to go to, click “Add Incoming WebHooks Integration”:

![](/assets/img/DES1_image7.png)
 
Take a note of the URL provided on the following screen, this URL is a unique endpoint that points to your particular slack channel. 

![](/assets/img/DES1_image8.png)


Solution – Step 3 Create the Python Virtual Env
Next, we need to create a virtual environment that our AWS – Lambda function will use to send data to our slack channel. Create a Folder Structure to hold your environment such as: 

{% highlight Python %}
C:\VirtualEnvs 
Open an Admin Command Prompt Window and issue the following commands :
cd C:\VirtualEnvs
python -m venv SlackNotifier
cd C:\VirtualEnvs\SlackNotifier\Scripts
Activate
pip install requests
cd C:\VirtualEnvs
python -m venv SlackNotifier
cd C:\VirtualEnvs\SlackNotifier\Scripts
Activate
pip install requests
deactivate
{% endhighlight %}
 
![](/assets/img/DES1_image9.png)

Next right click inside of C:\VirtualEnvs\SlackNotifier\Lib\site-packages and add a new file called lambda_function.py.
Add the following code, add the URL of your slack channel to the URL line.

{% highlight Python %}
import json
import requests

def lambda_handler(event, context):
    # TODO implement
    
    URL = """""" #add your webhookURL
    payload={"text": "A new user has signed up for you app!"}
    r = requests.post(URL,json=payload)
    print(requests.post(URL,json=payload))
    print(r.text)
    return {
        'statusCode': 200,
        'body': json.dumps('Message posted to Slack!')
    }

{% endhighlight %}

Select the entire contents of site-packages and zip them into “lambda_function.zip” .

![](/assets/img/DES1_image10.png)
 
Solution – Step 4 Create Super Admin Role in IAM
For our lambda function to work we need to assign permissions so that it can access DynamoDB, for this example we will simply create a SuperAdmin role and use that to ensure our function can perform all its duties.
For this login to your AWS console and navigate to IAM >> Roles.
 
![](/assets/img/DES1_image11.png)


Create Role
 
![](/assets/img/DES1_image12.png)

Select Lambda  >> Next : Permissions

![](/assets/img/DES1_image13.png)
 
Assign Admin rights to the role, Click Next : Tags

![](/assets/img/DES1_image14.png)

Click Next : Review

![](/assets/img/DES1_image15.png)

Name your role and click Create Role
 
![](/assets/img/DES1_image16.png)

Solution – Step 5 Create the Lambda Function
Now inside of you AWS console, navigate to the Lambda service. On the AWS Lambda dashboard, click create function.
 
![](/assets/img/DES1_image17.png)

Choose Author from Scratch, and select Python 3.8 for the Runtime and name “SlackNotifier” and under the choose or create an execution role dropdown select “Use Existing” and select the SuperAdmin role we created earlier.

![](/assets/img/DES1_image18.png)

Once the function is created you will see a screen as follows, all we need to do on this screen is upload the virtual environment that we created earlier.
Click Actions >> Upload a .zip File

![](/assets/img/DES1_image19.png)

Navigate to lambda_function.zip and upload.
 
![](/assets/img/DES1_image20.png)
 
![](/assets/img/DES1_image21.png)

 
![](/assets/img/DES1_image22.png)

![](/assets/img/DES1_image23.png)

 

We can setup a test event to make sure the function is posting to our channel correctly:
 
![](/assets/img/DES1_image24.png)

![](/assets/img/DES1_image25.png)

Now select Test from the Lambda Header menu:
 
![](/assets/img/DES1_image26.png)


You should get a message in your slack channel :
 
![](/assets/img/DES1_image27.png)

Now all we need to do is setup DynamoDB and configure it to use this function when new data is added.

![](/assets/img/DES1_image28.png)

Solution – Step 6 Create DynamoDB Table
Now in the AWS console navigate to the DynamoDB service and click create table , enter AppUsers as the table name and UserId as the partition key, click Create.

![](/assets/img/DES1_image29.png)

Wait for a few seconds and let the table get created, once it finished, we should have a view as follows: 

![](/assets/img/DES1_image30.png)

Solution – Step 7 Enable Streams
Now click manage Stream, 

![](/assets/img/DES1_image31.png)
 



Solution – Step 7 Create a trigger for Table
Now we create a trigger on the table, navigate to Triggers >> Existing Lambda function.

![](/assets/img/DES1_image32.png)

Point it to your existing lambda function:

![](/assets/img/DES1_image33.png)

![](/assets/img/DES1_image34.png)
 


Solution – Step 8 Test
Now that our trigger is setup, lets insert an Item into our dynamoDB table and see what happens.
Navigate to Items >> Create Item.

![](/assets/img/DES1_image35.png)

Enter a UserId and click Save.

![](/assets/img/DES1_image36.png)

And viola, if everything has been configured correctly, you will get a message inside your slack channel indicating a new user has signed up!
 

Next Steps
•	The beauty of DynamoDB is that it is schema-less, it provides ultimate flexibility to your data storage requirements. Try and add fields to the DynamoDB table and experiment with this flexibility.
•	This is a single example of what Lambda has to offer, experiment with the method of creating a virtual environment in Python and see what else you can accomplish (ex : send email, text, custom app integration).

