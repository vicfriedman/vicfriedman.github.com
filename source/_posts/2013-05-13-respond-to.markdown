---
layout: post
title: "respond_to"
date: 2013-05-13 12:01
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

My last post was about `method_missing`. Since that post, I've gotten to play around with `method_missing` a lot, and think I've begun to grasp its power. It's a crazy tool, and one that can be dangerous if not used wisely.

One thing I learned while rewriting the functionality of `method_missing` is that you must reopen and rewrite `respond_to` as well. `respond_to` is used to determine if an object responds to a method, i.e. allows the object to know a method is being called on it.

Becase `method_missing` allows you to call undefined methods, the object needs to know in some capicty what those are.
In the below example, I have written `method_missing` to be able to use `.get_*` and `.set_*` in order to access object in my database. The method name gets passed as a symbol to method missing, so it must be turned into a string before being used. 
    
    def self.method_missing(method_name, *args, &block)
      method_string = method_name.to_s
      if method_string.start_with?("get_")
        #do xyz
      elsif method_string.start)with?("set_")
        #do xyz
      else
        super
      end
    end

    def self.respond_to?(method_sym, include_private = false)
      if method_sym.to_s =~ /^get_(.*)$/ || method_sym.to_s =~ /^set_(.*)$/
        true
      else
        super
      end
    end


Here, `respond_to` now responds to my version of `method_missing`. I am essentially searching if the string version of my method includes `get_*` or `set_*` and if it does, it is a method that can be called on an object.

The `include_private = false` is necessary because by default, `respond_to` must be passed additional parameters to search for private methods. Methods are private for a reason, they don't need to be accessed everywhere and by every object. 

Also in both `method_missing` and `respond_to` my if/else clause ends by calling `super`. Essentially, if none of the above returns true, go back to your normal functionality and deliver me an error message.

Also note I am using class methods here. My practice with `method_missing` and `respond_to` was geared towards reading and writing objects to my database.

