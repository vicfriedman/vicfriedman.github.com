---
layout: post
title: "How--and when--To Use A Struct"
date: 2013-07-24 20:04
comments: true
categories: 
---

***Recently I've been going through my ever growing (and then shrinking, and then growing again)*** list of new things/situations I've been introduced to via my code base at work. Which lead me to dig a little deeper into Ruby structs.


__A struct is a simple way to bundle attributes together__ without having to explicitly define a class, all the while giving you accessor methods for those attributes. You can read the Ruby Docs about structs <a href="http://www.ruby-doc.org/core-1.9.3/Struct.html">here</a>.

__We'll use the example of creating people.__

__A struct can be created very easily in two different ways:__
      Struct.new("Person", :name, :age, :address)
      Person = Struct.new(:name, :age, :address)

So now we have our Person struct, `Struct::Person`. This gives us really easy accessor methods for all three attributes of the struct that we defined.

__We can easily create new instances of this struct:__
      Struct::Person.new("Victoria", 24, "1835 University Circle")
      victoria = Person.new("Victoria", 24, "1835 University Circle")

__So how do we access the attribtues of this new instance of the struct?__  
      victoria["name"] #=> "Victoria"
      victoria["age"] #=> 24
      victoria["address"] #=> "1835 University Circle"

__You can also change the values of an instance of the struct:__
      victoria["name"] = "Vic"
      victoria["name"] #=> "Vic"

__So what's the difference between Struct and OpenStruct?__ OpenStruct doesn't require explicitly defined attributes. You can create them on the fly.

    person = OpenStruct.new
    person.name = "Victoria"
    person.age = 24
    person.date_of_birth = "June 1, 1988"

Notice when we created the OpenStruct, we didn't define any attribute fields, just the name. 


__Both Struct and OpenStruct closely resemble a hash, so why not just use that?__
You should think of a Struct as a convience class when you just want to store attributes and read/write to them with regular methods. A hash is a possibility, but a hash isn't fixed. There can be an infinite and varying number of key-value pairs. A Struct is the way to go when modeling out more complex objects with a known set of attributes.
