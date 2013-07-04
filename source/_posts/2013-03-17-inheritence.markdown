---
layout: post
title: "inheritence"
date: 2013-03-17 16:57
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


One of the big differences between Ruby and other object oriented languages is that a class in Ruby can inherit from one and only one class. Essentially, each class can only have one parent, but many children. 

A great example to understand basic inheritence is to think about it in terms of animals. A cat is a mammal. And all mammals are animals. To break this down in Ruby, that inheritence would look like this:

    class Animal
    end

    class Mammal < Animal
    end

    class Cat < Mammal
    end

The `<` denotes inheritence. In this example, the class Cat inherits all the properties from the Mammal class, as well as the Animal class. 

    class Animal

      def breathe
        "inhale"
      end
    end

    class Mammal < Animal

      def hair
        "Mammals are the only category of animals that have true hair"
      end
    end

    class Cat < Mammal

      def meow
        "MEOW"
      end
    end

In this case, all instances of the Cat class say "MEOW", have true hair, and breathe. However, they only inherit directly from Mammal, and Mammal only directly inherits from Animal. No instances of Mammal say "MEOW". 


Another great way to extend functionality to a class in Ruby is through the use of Modules.

A Module is a collection of methods and constants.  A module is made up of resuable code that doesn't naturally form a class. Because it is not a class, a module can't have instance methods.

Modules allow you to implement mixins. A mixin is a way to either include or extend the functionality of your Module to mulitple Ruby classes. 

When you Include a module in a class you are including the module methods as instance methods of that class. When you Extend a module in a class, you are adding the module methods as class methods. Essentially, when you declare `extend` in your classes, you are saying `self.extend`.

I found a great example of this functionality in action via <a href="http://rubysource.com/ruby-mixins-2/">Ruby Source</a>. 

    module Logging
      def logger
        @logger ||= Logger.new(opts)
      end
    end

    class Person
      include Logging
        def relocate
          logger.debug "Relocating person..."
            # do relocation stuff
        end
    end

In this example, the Logging module provides an instance of a logger (`@logger`). Because class Person includes the Logging module, all of it's methods are mixed-in as instances methods. The method logger is now a method that acts on all instances of the Person class.

Another option is to extend the Logger module:

      module Logging

        def logger
          @@logger ||= Logger.new(opts)
        end

      end

      class Person
        extend Logging
          
          def relocate
            Person.logger.debug "Relocating person..."

            # could also access it with this

            self.class.logger.debug "Relocating person..."

          end
        end

In this example, the Logger module as a class variable `@@logger` which allows this module to be extended to multiple classes. Because the module is extended to the class Person, the method defined in the module can only be called on the class itself, here it is demonstrated in two ways:
    Person.logger.debug
and
    self.logger.debug






<ol><h3>Helpful Resources:</h3>
<li>Inheritence: <a href="http://rubylearning.com/satishtalim/ruby_inheritance.html">Ruby Learning</a></li>
<li>Modules: <a href="http://www.ruby-doc.org/docs/ProgrammingRuby/html/tut_modules.html">The Pragmatic Programmer's Guide</a> </li>
<li>More Modules: <a href="http://ruby-doc.org/core-2.0/Module.html">Ruby Documentation</a></li>
</ol>


