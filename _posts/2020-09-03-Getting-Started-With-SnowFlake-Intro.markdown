---
layout: post
title:  Getting-Started-With-SnowFlake
date:   2020-09-03
description: 2020-09-03-Getting-Started-With-SnowFlake
img: SF.png # Add image post (optional)
tags: [SnowFlake,SQL,AWS]

datatable: true
author: Ian Fogelman # Add name author (optional)
---

<meta property="og:title" content="Getting Started With SnowFlake">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">

<br>
<br>
Today we will be discussing a new and interesting cloud database technology called SnowFlake.
<br>
<br>

SnowFlake is a cloud based data warehouse solution which can be hosted in AWS, Azure or Google Cloud platform.
Initial trials of SnowFlake get a $400 credit 30 day trial, Snowflake breaks out charges into a credit system by compute and storage.
You can sign up for a free trial of SnowFlake here. 

<br>
<br>

For these post I am going to assume that you have signed up and stood up your test instance of SnowFlake.

<br>
<br>

We are going to cover how SnowFlake handles compute,loading some data from S3 into SnowFlake and parsing some JSON data into a flattened table format.

<br>
<br>
<h1>Compute</h1>
<br>
<br>
Compute in SnowFlake is handled by warehouses, warehouses can be sized according to need. For example you may have a warehouse for reporting, and another warehouse for data science query processing. You are not limited by the number of warehouses, these warehouses will consume credits when active and will auto suspend themselves when not in use. All these options can be configured in the Warehouses tab :

#![](/assets/img/SF1.PNG)

<br>
<br>
<h1>Loading Data into SnowFlake</h1>
<br>
<br>

