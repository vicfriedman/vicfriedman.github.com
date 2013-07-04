---
layout: post
title: "a lesson in linked lists"
date: 2013-04-22 13:43
comments: true
categories: 
---

<script type="text/javascript">

  var _gaq = _gaq || [];
  _gaq.push(['_setAccount', 'UA-38989132-1']);
  _gaq.push(['_trackPageview']);

  (function() {
    var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
    ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
    var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
  })();

</script>
***This morning, I learned what a linked list is.*** A linked list is a data structure that is comprised on a group of nodes. Essentially, each dataset is made up of it's information, and a pointer. A pointer is a link to the next node in the data sequence. In the simplest of terms, if I am a dataset, I only know about myself, and the person that comes after me. 

***Linked lists are most commonly compared to arrays.*** They both store data, and both have their pros and cons. In compiled languages, arrays are a fixed size. If you define an array to have 100 elements, it has 100 elements. While there are methods to resize an array in compiled languages, from what I've read, it's not an easy task. Linked lists allow for a significantly easier process for inserting and deleting data, without shifting entire groups of elements up or down. 

***So here is an example of a basic link list:***

{% img /images/linklist1.png %}

This small data sequence is going to be listed in alphabetical order. All linked lists start with HEAD, which denotes the begining of the data sequence. The head contains a pointer, which points to the first object in the data set. In this case, HEAD points to "Boston". The first dataset, contains the name of the city, and a pointer to the following dataset, here it is San Fransico. San Francisco points to the end, which is always signified by NULL. The last dataset will always point to NULL.

So in this case, Boston only knows that it exists, and knows about San Fransico. San Francisco only knows they exist, and that NULL exists. They don't know anything about Boston.

So the process for inserting another city is fairly simple. Say we wanted to insert New York. Because of the alphabetical order, you will want to insert it between Boston and San Francisco. To do so, all you need to do is make New York point to San Francisco, and change Boston to point to New York. There is no shifting of indexes, or anything like that. 

{% img /images/linklist2.png %}

However the process of searching a linked list is difficult. While in an array, if you were trying to find San Francisco you should simply do `array[2]` to access San Francisco, it's a much more expensive process in linked lists. In this final example, in order to search for San Francisco, you have to start at head, move through Boston and New York, to finally arrive at the data you are searching for. 


<ol>Resources:
<li>http://cslibrary.stanford.edu/103/LinkedListBasics.pdf</li>
<li>http://www.youtube.com/watch?v=LOHBGyK3Hbs</li>
<li>www.youtube.com/watch?v=oiW79L8VYXk</li>
</ol>