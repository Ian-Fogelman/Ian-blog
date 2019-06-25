---
layout: post
title:  An introduction to Data Quality Services
date:   2019-06-24
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: sq.jpg # Add image post (optional)
tags: [DQS, SQL, Data Cleasing]

datatable: true
author: # Add name author (optional)
---

The use case for DQS is simple, a quick way to audit and correct data through an abstraction layer. This abstraction layer can be
managed by someone other than a DBA or developer and help improve your database's data quality and over all employee productivity.

<br>
<br>

First step is to install the DQS client, you can search for data quality server installer on your machine running your SQL instance.

<br>
<br>
![00](/assets/img/DQS00.PNG)

<br>
<br>

Lets take a simple set of data as follows :

<br>
<br>

<div class="container-fluid">
    <table class="datatable table table-hover table-bordered">
      <thead>
        <tr>
          <th>ID</th>
          <th>Fname</th>
          <th>Lname</th>
        </tr>
      </thead>
      <tfoot>
      </tfoot>
      <tbody>
        <tr>
          <td>1</td>
          <td>Ian</td>
          <td>Fogelman</td>
        </tr>
        <tr>
          <td>2</td>
          <td>Bill</td>
          <td>Gates</td>
        </tr>
        <tr>
          <td>3</td>
          <td>Jim</td>
          <td>Brown</td>
        </tr>
		        <tr>
          <td>4</td>
          <td>D</td>
          <td>Smith</td>
        </tr>
		<tr>
          <td>5</td>
          <td>J</td>
          <td>Green</td>
        </tr>
      </tbody>
    </table>
  </div>


<br>
<br>

Suppose that we want to enforce a business rule that each first name must be at least 3 characters.
We will solve this using a DQS approach.


<br>
<br>
First step is to create a knowledge base
<br>
<br>
![1](/assets/img/DQS1.png)
<br>
<br>
Create New Domain
<br>
<br>
![2](/assets/img/DQS2.png)
<br>
<br>
Input a name and a description for your domain, Click publish
<br>
<br>
![3](/assets/img/DQS3.png)
<br>
<br>
Now go to data quality projects, New Data Quality Project
Name the project and select the knowledge base created, for this project we will be cleansing.
<br>
<br>
![4](/assets/img/DQS4.png)
<br>
<br>
On the next screen match each field name with a domain, in this case we only have first name.
<br>
<br>
![5](/assets/img/DQS5.png)
<br>
<br>
![6](/assets/img/DQS6.png)
<br>
<br>
![7](/assets/img/DQS7.png)
<br>
<br>
![8](/assets/img/DQS8.png)
<br>
<br>
![9](/assets/img/DQS9.png)
<br>
<br>
![0](/assets/img/DQS0.png)
<br>
<br>
