---
layout: post
title: "accepts_nested_attributes_for with name field in HTML"
date: 2013-10-09 20:34
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

My project at work is interesting because all the teams are very modularized. Yes, we all attend the same standup and have a general idea of what each individual is working on, but that being said the only time I'm really affected by what our front-end team does is when I have to include new markup on the rails side. I never even see the global.js or global.css files.

This week, I spent a lot of time working with one of our front-end developers. We split the work based on our specific abilities in an attempt to finish this sprint as quickly as possible. So this means she's controling the front end of one of our rails forms. I don't even have the html for the form in my code base. 

So this means in order to make this a form and have the user's input save to our database as objects, I had to coordinate the implementation of logic, including knowing what to pass in to the name field of the html for the part of the form the front-end developer controls.

I have three classes:

      class Gift
        has_many :gift_recipients
        has_many :recipients, :through => :gift_recipients

        accepts_nested_attributes_for :recipients
      end

      class Recipient
        has_many :gift_recipients
        belongs_to :gifts, :through => :gift_recipients
      end

      class Gift_Recipient
        belongs_to :gifts
        belongs_to :recipients
      end


My form in my views is used to be create a gift, and part of that form is also creating recipients for that gift. Essentially, a user checks a box and mini form appends to the page by javascript, allowing them to enter in a recipient's email and a message. 

`accepts_nested_attributes_for` allows the creation of a related object along with the main object. So in my example, I'm creating an instance of my `Gift` class while simultaneously creating an instance of my `Recipient` class. There is good documentation on this <a href="http://api.rubyonrails.org/classes/ActiveRecord/NestedAttributes/ClassMethods.html#method-i-accepts_nested_attributes_for">here</a>.

So now what? I don't have the html for the recipients section of the form in my code, so I have to use the HMTL `name` field to pass the user's input back to rails to save it in the database. I was familiar how to do this with just an attribute of an object. For an attribute of the gift, it would look like this:

      <input type="text" name="gift[name]" >

But how does that change with `accepts_nested_attributes_for` ?

    It's actually fairly simple:

    <input type="text" name="gift[recipients_attributes][0][email]">
    <input type="text" name="gift[recipients_attributes][0][message]">

This would be the input field for an email address for the first instance of `Recipient`. It follows the typical Rails convention of keeping the associated table name pluralized. The only thing to be aware of, is that the indexing changes based on how many instances of recipients you are creating.

So say my user pushes the button again and wants to create two recipients? The HTML would look a little something like this:

      <input type="text" name="gift[recipients_attributes][1][email]">
      <input type="text" name="gift[recipients_attributes][1][message]">


And voila, on submit, rails understands to create one instance of a gift, as well as two instances of recipients. The through relationship is what attaches my recipients to the gift, so there is naturally an association with that gift to those users.


