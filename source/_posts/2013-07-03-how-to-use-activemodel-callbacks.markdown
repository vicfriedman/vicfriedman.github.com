---
layout: post
title: "How To Use ActiveModel::Callbacks"
date: 2013-07-03 07:53
comments: true
categories: 
---

***My first big task at work has been to fix all the broken tests in our test suite.*** It's actually a great assignment. I'll learn all the intricacies and relationships of the code, without the risk of breaking something major...the tests are already broken.

__While reviewing one of the model specs today,__ I came something that I was not familiar with: ActiveModel::Callbacks 

__A callback is a method that acts as a hook into the life cycle of an object.__ Callbacks allow you to trigger logic before or after the alteration of an ActiveRecord object. 

__ActiveModel callbacks are an interface for any class to have ActiveRecord-like callbacks, used especially in the case of databaseless models. In <a href="http://railscasts.com/episodes/219-active-model?view=comments">this</a> Rails Cast, Ryan Bates gives a great intro to ActiveModel and how to get it up and running in your app. In the example of my code, we are just using the callbacks module. ActiveModel also provides a bunch of other modules, including but not limited to: naming, serialization, validations, etc. You can find the entire list <a href="http://api.rubyonrails.org/classes/ActiveModel.html">here</a>.

__The first step to use any of the modules is to `include` or `extend` the module in your class.__ See my <a href="http://vicfriedman.github.io/blog/2013/03/17/inheritence/">previous post</a> for an explanation on the difference between those options to mix in a modules functionality.


      class Pet
        extend ActiveModel::Callbacks
      end

Here, I chose to `extend` the module because I want the methods to become class methods.


__The next step is to define a list of methods to which you would like to attach the callbacks:__
      
      class Pet
        extend ActiveModel::Callbacks

        define_model_callbacks :create, :destroy, :update
      end

For example purposes, I chose to include three common callbacks. Once you defined the methods that are attached to callbacks, they now have access to all three standard callbacks `before`, `after`, and `around`. 
To take `create` as an example, I now have `before_create`, `after_create`, and `around_create` accessible to me.

__The next thing is to actually define the `create`, `destroy`, and `update methods`.__

      class Pet
        extend ActiveModel::Callbacks

        define_model_callbacks :create, :destroy, :update

        def create
          _run_create_callbacks do
            ##CODE TO EXECUTE HERE
          end
        end


        def update
          _run_update_callbacks do
            ##CODE TO EXECUTE HERE
          end
        end


        def destroy
          _run_destroy_callbacks do
            ##CODE TO EXECUTE HERE
          end
        end

      end


You can also shorthand this:
    def create; _run_create_callback { #CODE_HERE }; end
    def update; _run_update_callback { #CODE_HERE }; end
    def destroy; _run_destroy_callback { #CODE_HERE }; end




__And finally, now you can use your callbacks!__

      class MyClass
        extend ActiveModel::Callbacks

        define_model_callbacks :create, :destroy, :update

        before_update :my_method

        def create
          _run_create_callbacks do
            ##CODE TO EXECUTE HERE
          end
        end


        def update
          _run_update_callbacks do
            ##CODE TO EXECUTE HERE
          end
        end


        def destroy
          _run_destroy_callbacks do
            ##CODE TO EXECUTE HERE
          end
        end

        def my_method
          puts "This is my method"
        end

      end

I have used mine here, so before any instance of `MyClass` is updated, `my_method` is run, which will just print the string "This is my method".

Also, in playing around, I discovered that you can literally name your callbacks whatever you want. I tried with `fun`, which would look something like this:

      class Pet
        extend ActiveModel::Callbacks

        define_model_callbacks :fun

        before_fun :my_method

        def fun
          _run_fun_callbacks do
            ##CODE TO EXECUTE HERE
          end
        end

        def my_method
          puts "This is my method"
        end

      end

The possibilities are ENDLESS. Literally. 


<ol>Great Resources:
  <li>http://api.rubyonrails.org/classes/ActiveModel/Callbacks.html</li>
  <li>http://jeffgardner.org/2011/05/26/adding-activerecord-style-callbacks-to-activeresource-models/</li>
  <li>http://railscasts.com/episodes/219-active-model</li>
</ol>
