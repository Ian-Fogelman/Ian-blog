---
layout: post
title:  Creating C# CLR's For SQL Server
date:   2019-09-24
description: An Example of creating a CLR to be utalized in SQL Server
img: sq.jpg # Add image post (optional)
tags: [SQL, CLR, Management Studio]

datatable: true
author: Ian Fogelman # Add name author (optional)
---


Before we begin run the following code to prepare your SQL enviorment for CLR integration.

{% highlight SQL %}

EXEC sp_configure 'clr enabled', 1;  RECONFIGURE WITH OVERRIDE;

ALTER DATABASE Debug SET TRUSTWORTHY ON;

{% endhighlight %}



First Create the correct type of project...

![CLR Project Type](/assets/img/CLR1.PNG)

Next add the following depencicies via the nuget package manager....

![CLR Project Type](/assets/img/CLR2.PNG)

Next pase this code into the project.

{% highlight C# %}
using Microsoft.SqlServer.Server;
using System.Data.SqlClient;
using VaderSharp;

public class Sentiment
{
    [SqlFunction(DataAccess = DataAccessKind.Read)]
    public static double ParseSentiment(string input)
    {
        SentimentIntensityAnalyzer analyzer = new SentimentIntensityAnalyzer();
        var results = analyzer.PolarityScores(input);
        return results.Compound;

    }
}

{% endhighlight %}

Click build.

Make a note of the build location on your file system, we will need this for the T-SQL code to create the assembly that references the .dll that was just created.

Now that we have the dll built, we can call this function that will run the C# code from SQL server, to do this we must first create the assembly, then create a database object to reference that external assembly. This can be any type of database object ( stored procedure, table value function, scalar function) in this example I am using a scalar function.

{% highlight SQL %}

CREATE ASSEMBLY SentimentParser from 'C:\Users\XXX\source\repos\CLRFunc\CLRFunc\bin\Debug\CLRFunc.dll' WITH PERMISSION_SET = SAFE  
GO 

CREATE FUNCTION ParseSentiment(@input NVARCHAR(MAX)) 
RETURNS FLOAT   
AS EXTERNAL NAME SentimentParser.Sentiment.ParseSentiment;   
GO  

SELECT dbo.ParseSentiment('A BRIGHT AND BEAUTIFUL DAY'); 

{% endhighlight %}


