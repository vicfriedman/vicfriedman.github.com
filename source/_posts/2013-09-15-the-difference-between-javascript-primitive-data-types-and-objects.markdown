---
layout: post
title: "The difference between Javascript Primitive Data Types and Objects"
date: 2013-09-15 12:59
comments: true
categories: 
---
***I've recently been reading Object Oriented Javascript per a colleague's reccomendation.*** One thing I got very stuck on, especially coming from Ruby, has been the difference between primitive data types and objects. In Ruby, everything is an object so the concept of primitive data types confused me entirely. Especially this:

      var word = "hello"
      typeof word => "string"

      var word = new String("hello")
      typeof word => "object"

__<a href ="https://www.destroyallsoftware.com/talks/wat"> WAT.</a>__ Literally that's what I thought. Until I dug a little deeper.

Javascript has five primitive data types:<br>
1. Number<br>
2. String<br>
3. Boolean<br>
4. Undefined<br>
5. Null<br>

__Anything that doesn't belong to any of these five primitive types is considered an object.__

__BUT each of these five primitive data types has a corresponding object constructor.__ So to take my above example, you can create a string in two different ways.

__First as a primitive data type:__
      var word = "hello";

__And then as an object:__
      var word = new String("hello");


__The constructor function uses the `new` operator.__ A string object is very similar to a hash of characters.
    
    word;
    => String {0: "h", 1: "e", 2: "l", 3: "l", 4: "o", formatUnicorn: function, truncate: function, splitOnLast: function, contains: function}

The word variable returns a has with each letter index.

__And each new object has methods associated with them based on what constructor was used.__ To continue with the string I created, there are plenty of methods available for string manipulation. (I've been told it's best practice to use primitive data types for strings if you don't plan on using a lot of the methods.)

    word.length; => 5
    word[0]; => "h"
    word.split(""); => ["h", "e", "l", "l", "o"]
    word.toUpperCase(); => "HELLO"


One cool thing, Javascript gives you the ability to treat primitive data types as object. Let's take a primitive string:
      var greeting = "hello";
      greeting.size; => 5

In this case, behind the scenes, Javascript turns my primitive data type into an object, runs the `size` method, and then transforms it back to the primitive data type.

You can even check the different between the two:
      var greeting = "hello";
      var word = new String("hello");
      word == greeting; => false

Conceptually, these are two totally different things. They're not the same type, as per my original confusion one is a string and one is an object. Objects can have additional properties, so those two variables shouldn't be equal because they're not. 
