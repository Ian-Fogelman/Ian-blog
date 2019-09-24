---
layout: post
title:  Creating C# CLR's For SQL Server
date:   2019-09-24
description: An Example of creating a CLR to be utalized in SQL Server
img: py.png # Add image post (optional)
tags: [SQL, CLR, Management Studio]

datatable: true
author: Ian Fogelman # Add name author (optional)
---


Before we begin run the following code to prepare your SQL enviorment for CLR integration.


EXEC sp_configure 'clr enabled', 1;  RECONFIGURE WITH OVERRIDE;

ALTER DATABASE Debug SET TRUSTWORTHY ON;




First Create the correct type of project...

Next add the following depencicies via the nuget package manager....


Next pase this code into the project.


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

Click build.

Make a note of the build location on your file system, we will need this for the T-SQL code to create the assembly that references the .dll that was just created.



CREATE ASSEMBLY SentimentParser from 'C:\Users\XXX\source\repos\CLRFunc\CLRFunc\bin\Debug\CLRFunc.dll' WITH PERMISSION_SET = SAFE  
GO 

CREATE FUNCTION ParseSentiment(@input NVARCHAR(MAX)) 
RETURNS FLOAT   
AS EXTERNAL NAME SentimentParser.Sentiment.ParseSentiment;   
GO  

SELECT dbo.ParseSentiment('A BRIGHT AND BEAUTIFUL DAY');  



