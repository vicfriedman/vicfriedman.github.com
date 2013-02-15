---
layout: post
title: "The Each Method"
date: 2013-02-15 07:40
comments: true
categories: 
---

<p><strong>Today marks day 9 into my 60 day course</strong> at <a href="flatironschool.com"> the Flatiron School. </a> Essentially, I'm 15% done. I would best summarize the last 9 days with these simple words: anxiety, frustration, and (sometimes) victory.<p>

<p><strong>The thing about this course,</strong> essentially a "full-immersion language course", is the pace in which things are taught. It's fast. I was prepared for fast, but not this fast. We've been told from day one, "If you don't understand something now, don't worry. You will use these methods for the rest of your life, and at some point, they will make sense." I'm not used to moving on to new material before I grasp the current material, but we've also been told to trust the structure of the program, so I have been.</p>

<p><strong>So each day has been filled with new material,</strong> and thus new frustrations. A day hasn't gone by where I've been utterly and completely lost. I don't want to define myself as the potential crier for the class, but....I could be.</p>

<p><strong>Yesterday, after spending close to three hours on homework,</strong> I called it a night. And then something happened, a moment of realization. I UNDERSTAND ITERATIONS. Two days prior, literally a mere 48 hours, iterations were some mystical thing that I was struggling to grasp. And without even realizing it, I had been using the each method with ease in my programs. AND I DIDN'T EVEN REALIZE THAT I UNDERSTOOD THEM. The pace of the material kept me from noticing when material had sunk in.</p>

<p><strong>And with that introduction, I give you the each method.</strong></p>

<p>Say you have an array of candy:</p>

<img src ="/images/post_images/array.png" alt ="Image of and array containing reeces, twizzlers, mambas and snickers">


<p>The each method iterates, or loops, through each item in an array. In layman's terms, the method acts as a pointer and moves through the array, first to "reeces", then to "twizzlers", then "mambas", and finally to "snickers". Objects that can be iterated through are called enumerables. In Ruby, strings, hashes, and arrays are all enumerators.</p>

Once the array has been defined, we can implement the each method on that array.
The each method only returns the original array, meaning all it does is move through each index. Nothing is actually done to or with the data, and no new array is created.

<img src ="/images/post_images/array_each_do.png" alt ="Image of and array containing reeces, twizzlers, mambas and snickers">


<p>However, you can do things like this: </p>

<img src ="/images/post_images/array_puts.png" alt ="Image of and array containing reeces, twizzlers, mambas and snickers">


<p>Here I added a puts in my block of code. The |candy| refers to each type of candy included my array. In my string, I refer back to each individual instance of candy, which then includes it in the string.
But take note of the actual return value from Ruby, it is still the original array. Again, the each method does not do anything to the original array, it simply takes stock of each item in the array as it passes through and implements the code included in the block (which here is to puts my string).</p>
