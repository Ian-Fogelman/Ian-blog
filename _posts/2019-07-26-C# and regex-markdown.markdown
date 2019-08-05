---
layout: post
title:  C# Regex
date:   2019-07-26
description: C# and regex # Add post description (optional)
img: cs.png # Add image post (optional)
tags: [C#,Regex]

datatable: true
author: Ian Fogelman # Add name author (optional)
---

Oh hi didnt see you there... <br />
Say since your hear, have you ever wondered "hey I wanna read this csv data into my c# program, but I don't want to bother downloading the file.". <br />
If thats what your thinking, just take a look below friend, today I discuss how to do this exact task. <br />

As always, you can find the finished program here: <br />
https://rextester.com/WYCNA76267 

<br />
<br />
<br />



<br>
<br>
C# implementation of regex to check if parameter is a valid IP address.
<br>
<br>

{% highlight c# %}


//using System;
//using System.Collections.Generic;
//using System.Linq;
//using System.Text.RegularExpressions;


    public class Program
    {
        public static string CheckValidIP(string ipaddress)
        {
        string pattern = @"^((25[0-5]|2[0-4][0-9]|[01]?[0-9]{1,2})\.){3}(25[0-5]|2[0-4][0-9]|[01]?[0-9]{1,2})$";
        Match result = Regex.Match(ipaddress, pattern);
        if (result.Success) 
        {
            return result.Value.ToString();
        }
            
        else
        {
            return "Not Valid IP!";
        }
            
        }
        
        public static void Main(string[] args)
        { 
            Console.WriteLine(CheckValidIP("255.48.8.29"));
            Console.WriteLine(CheckValidIP("999.48.8.29"));
        }
    }
    
{% endhighlight %}   
