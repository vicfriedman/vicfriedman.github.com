---
layout: post
title: "sql has its moments"
date: 2013-06-25 21:44
comments: true
categories: 
---
IS THIS WORKING
__Today I learned, among a million other things, that I will never escape SQL statements.__ I think I've finally faced it. During week 6 at <a href="http://flatironschool.com">The Flatiron School</a>, when the magic of Rails and ActiveRecord was introduced, my entire class celebrated the fact SQL statements were abstracted away. We could write RUBY and still retrieve the same data, AUTOMAGICALLY. But let's face it... SQL will always be around.


__What do you do when you have a huge app running with over 100 instances,__ a mega-goliath database with more migrations and data entries than I've ever seen, and on top of those daunting figures, project managers wanting specific data for metrics.You use a SQL GUI and you write SQL statements.

__So this is how I came across a really interesting discovery.__ Well two actually: SQL has `count` and `distinct` keywords available with which to write queries. 


Here is an example of a table I was working on. This table is called photos:
  {% img /images/post_images/photos_table2.png %}



__I was told to write SQL queries to find the number of photos__ that have been uploaded by users in the last three weeks (so from after June 6), as well as the number of users that have updated photos.

__1. Number of photos uploaded by users on or after June 6, 2013:__

`select count(id) from photos where user_id is not null and created_at >= 6/6/2013`

This command is parsing through the data in our database, finding all instances where a user_id is present (in this case all of them), as well as parsing through the created_at dates to find a match and returning to us that number of photos.
This command should return five, as there are five photos uploaded by users in the sample database.


__2. Number of users who have uploaded photos on or after June 6, 2013:__

The next query was a little bit more challengeing because I was unaware of the SQL `distinct` method.

Originally, I had: 

`select count(profile_id) from photos where profile_id is not null and created_at >= 6/6/2013`

 But this query returns five.The problem with this query wasn't immediately obvious to me. Count doesn't discrimate against repeated entries. There are FIVE cells with a user_id, just not five unique id's. 

My first thought was the `uniq` method. Like when you have an array that contains duplicates, you can call `uniq` on that array to return that array with the duplicates removed. But unique is a reserved word in SQL, and is used to create unique indexes (an index can be created in a table to find data more easily and efficiently). So that's when I was introduced to `distinct`.

With that new knowledge, I was able to correct my first attempt at the query,

`select count(distinct user_id) from photos where user_id is not null and created_a >= 6/6/2013`

And that statement returns the correct result, which is three.
