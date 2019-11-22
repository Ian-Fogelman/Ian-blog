---
layout: post
title:  Getting Started With GraphQL
date:   2019-11-22
description: GraphQL getting started
img: sq.png # Add image post (optional)
tags: [GraphQL]

datatable: true
author: Ian Fogelman # Add name author (optional)
---
<meta property="og:title" content="Getting Started With GraphQL">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">

<br>
<br>

A simple example of a what graphQL is.
<br>
<br>

The best way to think of what GraphQL is, just think of it as short hand expressions to query Restful APIs.
<br>
<br>
This is achieved by applying an abstract layer, aka GraphQL server layer between your API endpoints and clients.
<br>
<br>
Now not all API's inheritly support graph QL, but there is an extensive list of example api's here : https://github.com/APIs-guru/graphql-apis .

<br>
<br>
You can also code up your own, but for this example I will be using the yelp GraphQL service.
In the example below I put the restful API that a subquery would come from. Notice that instead of querying an entire API end point and joining that data all back together you can write the shorthand graphQL query (essentially JSON format) to extract your data. The idea is efficiency behind your efforts, doing more with less code.

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
