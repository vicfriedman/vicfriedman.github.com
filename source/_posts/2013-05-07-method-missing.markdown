---
layout: post
title: "method_missing"
date: 2013-05-07 14:53
comments: true
categories: 
---
Last night I learned about Ruby's `method_missing`. 

As Ruby code is executed, it travels up the chain of inheritence to discover where your methods are defined. It will execute the first line of code that matches the method name in the method lookup path. 

And of course, when there is an instance where you attempt to call an undefined method, the `NoMethodError` exception is raised. When this happens, Ruby searches up levels of inheritence until it reaches class BasicObject, which is the parent class of all classes in Ruby. Once it reaches BasicObject and still is unable to find a method, `NoMethodError` is returned.

This is where `method_missing` comes in to play. `method_missing` is a type of metaprogramming that allows programmers to call methods without explicitely defining them.

    class Test
      def method_missing(name, *args, &block)
        puts "NAME: #{name}"
        puts "ARG: #{args}"
        puts "BLOCK: #{block}"
      end

    end
  
    t = Test.new

    t.some_method("arbitrary argument")

    #=> NAME: some_method
        ARG: ["arbitrary argument"]
        BLOCK: 

In this example, the only method defined in my class is `method_missing`. I passed in the method name, optional arguments, and an optional block. Once I instantiate a new instance of the Test class, I call a method `some_method` on it. But `some_method` hasn't been defined. Normally this would return `NoMethodError` but because I used `method_missing`, `some_method` is accessible to me. 

As you can see in my definition of `method_missing`, I asked it to print out the method name, any potential arguments, and any potential blocks. `some_method` has method name and an argument, but not a block. Everything I attempted to call on my instance of the Test class is executed thanks to `method_missing`.