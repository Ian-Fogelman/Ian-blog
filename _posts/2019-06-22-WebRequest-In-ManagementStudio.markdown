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
This endpoint returns the current position of the ISS in lat / long format.

<br>
<br>


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
EXEC @ret = sp_OAMethod @token, 'send', NULL, NULL;

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

