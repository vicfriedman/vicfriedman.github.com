---
layout: post
title: "How To Use ActiveResource (and how it works for you)"
date: 2013-07-09 20:22
comments: true
categories: 
---
***ActiveResource provides an interface for communication between Rails applications.*** I came across ActiveResource in my databaseless classes at work that I mentioned <a href="http://vicfriedman.github.io/blog/2013/07/03/how-to-use-activemodel-callbacks/">in my previous post</a>. Essentially, all of our user functionality (logging in, creating a profile with attributes, etc.) is handled by one application and the meat of the site by another.

__And this is where ActiveResource comes in handly.__ In the simplest of terms, ActiveResource::Base is the main class, which maps RESTful resources as models in a rails application. This boils down to your RESTful resources becoming mutable Ruby objects. ActiveResource acts just like ActiveRecord, so it's very easy to understand at a first look. The only difference is that ActiveResource deals with HTTP resources that are built on standard JSON or XML request formatting as opposed to dealing with a database. In fact, if I hadn't noticed the: `class Profile < ActiveResource::Base`, I might have thought I was dealing with ActiveRecord objects.

__So let's take the example of this Profile class.__

__The first step is to create a model class (one that does not have a migration)__ that inherits from  `ActiveResource::Base` and that provides a site class variable.

    class Profile < ActiveResource::Base 
      self.site = "http://api/profiles:3000"
    end

My Profile class will be using ActiveResource to talk back and forth with `"http://api/profiles:3000"`

__Object manipulation in ActiveResource uses standard RESTful requests:__<br>
  1. GET requests for finding and retrieving resources<br>
  2. POST requests for creating new resources<br>
  3. PUT requests for updating resources<br>
  4. DELETE requests for destroying resources

__Creating an object will come from sending a POST request to the api.__
Using methods available to ActiveResource, this will look like:
      Victoria = Person.new(:first_name => "Victoria")
Looks a lot like ActiveResource huh? So what is ActiveResource actually doing here? 
      POST http://api/profiles/localhost:3000/profiles.json
It's sending a POST` request to the JSON of the profiles application. The body of the request looks something like this:
      {"Profile": {"first_name": "Victoria", "last_name": "Friedman"} }
The response you should get back is a 201 HTTP status code, which means the request has been fulfilled, as well as notification that your Ruby object was created.

__So now that we have an instance of my Profile class, how about we try and retrieve it.__ Finding an object with ActiveResource uses a GET request. With ActiveResource methods, this looks like:
      Victoria = Profile.find(1)
Again, the exact same thing as ActiveRecord. So how does this GET request come into play? A GET request is sent to http://api/profiles:3000/profiles/1.json. That's the JSON to Victoria Friedman's profile. Each JSON element becomes an attribute of an instance of my Profile class. I can also find all people by calling:
      profiles = Profile.all
This will return an array of JSON that looks something like this:
      [ {"id": 1, "first_name": "Victoria", "last_name": "Friedman"},
        {"id": 2, "first_name": "Megan", "last_name": "Smith"},
        {"id": 3, "first_name": "Catherine", "last_name": "Brooks"},
      ]

__So now we want to update an instance of our Profile class.__ This works the same in ActiveResource as ActiveRecord but uses a PUT request:
      Victoria = Profile.find(1)
      Victoria.first_name => 'Victoria'
      Victoria.first_name = 'Vic'
      Victoria.save
Now my name is saved as Vic. This submits a PUT request with a body that looks like this:
      {"person": {"first_name": "Vic"}}
This request returns an empty response with a 204 HTTP status code.


__So lastly, let's delete a resource. ActiveResource sends a DELETE request to the api.__
      Vic = Person.find(1)
      Vic.destroy
      Vic.exists? => false
This DELETE request sends an empty 200 HTTP status code as response.

__And voila, ActiveResource!__<br>


__Resources:__<br>
1. http://apidock.com/rails/ActiveResource/Base<br>
2. http://railscasts.com/episodes/94-activeresource-basics