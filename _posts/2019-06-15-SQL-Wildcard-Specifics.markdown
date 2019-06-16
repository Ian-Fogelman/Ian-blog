---
layout: post
title:  On a meeting with adventures
date:   2019-06-15 
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: sq.jpg # Add image post (optional)
tags: [SQL, Like, Wildcard]

datatable: true
author: # Add name author (optional)
---

Today I will touch on the wild card functionality in SQL. 
Many times you will be searching for records in a database table by some type of name.
Instead of looking specifically for an exact match on a name you can specify a wildcard search in your where statement by using a like clause.

<br>
<br>

Lets take the following dataset into consideration:

  <div class="container-fluid">
    <table class="datatable table table-hover table-bordered">
      <thead>
        <tr>
          <th>ID</th>
          <th>Name</th>
          <th>Address</th>
          <th>Class</th>
        </tr>
      </thead>
      <tfoot>
      </tfoot>
      <tbody>
        <tr>
          <td>1</td>
          <td>Jane</td>
          <td>123 Beech street</td>
          <td>Math</td>
        </tr>
        <tr>
          <td>2</td>
          <td>John</td>
          <td>1234 Smith street</td>
          <td>Math</td>
        </tr>
        <tr>
          <td>3</td>
          <td>Jasmin</td>
          <td>4432 Etcher Rd</td>
          <td>Science</td>
        </tr>
      </tbody>
    </table>
  </div>
  
