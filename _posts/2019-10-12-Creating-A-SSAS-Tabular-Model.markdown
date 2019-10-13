---
layout: post
title:  An introduction to SSAS Tabular models
date:   2019-10-12
description: How to get started creating a SSAS tabular model + deployment
img: sq.jpg # Add image post (optional)
tags: [SSMS,SSAS,Business Intelligence,Sql Server]

datatable: true
author: Ian Fogelman # Add name author (optional)
---
<meta property="og:title" content="How to get started creating a SSAS tabular model + deployment">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">


<br>
<br>
Today I will cover how to get a SSAS tabular up and running. SSAS is an OLAP mining tool that can help an enterprise track and manage KPI data and also assist in the ware housing process. 
<br>
<br>

First thing is first, make sure you have SQL server data tools installed. I was using SQL server 2017, so I needed to use a seperate standalone installer, but you can will know you have installed correctly if when you open visual studio 2017 and click new project you see the business intelligence option.

<br>
<br>

Once you have SSDT installed, click new project and under analysis services click Tabular for the project type, give your project a descriptive name.
<br>
<br>
![Features](/assets/img/SSASI001.png)
<br>
<br>
Make a note of the instance that you have your datawarehouse on we will need that information during the setup of our tabular model.
<br>
<br>
![Features](/assets/img/SSASI002.png)
<br>
<br>
Select a the Workspace Server as your instance.
<br>
<br>
![Features](/assets/img/SSASI003.png)
<br>
<br>
Once the project loads right click on Data Sources -> Import from Data Source, choose SQL Server
<br>
<br>
This will prompt the Table Import Wizard, provide a SQL Server Authed login to this diaologue and select the database.
<br>
<br>
Under impersonation information, select service account
<br>
<br>
Now choose the tables that will be imported

<br>
<br>

The import will run and bring all the table data into the SSAS tabular model
![Features](/assets/img/SSASI008.png)

<br>
<br>

Notice that if you browse the diagram view all the relationships are mapped out
<br>
<br>

Finally lets connect to the SSAS model and run a simple query
![Features](/assets/img/SSASI010.png)

![Features](/assets/img/SSASI004.png)
![Features](/assets/img/SSASI005.png)
![Features](/assets/img/SSASI006.png)
![Features](/assets/img/SSASI007.png)

![Features](/assets/img/SSASI009.png)

