---
layout: post
title:  Python and SQLAlchemy
date:   2019-11-18
description: SQLalchemy enginge
img: sq.png # Add image post (optional)
tags: [Python,SQLAlchemy]

datatable: true
author: Ian Fogelman # Add name author (optional)
---
<meta property="og:title" content="Python and SQLAlchemy">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">

<br>
<br>

A quick example of how to create a SQLAlchemy engine connection string in python.
<br>
<br>

{% highlight graphQL %}

#https://api.yelp.com/v3/businesses/search
#https://www.yelp.com/developers/documentation/v3/business_search
{
    business(id: "garaje-san-francisco") {
        name
        id
        alias
        rating
        url
        phone
    		distance
    		is_closed
    
    		location {
    		  address1
    		  address2
    		  address3
    		  city
    		  state
    		  postal_code
    		  country
    		  formatted_address
    		}
    
    #https://api.yelp.com/v3/transactions/{transaction_type}/search
    transactions
    {
      restaurant_reservations {
        url
      }
    }
    
		#https://api.yelp.com/v3/businesses/{id}/reviews
    reviews{
      rating
      text
      user{
        profile_url
      }
    }
    
    }  
}

{% endhighlight %}
