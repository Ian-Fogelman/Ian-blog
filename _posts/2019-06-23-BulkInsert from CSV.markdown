---
layout: post
title:  Bulk Insert From CSV
date:   2019-06-23
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: sq.jpg # Add image post (optional)
tags: [SQL, CSV, BulkInsert]

datatable: true
author: # Add name author (optional)
---



{% highlight SQL %}

CREATE TABLE #TMP
(
[Internet access state] VARCHAR(MAX),
[Device Type] VARCHAR(MAX),
[Client Name] VARCHAR(MAX),
[Client IP address] VARCHAR(MAX)

)

BULK INSERT  #TMP
FROM 'C:\Users\XXX\YYYY\MyData.csv'
WITH (DATAFILETYPE = 'WIDECHAR',FIELDTERMINATOR = ',', ROWTERMINATOR = '\N');
GO

SELECT * FROM #TMP

{% endhighlight %}  
