---
layout: post
title:  On a meeting with adventures
date:   2019-06-23
description: A quick look at how to restore a database from a mdf file. # Add post description (optional)
img: sq.png # Add image post (optional)
tags: [SQL Server, Database backup, Database restore]

datatable: true
author: Ian Fogelman # Add name author (optional)
---

Today we discuss a powerful topic, restoring a database into your sql server instance.
This can be done for a variety of reasons, if there is a critical error commited that deletes sensitive data and you do not have a differential backup you could have to restore the entire database.
That scenario has only happened to me a couple of times in a production enviorment, I take care to approve all database updates and create table backups to avoid this situation.

For this demo we will be pulling a sample database from https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=3&ved=2ahUKEwjksJX49Z7jAhVnRN8KHTmRD6gQFjACegQIBhAB&url=https%3A%2F%2Fgithub.com%2FMicrosoft%2Fsql-server-samples%2Freleases%2Fdownload%2Fadventureworks2012%2Fadventure-works-2012-dw-data-file.mdf&usg=AOvVaw31N3kiGC6iwsbcWwq6gcYP.
The download should prompt when you navigate to the link.


Next export the mdf and ldf files out of the zip file.

Now go to your SQL server Management Studio interface, right click Databases.

Select Attach Database.
You will be prompted with the Attach Database dialogue box.
Click Add... 
Now Navigate to where you extracted the Adventure Works MDF file.
You can remove the LDF from the file selection.

The default location for mdf files are similar to :
C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\DATA\
You can change the location to another drive if you wish.
Verify that the current File Path is the actual path to the MDF file.
Now Click Ok.

Right click on databases and refresh, your new database should be in the database list now!
