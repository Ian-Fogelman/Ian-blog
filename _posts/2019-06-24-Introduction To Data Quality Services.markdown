---
layout: post
title:  An introduction to Data Quality Services
date:   2019-06-24
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: post-3.jpg # Add image post (optional)
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

![00](/assets/img/DQS00.jpg)

<br>
<br>
Create Knowledge Base
<br>
<br>
![0](/assets/img/DQS0.jpg)

<br>
<br>
Create New Domain
<br>
<br>

![1](/assets/img/DQS1.jpg)
![2](/assets/img/DQS2.jpg)
![3](/assets/img/DQS3.jpg)
![4](/assets/img/DQS4.jpg)
![5](/assets/img/DQS5.jpg)
![6](/assets/img/DQS6.jpg)
![7](/assets/img/DQS7.jpg)
![8](/assets/img/DQS8.jpg)
![9](/assets/img/DQS9.jpg)
