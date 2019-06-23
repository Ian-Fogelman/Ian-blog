---
layout: post
title:  WebRequest In ManagementStudio
date:   2019-06-22 
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: sq.jpg # Add image post (optional)
tags: [SQL, ManagementStudio, Restful, API]

datatable: true
author: # Add name author (optional)
---

Have you ever needed to read API data from an endpoint and then turn around and load that into your database? 
I would consider Python requests library in combination with SQLalchemy for a task like that, but did you know you can do it all in Management Studio?
<br>
<br>

Lets take a look at the following API endpoint : <strong > http://api.open-notify.org/iss-now.json </strong>.
<br>
This endpoint returns the current position of the ISS in lat / long format.

<br>
<br>

Before we are able to do this we must prepare our SQL enviorment, you might lack permissions to do this on a production instance, but a local SQL express version of SQL server will do just fine if that is the case.

<br>
<br>

{% highlight SQL %}
sp_configure 'show advanced options', 1 
GO 
RECONFIGURE; 
GO 
sp_configure 'Ole Automation Procedures', 1 
GO 
RECONFIGURE; 
GO 
sp_configure 'show advanced options', 1 
GO 
RECONFIGURE;
{% endhighlight %}  

<br>
<br>

Next we will declare variables for the endpoint, response and data.
You can change the operation type to POST data for instance, in this demo we will simply be using GET.

<br>
<br>


{% highlight SQL %}
DECLARE @authHeader NVARCHAR(64);
DECLARE @contentType NVARCHAR(64);
DECLARE @postData NVARCHAR(2000);
DECLARE @responseText NVARCHAR(2000);
DECLARE @responseXML NVARCHAR(2000);
DECLARE @ret INT;
DECLARE @status NVARCHAR(32);
DECLARE @statusText NVARCHAR(32);
DECLARE @token INT;
DECLARE @url NVARCHAR(256);
DECLARE @list VARCHAR(MAX)
DECLARE @CNT VARCHAR(MAX)


SET @authHeader = 'BASIC 0123456789ABCDEF0123456789ABCDEF';
SET @contentType = 'application/json';

SET @url = 'http://api.open-notify.org/iss-now.json'

-- Open the connection.
EXEC @ret = sp_OACreate 'MSXML2.ServerXMLHTTP', @token OUT;
IF @ret <> 0 RAISERROR('Unable to open HTTP connection.', 10, 1);

-- Send the request.
EXEC @ret = sp_OAMethod @token, 'open', NULL, 'get', @url, 'false';
EXEC @ret = sp_OAMethod @token, 'setRequestHeader', NULL, 'Authentication', @authHeader;
EXEC @ret = sp_OAMethod @token, 'setRequestHeader', NULL, 'Content-type', 'application/json';
EXEC @ret = sp_OAMethod @token, 'send', NULL, NULL; -- IF YOUR POSTING, CHANGE THE LAST NULL TO @postData

-- Handle the response.
EXEC @ret = sp_OAGetProperty @token, 'status', @status OUT;
EXEC @ret = sp_OAGetProperty @token, 'statusText', @statusText OUT;
EXEC @ret = sp_OAGetProperty @token, 'responseText', @responseText OUT;

-- Show the response.
PRINT 'Status: ' + @status + ' (' + @statusText + ')';
PRINT 'Response text: ' + @responseText;

-- Close the connection.
EXEC @ret = sp_OADestroy @token;
IF @ret <> 0 RAISERROR('Unable to close HTTP connection.', 10, 1);
{% endhighlight %} 

<br>
<br>

As you can see in the print window we will have our response from the ISS, pretty neat!


<br>
<br>


![My helpful screenshot](/assets/img/WA001.PNG)

