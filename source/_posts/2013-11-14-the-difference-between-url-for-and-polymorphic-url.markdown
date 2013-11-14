---
layout: post
title: "The Difference between url_for and polymorphic_url"
date: 2013-11-14 09:03
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


__`url_for` is a great Rails helper that returns a URL__ that has been rewritten according to the options hash passed to it and defined routes. Rails passes all the keys in the options hash to the routing module. There are a few exceptions, including (but not limited to) `:only_path`, `:trailing_slash`, and `:host`. For a full list please see the docs <a href="http://apidock.com/rails/ActionController/Base/url_for">here</a>. <br><br>

            <%= url_for :controller => "recipes", :action => "show", :id => 1 %>

This would generate a url that looks like this:'http://domain.com/recipes/show/10'<br><br>

**But let's say you were trying to generate a URL** for a polymorphic object, or a nested object, or you just want the absolute path. Let's take an image object. My image class is polymorphic, and thus belongs to every model as imageable.<br><br>

			<%= url_for @image :only_path => false %>

**Setting `:only_path` to false generates the full url instead** of just the relative path. `url_for` is really like `path_for`. You have to pass it an additional argument to return the full URL. But that example returns an error: Wrong number of arguments(2 for 1). Essentially in the example above, you're passing two arguments-- an object and a hash. <br><br>

But, if you only pass one argument, the object…<br><br>

      			<%=url_for @image %>

**This produces a relative path to the image.** When you pass an ActiveRecord object to url_for, it causes a lookup on the image class, essentially calling @image.to_params, and use image_path to generate the path.<br><br>

**So how then do you get the full URL** with the domain name without doing some hackey `"http://mydomain.com" + < my_query_string_generated_from_url_for >` ?<br><br>

**This is where `polymorphic_url` comes in.** `polymorphic_url` is a Rails helper method for calling a named route. It also provides support for nested resources. This helper method is designed to return a full URL, instead of just the relative path, without passing any arguments. If you did want the relative path, `polymorphic_path` does just that.<br><br>

So let's say we're on an image record<br><br>

			<%= polymorphic_url(@image) %>

That would be the same as calling `image_url(@image)`. Rails would return "http://mydomain.com/images/1"<br><br>

**So now say you want to access the images from the recipes.** This is where the nested support of polymorphic_url comes into play.<br><br>


			<%= polymorphic_url([recipe, image])%>

This would return "http://mydomain.com/recipes/1/images/1"<br><br>


So when do you use these handy helpers?A path should always be used unless you're doing a full redirect.<br><br>

**Additional Resources** <br>
1. <a href="http://apidock.com/rails/ActionController/Base/url_for">The Docs for url_for</a>.<br>
2.  <a href="http://apidock.com/rails/ActionController/PolymorphicRoutes/polymorphic_url">The Docs for polymorphic_url</a>




